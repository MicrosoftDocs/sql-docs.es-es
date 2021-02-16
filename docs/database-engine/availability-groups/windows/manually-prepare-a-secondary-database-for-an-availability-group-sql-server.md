---
title: Preparación de una base de datos secundaria para un grupo de disponibilidad
description: Una descripción de cómo preparar manualmente una base de datos secundaria para unirse a un grupo de disponibilidad Always On.
ms.custom: seodec18
ms.date: 07/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.preparedbs.f1
- sql13.swb.availabilitygroup.configsecondarydbs.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- secondary databases [SQL Server]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 9f2feb3c-ea9b-4992-8202-2aeed4f9a6dd
author: cawrites
ms.author: chadam
ms.openlocfilehash: 772c020101b8dd5252e3c5c6971b2129d9c93863
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348402"
---
# <a name="prepare-a-secondary-database-for-an-always-on-availability-group"></a>Preparación de una base de datos secundaria para un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
En este tema se describe cómo preparar una base de datos de un grupo de disponibilidad AlwaysOn en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell. Para preparar una base de datos hay que realizar dos pasos: 

1. Restaurar una copia de seguridad reciente de la base de datos y las copias de seguridad de registros subsiguientes en cada instancia del servidor que hospede la réplica secundaria, usando para ello RESTORE WITH NORECOVERY
2. Combinar la base de datos restaurada con el grupo de disponibilidad  
  
> [!TIP]  
>  Si tiene una configuración de trasvase de registros, es posible que pueda convertir la base de datos principal de trasvase de registros junto con una o varias de sus bases de datos secundarias en una réplica principal del grupo de disponibilidad y en una o varias réplicas secundarias. Para más información, vea [Requisitos previos para migrar desde grupos de trasvase de registros a grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md).  

##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Requisitos previos y restricciones  
  
-   Asegúrese de que el sistema en donde piensa colocar la base de datos posee una unidad de disco con espacio suficiente para las bases de datos secundarias.  
  
-   La base de datos secundaria debe tener el mismo nombre que la base de datos principal.  
  
-   Use RESTORE WITH NORECOVERY para cada operación de restauración.  
  
-   Si la base de datos secundaria debe residir en una ruta de acceso de archivo diferente (incluida la letra de unidad) de la de la base de datos principal, el comando de restauración debe utilizar la opción WITH MOVE para cada uno de los archivos de base de datos para especificarlos a la ruta de acceso de la base de datos secundaria.  
  
-   Si restaura la base de datos grupo de archivos por grupo de archivos, asegúrese de restaurar la base de datos completa.  
  
-   Después de restaurar la base de datos, debe restaurar (WITH NORECOVERY) cada copia de seguridad del registro creada desde la última copia de seguridad de datos restaurada.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   En las instancias independientes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]se recomienda que, si es posible, la ruta de acceso de archivo (incluida la letra de unidad) de una base de datos secundaria determinada sea idéntica a la de la base de datos principal correspondiente. Esto se debe a que si se mueven los archivos de base de datos al crear una base de datos secundaria, una operación posterior de agregar archivo podría producir un error en la base de datos secundaria y hacer que esta se suspenda.  
  
-   Antes de preparar las bases de datos secundarias, se recomienda encarecidamente suspender las copias de seguridad del registro programadas en las bases de datos del grupo de disponibilidad hasta que la inicialización de las réplicas secundarias se haya completado.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 Cuando se realiza una copia de seguridad de una base de datos, la [propiedad de base de datos TRUSTWORTHY](../../../relational-databases/security/trustworthy-database-property.md) se establece en OFF. Por lo tanto, TRUSTWORTHY está siempre en OFF en una base de datos que se acaba de restaurar.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, los permisos BACKUP DATABASE y BACKUP LOG corresponden a los miembros del rol fijo de servidor **sysadmin** y de los roles fijos de base de datos **db_owner** y **db_backupoperator** . Para obtener más información, vea [BACKUP &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-transact-sql.md).  
  
 Cuando la base de datos que se va a restaurar no existe en la instancia de servidor, la instrucción RESTORE requiere permisos CREATE DATABASE. Para obtener más información, vea [RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-transact-sql.md).  
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Usar SQL Server Management Studio  
  
