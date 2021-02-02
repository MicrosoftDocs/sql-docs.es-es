---
title: Restaurar una copia de seguridad de registros de transacciones (SQL Server) | Microsoft Docs
description: En este artículo se describe cómo restaurar una copia de seguridad del registro de transacciones en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restoretlog.general.f1
- sql13.swb.restoretlog.options.f1
helpviewer_keywords:
- restore log
- backing up transaction logs [SQL Server], restoring
- transaction log backups [SQL Server], restoring
- restoring transaction logs [SQL Server], restoring backups
- transaction log restores [SQL Server], SQL Server Management Studio
ms.assetid: 1de2b888-78a6-4fb2-a647-ba4bf097caf3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7d18348dbfc80a6f3b5bec94eb47a8bf17baa76a
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075837"
---
# <a name="restore-a-transaction-log-backup-sql-server"></a>Restaurar una copia de seguridad de registros de transacciones (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo restaurar una copia de seguridad del registro de transacciones en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Requisitos previos](#Prerequisites)  
  
     [Seguridad](#Security)  
  
-   **Para restaurar una copia de seguridad del registro de transacciones, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Las copias de seguridad deben restaurarse en el mismo orden en el que se crearon. Para que pueda restaurar una copia de seguridad determinada del registro de transacciones, primero debe restaurar las siguientes copias de seguridad anteriores sin revertir las transacciones sin confirmar, es decir, WITH NORECOVERY:  
  
    -   La copia de seguridad completa de la base de datos y la última copia de seguridad diferencial, si hay alguna, anteriores a la copia de seguridad del registro de transacciones. La base de datos debe haber utilizado el modelo de recuperación completa o el modelo de recuperación optimizado para cargas masivas de registros antes de crearse la copia de seguridad completa o diferencial más reciente.  
  
    -   Todas las copias de seguridad de registros de transacciones realizadas después de la copia de seguridad completa o de la copia de seguridad diferencial (si restaura una) y antes de la copia de seguridad especificada del registro de transacciones. Las copias de seguridad de registros deben haberse aplicado en la secuencia en que fueron creadas, sin saltos en la cadena de registros.  
  
         Para obtener más información sobre las copias de seguridad del registro de transacciones, vea [Copias de seguridad de registros de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md) y [Aplicar copias de seguridad de registros de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Los permisos RESTORE se conceden a los roles en los que la información acerca de la pertenencia está siempre disponible para el servidor. Debido a que la pertenencia a un rol fijo de base de datos solo se puede comprobar cuando la base de datos es accesible y no está dañada, lo que no siempre ocurre cuando se ejecuta RESTORE, los miembros del rol fijo de base de datos **db_owner** no tienen permisos RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
> [!WARNING]  
>  El proceso normal de una restauración consiste en seleccionar las copias de seguridad de registros en el cuadro de diálogo **Restaurar base de datos** junto con las copias de seguridad de datos y diferenciales.  
  
#### <a name="to-restore-a-transaction-log-backup"></a>Para restaurar una copia de seguridad del registro de transacciones  
  
1.  Después de conectarse a la instancia apropiada de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol correspondiente.  
  
2.  Expanda **Bases de datos** y, en función de la base de datos, seleccione la base de datos de un usuario o expanda **Bases de datos del sistema** y seleccione una base de datos del sistema.  
  
3.  Haga clic con el botón derecho en la base de datos, seleccione **Tareas**, **Restaurar** y, después, haga clic en **Registro de transacciones**, con lo que se abre el cuadro de diálogo **Restaurar registro de transacciones** .  
  
    > [!NOTE]  
    >  Si la opción **Registro de transacciones** está atenuada, es posible que primero deba restaurar una copia de seguridad completa o diferencial. Use el cuadro de diálogo de copia de seguridad **Base de datos** .  
  
4.  En el cuadro de lista **Base de datos** de la página **General** , seleccione el nombre de una base de datos. Solo aparecerán las bases de datos en estado de restauración.  
  
5.  Para especificar el origen y la ubicación de los conjuntos de copias de seguridad que se deben restaurar, haga clic en una de las opciones siguientes:  
  
    -   **Desde copias de seguridad de bases de datos anteriores**  
  
         Seleccione la base de datos que desea restaurar en la lista desplegable. La lista solo contiene las bases de datos de las que se han realizado copias de seguridad de acuerdo con el historial de copias de seguridad de **msdb** .  
  
    -   **Desde archivo o cinta**  
  
         Haga clic en el botón de exploración ( **...** ) para abrir el cuadro de diálogo **Seleccionar dispositivos de copia de seguridad** . En el cuadro **Tipo de medio de copia de seguridad** , seleccione uno de los tipos de dispositivo. Para seleccionar uno o varios dispositivos del cuadro **Medio de copia de seguridad** , haga clic en **Agregar**.  
  
         Después de agregar los dispositivos que desee al cuadro de lista **Medio de copia de seguridad** , haga clic en **Aceptar** para volver a la página **General** .  
  
6.  En la cuadrícula **Seleccionar las copias de seguridad del registro de transacciones que se van a restaurar** , seleccione las copias de seguridad que desea restaurar. En esta cuadrícula se muestran las copias de seguridad del registro de transacciones disponibles para la base de datos seleccionada. Una copia de seguridad del registro de transacciones solo está disponible si el **Primer LSN** es superior al **Último LSN** de la base de datos. Las copias de seguridad de registros aparecen en el orden de los números de secuencia de registro (LSN) que contienen y se deben restaurar en este orden.  
  
     En la tabla siguiente se muestran los encabezados de columna de la cuadrícula y se describen sus valores.  
  
    |Encabezado|Value|  
    |------------|-----------|  
    |**Restauración**|Las casillas seleccionadas indican los conjuntos de copias de seguridad que se restaurarán.|  
    |**Nombre**|Nombre del conjunto de copia de seguridad.|  
    |**Componente**|Componente cuya copia de seguridad se ha realizado: **Base de datos**, **Archivo** o \<blank> (para registros de transacciones).|  
    |**Base de datos**|Nombre de la base de datos que forma parte de la operación de copia de seguridad.|  
    |**Fecha de inicio**|Fecha y hora de inicio de la operación de copia de seguridad, según la configuración regional del cliente.|  
    |**Fecha final**|Fecha y hora de finalización de la operación de copia de seguridad, según la configuración regional del cliente.|  
    |**Primer LSN**|Número de secuencia de registro de la primera transacción del conjunto de copias de seguridad. En blanco para las copias de seguridad de archivos.|  
    |**Último LSN**|Número de flujo de registro de la última transacción del conjunto de copias de seguridad. En blanco para las copias de seguridad de archivos.|  
    |**LSN de punto de comprobación**|Número de flujo de registro del punto de comprobación más reciente en el momento en que se creó la copia de seguridad.|  
    |**LSN completo**|Número de secuencia de registro de la copia de seguridad completa más reciente de la base de datos.|  
    |**Server**|Nombre de la instancia del motor de base de datos que realizó la operación de copia de seguridad.|  
    |**Nombre de usuario**|Nombre del usuario que realizó la operación de copia de seguridad.|  
    |**Tamaño**|Tamaño del conjunto de copias de seguridad, en bytes.|  
    |**Posición**|Posición del conjunto de copias de seguridad en el volumen.|  
    |**Expiración**|Fecha y hora de expiración del conjunto de copia de seguridad.|  
  
7.  Seleccione uno de los siguientes:  
  
    -   **A un momento dado**  
  
         Puede conservar el valor predeterminado (**Lo más reciente posible**) o seleccionar una fecha y hora determinadas haciendo clic en el botón Examinar, que abre el cuadro de diálogo **Restauración a un momento dado** .  
  
    -   **Transacción marcada**  
  
         Restaura la base de datos al estado de una transacción previamente marcada. Al seleccionar esta opción aparece el cuadro **Seleccionar transacción marcada** , en el que se muestra una cuadrícula con las transacciones marcadas disponibles en las copias de seguridad del registro de transacciones seleccionadas.  
  
         De forma predeterminada, la restauración se realiza hasta la transacción marcada, sin incluirla. Para restaurar también la transacción marcada, seleccione **Incluir transacción marcada**.  
  
         En la tabla siguiente se muestran los encabezados de columna de la cuadrícula y se describen sus valores.  
  
        |Encabezado|Value|  
        |------------|-----------|  
        |\<blank>|Muestra una casilla para seleccionar la marca.|  
        |**Marca de transacción**|Nombre de la transacción marcada especificada por el usuario cuando se confirmó la transacción.|  
        |**Date**|Fecha y hora de confirmación de la transacción. La fecha y hora de la transacción se muestran tal como están registradas en la tabla **msdbgmarkhistory** , no en la fecha y hora del equipo cliente.|  
        |**Descripción**|Descripción de la transacción marcada especificada por el usuario al confirmar la transacción (si la hay).|  
        |**LSN**|Número de flujo de registro de la transacción marcada.|  
        |**Base de datos**|Nombre de la base de datos en la que se confirmó la transacción marcada.|  
        |**Nombre de usuario**|Nombre del usuario de la base de datos que confirmó la transacción marcada.|  
  
8.  Para ver o seleccionar las opciones avanzadas, haga clic en **Opciones** , en el panel **Seleccionar una página** .  
  
9. En la sección **Opciones de restauración** , las opciones son:  
  
    -   **Conservar la configuración de replicación (WITH KEEP_REPLICATION)**  
  
         Conserva la configuración de replicación cuando se restaura una base de datos publicada en un servidor distinto de aquel en que se creó.  
  
         Esta opción solo está disponible con la opción **Dejar la base de datos lista para su uso revirtiendo las transacciones no confirmadas** (descrita más adelante), que equivale a restaurar una copia de seguridad con la opción **RECOVERY**.  
  
         Activar esta opción equivale a usar la opción **KEEP_REPLICATION** en una instrucción **RESTORE** de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
    -   **Preguntar antes de restaurar cada copia de seguridad**  
  
         Antes de restaurar cada conjunto de copia de seguridad (después del primero), esta opción abre el cuadro de diálogo **Continuar con la restauración** , en el que se solicita si quiere continuar con la secuencia de restauración. En este cuadro de diálogo se muestra el nombre del siguiente conjunto de medios (si lo hay), el nombre del conjunto de copias de seguridad y la descripción del conjunto de copias de seguridad.  
  
         Esta opción resulta especialmente útil cuando se deben intercambiar cintas de distintos conjuntos de medios. Por ejemplo, se puede utilizar cuando el servidor solo dispone de un dispositivo de cinta. Espere hasta que esté listo para continuar antes de hacer clic en **Aceptar**.  
  
         Si hace clic en **No** , la base de datos se quedará en estado de restauración. Para mayor comodidad, puede continuar con la secuencia de restauración una vez completada la última restauración. Si la copia de seguridad siguiente es de datos o diferencial, vuelva a utilizar la tarea **Restaurar base de datos** . Si la siguiente copia de seguridad es una copia de seguridad de registros, utilice la tarea **Restaurar registro de transacciones** .  
  
    -   **Restringir el acceso a la base de datos restaurada (WITH RESTRICTED_USER)**  
  
         Hace que la base de datos restaurada esté disponible solo para los miembros de **db_owner**, **dbcreator** o **sysadmin**.  
  
         Activar esta opción equivale al uso de la opción **RESTRICTED_USER** en una instrucción **RESTORE** de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
10. Para las opciones **Estado de recuperación** , especifique el estado de la base de datos después de la operación de restauración.  
  
    -   **Revertir las transacciones no confirmadas para dejar la base de datos lista para su uso. No pueden restaurarse registros de transacciones adicionales. (RESTORE WITH RECOVERY)**  
  
         Recupera la base de datos. Esta opción es equivalente a la opción **RECOVERY** en una instrucción **RESTORE** de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
         Elíjala solo si no desea restaurar ningún archivo de registro.  
  
    -   **Dejar la base de datos no operativa y no revertir las transacciones no confirmadas. Pueden restaurarse registros de transacciones adicionales. (RESTORE WITH NORECOVERY)**  
  
         Deja la base de datos sin recuperar, en el estado **RESTORING** . Esta opción es equivalente a la opción **NORECOVERY** en una instrucción **RESTORE** de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
         Si elige esta opción, la opción **Conservar la configuración de replicación** no estará disponible.  
  
        > [!IMPORTANT]  
        >  Para un reflejo o una base de datos secundaria, seleccione siempre esta opción.  
  
    -   **Dejar la base de datos en modo de solo lectura. Deshacer las transacciones sin confirmar, pero guardar las acciones de deshacer en un archivo para que los efectos de recuperación puedan invertirse. (RESTORE WITH STANDBY)**  
  
         Deja la base de datos en estado de espera. Esta opción es equivalente a la opción **STANDBY** en una instrucción **RESTORE** de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
         Si elige esta opción, debe especificar un archivo en espera.  
  
11. O bien, especifique un nombre de archivo en espera en el cuadro de texto **Archivo en espera** . Esta opción es obligatoria si se deja la base de datos en modo de solo lectura. Puede buscar el archivo en espera o escribir su ruta de acceso en el cuadro de texto.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
> [!IMPORTANT]  
>  Se recomienda que especifique siempre WITH NORECOVERY o WITH RECOVERY en todas las instrucciones RESTORE para evitar ambigüedades. Esto es especialmente importante cuando se escriben scripts.  
  
#### <a name="to-restore-a-transaction-log-backup"></a>Para restaurar una copia de seguridad del registro de transacciones  
  
1.  Ejecute la instrucción RESTORE LOG para aplicar la copia de seguridad del registro de transacciones especificando:  
  
    -   El nombre de la base de datos a la que se aplicará el registro de transacciones.  
  
    -   El dispositivo de copia de seguridad desde el que se restaurará la copia de seguridad del registro de transacciones.  
  
    -   La cláusula NORECOVERY.  
  
     La sintaxis básica de esta instrucción es la siguiente:  
  
     RESTORE LOG *nombre_de_base_de_datos* FROM <dispositivo_de_copia_de_seguridad> WITH NORECOVERY.  
  
     Donde *nombre_de_base_de_datos* es el nombre de la base de datos y <dispositivo_de_copia_de_seguridad> es el nombre del dispositivo que contiene la copia de seguridad de registros que se va a restaurar.  
  
2.  Repita el paso 1 para cada copia de seguridad del registro de transacciones que tenga que aplicar.  
  
3.  Después de restaurar la copia de seguridad más reciente en la secuencia de restauración, use una de las instrucciones siguientes para recuperar la base de datos:  
  
    -   Recuperar la base de datos como parte de la última instrucción RESTORE LOG:  
  
        ```  
        RESTORE LOG <database_name> FROM <backup_device> WITH RECOVERY;  
        GO  
        ```  
  
    -   Esperar a recuperar la base de datos utilizando otra instrucción RESTORE DATABASE:  
  
        ```  
        RESTORE LOG <database_name> FROM <backup_device> WITH NORECOVERY;   
        RESTORE DATABASE <database_name> WITH RECOVERY;  
        GO  
        ```  
  
         Si espera a recuperar la base de datos tendrá la oportunidad de comprobar que ha restaurado todas las copias de seguridad de registros necesarias. Este método suele ser aconsejable al realizar restauraciones a un momento dado.  
  
    > [!IMPORTANT]  
    >  Si está creando una base de datos reflejada, omita el paso de recuperación. Una base de datos reflejada debe permanecer en el estado RESTORING.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 La base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] usa el modelo de recuperación simple de forma predeterminada. En los siguientes ejemplos se requiere la modificación de la base de datos para utilizar el modelo de recuperación completa, como se indica a continuación:  
  
```sql  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
```  
  
#### <a name="a-applying-a-single-transaction-log-backup"></a>A. Aplicar una sola copia de seguridad del registro de transacciones  
 En el siguiente ejemplo se empieza con la restauración de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] utilizando una copia de seguridad completa de la base de datos que se encuentra en un dispositivo de copia de seguridad denominado `AdventureWorks2012_1`. A continuación, se aplica la primera copia de seguridad del registro de transacciones, que se encuentra en un dispositivo de copia de seguridad denominado `AdventureWorks2012_log`. Finalmente, el ejemplo recupera la base de datos.  
  
