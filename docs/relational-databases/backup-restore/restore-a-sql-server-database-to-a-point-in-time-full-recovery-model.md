---
description: Restaurar una base de datos de SQL Server a un momento dado (modelo de recuperación completa)
title: Restaurar una base de datos de SQL Server a un momento dado (modelo de recuperación completa) | Microsoft Docs
decription: Learn how to restore a database to a point in time in SQL Server using SQL Server Management Studio or Transact-SQL in the full or bulk-logged recovery models.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- STOPAT clause [RESTORE LOG statement]
- point in time recovery [SQL Server]
- restoring databases [SQL Server], point in time
ms.assetid: 3a5daefd-08a8-4565-b54f-28ad01a47d32
author: cawrites
ms.author: chadam
ms.openlocfilehash: 86f23acdc6dd82210c5747915d78aafc2344fc6b
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100348107"
---
# <a name="restore-a-sql-server-database-to-a-point-in-time-full-recovery-model"></a>Restaurar una base de datos de SQL Server a un momento dado (modelo de recuperación completa)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo restaurar una base de datos a un momento dado en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Este tema solo es pertinente para las bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que usan los modelos de recuperación optimizados para cargas masivas de registros.  
  
> [!IMPORTANT]  
>  En el modelo de recuperación optimizado para cargas masivas de registros, si la copia de seguridad de registros contiene cambios registrados de forma masiva, no es posible la recuperación a un momento dado de la copia de seguridad. La base de datos debe recuperarse al final de la copia de seguridad del registro de transacciones.  
  
-   **Antes de empezar:**  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para restaurar una base de datos de SQL Server a un momento dado, mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Usar STANDBY para encontrar un momento dado desconocido.  
  
-   Especificar el momento dado al principio de una secuencia de restauración.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Si la base de datos que se va a restaurar no existe, el usuario debe tener permisos CREATE DATABASE para poder ejecutar RESTORE. Si la base de datos existe, los permisos RESTORE corresponden de forma predeterminada a los miembros de los roles fijos de servidor **sysadmin** y **dbcreator** , y al propietario (**dbo**) de la base de datos (para la opción FROM DATABASE_SNAPSHOT, la base de datos siempre existe).  
  
 Los permisos RESTORE se conceden a los roles en los que la información acerca de la pertenencia está siempre disponible para el servidor. Debido a que la pertenencia a un rol fijo de base de datos solo se puede comprobar cuando la base de datos es accesible y no está dañada, lo que no siempre ocurre cuando se ejecuta RESTORE, los miembros del rol fijo de base de datos **db_owner** no tienen permisos RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para restaurar una base de datos a un momento dado**  
  
1.  En el Explorador de objetos, conéctese a la instancia adecuada de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]y expanda el árbol de servidores.  
  
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
  
6.  Haga clic **Escala de tiempo** para tener acceso al cuadro de diálogo **Escala de tiempo de la copia de seguridad**.  
  
7.  En la sección **Restaurar en** , haga clic **Fecha y hora específicas**.  
  
8.  Use los cuadros **Fecha** y **Hora** o la barra deslizante para especificar una fecha y una hora en las que debe detenerse la restauración. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Use el cuadro **Intervalo de escala de tiempo** para cambiar la cantidad de tiempo que se muestra en la escala de tiempo.  
  
