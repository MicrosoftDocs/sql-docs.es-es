---
title: sys.dm_db_xtp_checkpoint_files (Transact-SQL) | Microsoft Docs
description: Muestra información acerca de los archivos de punto de comprobación, incluidos el tamaño del archivo, la ubicación física y el identificador de la transacción. Obtenga información acerca de cómo esta vista difiere en las versiones de SQL Server.
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: in-memory-oltp
ms.topic: reference
f1_keywords:
- dm_db_xtp_checkpoint_files
- sys.dm_db_xtp_checkpoint_files_TSQL
- dm_db_xtp_checkpoint_files_TSQL
- sys.dm_db_xtp_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_files dynamic management view
ms.assetid: ac8e6333-7a9f-478a-b446-5602283e81c9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 44c8b94a441e276561f3eb8fff165fede678e952
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160181"
---
# <a name="sysdm_db_xtp_checkpoint_files-transact-sql"></a>sys.dm_db_xtp_checkpoint_files (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Muestra información sobre los archivos de puntos de comprobación, incluidos el tamaño de archivo, la ubicación física y el identificador de transacción.  
  
> **Nota:** En el punto de control actual que no se ha cerrado, la columna de estado de s estará `ys.dm_db_xtp_checkpoint_files` en construcción para los nuevos archivos. Un punto de control se cierra automáticamente cuando hay suficiente crecimiento del registro de transacciones desde el último punto de control o si se emite el `CHECKPOINT` comando ([Checkpoint &#40;TRANSACT-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)).  
  
 Un grupo de archivos optimizados para memoria usa internamente archivos de solo anexar para almacenar las filas insertadas y eliminadas para las tablas en memoria. Hay dos tipos de archivos: Un archivo de datos contiene filas insertadas mientras que un archivo Delta contiene referencias a las filas eliminadas. 
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] es sustancialmente diferente de las versiones más recientes y se describe más abajo en el tema en [SQL Server 2014](#bkmk_2014).  
  
 Para obtener más información, vea [crear y administrar el almacenamiento de objetos de Memory-Optimized](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md).  
  
##  <a name="sssql15-and-later"></a><a name="bkmk_2016"></a> [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores  
 En la tabla siguiente se describen las columnas de `sys.dm_db_xtp_checkpoint_files` , a partir de **[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]** .  
  
|Nombre de la columna|Tipo|Descripción|  
|-----------------|----------|-----------------|  
|container_id|**int**|Identificador del contenedor (representado como un archivo de tipo FILESTREAM en sys.database_files) del que forma parte el archivo de datos o delta. Combinaciones con file_id en [sys.database_files &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|GUID del contenedor del que forma parte el archivo raíz, de datos o delta. Combina con file_guid en la tabla sys.database_files.|  
|checkpoint_file_id|**uniqueidentifier**|GUID del archivo de punto de comprobación.|  
|relative_file_path|**nvarchar(256)**|Ruta de acceso del archivo en relación con el contenedor al que está asignado.|  
|file_type|**smallint**|-1 gratis<br /><br /> 0 para el archivo de datos.<br /><br /> 1 para el archivo DELTA.<br /><br /> 2 para el archivo raíz<br /><br /> 3 para archivo de datos de gran tamaño|  
|file_type_desc|**nvarchar(60)**|LIBRE: todos los archivos que se mantienen como disponibles están disponibles para su asignación. Los archivos libres pueden variar en función del tamaño según las necesidades previstas del sistema. El tamaño máximo es 1 GB.<br /><br /> Los archivos de datos de datos contienen filas que se han insertado en tablas optimizadas para memoria.<br /><br /> Los archivos Delta Delta contienen referencias a las filas de los archivos de datos que se han eliminado.<br /><br /> Los archivos raíz raíz contienen metadatos del sistema para los objetos optimizados para memoria y compilados de forma nativa.<br /><br /> DATOS grandes: los archivos de datos grandes contienen valores insertados en columnas (n) VARCHAR (Max) y varbinary (Max), así como los segmentos de columna que forman parte de los índices de almacén de columnas en las tablas optimizadas para memoria.|  
|internal_storage_slot|**int**|El índice del archivo en la matriz de almacenamiento interno. NULL para ROOT o para un Estado distinto de 1.|  
|checkpoint_pair_file_id|**uniqueidentifier**|Archivo de datos o DELTA correspondiente. NULL para la raíz.|  
|file_size_in_bytes|**bigint**|Tamaño del archivo en el disco.|  
|file_size_used_in_bytes|**bigint**|Para los pares de archivos de punto de comprobación que todavía se están llenando, esta columna se actualizará después del punto de comprobación siguiente.|  
|logical_row_count|**bigint**|Para los datos, el número de filas insertadas.<br /><br /> Para Delta, número de filas eliminadas después de contabilizar la tabla Drop.<br /><br /> Para root, NULL.|  
|state|**smallint**|0: CREACIÓN PRECREADA<br /><br /> 1: EN CONSTRUCCIÓN<br /><br /> 2 - ACTIVE<br /><br /> 3-DESTINO DE COMBINACIÓN<br /><br /> 8: ESPERANDO EL TRUNCAMIENTO DEL REGISTRO|  
|state_desc|**nvarchar(60)**|CREADO previamente: se preasignan varios archivos de punto de comprobación para minimizar o eliminar cualquier espera para asignar nuevos archivos a medida que se ejecutan transacciones. Estos archivos creados con precreación pueden variar de tamaño, en función de las necesidades estimadas de la carga de trabajo, pero no contienen datos. Se trata de una sobrecarga de almacenamiento en las bases de datos con un grupo de archivos MEMORY_OPTIMIZED_DATA.<br /><br /> EN la construcción: estos archivos de punto de comprobación están en construcción, lo que significa que se rellenan en función de las entradas de registro generadas por la base de datos y que todavía no forman parte de un punto de control.<br /><br /> ACTIVE: contienen las filas insertadas o eliminadas de puntos de control cerrados anteriores. Contienen el contenido de las tablas que el área lee en la memoria antes de aplicar la parte activa del registro de transacciones en el reinicio de la base de datos. Esperamos que el tamaño de estos archivos de punto de comprobación sea aproximadamente 2x del tamaño en memoria de las tablas optimizadas para memoria, suponiendo que la operación de combinación se mantiene al día con la carga de trabajo transaccional.<br /><br /> DESTINO de combinación: el destino de las operaciones de combinación: estos archivos de punto de control almacenan las filas de datos consolidadas de los archivos de origen que se identificaron mediante la Directiva de combinación. Una vez instalada la mezcla, TARGET MERGE cambia al estado ACTIVE.<br /><br /> ESPERANDO el TRUNCAMIENTO del registro: una vez que se ha instalado la combinación y que el destino de la combinación es parte del punto de comprobación duradero, los archivos de punto de comprobación de origen de combinación pasan a este estado. Los archivos de este estado son necesarios para la corrección operativa de la base de datos con la tabla optimizada para memoria.  Por ejemplo, para recuperarse de un punto de comprobación durable para ir a un momento anterior en el tiempo.|  
|lower_bound_tsn|**bigint**|Límite inferior de la transacción en el archivo; es NULL si el estado no es (1,1).|  
|upper_bound_tsn|**bigint**|Límite superior de la transacción en el archivo; es NULL si el estado no es (1,1).|  
|begin_checkpoint_id|**bigint**|IDENTIFICADOR del punto de control inicial.|  
|end_checkpoint_id|**bigint**|IDENTIFICADOR del punto de control final.|  
|last_updated_checkpoint_id|**bigint**|IDENTIFICADOR del último punto de control que actualizó este archivo.|  
|encryption_status|**smallint**|0, 1, 2|  
|encryption_status_desc|**nvarchar(60)**|0 => SIN CIFRAR<br /><br /> 1 => CIFRADO CON LA CLAVE 1<br /><br /> 2 => CIFRADO CON LA CLAVE 2. Válido solo para archivos activos.|  
  
##  <a name="sssql14"></a><a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 En la tabla siguiente se describen las columnas de para `sys.dm_db_xtp_checkpoint_files` **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** .  
  
|Nombre de la columna|Tipo|Descripción|  
|-----------------|----------|-----------------|  
|container_id|**int**|Identificador del contenedor (representado como un archivo de tipo FILESTREAM en sys.database_files) del que forma parte el archivo de datos o delta. Combinaciones con file_id en [sys.database_files &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|GUID del contenedor del que forma parte el archivo de datos o delta.|  
|checkpoint_file_id|**GUID**|Identificador del archivo de datos o delta.|  
|relative_file_path|**nvarchar(256)**|Ruta de acceso al archivo de datos o delta, con respecto a la ubicación del contenedor.|  
|file_type|**tinyint**|0 para el archivo de datos.<br /><br /> 1 para el archivo delta.<br /><br /> Es NULL si la columna state está establecida en 7.|  
|file_type_desc|**nvarchar(60)**|El tipo de archivo: DATA_FILE, DELTA_FILE o NULL si la columna State está establecida en 7.|  
|internal_storage_slot|**int**|El índice del archivo en la matriz de almacenamiento interno. Es NULL si la columna state está establecida en 2 o 3.|  
|checkpoint_pair_file_id|**uniqueidentifier**|Archivo de datos o delta correspondiente.|  
|file_size_in_bytes|**bigint**|Tamaño del archivo que se usa. Es NULL si la columna state está establecida en 5, 6 o 7.|  
|file_size_used_in_bytes|**bigint**|Tamaño usado del archivo que se emplea. Es NULL si la columna state está establecida en 5, 6 o 7.<br /><br /> Para los pares de archivos de punto de comprobación que todavía se están llenando, esta columna se actualizará después del punto de comprobación siguiente.|  
|inserted_row_count|**bigint**|Número de filas del archivo de datos.|  
|deleted_row_count|**bigint**|Número de filas eliminadas del archivo delta.|  
|drop_table_deleted_row_count|**bigint**|Número de filas de los archivos de datos afectados por una operación de quitar tabla. Se aplica a los archivos de datos cuando la columna state es igual a 1.<br /><br /> Muestra los números de filas eliminadas de las tablas quitadas. Las estadísticas drop_table_deleted_row_count se compilan una vez que se completa la recolección de elementos no utilizados de memoria de las filas de las tablas quitadas y cuando se toma un punto de comprobación. Si reinicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antes de que las estadísticas de tablas quitadas se reflejen en esta columna, se actualizarán las estadísticas como parte de la recuperación. El proceso de recuperación no carga filas de tablas quitadas. Las estadísticas de tablas quitadas se compilan durante la fase de carga y se muestran en esta columna cuando se completa la recuperación.|  
|state|**int**|0: CREACIÓN PRECREADA<br /><br /> 1: EN CONSTRUCCIÓN<br /><br /> 2 - ACTIVE<br /><br /> 3-DESTINO DE COMBINACIÓN<br /><br /> 4: ORIGEN COMBINADO<br /><br /> 5: NECESARIO PARA LA COPIA DE SEGURIDAD/ALTA DISPONIBILIDAD<br /><br /> 6-EN TRANSICIÓN A MARCADORES DE EXCLUSIÓN<br /><br /> 7-DESECHO|  
|state_desc|**nvarchar(60)**|CREADO previamente: un pequeño conjunto de pares de archivos Delta y de datos, también conocidos como pares de archivos de punto de comprobación (CFP), se conservan preasignados para minimizar o eliminar las esperas de asignación de archivos nuevos a medida que se ejecutan transacciones. Son de tamaño completo con un tamaño de archivo de datos de 128 MB y un tamaño de archivo delta de 8 MB pero no contienen ningún dato. El número de CFP se calcula como el número de procesadores o programadores lógicos (uno por núcleo, sin valor máximo) con un mínimo de 8. Se trata de una sobrecarga de almacenamiento fija en bases de datos con tablas optimizadas para memoria.<br /><br /> EN construcción: conjunto de CFP que almacenan filas de datos recién insertadas y posiblemente eliminadas desde el último punto de control.<br /><br /> ACTIVE: contienen las filas insertadas y eliminadas de puntos de comprobación cerrados anteriores. Estos CFP contienen todas las filas insertadas y eliminadas necesarias antes de aplicar la parte activa del registro de transacciones en el reinicio de la base de datos. El tamaño de estos CFP será aproximadamente el doble del tamaño en memoria de las tablas optimizadas para memoria, siempre y cuando la operación de combinación esté actualizada con la carga de trabajo transaccional.<br /><br /> DESTINO de combinación: el PPC almacena las filas de datos consolidadas de las PPC (s) que se identificaron mediante la Directiva de combinación. Una vez instalada la mezcla, TARGET MERGE cambia al estado ACTIVE.<br /><br /> ORIGEN combinado: una vez instalada la operación de combinación, el CFP de origen se marca como origen combinado. Tenga en cuenta que el evaluador de la directiva de mezcla puede identificar varias mezclas pero un CFP solo puede participar en una operación de mezcla.<br /><br /> REQUERIDO para BACKUP/HA: una vez que se ha instalado la fusión mediante combinación y el destino de fusión mediante combinación es parte del punto de comprobación duradero, la CFP de origen de mezcla pasa a este estado. Los CFP que están en este estado son necesarios para el correcto funcionamiento de la base de datos con tablas optimizadas para memoria.  Por ejemplo, para recuperarse de un punto de comprobación durable para ir a un momento anterior en el tiempo. Un CFP se puede marcar para la recolección de elementos no utilizados una vez que el punto de truncamiento del registro sobrepasa los límites de su intervalo de transacción.<br /><br /> EN transición a marcadores de exclusión: el motor de OLTP In-Memory no necesita estos CFP y puede ser recolectados como elementos no utilizados. Este estado indica que estos CFP están esperando que el subproceso de fondo haga la transición al estado siguiente, que es TOMBSTONE.<br /><br /> Marcadores de exclusión: estos CFP están a la espera de ser recolectados por el recolector de elementos no utilizados de FileStream. ([sp_filestream_force_garbage_collection &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md))|  
|lower_bound_tsn|**bigint**|El límite inferior de las transacciones contenidas en el archivo. Es NULL si la columna state no tiene el valor 2, 3 o 4.|  
|upper_bound_tsn|**bigint**|El límite superior de las transacciones contenidas en el archivo. Es NULL si la columna state no tiene el valor 2, 3 o 4.|  
|last_backup_page_count|**int**|Recuento de páginas lógicas que se determina en la última copia de seguridad. Se aplica cuando la columna state está establecida en 2, 3, 4 o 5. Es NULL si no se conoce el recuento de páginas.|  
|delta_watermark_tsn|**int**|Transacción del último punto de comprobación que escribió en este archivo delta. Es el límite para el archivo delta.|  
|last_checkpoint_recovery_lsn|**nvarchar (23)**|Número de secuencia de registro de recuperación del último punto de comprobación que todavía necesita el archivo.|  
|tombstone_operation_lsn|**nvarchar (23)**|El archivo se eliminará una vez que tombstone_operation_lsn esté detrás del número de secuencia de registro del truncamiento del registro.|  
|logical_deletion_log_block_id|**bigint**|Solo se aplica al estado 5.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW DATABASE STATE` en el servidor.  
  
## <a name="use-cases"></a>Casos de uso  
 Puede calcular el almacenamiento utilizado por In-Memory OLTP como se indica a continuación:  
  
```  
-- total storage used by In-Memory OLTP  
SELECT SUM (file_size_in_bytes)/(1024*1024) as file_size_in_MB  
FROM sys.dm_db_xtp_checkpoint_files  
```  
  
  
Para ver un desglose del uso de almacenamiento por estado y tipo de archivo, ejecute la siguiente consulta:
  
```  
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(file_size_in_bytes) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  

  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de tablas optimizadas para memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