```sql  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorks2012_1  
   WITH NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 1,  
   WITH NORECOVERY;  
GO  
RESTORE DATABASE AdventureWorks2012  
   WITH RECOVERY;  
GO  
```  
  
#### <a name="b-applying-multiple-transaction-log-backups"></a>B. Aplicar varias copias de seguridad del registro de transacciones  
 En el siguiente ejemplo se empieza con la restauración de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] utilizando una copia de seguridad completa de la base de datos que se encuentra en un dispositivo de copia de seguridad denominado `AdventureWorks2012_1`. A continuación, se aplican, una por una, las tres primeras copias de seguridad del registro de transacciones, que se encuentran en un dispositivo de copia de seguridad denominado `AdventureWorks2012_log`. Finalmente, el ejemplo recupera la base de datos.  
  
```sql  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorks2012_1  
   WITH NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 1,  
   NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 2,  
   WITH NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 3,  
   WITH NORECOVERY;  
GO  
RESTORE DATABASE AdventureWorks2012  
   WITH RECOVERY;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Realizar una copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Restaurar una base de datos según el punto de error en el modelo de recuperación completa &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Restaurar una base de datos de SQL Server a un momento dado &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
-   [Restaurar una base de datos en una transacción marcada &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Consulte también  
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Aplicar copias de seguridad de registros de transacción &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)  
  
  