> [!NOTE]  
>  Si las rutas de acceso de los archivos de copia de seguridad y restauración son idénticas entre la instancia del servidor que hospeda la réplica principal y cada instancia que hospeda la réplica secundaria, debe poder crear bases de datos de réplica secundaria con el [Asistente para nuevo grupo de disponibilidad](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md), el [Asistente para agregar una réplica al grupo de disponibilidad](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md) o el [Asistente para agregar una base de datos al grupo de disponibilidad](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md).  
  
 **Para preparar una base de datos secundaria**  
  
1.  A menos que ya tenga una copia de seguridad reciente de la base de datos principal, cree una nueva copia de seguridad de base de datos completa o diferencial. Como práctica recomendada, coloque esta copia de seguridad y las copias de seguridad del registro subsiguientes en el recurso compartido de red recomendado.  
  
2.  Cree al menos una nueva copia de seguridad del registro de la base de datos principal.

   >[!NOTE]
   >Puede que no sea necesaria una copia de seguridad del registro de transacciones si no se ha realizado previamente una copia de seguridad del registro de transacciones en la base de datos de la réplica principal. Microsoft recomienda hacer una copia de seguridad del registro de transacciones cada vez que una base de datos se combine con el grupo de disponibilidad. 
  
3.  En la instancia del servidor que hospeda la réplica secundaria, restaure la copia de seguridad completa de la base de datos principal (y opcionalmente una copia de seguridad diferencial) seguida de las copias de seguridad del registro subsiguientes.  
  
     En la página **Opciones de RESTORE DATABASE**, seleccione **Dejar la base de datos no operativa y no revertir transacciones no confirmadas. Pueden restaurarse registros de transacciones adicionales. (RESTORE WITH NORECOVERY)** .  
  
     Si las rutas de acceso de archivos de la base de datos principal y la base de datos secundaria difieren, por ejemplo, si la base de datos principal se encuentra en la unidad "F:" pero la instancia de servidor que hospeda la réplica secundaria no tiene unidad "F:", incluya la opción MOVE en la cláusula WITH.  
  
