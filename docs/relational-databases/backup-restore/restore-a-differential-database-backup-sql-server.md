---
title: 'Restauración de base de datos: diferencial'
description: En este artículo se describe cómo restaurar una copia de seguridad diferencial de una base de datos en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full differential backups [SQL Server]
- restoring databases [SQL Server], full differential backups
- database backups [SQL Server], full differential backups
- database restores [SQL Server], full differential backups
- backing up databases [SQL Server], full differential backups
ms.assetid: 0dd971a4-ee38-4dd3-9f30-ef77fc58dd11
author: cawrites
ms.author: chadam
ms.openlocfilehash: 028e5b2d1f26da698edae325b003fb402f721ee1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075847"
---
# <a name="restore-a-differential-database-backup-sql-server"></a>Restauración de una copia de seguridad diferencial de la base de datos (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo restaurar una copia de seguridad diferencial de la base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Requisitos previos](#Prerequisites)  
  
     [Seguridad](#Security)  
  
-   **Para restaurar una copia de seguridad diferencial de la base de datos, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   RESTORE no se permite en una transacción explícita o implícita.  
  
-   Las copias de seguridad que se crean en una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se pueden restaurar en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], puede restaurar una base de datos de usuario a partir de una copia de seguridad de la base de datos creada utilizando [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o una versión posterior.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   En el modelo de recuperación optimizado para cargas masivas de registros o completa, para poder restaurar una base de datos, se debe realizar una copia de seguridad del registro de transacciones activo (conocido como final del registro). Para obtener más información, vea [Realizar copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Si la base de datos que se va a restaurar no existe, el usuario debe tener permisos CREATE DATABASE para poder ejecutar RESTORE. Si la base de datos existe, los permisos RESTORE corresponden de forma predeterminada a los miembros de los roles fijos de servidor **sysadmin** y **dbcreator** , y al propietario (**dbo**) de la base de datos (para la opción FROM DATABASE_SNAPSHOT, la base de datos siempre existe).  
  
 Los permisos RESTORE se conceden a los roles en los que la información acerca de la pertenencia está siempre disponible para el servidor. Debido a que la pertenencia a un rol fijo de base de datos solo se puede comprobar cuando la base de datos es accesible y no está dañada, lo que no siempre ocurre cuando se ejecuta RESTORE, los miembros del rol fijo de base de datos **db_owner** no tienen permisos RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-restore-a-differential-database-backup"></a>Para restaurar una copia de seguridad diferencial de la base de datos  
  
1.  Después de conectarse a la instancia adecuada de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol de servidores.  
  
2.  Expanda **Bases de datos**. En función de la base de datos, seleccione una base de datos de usuario o expanda **Bases de datos del sistema** y, a continuación, seleccione una base de datos del sistema.  
  
3.  Haga clic con el botón derecho en la base de datos, seleccione **Tareas**, **Restaurar** y luego haga clic en **Base de datos**.  
  
4.  En la página **General** , use la sección **Origen** para especificar el origen y la ubicación de los conjuntos de copias de seguridad que se deben restaurar. Seleccione una de las siguientes opciones:  
  
    -   **Base de datos**  
  
         Seleccione la base de datos que desea restaurar en la lista desplegable. La lista solo contiene las bases de datos de las que se han realizado copias de seguridad de acuerdo con el historial de copias de seguridad de **msdb** .  
  
    > [!NOTE]  
    >  Si la copia de seguridad se toma desde un servidor diferente, el servidor de destino no tendrá la información del historial de copia de seguridad de la base de datos especificada. En este caso, seleccione **Dispositivo** para especificar manualmente el archivo o dispositivo que se va a restaurar.  
  
    -   **Dispositivo**  
  
         Haga clic en el botón de exploración ( **...** ) para abrir el cuadro de diálogo **Seleccionar dispositivos de copia de seguridad** . En el cuadro **Tipo de medio de copia de seguridad** , seleccione uno de los tipos de dispositivo. Para seleccionar uno o varios dispositivos del cuadro **Medio de copia de seguridad** , haga clic en **Agregar**.  
  
         Después de agregar los dispositivos que desee al cuadro de lista **Medio de copia de seguridad** , haga clic en **Aceptar** para volver a la página **General** .  
  
         En el cuadro de lista **Origen: Dispositivo: Base de datos**, seleccione el nombre de la base de datos que se debe restaurar.  
  
         **Nota** : esta lista solo está disponible cuando se selecciona **Dispositivo** . Solo estarán disponibles las bases de datos que tienen copias de seguridad en el dispositivo seleccionado.  
  
5.  En la sección **Destino** , el cuadro **Base de datos** se rellena automáticamente con el nombre de la base de datos que se va a restaurar. Para cambiar el nombre de la base de datos, especifique el nuevo nombre en el cuadro **Base de datos** .  
  
    > [!NOTE]  
    >  Para detener la restauración en un momento dado específico, haga clic en **Escala de tiempo** para obtener acceso al cuadro de diálogo **Escala de tiempo de la copia de seguridad** . Para obtener ayuda sobre cómo detener la restauración de una base de datos a un momento dado, consulte [Restaurar una base de datos de SQL Server a un momento dado &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
6.  En la cuadrícula **Conjuntos de copia de seguridad para restaurar** , seleccione las copias de seguridad mediante la copia de seguridad diferencial que desea restaurar.  
  
     Para obtener información sobre las columnas de la cuadrícula **Conjuntos de copia de seguridad que se van a restaurar** , vea [Restaurar la base de datos &#40;página General&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)).  
  
7.  En la página **Opciones** , en el panel **Opciones de restauración** , puede seleccionar una de las opciones siguientes si son apropiadas para su situación:  
  
    -   **Sobrescribir la base de datos existente (WITH REPLACE)**  
  
    -   **Conservar la configuración de replicación (WITH KEEP_REPLICATION)**  
  
    -   **Preguntar antes de restaurar cada copia de seguridad**  
  
    -   **Restringir el acceso a la base de datos restaurada (WITH RESTRICTED_USER)**  
  
     Para obtener más información sobre estas opciones, vea [Restaurar base de datos &#40;página Opciones&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
8.  Seleccione una opción en el cuadro **Estado de recuperación** . Este cuadro determina el estado de la base de datos después de la operación de restauración.  
  
    -   **RESTORE WITH RECOVERY** es el comportamiento predeterminado que deja la base de datos lista para usarse mediante la reversión de las transacciones no confirmadas. No pueden restaurarse registros de transacciones adicionales. Seleccione esta opción si va a restaurar ahora todas las copias de seguridad necesarias.  
  
    -   **RESTORE WITH NORECOVERY** deja la base de datos no operativa y no revierte las transacciones no confirmadas. Pueden restaurarse registros de transacciones adicionales. La base de datos no puede se usar hasta que se recupera.  
  
    -   **RESTORE WITH STANDBY** deja la base de datos en modo de solo lectura. Deshace las transacciones sin confirmar, pero guarda las acciones de deshacer en un archivo en espera para que los efectos de la recuperación puedan revertirse.  
  
     Para obtener descripciones de las opciones, consulte [Restaurar base de datos &#40;página Opciones&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
9. Habrá errores en las operaciones de restauración si hay conexiones activas en la base de datos. Active la opción **Cerrar conexiones existentes** para asegurarse de que se cierren todas las conexiones activas entre [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y la base de datos.  
  
10. Seleccione **Preguntar antes de restaurar cada copia de seguridad** si desea que se le pregunte en cada operación de restauración. No suele ser necesario a menos que la base de datos sea grande y desee supervisar el estado de la operación de restauración.  
  
11. También puede usar la página **Archivos** para restaurar la base de datos a una nueva ubicación. Para obtener ayuda sobre cómo trasladar una base de datos, vea [Restaurar una base de datos a una nueva ubicación &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md).  
  
12. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-restore-a-differential-database-backup"></a>Para restaurar una copia de seguridad diferencial de la base de datos  
  
1.  Ejecute la instrucción RESTORE DATABASE con la cláusula NORECOVERY para restaurar la copia de seguridad de base de datos completa anterior a la copia de seguridad diferencial de la base de datos. Para más información, vea: [Cómo: restaurar una copia de seguridad completa](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md).  
  
2.  Ejecute la instrucción RESTORE DATABASE para restaurar la copia de seguridad diferencial de la base de datos especificando:  
  
    -   El nombre de la base de datos a la que se aplicará la copia de seguridad diferencial de la base de datos.  
  
    -   El dispositivo de copia de seguridad desde el que se restaura la copia de seguridad diferencial de la base de datos.  
  
    -   La cláusula NORECOVERY, si dispone de copias de seguridad del registro de transacciones que deban aplicarse después de que se restaure la copia de seguridad diferencial de la base de datos. En caso contrario, especifique la cláusula RECOVERY.  
  
3.  Con el modelo de recuperación completa o modelo de recuperación optimizado para cargas masivas de registros, la restauración de una copia de seguridad diferencial de la base de datos restaura la base de datos hasta el momento en que se completó la copia de seguridad diferencial de la base de datos. Para recuperar hasta el momento del error, debe aplicar todas las copias de seguridad del registro de transacciones creadas después de la última copia de seguridad diferencial de la base de datos. Para obtener más información, vea [Aplicar copias de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
  
#### <a name="a-restoring-a-differential-database-backup"></a>A. Restaurar una copia de seguridad diferencial de la base de datos  
 En este ejemplo se restaura una copia de seguridad completa y una copia de seguridad diferencial de la base de datos `MyAdvWorks` .  
  
```sql  
-- Assume the database is lost, and restore full database,   
-- specifying the original full database backup and NORECOVERY,   
-- which allows subsequent restore operations to proceed.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH NORECOVERY;  
GO  
-- Now restore the differential database backup, the second backup on   
-- the MyAdvWorks_1 backup device.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH FILE = 2,  
   RECOVERY;  
GO  
```  
  
#### <a name="b-restoring-a-database-differential-database-and-transaction-log-backup"></a>B. Restaurar una base de datos, una base de datos diferencial y una copia de seguridad del registro de transacciones  
 En este ejemplo se restaura una copia de seguridad completa, una copia de seguridad diferencial y una copia de seguridad del registro de transacciones de la base de datos `MyAdvWorks` .  
  
```sql  
-- Assume the database is lost at this point. Now restore the full   
-- database. Specify the original full database backup and NORECOVERY.  
-- NORECOVERY allows subsequent restore operations to proceed.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH NORECOVERY;  
GO  
-- Now restore the differential database backup, the second backup on   
-- the MyAdvWorks_1 backup device.  
RESTORE DATABASE MyAdvWorks  
   FROM MyAdvWorks_1  
   WITH FILE = 2,  
   NORECOVERY;  
GO  
-- Now restore each transaction log backup created after  
-- the differential database backup.  
RESTORE LOG MyAdvWorks  
   FROM MyAdvWorks_log1  
   WITH NORECOVERY;  
GO  
RESTORE LOG MyAdvWorks  
   FROM MyAdvWorks_log2  
   WITH RECOVERY;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Crear una copia de seguridad diferencial de una base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
-   [Restaurar una copia de seguridad de registros de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Copias de seguridad diferenciales &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