9. Tras especificar un momento específico, el Asesor para recuperación de base de datos se asegura de que solo estén seleccionadas en la columna **Restaurar** de la cuadrícula **Conjuntos de copia de seguridad para restaurar** las copias de seguridad necesarias para restaurar a ese momento dado. Estas copias de seguridad seleccionadas componen el plan de restauraciones recomendado para la restauración a un momento dado. Solo deben utilizarse las copias de seguridad seleccionadas para la operación de restauración a un momento dado.  
  
     Para obtener información sobre las columnas de la cuadrícula **Conjuntos de copia de seguridad que se van a restaurar** , vea [Restaurar la base de datos &#40;página General&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)). Para obtener más información sobre el Asistente para recuperación de base de datos, vea [Información general sobre restauración y recuperación &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  
  
10. En la página **Opciones** , en el panel **Opciones de restauración** , puede seleccionar una de las opciones siguientes si son apropiadas para su situación:  
  
    -   **Sobrescribir la base de datos existente (WITH REPLACE)**  
  
    -   **Conservar la configuración de replicación (WITH KEEP_REPLICATION)**  
  
    -   **Restringir el acceso a la base de datos restaurada (WITH RESTRICTED_USER)**  
  
     Para obtener más información sobre estas opciones, vea [Restaurar base de datos &#40;página Opciones&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
11. Seleccione una opción en el cuadro **Estado de recuperación** . Este cuadro determina el estado de la base de datos después de la operación de restauración.  
  
    -   **RESTORE WITH RECOVERY** es el comportamiento predeterminado que deja la base de datos lista para usarse mediante la reversión de las transacciones no confirmadas. No pueden restaurarse registros de transacciones adicionales. Seleccione esta opción si va a restaurar ahora todas las copias de seguridad necesarias.  
  
    -   **RESTORE WITH NORECOVERY** deja la base de datos no operativa y no revierte las transacciones no confirmadas. Pueden restaurarse registros de transacciones adicionales. La base de datos no puede se usar hasta que se recupera.  
  
    -   **RESTORE WITH STANDBY** deja la base de datos en modo de solo lectura. Deshace las transacciones sin confirmar, pero guarda las acciones de deshacer en un archivo en espera para que los efectos de la recuperación puedan revertirse.  
  
     Para obtener descripciones de las opciones, consulte [Restaurar base de datos &#40;página Opciones&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
12. **Realizar copia del final del registro de la cola antes de la restauración** se seleccionará si es necesario en el momento dado que ha seleccionado. No es necesario modificar este valor, pero puede optar por realizar la copia del final del registro incluso si no es necesario.  
  
13. Puede haber errores en las operaciones de restauración si hay conexiones activas con la base de datos. Active la opción **Cerrar conexiones existentes** para asegurarse de que se cierren todas las conexiones activas entre [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y la base de datos. Esta casilla establece la base de datos en modo de usuario único antes de realizar las operaciones de restauración, y establece la base de datos en modo multiusuario una vez completadas.  
  
14. Seleccione **Preguntar antes de restaurar cada copia de seguridad** si desea que se le pregunte en cada operación de restauración. No suele ser necesario a menos que la base de datos sea grande y desee supervisar el estado de la operación de restauración.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Antes de empezar**  
  
 La restauración a un momento específico siempre se realiza a partir de una copia de seguridad de registros. En cada instrucción RESTORE LOG de la secuencia de restauración, debe especificar el momento de destino o transacción en una cláusula STOPAT idéntica. Como requisito previo para la restauración a un momento específico, debe restaurar primero una copia de seguridad completa de la base de datos cuyo punto final sea anterior al momento de restauración de destino. La copia de seguridad completa de la base de datos puede ser anterior a la copia de seguridad completa de la base de datos más reciente siempre y cuando restaure cada copia de seguridad del registro siguiente, hasta la copia de seguridad del registro que contiene el momento específico de destino, inclusive.  
  
 Para ayudarle a identificar qué copia de seguridad de base de datos debe restaurar, puede especificar opcionalmente la cláusula WITH STOPAT en la instrucción RESTORE DATABASE para que se produzca un error si una copia de seguridad de los datos es demasiado reciente para el momento específico de destino. La copia de seguridad completa de los datos se restaura siempre, aunque contenga el momento de destino.  
  
 **Sintaxis básica de [!INCLUDE[tsql](../../includes/tsql-md.md)]**  
  
 RESTORE LOG *nombre_de_base_de_datos* FROM <dispositivo_de_copia_de_seguridad> WITH STOPAT **=** _hora_ **,** RECOVERY...  
  
 El punto de recuperación es la última confirmación de transacción que se ha producido durante o antes del valor **datetime** que se especifique en *hora*.  
  
 Para restaurar únicamente las modificaciones realizadas antes de un momento concreto, especifique WITH STOPAT **=** _hora_ para cada copia de seguridad que restaure. Esto garantiza que no se pasará el momento de destino.  
  
 **Para restaurar una base de datos a un momento dado**  
  
> [!NOTE]  
>  Para ver un ejemplo de este procedimiento, vea [Ejemplo (Transact-SQL)](#TsqlExample)más adelante en esta sección.  
  
1.  Conéctese a la instancia de servidor en la que desea restaurar la base de datos.  
  
2.  Ejecute la instrucción RESTORE DATABASE con la opción NORECOVERY.  
  
    > [!NOTE]  
    >  Si una secuencia de restauración parcial excluye cualquier grupo de archivos [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md) , no se admite la restauración a un momento dado. Puede forzarse la continuación de la secuencia de restauración. Sin embargo, no se podrán restaurar los grupos de archivos FILESTREAM omitidos en la instrucción RESTORE. Para forzar una restauración a un momento específico, especifique la opción CONTINUE_AFTER_ERROR junto con la opción STOPAT, STOPATMARK o STOPBEFOREMARK, que debe especificar también en las instrucciones RESTORE LOG siguientes. Si se especifica CONTINUE_AFTER_ERROR, la secuencia de restauración parcial será correcta y el grupo de archivos FILESTREAM no será recuperable.  
  
3.  Restaure la última copia de seguridad de base de datos diferencial, si la hubiera, sin recuperar la base de datos (RESTORE DATABASE *database_name* FROM *backup_device* WITH NORECOVERY).  
  
4.  Aplique cada copia de seguridad del registro de transacciones en la misma secuencia en que fueron creadas, especificando la hora a la que tiene previsto detener la restauración del registro (RESTORE DATABASE *nombre_de_base_de_datos* FROM <dispositivo_de_copia_de_seguridad> WITH STOPAT **=** _hora_ **,** RECOVERY).  
  
    > [!NOTE]  
    >  Las opciones RECOVERY y STOPAT. Si la copia de seguridad de registros de transacciones no contiene la hora solicitada (por ejemplo, si la hora especificada está fuera de los límites del intervalo cubierto por el registro de transacciones), se genera una advertencia y no se recupera la base de datos.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 En el ejemplo siguiente se restaura una base de datos al estado en que se encontraba a las `12:00 AM` del `April 15, 2020` y se muestra una operación de restauración que implica varias copias de seguridad de registros. En el dispositivo de copia de seguridad, `AdventureWorksBackups`, la copia de seguridad de base de datos completa que se va a restaurar es el tercer conjunto de copia de seguridad en el dispositivo (`FILE = 3`), la primera copia de seguridad de registros es el cuarto conjunto de copia de seguridad (`FILE = 4`) y la segunda copia de seguridad de registros es el quinto conjunto de copia de seguridad (`FILE = 5`).  
  
> [!IMPORTANT]  
>  La base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] utiliza el modelo de recuperación simple. Con el fin de permitir copias de seguridad de registros, antes de realizar una copia de seguridad completa de la base de datos, la base de datos se ha configurado para usar el modelo de recuperación completa mediante `ALTER DATABASE AdventureWorks SET RECOVERY FULL`.  
  
```  
RESTORE DATABASE AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=3, NORECOVERY;  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
RESTORE DATABASE AdventureWorks WITH RECOVERY;   
GO  
  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Realizar una copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Restaurar una base de datos según el punto de error en el modelo de recuperación completa &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Restaurar una base de datos en una transacción marcada &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
-   [Recuperar a un número de secuencia de registro &#40;SQL Server&#41;](../../relational-databases/backup-restore/recover-to-a-log-sequence-number-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ToPointInTime%2A> (SMO)  
  
## <a name="see-also"></a>Consulte también  
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
  
