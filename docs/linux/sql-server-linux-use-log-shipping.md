---
title: Configuración del trasvase de registros para SQL Server en Linux
description: En este tutorial se muestra un ejemplo básico de cómo replicar una instancia de SQL Server en Linux en una instancia secundaria mediante el trasvase de registros.
author: VanMSFT
ms.author: vanto
ms.date: 07/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 9385bcc04935c90ff060201d38e676b6da226e33
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346214"
---
# <a name="get-started-with-log-shipping-on-linux"></a>Introducción al trasvase de registros en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

El trasvase de registros de SQL Server es una configuración de alta disponibilidad donde una base de datos de un servidor principal se replica en uno o varios servidores secundarios. En pocas palabras, se restaura una copia de seguridad de la base de datos de origen en el servidor secundario. Después, el servidor principal crea copias de seguridad del registro de transacciones periódicamente y los servidores secundarios las restauran, lo que actualiza la copia secundaria de la base de datos. 

  ![Diagrama en el que se muestra el flujo de trabajo de trasvase de registros.](https://preview.ibb.co/hr5Ri5/logshipping.png)

Tal y como se describe en esta imagen, una sesión de trasvase de registros consta de los siguientes pasos:

- Realización de una copia de seguridad del archivo de registro de transacciones en la instancia principal de SQL Server
- Copia del archivo de copia de seguridad del registro de transacciones a través de la red en una o varias instancias secundarias de SQL Server
- Restauración del archivo de copia de seguridad del registro de transacciones en la instancia secundaria de SQL Server

## <a name="prerequisites"></a>Prerequisites
- [Instalación del Agente SQL Server en Linux](./sql-server-linux-setup-sql-agent.md)

## <a name="setup-a-network-share-for-log-shipping-using-cifs"></a>Configuración de un recurso compartido de red para el trasvase de registros mediante CIFS 

> [!NOTE] 
> En este tutorial se usa CIFS + Samba para configurar el recurso compartido de red. Si quiere usar NFS, deje un comentario y lo agregaremos al documento.       

### <a name="configure-primary-server"></a>Configuración del servidor principal
-   Ejecute lo siguiente para instalar Samba:

    ```bash
    sudo apt-get install samba #For Ubuntu
    sudo yum -y install samba #For RHEL/CentOS
    ```
-   Cree un directorio para almacenar los registros para el trasvase de registros y proporcione a MSSQL los permisos necesarios.

    ```bash
    mkdir /var/opt/mssql/tlogs
    chown mssql:mssql /var/opt/mssql/tlogs
    chmod 0700 /var/opt/mssql/tlogs
    ```

-   Edite el archivo/etc/samba/smb.conf (necesita permisos de raíz para hacerlo) y agregue la siguiente sección:

    ```bash
    [tlogs]
    path=/var/opt/mssql/tlogs
    available=yes
    read only=yes
    browsable=yes
    public=yes
    writable=no
    ```

-   Cree un usuario de MSSQL para Samba.

    ```bash
    sudo smbpasswd -a mssql
    ```

-   Reinicie los servicios de Samba.
    ```bash
    sudo systemctl restart smbd.service nmbd.service
    ```
 
### <a name="configure-secondary-server"></a>Configuración del servidor secundario

-   Ejecute lo siguiente para instalar el cliente de CIFS.
    ```bash   
    sudo apt-get install cifs-utils #For Ubuntu
    sudo yum -y install cifs-utils #For RHEL/CentOS
    ```

-   Cree un archivo para almacenar sus credenciales. Use la contraseña que ha establecido recientemente para la cuenta Samba de MSSQL. 

    ```console
        vim /var/opt/mssql/.tlogcreds
        #Paste the following in .tlogcreds
        username=mssql
        domain=<domain>
        password=<password>
    ```

-   Ejecute los siguientes comandos para crear un directorio vacío para montar y establecer el permiso y la propiedad correctamente:
    ```bash   
    mkdir /var/opt/mssql/tlogs
    sudo chown root:root /var/opt/mssql/tlogs
    sudo chmod 0550 /var/opt/mssql/tlogs
    sudo chown root:root /var/opt/mssql/.tlogcreds
    sudo chmod 0660 /var/opt/mssql/.tlogcreds
    ```

-   Agregue la línea a etc/fstab para conservar el recurso compartido. 

    ```console
        //<ip_address_of_primary_server>/tlogs /var/opt/mssql/tlogs cifs credentials=/var/opt/mssql/.tlogcreds,ro,uid=mssql,gid=mssql 0 0
    ```

-   Monte los recursos compartidos.
    ```bash   
    sudo mount -a
    ```
       
## <a name="setup-log-shipping-via-t-sql"></a>Configuración del trasvase de registros mediante T-SQL

- Ejecute este script desde el servidor principal:

    ```sql
    BACKUP DATABASE SampleDB
    TO DISK = '/var/opt/mssql/tlogs/SampleDB.bak'
    GO
    ```

    ```sql
    DECLARE @LS_BackupJobId AS uniqueidentifier 
    DECLARE @LS_PrimaryId   AS uniqueidentifier 
    DECLARE @SP_Add_RetCode As int 
    EXECUTE @SP_Add_RetCode = master.dbo.sp_add_log_shipping_primary_database 
             @database = N'SampleDB' 
            ,@backup_directory = N'/var/opt/mssql/tlogs' 
            ,@backup_share = N'/var/opt/mssql/tlogs' 
            ,@backup_job_name = N'LSBackup_SampleDB' 
            ,@backup_retention_period = 4320
            ,@backup_compression = 2
            ,@backup_threshold = 60 
            ,@threshold_alert_enabled = 1
            ,@history_retention_period = 5760 
            ,@backup_job_id = @LS_BackupJobId OUTPUT 
            ,@primary_id = @LS_PrimaryId OUTPUT 
            ,@overwrite = 1 

    IF (@@ERROR = 0 AND @SP_Add_RetCode = 0) 
    BEGIN 

    DECLARE @LS_BackUpScheduleUID   As uniqueidentifier 
    DECLARE @LS_BackUpScheduleID    AS int 

    EXECUTE msdb.dbo.sp_add_schedule 
            @schedule_name =N'LSBackupSchedule' 
            ,@enabled = 1 
            ,@freq_type = 4 
            ,@freq_interval = 1 
            ,@freq_subday_type = 4 
            ,@freq_subday_interval = 15 
            ,@freq_recurrence_factor = 0 
            ,@active_start_date = 20170418 
            ,@active_end_date = 99991231 
            ,@active_start_time = 0 
            ,@active_end_time = 235900 
            ,@schedule_uid = @LS_BackUpScheduleUID OUTPUT 
            ,@schedule_id = @LS_BackUpScheduleID OUTPUT 

    EXECUTE msdb.dbo.sp_attach_schedule 
            @job_id = @LS_BackupJobId 
            ,@schedule_id = @LS_BackUpScheduleID  

    EXECUTE msdb.dbo.sp_update_job 
            @job_id = @LS_BackupJobId 
            ,@enabled = 1 
            
    END 

    EXECUTE master.dbo.sp_add_log_shipping_alert_job 

    EXECUTE master.dbo.sp_add_log_shipping_primary_secondary 
            @primary_database = N'SampleDB' 
            ,@secondary_server = N'<ip_address_of_secondary_server>' 
            ,@secondary_database = N'SampleDB' 
            ,@overwrite = 1 
    ```


- Ejecute este script desde el servidor secundario:

    ```sql
    RESTORE DATABASE SampleDB FROM DISK = '/var/opt/mssql/tlogs/SampleDB.bak'
    WITH NORECOVERY;
    ```
    
    ```sql
    DECLARE @LS_Secondary__CopyJobId    AS uniqueidentifier 
    DECLARE @LS_Secondary__RestoreJobId AS uniqueidentifier 
    DECLARE @LS_Secondary__SecondaryId  AS uniqueidentifier 
    DECLARE @LS_Add_RetCode As int 

    EXECUTE @LS_Add_RetCode = master.dbo.sp_add_log_shipping_secondary_primary 
            @primary_server = N'<ip_address_of_primary_server>' 
            ,@primary_database = N'SampleDB' 
            ,@backup_source_directory = N'/var/opt/mssql/tlogs/' 
            ,@backup_destination_directory = N'/var/opt/mssql/tlogs/' 
            ,@copy_job_name = N'LSCopy_SampleDB' 
            ,@restore_job_name = N'LSRestore_SampleDB' 
            ,@file_retention_period = 4320 
            ,@overwrite = 1 
            ,@copy_job_id = @LS_Secondary__CopyJobId OUTPUT 
            ,@restore_job_id = @LS_Secondary__RestoreJobId OUTPUT 
            ,@secondary_id = @LS_Secondary__SecondaryId OUTPUT 

    IF (@@ERROR = 0 AND @LS_Add_RetCode = 0) 
    BEGIN 

    DECLARE @LS_SecondaryCopyJobScheduleUID As uniqueidentifier 
    DECLARE @LS_SecondaryCopyJobScheduleID  AS int 

    EXECUTE msdb.dbo.sp_add_schedule 
            @schedule_name =N'DefaultCopyJobSchedule' 
            ,@enabled = 1 
            ,@freq_type = 4 
            ,@freq_interval = 1 
            ,@freq_subday_type = 4 
            ,@freq_subday_interval = 15 
            ,@freq_recurrence_factor = 0 
            ,@active_start_date = 20170418 
            ,@active_end_date = 99991231 
            ,@active_start_time = 0 
            ,@active_end_time = 235900 
            ,@schedule_uid = @LS_SecondaryCopyJobScheduleUID OUTPUT 
            ,@schedule_id = @LS_SecondaryCopyJobScheduleID OUTPUT 

    EXECUTE msdb.dbo.sp_attach_schedule 
            @job_id = @LS_Secondary__CopyJobId 
            ,@schedule_id = @LS_SecondaryCopyJobScheduleID  

    DECLARE @LS_SecondaryRestoreJobScheduleUID  As uniqueidentifier 
    DECLARE @LS_SecondaryRestoreJobScheduleID   AS int 

    EXECUTE msdb.dbo.sp_add_schedule 
            @schedule_name =N'DefaultRestoreJobSchedule' 
            ,@enabled = 1 
            ,@freq_type = 4 
            ,@freq_interval = 1 
            ,@freq_subday_type = 4 
            ,@freq_subday_interval = 15 
            ,@freq_recurrence_factor = 0 
            ,@active_start_date = 20170418 
            ,@active_end_date = 99991231 
            ,@active_start_time = 0 
            ,@active_end_time = 235900 
            ,@schedule_uid = @LS_SecondaryRestoreJobScheduleUID OUTPUT 
            ,@schedule_id = @LS_SecondaryRestoreJobScheduleID OUTPUT 

    EXECUTE msdb.dbo.sp_attach_schedule 
            @job_id = @LS_Secondary__RestoreJobId 
            ,@schedule_id = @LS_SecondaryRestoreJobScheduleID  
            
    END 
    DECLARE @LS_Add_RetCode2    As int 
    IF (@@ERROR = 0 AND @LS_Add_RetCode = 0) 
    BEGIN 

    EXECUTE @LS_Add_RetCode2 = master.dbo.sp_add_log_shipping_secondary_database 
            @secondary_database = N'SampleDB' 
            ,@primary_server = N'<ip_address_of_primary_server>' 
            ,@primary_database = N'SampleDB' 
            ,@restore_delay = 0 
            ,@restore_mode = 0 
            ,@disconnect_users  = 0 
            ,@restore_threshold = 45   
            ,@threshold_alert_enabled = 1 
            ,@history_retention_period  = 5760 
            ,@overwrite = 1 

    END 

    IF (@@error = 0 AND @LS_Add_RetCode = 0) 
    BEGIN 

    EXECUTE msdb.dbo.sp_update_job 
            @job_id = @LS_Secondary__CopyJobId 
            ,@enabled = 1 

    EXECUTE msdb.dbo.sp_update_job 
            @job_id = @LS_Secondary__RestoreJobId 
            ,@enabled = 1 

    END 
    ```

## <a name="verify-log-shipping-works"></a>Comprobación de que el trasvase de registros funciona

- Compruebe que el trasvase de registros funciona iniciando el siguiente trabajo en el servidor principal:

    ```sql
    USE msdb ;  
    GO  

    EXECUTE dbo.sp_start_job N'LSBackup_SampleDB' ;  
    GO  
    ```

- Compruebe que el trasvase de registros funciona iniciando el siguiente trabajo en el servidor secundario:
 
    ```sql
    USE msdb ;  
    GO  

    EXECUTE dbo.sp_start_job N'LSCopy_SampleDB' ;  
    GO  
    EXECUTE dbo.sp_start_job N'LSRestore_SampleDB' ;  
    GO  
    ```
 - Ejecute el comando siguiente para comprobar que la conmutación por error del trasvase de registros funciona:
 
    > [!WARNING]
    > Este comando pondrá en línea la base de datos secundaria e interrumpirá la configuración del trasvase de registros. Tendrá que volver a configurar el trasvase de registros después de ejecutar este comando.
 
    ```sql
    RESTORE DATABASE SampleDB WITH RECOVERY;
    ```