---
title: Restaurar una base de datos a una nueva ubicación (SQL Server) | Microsoft Docs
description: En este artículo se muestra cómo restaurar una base de datos de SQL Server en una nueva ubicación y cómo cambiar el nombre de la base de datos mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restoring databases [SQL Server], moving
- database restores [SQL Server], creating new databases
- restoring [SQL Server], with move
- restoring databases [SQL Server], creating new databases
- database backups [SQL Server], creating new database from
- backing up databases [SQL Server], creating new database from
- restoring databases [SQL Server], renaming
- database creation [SQL Server], restoring with move
ms.assetid: 4da76d61-5e11-4bee-84f5-b305240d9f42
author: cawrites
ms.author: chadam
ms.openlocfilehash: c0b65303e9c23977f658e3efef0d87a6713d55e4
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075827"
---
# <a name="restore-a-database-to-a-new-location-sql-server"></a>Restaurar una base de datos a una nueva ubicación (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se explica cómo restaurar una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una nueva ubicación y, opcionalmente, cómo cambiar el nombre de la base de datos, en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante SQL Server Management Studio(SSMS) o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Puede mover una base de datos a una nueva ruta de acceso de directorio o crear una copia de una base de datos en la misma instancia de servidor o en una instancia de servidor diferente.  
    
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El administrador del sistema encargado de restaurar una copia de seguridad de la base de datos completa debe ser la única persona que esté utilizando la base de datos que se va a restaurar.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   En el modelo de recuperación optimizado para cargas masivas de registros o completo, para poder restaurar una base de datos, se debe realizar una copia de seguridad del registro de transacciones activo. Para obtener más información, vea [Realizar copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)).  

-   Para restaurar una base de datos cifrada, **debe tener acceso al certificado o la clave asimétrica usados para cifrar la base de datos**. La base de datos no se puede restaurar sin ese certificado o esa clave asimétrica. Debe conservar el certificado usado para cifrar la clave de cifrado de base de datos durante el tiempo que necesite guardar la copia de seguridad. Para obtener más información, consulte [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Para consideraciones adicionales sobre cómo mover una base de datos, consulte [Copiar bases de datos con Copias de seguridad y restauración](../../relational-databases/databases/copy-databases-with-backup-and-restore.md).  
  
-   Si restaura una base de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o posterior a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], la base de datos se actualiza automáticamente. Normalmente, la base de datos está disponible inmediatamente. Pero si la base de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] tiene índices de texto completo, el proceso de actualización los importa, los restablece o los vuelve a generar, en función del valor de la propiedad del servidor  **upgrade_option** . Si la opción de actualización se establece en importar (**upgrade_option** = 2) o en volver a generar (**upgrade_option** = 0), los índices de texto completo no estarán disponibles durante la actualización. Dependiendo de la cantidad de datos que se indicen, la importación puede requerir varias horas y volver a generar puede requerir hasta diez veces más. Observe también que cuando la opción de actualización se establece en importar, se vuelven a generar los índices de texto completo asociados si no se dispone de un catálogo de texto completo. Para cambiar el valor de la propiedad de servidor **upgrade_option** , use [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 Por razones de seguridad, se recomienda no adjuntar ni restaurar bases de datos de orígenes desconocidos o que no sean de confianza. Es posible que dichas bases de datos contengan código malintencionado que podría ejecutar código [!INCLUDE[tsql](../../includes/tsql-md.md)] no deseado o provocar errores al modificar el esquema o la estructura de la base de datos física. Para usar una base de datos desde un origen desconocido o que no sea de confianza, ejecute [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) en la base de datos de un servidor que no sea de producción y examine también el código, como procedimientos almacenados u otro código definido por el usuario, en la base de datos.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Si la base de datos que se va a restaurar no existe, el usuario debe tener permisos CREATE DATABASE para poder ejecutar RESTORE. Si la base de datos existe, los permisos RESTORE corresponden de forma predeterminada a los miembros de los roles fijos de servidor **sysadmin** y **dbcreator** , y al propietario (**dbo**) de la base de datos.  
  
 Los permisos RESTORE se conceden a los roles en los que la información acerca de la pertenencia está siempre disponible para el servidor. Debido a que la pertenencia a un rol fijo de base de datos solo se puede comprobar cuando la base de datos es accesible y no está dañada, lo que no siempre ocurre cuando se ejecuta RESTORE, los miembros del rol fijo de base de datos **db_owner** no tienen permisos RESTORE.  
  
  
## <a name="restore-a-database-to-a-new-location-optionally-rename-the-database-using-ssms"></a>Restaurar una base de datos en una nueva ubicación; opcionalmente, cambiar el nombre de la base de datos con SSMS 

  
1.  Conéctese a la instancia adecuada de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]y después, en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol.  
  
