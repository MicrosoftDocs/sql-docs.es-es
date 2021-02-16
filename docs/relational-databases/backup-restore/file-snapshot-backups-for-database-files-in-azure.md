---
title: Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure | Microsoft Docs
description: La copia de seguridad de instantánea de archivos de SQL Server usa instantáneas de Azure para proporcionar copias de seguridad y restauraciones más rápidas de los archivos de base de datos almacenados con el servicio Azure Blob Storage.
ms.custom: ''
ms.date: 05/23/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 17a81fcd-8dbd-458d-a9c7-2b5209062f45
author: cawrites
ms.author: chadam
ms.openlocfilehash: 64eced0d45854db8d970c9cfbd15132ef00f070a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345577"
---
# <a name="file-snapshot-backups-for-database-files-in-azure"></a>Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] La copia de seguridad de instantánea de archivos usa las instantáneas de Azure para proporcionar copias de seguridad prácticamente instantáneas y restauraciones más rápidas de los archivos de base de datos almacenados mediante el servicio de almacenamiento de blobs de Azure. Esta capacidad le permite simplificar las directivas de copia de seguridad y restauración. Para obtener una demostración en vivo, consulte [Demo of Point in Time Restore](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)(Demostración de restauración a un momento dado). Para obtener más información sobre cómo almacenar los archivos de base de datos mediante el servicio de almacenamiento de blobs de Azure, vea [Archivos de datos de SQL Server en Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md).  
  
 ![Diagrama de arquitectura de copia de seguridad de instantánea](../../relational-databases/backup-restore/media/snapshotbackups.PNG "Diagrama de arquitectura de copia de seguridad de instantánea")  
  
 **Descargar**  
  
-   Para descargar [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], vaya al  **[Centro de evaluación](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)** .  
  
