---
title: Preparación de una base de datos para la creación de reflejo
description: Aprenda a preparar una base de datos de SQL Server para la creación de reflejo de la base de datos mediante SQL Server Management Studio o Transact-SQL en SQL Server.
ms.custom: seo-lt-2019
ms.date: 11/10/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], preparing for mirroring
- logins [SQL Server], database mirroring
- mirror database [SQL Server]
ms.assetid: 8676f9d8-c451-419b-b934-786997d46c2b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b55c6f886fb875c3091b8b2090a41bbe4cf832d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343713"
---
# <a name="prepare-a-mirror-database-for-mirroring-sql-server"></a>Preparar una base de datos reflejada para la creación de reflejo (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Para poder empezar una sesión de creación de reflejo de la base de datos, el propietario de la base de datos o el administrador del sistema debe asegurarse de que la base de datos reflejada se ha creado y está preparada para la creación de reflejo. La creación de una base de datos reflejada requiere como mínimo que se haga una copia de seguridad completa de la base de datos principal y la subsiguiente copia de seguridad de registros, y que ambas se restauren en la instancia del servidor reflejado mediante WITH NORECOVERY.  
  
 En este tema se describe cómo preparar una base de datos reflejada en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   **Antes de empezar:**  
  
     [Requisitos](#Requirements)  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   [Para preparar una base de datos reflejada existente para reiniciar la creación de reflejo](#PrepareToRestartMirroring)  
  
-   [Para preparar una nueva base de datos reflejada](#CombinedProcedure)  
  
-   **Seguimiento:**  [Después de preparar una base de datos reflejada](#FollowUp)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="requirements"></a><a name="Requirements"></a> Requisitos  
  
-   Las instancias de servidor principal y reflejado deben ejecutarse en la misma versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aunque es posible que el servidor reflejado tenga una versión posterior de SQL Server, esta configuración solo se recomienda durante un proceso de actualización planeado cuidadosamente. En tal configuración, se corre el riesgo de una conmutación por error automática, en la que el movimiento de datos se suspende automáticamente debido a que los datos no pueden pasarse a una versión inferior de SQL Server. Para más información, consulte [Upgrading Mirrored Instances](../../database-engine/database-mirroring/upgrading-mirrored-instances.md).  
  
-   Las instancias de servidor principal y reflejado deben ejecutarse en la misma edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información sobre la compatibilidad con la creación de reflejo de la base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], vea [Ediciones y características admitidas de SQL Server 2017](~/sql-server/editions-and-components-of-sql-server-2017.md).  
  
-   La base de datos debe usar el modelo de recuperación completa.  
  
     Para obtener más información, vea [Ver o cambiar el modelo de recuperación de una base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md) o [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) y [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
-   La base de datos reflejada debe tener el mismo nombre que la base de datos principal.  
  
-   Para que la creación de reflejo funcione, la base de datos reflejada debe permanecer en estado RESTORING. Al preparar una base de datos reflejada, debe usar RESTORE WITH NORECOVERY para cada operación de restauración. Como mínimo, necesitará restaurar (WITH NORECOVERY) una copia de seguridad completa de la base de datos principal, seguida de todas las copias de seguridad de registros subsiguientes.  
  
-   El sistema donde piensa crear la base de datos reflejada debe poseer una unidad de disco con espacio suficiente para alojar la base de datos reflejada.  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   No puede reflejar las bases de datos del sistema **master**, **msdb**, **temp** o **model** .  
  
-   No se puede crear el reflejo de una base de datos que pertenezca a un [grupo de disponibilidad AlwaysOn](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Use una copia de seguridad completa muy reciente o una copia de seguridad diferencial reciente de la base de datos principal.  
  
-   Si se programa un trabajo de copia de seguridad de registros para que se ejecute muy a menudo en la base de datos principal, puede que sea necesario deshabilitar el trabajo de copia de seguridad hasta que se haya iniciado la creación de reflejo.  
  
-   Si es posible, la ruta de acceso (incluida la letra de unidad) de la base de datos reflejada debería ser idéntica a la de la base de datos principal.  
  
     Si las rutas de acceso de archivo deben ser diferentes (por ejemplo, si la base de datos principal se encuentra en la unidad 'F:' pero el sistema reflejado no tiene unidad F:), se debe incluir la opción MOVE en RESTORE STATEMENT.  
  
    > [!IMPORTANT]  
    >  Para poder agregar un archivo durante una sesión de creación de reflejo sin influir en la sesión, la ruta de acceso del archivo debe existir en ambos servidores. Por consiguiente, si mueve los archivos de base de datos al crear la base de datos reflejada, se podría producir un error en una operación posterior para agregar un archivo en la base de datos reflejada y provocar la suspensión de la creación del reflejo. Para obtener información sobre cómo actuar si una operación de creación de archivo no ha podido realizarse, vea [Solucionar problemas de configuración de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md).  
  
-   Si la base de datos principal tiene algunos catálogos de texto completo, se recomienda ver [Creación de reflejo de la base de datos y catálogos de texto completo &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md).  
  
-   Para una base de datos de producción, realice siempre la copia de seguridad en un dispositivo diferente.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 TRUSTWORTHY se establece en OFF cuando se hace una copia de seguridad de base de datos. Por lo tanto, TRUSTWORTHY está siempre en OFF en una nueva base de datos reflejada. Si es preciso que la base de datos sea de confianza después de una conmutación por error, son necesarios pasos de configuración adicionales. Para más información, consulte [Configurar una base de datos reflejada para usar la propiedad Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md).  
  
 Para obtener más información sobre cómo habilitar el descifrado automático de la clave maestra de la base de datos de una base de datos reflejada, vea [Establecer una base de datos reflejada cifrada](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Propietario de la base de datos o administrador del sistema.  
  
##  <a name="to-prepare-an-existing-mirror-database-to-restart-mirroring"></a><a name="PrepareToRestartMirroring"></a> Para preparar una base de datos reflejada existente para reiniciar la creación de reflejo  
 Si la creación de reflejo se ha eliminado y la base de datos reflejada sigue en el estado de recuperación (RECOVERING), se puede reiniciar la creación de reflejo.  
  
1.  Realice al menos una copia de seguridad de registros de la base de datos principal. Para obtener más información, vea [Realizar copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)).  
  
2.  En la base de datos reflejada, use RESTORE WITH NORECOVERY para restaurar todas las copias de seguridad de registros que se realizaron en la base de datos principal desde que se quitó la creación de reflejo. Para más información, consulte [Restaurar una copia de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md).  
  
##  <a name="to-prepare-a-new-mirror-database"></a><a name="CombinedProcedure"></a> Para preparar una nueva base de datos reflejada  
 **Para preparar una base de datos reflejada**  
  
> [!NOTE]  
>  Para obtener un ejemplo de [!INCLUDE[tsql](../../includes/tsql-md.md)] de este procedimiento, vea [Ejemplo (Transact-SQL)](#TsqlExample)más adelante en esta sección.  
  
1.  Conéctese a la instancia del servidor principal.  
  
2.  Cree una copia de seguridad completa o una copia de seguridad diferencial de la base de datos principal.  
  
    -   [Crear una copia de seguridad completa de base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
    -   [Crear una copia de seguridad diferencial de una base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md).  
  
3.  Normalmente, necesita realizar al menos una copia de seguridad de registros de la base de datos principal. Sin embargo, puede que no se necesite una copia de seguridad de registros si la base de datos se ha creado recientemente y no se ha hecho todavía ninguna copia de seguridad de registros o si el modelo de recuperación ha cambiado recientemente de SIMPLE a FULL.  
  
    -   [Realizar una copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
4.  A menos que las copias de seguridad estén en una unidad de red que sea accesible desde ambos sistemas, copie las copias de seguridad de la base de datos y de registros al sistema que hospedará la instancia de servidor reflejado.  
  
5.  Conéctese a la instancia del servidor reflejado.  
  
6.  Con RESTORE WITH NORECOVERY, cree la base de datos reflejada restaurando la copia de seguridad completa y, opcionalmente, la copia de seguridad diferencial más reciente, en la instancia de servidor reflejado.  
  
    > [!NOTE]  
    >  Si restaura la base de datos grupo de archivos por grupo de archivos, asegúrese de restaurar la base de datos completa.  
  
    -   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
    -   [RESTORE &amp;#40;Transact-SQL&amp;#41;](../../t-sql/statements/restore-statements-transact-sql.md) y [RESTORE Arguments &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
7.  Con RESTORE WITH NORECOVERY, aplique la copia de seguridad o las copias de seguridad de registros pendientes en la base de datos reflejada.  
  
    -   [Restaurar una copia de seguridad de registros de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 Para poder iniciar una sesión de creación de reflejo de la base de datos, debe crear la base de datos reflejada. Debe hacerlo inmediatamente antes de iniciar la sesión de creación de reflejo.  
  
 En este ejemplo se utiliza la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , que usa de forma predeterminada un modelo de recuperación simple.  
  
1.  Para utilizar la creación de reflejo de la base de datos con la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , modifíquela para que utilice el modelo de recuperación completa.  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
    SET RECOVERY FULL;  
    GO  
    ```  
  
2.  Después de modificar el modelo de recuperación de la base de datos de SIMPLE a FULL, cree una copia de seguridad completa, que puede usarse para crear la base de datos reflejada. Puesto que se ha cambiado recientemente el modelo de recuperación, se especifica la opción WITH FORMAT para crear un conjunto de medios. Esto es útil para separar las copias de seguridad con el modelo de recuperación completa a partir de cualquier copia de seguridad anterior realizada con el modelo de recuperación simple. Para este ejemplo, el archivo de copia de seguridad (`C:\AdventureWorks.bak`) se crea en la misma unidad que la base de datos.  
  
    > [!NOTE]  
    >  Para una base de datos de producción, siempre se debe realizar la copia de seguridad en un dispositivo independiente.  
  
     En la instancia de servidor principal (en `PARTNERHOST1`), cree una copia de seguridad completa de la base de datos principal, como se indica a continuación:  
  
    ```  
    BACKUP DATABASE AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
        WITH FORMAT  
    GO  
    ```  
  
3.  Copie la copia de seguridad completa en el servidor reflejado.  
  
4.  Con RESTORE WITH NORECOVERY, restaure la copia de seguridad completa en la instancia de servidor reflejado. El comando de restauración depende de si las rutas de acceso de las bases de datos principal y reflejada son idénticas.  
  
    -   **Si las rutas de acceso son idénticas:**  
  
         En la instancia del servidor reflejado (en `PARTNERHOST5`), restaure la copia de seguridad completa como se indica a continuación:  
  
        ```  
        RESTORE DATABASE AdventureWorks   
            FROM DISK = 'C:\AdventureWorks.bak'   
            WITH NORECOVERY  
        GO  
        ```  
  
    -   **Si las rutas de acceso son distintas:**  
  
         Si la ruta de acceso de la base de datos reflejada difiere de la de la base de datos principal (por ejemplo, letras de unidad diferentes), la creación de la base de datos reflejada requiere que la operación de restauración incluya una cláusula MOVE.  
  
        > [!IMPORTANT]  
        >  Si los nombres de las rutas de acceso de las bases de datos principal y reflejada son distintos, no se puede agregar ningún archivo. Esto es debido a que al recibir el registro para la operación de agregar un archivo, la instancia del servidor reflejado intenta colocar el nuevo archivo en la ubicación utilizada por la base de datos principal.  
  
         Por ejemplo, el siguiente comando restaura una copia de seguridad de una base de datos principal que reside en C:\Archivos de programa\Microsoft SQL Server\MSSQL.*n*\MSSQL\Data\ en una ubicación distinta, D:\Archivos de programa\Microsoft SQL Server\MSSQL.*n*\MSSQL\Dat`a\`, donde va a residir la base de datos reflejada.  
  
        ```  
        RESTORE DATABASE AdventureWorks  
           FROM DISK='C:\AdventureWorks.bak'  
           WITH NORECOVERY,   
              MOVE 'AdventureWorks_Data' TO   
                 'D:\Program Files\Microsoft SQL Server\MSSQL.n\MSSQL\Data\AdventureWorks_Data.mdf',   
              MOVE 'AdventureWorks_Log' TO  
                 'D:\Program Files\Microsoft SQL Server\MSSQL.n\MSSQL\Data\AdventureWorks_Log.ldf';  
        GO  
        ```  
  
5.  Una vez creada la copia de seguridad completa, debe crearse una copia de seguridad de registros en la base de datos principal. Por ejemplo, la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] realiza una copia de seguridad del registro en el mismo archivo que se utilizó en la anterior copia de seguridad completa:  
  
    ```  
    BACKUP LOG AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
    GO  
    ```  
  
6.  Para poder iniciar la creación de reflejo, se debe aplicar la copia de seguridad de registros obligatoria (y las copias de seguridad de registros subsiguientes).  
  
     Por ejemplo, la siguiente instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] restaura el primer registro de `C:\AdventureWorks.bak`:  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  Si se realizan copias de seguridad de registros adicionales antes de iniciar la creación de reflejo, se deben restaurar todas las copias de seguridad de registros, por orden, en el servidor reflejado mediante WITH NORECOVERY.  
  
     Por ejemplo, la siguiente instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] restaura dos registros adicionales de `C:\AdventureWorks.bak`:  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=2, NORECOVERY  
    GO  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\AdventureWorks.bak'   
        WITH FILE=3, NORECOVERY  
    GO  
    ```  
  
 Para ver un ejemplo completo de la configuración de la creación de reflejo de la base de datos, en el que se muestra la configuración de seguridad, se prepara la base de datos reflejada, se configuran los asociados y se agrega un testigo, vea [Configurar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
##  <a name="follow-up-after-preparing-a-mirror-database"></a><a name="FollowUp"></a> Seguimiento: Después de preparar una base de datos reflejada  
  
1.  Si se han realizado copias de seguridad de registros adicionales desde la operación RESTORE LOG más reciente, se debe aplicar manualmente cada copia de seguridad de registros adicional, utilizando RESTORE WITH NORECOVERY.  
  
2.  Inicie la sesión de creación de reflejo. Para más información, consulte [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md) o [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md).  
  
3.  Si ha deshabilitado el trabajo de copia de seguridad de la base de datos principal, vuelva a habilitar el trabajo.  
  
4.  Si la base de datos necesita marcarse como de confianza después de una conmutación por error, es necesario realizar pasos adicionales de configuración después de iniciar la creación de reflejo. Para más información, consulte [Configurar una base de datos reflejada para usar la propiedad Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Crear una copia de seguridad completa de base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Restaurar una copia de seguridad de registros de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Establecer una base de datos reflejada cifrada](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
-   [Configurar una base de datos reflejada para usar la propiedad Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
## <a name="see-also"></a>Consulte también  
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Configurar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Realizar copias de seguridad de los catálogos de texto completo y restaurarlos](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)   
 [Creación de reflejo de la base de datos y catálogos de texto completo &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)   
 [Replicación y creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE &#40;argumentos, Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md)  
  
  