2.  Haga clic con el botón derecho en **Bases de datos** y, luego, haga clic en **Restaurar base de datos**. Se abre el cuadro de diálogo **Restaurar base de datos** .  
  
3.  En la página **General** , use la sección **Origen** para especificar el origen y la ubicación de los conjuntos de copias de seguridad que se deben restaurar. Seleccione una de las siguientes opciones:  
  
    -   **Base de datos**  
  
         Seleccione la base de datos que desea restaurar en la lista desplegable. La lista solo contiene las bases de datos de las que se han realizado copias de seguridad de acuerdo con el historial de copias de seguridad de **msdb** .  
  
    > **NOTA:** Si la copia de seguridad se toma desde un servidor diferente, el servidor de destino no tendrá la información del historial de copia de seguridad de la base de datos especificada. En este caso, seleccione **Dispositivo** para especificar manualmente el archivo o dispositivo que se va a restaurar.  
  
    1.  **Dispositivo**  
  
         Haga clic en el botón de exploración ( **...** ) para abrir el cuadro de diálogo **Seleccionar dispositivos de copia de seguridad** . En el cuadro **Tipo de medio de copia de seguridad** , seleccione uno de los tipos de dispositivo. Para seleccionar uno o varios dispositivos del cuadro **Medio de copia de seguridad** , haga clic en **Agregar**.  
  
         Después de agregar los dispositivos que desee al cuadro de lista **Medio de copia de seguridad** , haga clic en **Aceptar** para volver a la página **General** .  
  
         En el cuadro de lista **Origen: Dispositivo: Base de datos**, seleccione el nombre de la base de datos que se debe restaurar.  
  
         **Nota** : esta lista solo está disponible cuando se selecciona **Dispositivo** . Solo estarán disponibles las bases de datos que tienen copias de seguridad en el dispositivo seleccionado.  
  
4.  En la sección **Destino** , el cuadro **Base de datos** se rellena automáticamente con el nombre de la base de datos que se va a restaurar. Para cambiar el nombre de la base de datos, especifique el nuevo nombre en el cuadro **Base de datos** .  
  
5.  En el cuadro **Restaurar en** , deje el valor predeterminado **A la última copia de seguridad tomada** o haga clic en **Escala de tiempo** para acceder al cuadro de diálogo **Escala de tiempo de copia de seguridad** para seleccionar manualmente un momento a fin de que se detenga la acción de recuperación. Vea [Backup Timeline](../../relational-databases/backup-restore/backup-timeline.md) para obtener más información acerca de cómo designar un momento específico.  
  