-   ¿Tiene una cuenta de Azure?  Si es así, haga clic **[aquí](https://azure.microsoft.com/services/virtual-machines/sql-server/)** para poner en marcha una máquina virtual con [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ya instalado.  
  
## <a name="using-azure-snapshots-to-back-up-database-files-stored-in-azure"></a>Uso de instantáneas de Azure para hacer copias de seguridad de los archivos de base de datos almacenados en Azure  
  
### <a name="what-is-a-ssnoversion-file-snapshot-backup"></a>¿Qué es una copia de seguridad de instantánea de archivos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ?  
 Una copia de seguridad de instantánea de archivos se compone de un conjunto de instantáneas de Azure de los blobs que contienen los archivos de base de datos, más un archivo de copia de seguridad que incluye punteros a esas instantáneas de archivos. Cada instantánea de archivos se almacena en el contenedor junto con el blob base. Se puede especificar que el archivo de copia de seguridad se escriba en una dirección URL, un disco o una cinta. La opción recomendada es la de dirección URL. Consulte [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md) para obtener más información sobre copias de seguridad y [Copia de seguridad en URL de SQL Server](../../relational-databases/backup-restore/sql-server-backup-to-url.md) para copias de seguridad en la dirección URL.  
  
 ![Arquitectura de la característica de instantánea](../../relational-databases/backup-restore/media/snapshotbackups-flat.png "Arquitectura de la característica de instantánea")  
  
 Si se elimina el blob de base, se invalidará el conjunto de copia de seguridad y no se podrán anular los blobs que contengan instantáneas de archivos (a menos que especifique expresamente que se anule el blob con todas sus instantáneas de archivo). Por otro lado, anular una base de datos o un archivo de datos no elimina el blob de base ni ninguno de sus archivos de instantáneas. Además, aunque se elimine el archivo de copia de seguridad, no se elimina ninguna de las instantáneas de archivos del conjunto de copia de seguridad. Para eliminar un conjunto de copia de seguridad de instantánea de archivos, use el procedimiento almacenado del sistema **sys.sp_delete_backup** .  
  
 **Copia de seguridad de bases de datos completa:** si se usa una copia de seguridad de instantánea de archivos para realizar una copia de seguridad completa de las bases de datos, se crea una instantánea de Azure de todos los archivos de datos y de registro que incluye la base de datos, se establece la cadena de copia de seguridad del registro de transacciones y se escribe la ubicación de las instantáneas de archivos en el archivo de copia de seguridad.  
  
 **Copia de seguridad del registro de transacciones:** si se usa una copia de seguridad de instantánea de archivos para realizar una copia de seguridad del registro de transacciones, se crea una instantánea de archivos de cada archivo de base de datos (no solo el registro de transacciones), se registra la información de ubicación de la instantánea de archivos en el archivo de copia de seguridad y se trunca el archivo del registro de transacciones.  
  
> [!IMPORTANT]  
>  Tras la primera copia de seguridad completa que se requiere para establecer la cadena de copia de seguridad del registro de transacciones (que puede ser una copia de seguridad de instantánea de archivos), solo será necesario realizar copias de seguridad del registro de transacciones. Esto se debe a que cada conjunto de copia de seguridad de instantánea de archivos del registro de transacciones contiene instantáneas de todos los archivos de base de datos y se puede usar para realizar una restauración de base de datos o una restauración del registro. Después de la primera copia de seguridad de base de datos completa, no será necesario realizar más copias de seguridad completas o diferenciales porque el servicio de almacenamiento de blobs de Azure se encarga de controlar las diferencias entre las instantáneas de archivos y el estado actual del blob de base para cada archivo de base de datos.  
  
> [!NOTE]  
>  Para obtener un tutorial sobre el uso de SQL Server 2016 con el servicio Microsoft Azure Blob Storage, vea [Tutorial: Uso del servicio Microsoft Azure Blob Storage con bases de datos de SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
### <a name="restore-using-file-snapshot-backups"></a>Restauración con copias de seguridad de instantánea de archivos  
 Dado que cada conjunto de copia de seguridad de instantánea de archivos contiene una instantánea de cada archivo de base de datos, el proceso de restauración requiere a lo sumo dos conjuntos adyacentes de copias de seguridad de instantánea de archivos. Esto es cierto independientemente de si el conjunto de copia de seguridad corresponde a una copia de seguridad de la base de datos completa o a una copia de seguridad del registro. Esto es muy diferente al proceso de restauración en el que se usan archivos de copia de seguridad de secuencias tradicional. Con la copia de seguridad de secuencias tradicional, el proceso de restauración exige usar una cadena completa de conjuntos de copia de seguridad: la copia de seguridad completa, una copia de seguridad diferencial y una o varias copias del registro de transacciones. La parte de la recuperación del proceso de restauración sigue siendo la misma con independencia de si la restauración usa una copia de seguridad de instantánea de archivos o un conjunto de copia de seguridad de secuencias.  
  
 **A la hora de un conjunto de copia de seguridad:** para realizar una operación RESTORE DATABASE que restaure una base de datos a la hora de un determinado conjunto de copia de seguridad de instantánea de archivos, tan solo es necesario el conjunto de copia de seguridad específico, más los blobs de base. Dado que para realizar una operación RESTORE DATABASE puede usar un conjunto de copia de seguridad de instantánea de archivos del registro de transacciones, lo normal es utilizar un conjunto de copia de seguridad del registro de transacciones para llevar a cabo este tipo de operación RESTORE DATABASE. En raras ocasiones usará un conjunto de copia de seguridad de base de datos completa. Al final de este tema se incluye un ejemplo en el que se demuestra esta técnica.  
  
 **A un punto en el tiempo entre dos conjuntos de copia de seguridad de instantánea de archivos:** para realizar una operación RESTORE DATABASE que restaure una base de datos a un momento específico en el tiempo entre la hora de dos conjuntos adyacentes de copia de seguridad del registro de transacciones, solo se requieren dos conjuntos: uno antes y otro después del punto en el tiempo al que se quiere restaurar la base de datos. Para lograrlo, se realizan las siguientes operaciones: una operación RESTORE DATABASE WITH NORECOVERY con el conjunto de copia de seguridad de instantánea de archivos del registro de transacciones del punto en el tiempo anterior; y una operación RESTORE LOG WITH RECOVERY con el conjunto de copia de seguridad de instantánea de archivos del registro de transacciones del punto en el tiempo posterior y usando un argumento STOPAT para especificar el punto en el tiempo en el que se detendrá la recuperación de la copia de seguridad del registro de transacciones. Al final de este tema se incluye un ejemplo en el que se demuestra esta técnica. Para obtener una demostración en vivo, consulte [Demo of Point in Time Restore](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)(Demostración de restauración a un momento dado).  
  
### <a name="file-backup-set-maintenance"></a>Mantenimiento de conjuntos de copia de seguridad de archivos  
 **Eliminar un conjunto de copia de seguridad de instantánea de archivos:** no se permite usar el argumento FORMAT para sobrescribir un conjunto de copia de seguridad de instantánea de archivos. El motivo es evitar que queden huérfanas las instantáneas de archivos creadas con la copia de seguridad de instantánea de archivos original. Para eliminar un conjunto de copia de seguridad de instantánea de archivos, use el procedimiento almacenado del sistema **sys.sp_delete_backup** . Este procedimiento almacenado elimina el archivo de copia de seguridad y las instantáneas de archivos que componen el conjunto de copia de seguridad. Cualquier otro método para eliminar un conjunto de copia de seguridad de instantánea de archivos puede eliminar el archivo de copia de seguridad sin eliminar las instantáneas de archivos del conjunto de copia de seguridad.  
  
 **Eliminar instantáneas de archivos de copia de seguridad huérfanas:** es posible que haya instantáneas de archivos huérfanas si se elimina el archivo de copia de seguridad sin usar el procedimiento almacenado del sistema **sys.sp_delete_backup** o si se anula una base de datos o un archivo de base de datos mientras los blobs que contienen la base de datos o el archivo de base de datos tienen instantáneas de archivos de copia de seguridad asociadas a ellos. Para identificar las instantáneas de archivos que puedan estar huérfanas, use la función del sistema **sys.fn_db_backup_file_snapshots** para hacer una lista con todas las instantáneas de los archivos de la base de datos. Para identificar las instantáneas de archivos que forman parte de un determinado conjunto de copia de seguridad de instantánea de archivos, use el procedimiento almacenado del sistema RESTORE FILELISTONLY. Después, puede usar el procedimiento almacenado del sistema **sys.sp_delete_backup_file_snapshot** para eliminar una instantánea de archivos de copia de seguridad que se haya quedado huérfana. Al final de este tema podrá consultar ejemplos en los que se usan esta función de sistema y estos procedimientos almacenados del sistema. Para obtener más información, vea [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md), [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md), [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) y [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
### <a name="considerations-and-limitations"></a>Consideraciones y limitaciones  
 **Almacenamiento premium:** las siguientes limitaciones se aplican al usar Premium Storage.  
  
-   El propio archivo de copia de seguridad no puede almacenarse usando almacenamiento premium.  
  
-   La frecuencia de las copias de seguridad no puede ser inferior a 10 minutos.  
  
-   El número máximo de instantáneas que se pueden almacenar es 100.  
  
-   Es necesario RESTORE WITH MOVE.  
  
-   Para obtener más información sobre Premium Storage, consulte [Premium Storage: almacenamiento de alto rendimiento para cargas de trabajo de máquinas virtuales de Azure](/azure/virtual-machines/disks-types)  
  
 **Cuenta de almacenamiento única:** la instantánea de archivos y los blobs de destino deben usar la misma cuenta de almacenamiento.  
  
 **Modelo de recuperación masiva:** cuando se usa un modelo de recuperación optimizado para cargas masivas de registros y se trabaja con una copia de seguridad del registro de transacciones que contiene transacciones registradas al mínimo, no se puede hacer una restauración del registro (incluida una recuperación a un momento dado) mediante la copia de seguridad del registro de transacciones. En su lugar, realice una restauración de base de datos al momento del conjunto de copia de seguridad de instantánea de archivos. Esta limitación es idéntica a la limitación de la copia de seguridad de secuencias.  
  
 **Restauración con conexión:** al utilizar las copias de seguridad de instantánea, no se pueden realizar restauraciones con conexión. Para obtener más información sobre la restauración con conexión, vea [Restauración con conexión &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
 **Facturación:** el uso de la copia de seguridad de instantánea de archivos de SQL Server conlleva cargos adicionales con el cambio de datos. Para obtener más información, vea [Introducción a cómo las instantáneas pueden incrementar los costos](/rest/api/storageservices/Understanding-How-Snapshots-Accrue-Charges).  
  
 **Archivado:** si quiere archivar una copia de seguridad de instantánea de archivos, puede hacerlo en almacenamiento de blobs o en copia de seguridad de secuencias. Para archivar en el almacenamiento de blobs, copie las instantáneas del conjunto de copia de seguridad de instantánea de archivos en blobs diferentes. Para archivar en una copia de seguridad de secuencias, restaure la copia de seguridad de instantánea de archivos como una base de datos nueva y luego realice una copia de seguridad de secuencias normal con compresión y/o cifrado y archívela durante el tiempo que desee, independientemente de los blobs de base.  
  
> [!IMPORTANT]  
>  El mantenimiento de varias copias de seguridad de instantáneas de archivos solo supone una pequeña sobrecarga en el rendimiento. Sin embargo, un número excesivo de copias de seguridad de instantáneas de archivos puede afectar al rendimiento de E/S en la base de datos. Le recomendamos que únicamente mantenga las copias de seguridad de instantáneas de archivos que sean necesarias para su objetivo de punto de recuperación.  
  
## <a name="backing-up-the-database-and-log-using-a-file-snapshot-backup"></a>Copia de seguridad de la base de datos y el registro mediante una copia de seguridad de instantánea de archivos  
 En el ejemplo siguiente se usa la copia de seguridad de instantánea de archivos para realizar una copia de seguridad de la base de datos de ejemplo AdventureWorks2016 en una dirección URL.  
  
```  
-- To permit log backups, before the full database backup, modify the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE AdventureWorks2016  
   SET RECOVERY FULL;  
GO  
-- Back up the full AdventureWorks2016 database.  
BACKUP DATABASE AdventureWorks2016   
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak'   
WITH FILE_SNAPSHOT;  
GO  
-- Back up the AdventureWorks2016 log using a time stamp in the backup file name.  
DECLARE @Log_Filename AS VARCHAR (300);  
SET @Log_Filename = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_Log_'+   
REPLACE (REPLACE (REPLACE (CONVERT (VARCHAR (40), GETDATE (), 120), '-','_'),':', '_'),' ', '_') + '.trn';  
BACKUP LOG AdventureWorks2016  
 TO URL = @Log_Filename WITH FILE_SNAPSHOT;  
GO  
```  
  
## <a name="restoring-from-a-ssnoversion-file-snapshot-backup"></a>Restauración a partir de una copia de seguridad de instantánea de archivos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 En el ejemplo siguiente, se restaura la base de datos AdventureWorks2016 mediante un conjunto de copia de seguridad de instantánea de archivos del registro de transacciones y se muestra una operación de recuperación. Observe que puede restaurar una base de datos a partir de un único conjunto de copia de seguridad de instantánea de archivos del registro de transacciones.  
  
```  
RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.trn'   
WITH RECOVERY, REPLACE;  
GO  
```  
  
## <a name="restoring-from-a-ssnoversion-file-snapshot-backup-to-a-point-in-time"></a>Restauración a partir de una copia de seguridad de instantánea de archivos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un momento dado  
 En el ejemplo siguiente se restaura la base de datos AdventureWorks2016 a su estado en un momento en el tiempo. Para ello, se usan dos conjuntos de copia de seguridad de instantánea de archivos del registro de transacciones y se muestra una operación de recuperación.  
  
```  
RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.trn'   
WITH NORECOVERY,REPLACE;  
GO   
  
RESTORE LOG AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_18_00_00.trn'   
WITH RECOVERY,STOPAT = 'May 18, 2015 5:35 PM';  
GO  
```  
  
## <a name="deleting-a-database-file-snapshot-backup-set"></a>Eliminación de un conjunto de copia de seguridad de instantánea de archivos de base de datos  
 Para eliminar un conjunto de copia de seguridad de instantánea de archivos, use el procedimiento almacenado del sistema **sys.sp_delete_backup** . Especifique el nombre de la base de datos para que el sistema compruebe que el conjunto de copia de seguridad de instantánea de archivos especificado es efectivamente una copia de seguridad de la base de datos especificada. Si no se especifica ningún nombre de base de datos, se eliminará el conjunto de copia de seguridad especificado con sus instantáneas de archivos sin realizar esa validación. Para obtener más información, vea [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md).  
  
> [!WARNING]  
>  Si se usa otro método (como el Portal de administración de Microsoft Azure o el visor de Almacenamiento de Azure en SQL Server Management Studio) para intentar eliminar un conjunto de copia de seguridad de instantánea de archivos, las instantáneas de archivos del conjunto de copia de seguridad no se eliminarán. Estas herramientas solo eliminarán el archivo de copia de seguridad que contiene los punteros a las instantáneas de archivos del conjunto de copia de seguridad de instantánea de archivos. Para identificar las instantáneas de archivo de copia de seguridad que permanecen después de que un archivo de copia de seguridad se haya eliminado de forma incorrecta, use la función del sistema **sys.fn_db_backup_file_snapshots** y, después, use el procedimiento almacenado del sistema **sys.sp_delete_backup_file_snapshot** para eliminar una instantánea de archivo de copia de seguridad individual.  
  
 En el ejemplo siguiente se elimina el conjunto de copia de seguridad de instantánea de archivos especificado, incluidos el archivo de copia de seguridad y las instantáneas de archivos que incluyen el conjunto de copia de seguridad especificado.  
  
```  
sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak', 'adventureworks2016' ;  
GO  
  
```  
  
## <a name="viewing-database-backup-file-snapshots"></a>Visualización de instantáneas de archivos de copia de seguridad de base de datos  
 Para ver instantáneas de archivos del blob de base para cada archivo de base de datos, use la función del sistema **sys.fn_db_backup_file_snapshots** . Esta función del sistema permite ver todas las instantáneas de archivos de copia de seguridad de cada blob de base para una base de datos almacenada con el servicio de almacenamiento de blobs de Azure. Un ejemplo de uso principal de esta función es identificar las instantáneas de archivo de copia de seguridad de una base de datos que se mantienen cuando se elimina el archivo de copia de seguridad de un conjunto de copia de seguridad de instantánea de archivo mediante un mecanismo distinto del procedimiento almacenado del sistema **sys.sp_delete_backup** . Para determinar las instantáneas de archivo de copia de seguridad que forman parte de los conjuntos de copia de seguridad intactos y aquellas que no forman parte de esos conjuntos intactos, use el procedimiento almacenado del sistema **RESTORE FILELISTONLY**  para enumerar las instantáneas de archivo que pertenecen a cada archivo de copia de seguridad. Para obtener más información, vea [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) y [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
 En el ejemplo siguiente se devuelve la lista de todas las instantáneas de archivos de copia de seguridad correspondientes a la base de datos especificada.  
  
```  
--Either specify the database name or set the database context  
USE AdventureWorks2016  
select * from sys.fn_db_backup_file_snapshots (null) ;  
GO  
select * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016') ;  
GO  
  
```  
  
## <a name="deleting-an-individual-database-backup-file-snapshot"></a>Eliminación de una única instantánea de archivos de copia de seguridad de base de datos  
 Para eliminar una instantánea de archivo de copia de seguridad de un blob de base de datos, use el procedimiento almacenado del sistema **sys.sp_delete_backup_file_snapshot** . Un ejemplo de uso principal de este procedimiento almacenado del sistema es eliminar los archivos de instantánea de archivo huérfanos que permanecen después de que se haya eliminado un archivo de copia de seguridad usando un método distinto del procedimiento almacenado del sistema **sys.sp_delete_backup** . Para obtener más información, vea [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md).  
  
> [!WARNING]  
>  La eliminación de una instantánea de archivos individual que forma parte de un conjunto de copia de seguridad de instantánea de archivos invalidará el conjunto de copia de seguridad.  
  
 En el ejemplo siguiente se elimina la instantánea de archivos de copia de seguridad especificada. La dirección URL de la copia de seguridad especificada se obtuvo mediante la función del sistema **sys.fn_db_backup_file_snapshots** .  
  
```  
sys.sp_delete_backup_file_snapshot N'adventureworks2016', N'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016Data.mdf?snapshot=2015-05-29T21:31:31.6502195Z';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Tutorial: Uso del servicio Microsoft Azure Blob Storage con bases de datos de SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
