---
description: Enable Stretch Database for a database
title: Enable Stretch Database for a database
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, enabling database
- enabling database for Stretch Database
ms.assetid: 37854256-8c99-4566-a552-432e3ea7c6da
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 0b93f34e5a5b716c649f0902dabefe651253c9ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079986"
---
# <a name="enable-stretch-database-for-a-database"></a>Enable Stretch Database for a database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Si quiere configurar una base de datos de Stretch Database, seleccione **Tareas | Stretch | Habilitar** en una base de datos de SQL Server Management Studio a fin de abrir el asistente **Habilitar base de datos para Stretch** . También puede utilizar Transact-SQL a fin de habilitar Stretch Database para una base de datos.  
  
 Si selecciona **Tareas | Stretch | Habilitar** para una tabla individual y aún no se ha habilitado la base de datos para Stretch Database, el asistente configura la base de datos para Stretch Database y le permite seleccionar tablas como parte del proceso. Siga los pasos de este artículo, en lugar de los pasos descritos en [Enable Stretch Database for a table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) (Habilitar Stretch Database para una tabla).  
  
 Para habilitar Stretch Database en una base de datos o tabla, se requieren permisos db_owner. La habilitación de Stretch Database en una base de datos también requiere permisos CONTROL DATABASE.  

> [!NOTE]
> Posteriormente, si deshabilita Stretch Database, recuerde que al deshabilitar Stretch Database para una tabla o una base de datos no se elimina el objeto remoto. Si quiere eliminar la tabla o la base de datos remotas, tiene que quitarlas mediante el Portal de administración de Azure. Los objetos remotos siguen acumulando gastos de Azure hasta que se eliminan manualmente. 
 
## <a name="before-you-get-started"></a>Antes de comenzar  
  
-   Antes de configurar una base de datos para Stretch, se recomienda ejecutar el Asesor de Stretch Database con el objetivo de identificar las bases de datos y las tablas aptas para Stretch. El Asesor de Stretch Database también identifica los problemas de bloqueo. Para obtener más información, vea [Identificar bases de datos y tablas para Stretch Database al ejecutar el Asesor de Stretch Database](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md).  
  
-   Consulte [Limitaciones de Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md).  
  