6.  En la cuadrícula **Conjuntos de copia de seguridad que se van a restaurar** , seleccione las copias de seguridad que desea restaurar. En esta cuadrícula se muestran las copias de seguridad disponibles en la ubicación especificada. De forma predeterminada, se sugiere un plan de recuperación. Para anular el plan de recuperación sugerido, puede cambiar las selecciones de la cuadrícula. Se anula automáticamente la selección de las copias de seguridad que dependen de la restauración de una copia de seguridad anterior cuando se anula la selección de una copia de seguridad anterior.  
  
     Para obtener información sobre las columnas de la cuadrícula **Conjuntos de copia de seguridad que se van a restaurar** , vea [Restaurar la base de datos &#40;página General&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)).  
  
7.  Para especificar la nueva ubicación de los archivos de base de datos, seleccione la página **Archivos** y, a continuación, haga clic **Reubicar todos los archivos en la carpeta**. Proporcione una nueva ubicación para **Carpeta de archivos de datos** y **Carpeta de archivos de registro**. Para obtener más información sobre esta cuadrícula, vea [Restaurar base de datos &#40;página Archivos&#41;](../../relational-databases/backup-restore/restore-database-files-page.md).  
  
8.  Si lo desea, en la página **Opciones** ajuste las opciones. Para obtener más información sobre estas opciones, vea [Restaurar base de datos &#40;página Opciones&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  

 ## <a name="restore-database-to-a-new-location-optionally-rename-the-database-using-t-sql"></a>Restaurar una base de datos en una nueva ubicación; opcionalmente, cambiar el nombre de la base de datos mediante T-SQL
 
 
1.  Opcionalmente, determine los nombres lógicos y físicos de los archivos del conjunto de copia de seguridad que contiene la copia de seguridad de base de datos completa que desea restaurar. Esta instrucción devuelve una lista con los archivos de base de datos y de registro del conjunto de copia de seguridad. La sintaxis básica es la siguiente:  
  
     RESTORE FILELISTONLY FROM *<dispositivo_copia_seguridad>* WITH FILE = *backup_set_file_number* 
  
     Aquí, *backup_set_file_number* indica la posición de la copia de seguridad en el conjunto de medios. Puede obtener la posición  de un conjunto de copia de seguridad utilizando la instrucción [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) . Para obtener más información, vea "Especificar un conjunto de copia de seguridad" en [RESTORE &#40;argumentos, Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
     Esta instrucción también admite varias opciones WITH. Para obtener más información, vea [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
2.  Use la instrucción [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md) para restaurar la copia de seguridad completa de la base de datos. De manera predeterminada, los archivos de datos y de registro se restauran en sus ubicaciones originales. Para cambiar la ubicación de una base de datos, use la opción MOVE para mover cada uno de los archivos de la base de datos y evitar conflictos con los archivos existentes.  

  A continuación se muestra la sintaxis básica de [!INCLUDE[tsql](../../includes/tsql-md.md)] para restaurar la base de datos en una ubicación nueva y con un nombre nuevo:  
  ```sql
  RESTORE DATABASE *new_database_name*  
  
  FROM *backup_device* [ ,...*n* ]  
  
  [ WITH  
 
   {  
  
      [ **RECOVERY** | NORECOVERY ]  
  
      [ , ] [ FILE ={ *backup_set_file_number* | @*backup_set_file_number* } ]  
  
      [ , ] MOVE '*logical_file_name_in_backup*' TO '*operating_system_file_name*' [ ,...*n* ]  
  
  }  
  
  ;  
  ```
  > [!NOTE] 
  > Cuando prepare la reubicación de una base de datos en un disco diferente, compruebe si hay espacio suficiente e identifique cualquier posible conflicto con los archivos existentes. Para ello, se usa una instrucción [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md) que especifica los mismos parámetros MOVE que tiene previsto usar en la instrucción RESTORE DATABASE.  
  
  En la tabla siguiente se describen los argumentos de esta instrucción RESTORE para restaurar una base de datos en una nueva ubicación. Para obtener más información sobre estos argumentos, vea [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
  *new_database_name*  
  El nuevo nombre para la base de datos.  
  
  > [!NOTE]
  > Si va a restaurar la base de datos en otra instancia de servidor, puede usar el nombre original de la base de datos en lugar de uno nuevo.  
  
  *backup_device* [ **,** ...*n* ]  
  Especifica una lista que contiene entre 1 y 64 dispositivos de copia de seguridad (separados por comas) desde los que se restaurará la copia de seguridad de la base de datos. Puede especificar un dispositivo físico de copia de seguridad o puede especificar el dispositivo de copia de seguridad lógico correspondiente, si se definió. Para especificar un dispositivo de copia de seguridad físico, use la opción DISK o TAPE:  
  
  { DISK | TAPE } **=** _physical_backup_device_name_  
  
  Para obtener más información, vea [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
  { **RECOVERY** | NORECOVERY }  
  Si la base de datos usa el modelo de recuperación completa, es posible que deba aplicar copias de seguridad de registros de transacciones después de restaurar la base de datos. En este caso, especifique la opción NORECOVERY.  
  
  En caso contrario, use la opción RECOVERY, que es la predeterminada.  
  
  FILE = { *backup_set_file_number* | @*backup_set_file_number* }  
  Identifica el conjunto de copia de seguridad que se va a restaurar. Por ejemplo, si *backup_set_file_number* es **1** , indica el primer conjunto de copia de seguridad del medio de copia, y si *backup_set_file_number* es **2** , indica el segundo conjunto de copia de seguridad. Puede obtener el valor *backup_set_file_number* de un conjunto de copia de seguridad mediante la instrucción [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) .  
  
  Cuando no se especifica esta opción, el comportamiento predeterminado es usar el primer conjunto de copia de seguridad del dispositivo de copia de seguridad.  
  
  Para obtener más información, vea "Especificar un conjunto de copia de seguridad" en [RESTORE &#40;argumentos, Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
  MOVE **'** _logical_file_name_in_backup_ **'** TO **'** _operating_system_file_name_ **'** [ **,** ...*n* ]  
  Especifica que el archivo de datos o de registro especificado por *logical_file_name_in_backup* debe restaurarse en la ubicación especificada por *operating_system_file_name*. Especifique una instrucción MOVE para cada archivo lógico del conjunto de copia de seguridad que desee restaurar en otra ubicación.  
  
  |Opción|Descripción|  
  |------------|-----------------|  
  |*logical_file_name_in_backup*|Especifica el nombre lógico de un archivo de datos o de registro del conjunto de copia de seguridad. El nombre de archivo lógico de un archivo de datos o de registro de un conjunto de copia de seguridad coincide con el nombre lógico que tenía en la base de datos cuando se creó el conjunto de copia de seguridad.<br /><br /> <br /><br /> Nota: Use [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md) para obtener una lista de los archivos lógicos del conjunto de copia de seguridad.|  
  |*operating_system_file_name*|Especifica una nueva ubicación para el archivo especificado por *logical_file_name_in_backup*. El archivo se restaurará en esta ubicación.<br /><br /> Opcionalmente, *operating_system_file_name* especifica un nombre de archivo nuevo para el archivo restaurado. Lo cual sería necesario si fuese a crear una copia de una base de datos existente en la misma instancia de servidor.|  
  |*n*|Es un marcador de posición que indica que puede especificar instrucciones MOVE adicionales.|  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 En este ejemplo se crea una base de datos denominada `MyAdvWorks` restaurando una copia de seguridad de la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , que incluye dos archivos: [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]_Data y [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]_Log. Esta base de datos usa el modelo de recuperación simple. La base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] ya existe en la instancia de servidor y, por lo tanto, los archivos de la copia de seguridad deben restaurarse en una nueva ubicación. La instrucción RESTORE FILELISTONLY se utiliza para determinar el número y los nombres de los archivos de la base de datos que se restaura. La copia de seguridad de la base de datos es el primer conjunto de copia de seguridad del dispositivo de copia de seguridad.  
  
> **NOTA:** En los ejemplos de copia de seguridad y restauración del registro de transacciones, que incluyen restauraciones a un momento dado, se utiliza la base de datos `MyAdvWorks_FullRM` , creada a partir de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] igual que en el siguiente ejemplo `MyAdvWorks` . Pero la base de datos `MyAdvWorks_FullRM` resultante se debe cambiar para usar el modelo de recuperación completa mediante la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] siguiente: ALTER DATABASE <nombreDeBaseDeDatos> SET RECOVERY FULL.  
  
```sql  
USE master;  
GO  
-- First determine the number and names of the files in the backup.  
-- AdventureWorks2012_Backup is the name of the backup device.  
RESTORE FILELISTONLY  
   FROM AdventureWorks2012_Backup;  
-- Restore the files for MyAdvWorks.  
RESTORE DATABASE MyAdvWorks  
   FROM AdventureWorks2012_Backup  
   WITH RECOVERY,  
   MOVE 'AdventureWorks2012_Data' TO 'D:\MyData\MyAdvWorks_Data.mdf',   
   MOVE 'AdventureWorks2012_Log' TO 'F:\MyLog\MyAdvWorks_Log.ldf';  
GO  
  
```  
  
 Para obtener un ejemplo de cómo crear una copia de seguridad completa de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , vea [Crear una copia de seguridad completa de base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
  
-   [Crear una copia de seguridad completa de base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Realizar una copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Restaurar una copia de seguridad de registros de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia del servidor &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Copiar bases de datos con Copias de seguridad y restauración](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)  
  
  