4.  Para completar la configuración de la base de datos secundaria, debe unirla al grupo de disponibilidad. Para obtener más información, vea [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Para obtener información sobre cómo realizar estas operaciones de copia de seguridad y restauración, vea [Tareas de copia de seguridad y restauración relacionadas](#RelatedTasks), más adelante en esta sección.  
  
###  <a name="related-backup-and-restore-tasks"></a><a name="RelatedTasks"></a> Tareas de copia de seguridad y restauración relacionadas  
 **Para crear una copia de seguridad de la base de datos**  
  
-   [Crear una copia de seguridad completa de base de datos &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Crear una copia de seguridad diferencial de una base de datos &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
 **Para crear una copia de seguridad del registro**  
  
-   [Realizar una copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](../../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
 **Para restaurar copias de seguridad**  
  
-   [Restore a Database Backup Using SSMS](../../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Restaurar una copia de seguridad diferencial de la base de datos &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)  
  
-   [Restaurar una copia de seguridad de registros de transacciones &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [Restaurar una base de datos a una nueva ubicación &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para preparar una base de datos secundaria**  
  
> [!NOTE]  
>  Para obtener un ejemplo de este procedimiento, vea [Ejemplo (Transact-SQL)](#ExampleTsql), anteriormente en este tema.  
  
1.  A menos que tenga una copia de seguridad completa reciente de la base de datos principal, conéctese a la instancia del servidor que hospeda la réplica principal y cree una copia de seguridad completa de la base de datos. Como práctica recomendada, coloque esta copia de seguridad y las copias de seguridad del registro subsiguientes en el recurso compartido de red recomendado.  
  
2.  En la instancia del servidor que hospeda la réplica secundaria, restaure la copia de seguridad completa de la base de datos principal (y opcionalmente una copia de seguridad diferencial) seguida de todas las copias de seguridad del registro subsiguientes. Use WITH NORECOVERY para cada operación de restauración.  
  
     Si las rutas de acceso de archivos de la base de datos principal y la base de datos secundaria difieren, por ejemplo, si la base de datos principal se encuentra en la unidad "F:" pero la instancia de servidor que hospeda la réplica secundaria no tiene unidad "F:", incluya la opción MOVE en la cláusula WITH.  
  
3.  Si se han realizado copias de seguridad del registro de la base de datos principal desde que se realizó la copia de seguridad del registro obligatoria, deben copiarse también en la instancia de servidor que hospeda la réplica secundaria y aplicar cada una de ellas a la base de datos secundaria, empezando con la más antigua y utilizando siempre RESTORE WITH NORECOVERY.  
  
    > [!NOTE]  
    >  Ni existiría una copia de seguridad del registro si la base de datos principal se ha creado recientemente y no se ha hecho todavía ninguna copia de seguridad del registro, o si el modelo de recuperación ha cambiado recientemente de SIMPLE a FULL.  
  
4.  Para completar la configuración de la base de datos secundaria, debe unirla al grupo de disponibilidad. Para obtener más información, vea [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Para obtener información sobre cómo realizar estas operaciones de copia de seguridad y restauración, vea [Tareas de copia de seguridad y restauración relacionadas](#RelatedTasks), más adelante en este tema.  
  
###  <a name="transact-sql-example"></a><a name="ExampleTsql"></a> Ejemplo de Transact-SQL  
 En el siguiente ejemplo se prepara una base de datos secundaria. En este ejemplo se utiliza la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] , que usa de forma predeterminada un modelo de recuperación simple.  
  
1.  Para utilizar la base de datos [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] , modifíquela para que utilice el modelo de recuperación completa:  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE MyDB1   
    SET RECOVERY FULL;  
    GO  
    ```  
  
2.  Después de modificar el modelo de recuperación de la base de datos de SIMPLE a FULL, cree una copia de seguridad completa, que puede usarse para crear la base de datos secundaria. Puesto que se ha cambiado recientemente el modelo de recuperación, se especifica la opción WITH FORMAT para crear un conjunto de medios. Esto es útil para separar las copias de seguridad con el modelo de recuperación completa a partir de cualquier copia de seguridad anterior realizada con el modelo de recuperación simple. Para este ejemplo, el archivo de copia de seguridad (C:\\[!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)].bak) se crea en la misma unidad que la base de datos.  
  
    > [!NOTE]  
    >  Para una base de datos de producción, siempre se debe realizar la copia de seguridad en un dispositivo independiente.  
  
     En la instancia del servidor que hospeda la réplica principal (`INSTANCE01`), cree una copia de seguridad completa de la base de datos principal del modo siguiente:  
  
    ```  
    BACKUP DATABASE MyDB1   
        TO DISK = 'C:\MyDB1.bak'   
        WITH FORMAT  
    GO  
    ```  
  
3.  Copie la copia de seguridad completa en la instancia del servidor que hospeda la réplica secundaria.  
  
4.  Restaure la copia de seguridad completa en la instancia del servidor que hospeda la réplica secundaria utilizando RESTORE WITH NORECOVERY. El comando de restauración depende de si las rutas de acceso de las bases de datos principal y secundaria son idénticas.  
  
    -   **Si las rutas de acceso son idénticas:**  
  
         En el equipo que hospeda la réplica secundaria, restaure la copia de seguridad completa del siguiente modo:  
  
        ```  
        RESTORE DATABASE MyDB1   
            FROM DISK = 'C:\MyDB1.bak'   
            WITH NORECOVERY  
        GO  
        ```  
  
    -   **Si las rutas de acceso son distintas:**  
  
         Si la ruta de acceso de la base de datos secundaria difiere de la de la base de datos principal (por ejemplo, letras de unidad diferentes), la creación de la base de datos secundaria requiere que la operación de restauración incluya una cláusula MOVE.  
  
        > [!IMPORTANT]  
        >  Si los nombres de las rutas de acceso de las bases de datos principal y secundaria son distintos, no se puede agregar ningún archivo. Esto es debido a que al recibir el registro para la operación de agregar un archivo, la instancia del servidor de la réplica secundaria intenta colocar el nuevo archivo en la misma ruta de acceso utilizada por la base de datos principal.  
  
         Por ejemplo, el siguiente comando restaura una copia de seguridad de una base de datos principal que reside en el directorio de datos de la instancia predeterminada de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)], C:\Archivos de programa\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\DATA. La operación de restauración de base de datos debe mover la base de datos al directorio de datos de una instancia remota de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] denominada (*AlwaysOn1*), que hospeda la réplica secundaria en otro nodo de clúster. Allí, los archivos de registro y datos se restauran en el directorio *C:\Archivos de programa\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA* . La operación de restauración usa WITH NORECOVERY para la base de datos secundaria en la base de datos de restauración.  
  
        ```  
        RESTORE DATABASE MyDB1  
          FROM DISK='C:\MyDB1.bak'  
         WITH NORECOVERY,   
            MOVE 'MyDB1_Data' TO   
             'C:\Program Files\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA\MyDB1_Data.mdf',   
            MOVE 'MyDB1_Log' TO  
             'C:\Program Files\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA\MyDB1_Data.ldf';  
        GO  
        ```  
  
5.  Una vez restaurada la copia de seguridad completa, debe crearse una copia de seguridad del registro en la base de datos principal. Por ejemplo, la siguiente instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)] realiza una copia de seguridad del registro en un archivo de copia de seguridad denominado *E:\MyDB1_log.trn*:  
  
    ```  
    BACKUP LOG MyDB1   
      TO DISK = 'E:\MyDB1_log.trn'   
    GO  
    ```  
  
6.  Para poder unir la base de datos a la réplica secundaria, se debe aplicar la copia de seguridad de registros obligatoria (y las copias de seguridad de registros subsiguientes).  
  
     Por ejemplo, la siguiente instrucción de [!INCLUDE[tsql](../../../includes/tsql-md.md)] restaura el primer registro de *C:\MyDB1.trn*:  
  
    ```  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  Si las copias de seguridad del registro adicionales se producen antes de que la base de datos se una a la réplica secundaria, también debe restaurar todas las copias de seguridad del registro, de forma secuencial, a la instancia del servidor que hospeda la réplica secundaria mediante RESTORE WITH NORECOVERY.  
  
     Por ejemplo, la siguiente instrucción de [!INCLUDE[tsql](../../../includes/tsql-md.md)] restaura dos registros adicionales de *E:\MyDB1_log.trn*:  
  
    ```  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=2, NORECOVERY  
    GO  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=3, NORECOVERY  
    GO  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
 **Para preparar una base de datos secundaria**  
  
1.  Si necesita crear una copia de seguridad reciente de la base de datos principal, cambie el directorio (**cd**) a la instancia del servidor que hospeda la réplica principal.  
  
2.  Use el cmdlet **Backup-SqlDatabase** para crear cada una de las copias de seguridad.  
  
3.  Cambie el directorio (**cd**) a la instancia del servidor que hospeda la réplica secundaria.  
  
4.  Para restaurar las copias de seguridad de base de datos y del registro de cada base de datos principal, use el cmdlet **restore-SqlDatabase** y especifique el parámetro de restauración **NoRecovery** . Si las rutas de acceso de archivo difieren entre equipos que hospedan la réplica principal y la réplica secundaria de destino, utilice también el parámetro de restauración **RelocateFile** .  
  
    > [!NOTE]  
    >  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
5.  Para completar la configuración de la base de datos secundaria, debe unirla al grupo de disponibilidad. Para obtener más información, vea [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
 **Para configurar y usar el proveedor de SQL Server PowerShell**  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="sample-backup-and-restore-script-and-command"></a><a name="ExamplePSscript"></a> Ejemplo de comando y script de copias de seguridad y restauración  
 Los siguientes comandos de PowerShell realizan una copia de seguridad del registro de transacciones y una copia de seguridad completa de la base de datos en un recurso compartido de red y restauran las copias de seguridad de ese recurso compartido. En este ejemplo se supone que la ruta de acceso de archivo en que se restaura la base de datos es la misma que la ruta de acceso de archivo en que se creo la copia de seguridad de la base de datos.  
  
```  
# Create database backup  
Backup-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.bak" -ServerInstance "SourceMachine\Instance"  
# Create log backup  
Backup-SqlDatabase -Database "MyDB1" -BackupAction "Log" -BackupFile "\\share\backups\MyDB1.trn" -ServerInstance "SourceMachine\Instance"  
# Restore database backup   
Restore-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.bak" -NoRecovery -ServerInstance "DestinationMachine\Instance"  
# Restore log backup   
Restore-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.trn" -RestoreAction "Log" -NoRecovery -ServerInstance "DestinationMachine\Instance"  
  
```  
  
##  <a name="next-steps"></a><a name="FollowUp"></a> Pasos siguientes  
 Para completar la configuración de la base de datos secundaria, debe unir la base de datos que se acaba de restaurar al grupo de disponibilidad. Para obtener más información, vea [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-transact-sql.md)   
 [Argumentos RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-arguments-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-transact-sql.md)   
 [Solucionar problemas relativos a una operación de agregar archivos con error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