-   Stretch Database migra los datos a Azure. Por lo tanto, necesita una cuenta de Azure y una suscripción para la facturación. Para obtener una cuenta de Azure, [haga clic aquí](https://azure.microsoft.com/pricing/free-trial/).  
  
-   Disponga de la información de conexión e inicio de sesión que necesita para crear un nuevo servidor de Azure o para seleccionar un servidor existente de Azure.  
  
##  <a name="prerequisite-enable-stretch-database-on-the-server"></a><a name="EnableTSQLServer"></a> Requisito previo: habilitar Stretch Database en el servidor  
 Antes de poder habilitar Stretch Database en una base de datos o una tabla, tendrá que hacerlo en el servidor local. Esta operación requiere permisos sysadmin o serveradmin.  
  
-   Si tiene los permisos administrativos necesarios, el asistente **Habilitar base de datos para Stretch** configurará el servidor para Stretch.  
  
-   Si no dispone de los permisos necesarios, un administrador tendrá que habilitar la opción manualmente mediante la ejecución de **sp_configure** antes de usar el asistente, o bien el propio administrador tendrá que ejecutar el asistente.  
  
 Para habilitar Stretch Database en el servidor manualmente, ejecute **sp_configure** y active la opción **Archivo de datos remotos** . En el ejemplo siguiente se habilita la opción **remote data archive** estableciendo su valor en 1.  
  
```sql
EXEC sp_configure 'remote data archive' , '1';  
GO

RECONFIGURE;  
GO  
```  
  
 Para obtener más información, vea [Configure the remote data archive Server Configuration Option (Configuración de la opción de configuración del servidor Archivo de datos remotos)](../../database-engine/configure-windows/configure-the-remote-data-archive-server-configuration-option.md) y [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
##  <a name="use-the-wizard-to-enable-stretch-database-on-a-database"></a><a name="Wizard"></a> Uso del Asistente para habilitar Stretch Database en una base de datos  
 Para obtener información sobre el asistente Habilitar base de datos para Stretch, incluidos los datos que se deben especificar y las decisiones que hay que tomar, vea [Get started by running the Enable Database for Stretch Wizard (Introducción mediante la ejecución del Asistente para Habilitar base de datos para Stretch)](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md).  
  
##  <a name="use-transact-sql-to-enable-stretch-database-on-a-database"></a><a name="EnableTSQLDatabase"></a> Uso de Transact-SQL para habilitar Stretch Database en una base de datos  
 Antes de habilitar Stretch Database en tablas individuales, debe habilitarlo en la base de datos.  
  
 Para habilitar Stretch Database en una base de datos o tabla, se requieren permisos db_owner. La habilitación de Stretch Database en una base de datos también requiere permisos CONTROL DATABASE.  
  
1.  Antes de comenzar, elija un servidor de Azure existente para los datos que migra Stretch Database o cree un servidor nuevo de Azure.  
  
2.  En el servidor de Azure, cree una regla de firewall con el intervalo de direcciones IP de SQL Server que permita a esta herramienta comunicarse con el servidor remoto.  

    Puede encontrar fácilmente los valores que necesita y crear la regla de firewall intentando conectarse al servidor de Azure desde el Explorador de objetos en SQL Server Management Studio (SSMS). SSMS le ayuda a crear la regla abriendo el siguiente cuadro de diálogo, que ya incluye los valores de dirección IP necesarios.
    
    ![Regla de firewall para Stretch](../../sql-server/stretch-database/media/firewall-rule-for-stretch.png)
  
3.  Si desea configurar una base de datos de SQL Server para Stretch Database, esta debe disponer de una clave maestra de base de datos. Dicha clave protege las credenciales que Stretch Database utiliza para conectarse a la base de datos remota. Este es un ejemplo que crea una nueva clave maestra de base de datos.  
  
    ```sql  
    USE <database>; 
    GO  
  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD='<password>'; 
    GO
    ```  
    Para obtener más información sobre la clave maestra de base de datos, vea [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) y [Crear la clave maestra de una base de datos](../../relational-databases/security/encryption/create-a-database-master-key.md).
    
4.  Cuando configure una base de datos para Stretch Database, tendrá que proporcionar unas credenciales que Stretch Database utilizará a fin de establecer la comunicación entre el servidor SQL Server local y el servidor remoto de Azure. Tiene dos opciones.  
  
    -   Puede especificar unas credenciales de administrador.  
  
        -   Si habilita Stretch Database ejecutando el asistente, puede crear las credenciales entonces.  
  
        -   Si piensa habilitar Stretch Database mediante la ejecución de **ALTER DATABASE**, tendrá que crear las credenciales manualmente antes de ejecutar **ALTER DATABASE** para habilitar Stretch Database. 
        
        Este es un ejemplo que crea una nueva credencial.
  
        ```sql  
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>  
            WITH IDENTITY = '<identity>' , SECRET = '<secret>' ;
        GO   
        ```  

         Para obtener más información sobre la credencial, vea [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md). Para crear las credenciales, necesitará permisos ALTER ANY CREDENTIAL.  

    -   Puede usar una cuenta de servicio federado para que el servidor SQL Server se comunique con el servidor remoto de Azure cuando se cumplen las siguientes condiciones.  
  
        -   La cuenta de servicio con la que se está ejecutando la instancia de SQL Server es una cuenta de dominio.  
  
        -   La cuenta de dominio pertenece a un dominio cuyo Active Directory está federado con Azure Active Directory.  
  
        -   El servidor remoto de Azure está configurado para admitir la autenticación de Azure Active Directory.  
  
        -   La cuenta de servicio con la que se ejecuta la instancia de SQL Server debe configurarse como una cuenta dbmanager o sysadmin en el servidor remoto de Azure.  
  
5.  Si desea configurar una base de datos para Stretch Database, ejecute el comando ALTER DATABASE.  
  
    1.  En lo que respecta al argumento SERVER, especifique el nombre de un servidor de Azure existente, incluida la parte `.database.windows.net` del nombre, por ejemplo, `MyStretchDatabaseServer.database.windows.net`.  
  
    2.  Proporcione unas credenciales de administrador existentes con el argumento de CREDENTIAL o especifique FEDERATED_SERVICE_ACCOUNT = ON. En el ejemplo siguiente se proporciona una credencial existente.  
  
    ```sql  
    ALTER DATABASE <database name>  
        SET REMOTE_DATA_ARCHIVE = ON  
            (  
                SERVER = '<server_name>' ,  
                CREDENTIAL = <db_scoped_credential_name>  
            ) ;  
    GO
    ```  
  
## <a name="next-steps"></a>Pasos siguientes  
-   [Enable Stretch Database for a table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) (Habilitación de Stretch Database para una tabla) para habilitar tablas adicionales.  
  
-   [Monitor and troubleshoot data migration &#40;Stretch Database&#41; (Supervisar y solucionar problemas de migración de datos &#40;Stretch Database&#41;)](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) para ver el estado de la migración de datos.  
  
-   [Pause and resume data migration &#40;Stretch Database&#41; (Pausar y reanudar la migración de datos &#40;Stretch Database&#41;)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
-   [Administración y solución de problemas de Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
-   [Copia de seguridad y restauración de bases de datos habilitadas para Stretch](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
  
-   [Restauración de bases de datos habilitadas para Stretch](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
## <a name="see-also"></a>Consulte también  
 [Identificar bases de datos y tablas para Stretch Database al ejecutar el Asesor de Stretch Database](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md)   
 [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  
