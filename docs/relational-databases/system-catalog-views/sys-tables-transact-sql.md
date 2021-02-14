---
description: sys.tables (Transact-SQL)
title: Sys. Tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- tables_TSQL
- sys.tables_TSQL
- sys.tables
- tables
dev_langs:
- TSQL
helpviewer_keywords:
- sys.tables catalog view
ms.assetid: 8c42eba1-c19f-4045-ac82-b97a5e994090
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: df49c9e0c270629fdf00a9ac4ab19804e6a3d9c0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100352927"
---
# <a name="systables-transact-sql"></a>sys.tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila para cada tabla de usuario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|\<inherited columns>||Para obtener una lista de las columnas que hereda esta vista, vea [Sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|lob_data_space_id|**int**|Un valor diferente de cero es el identificador del espacio de datos (grupo de archivos o esquema de partición) que contiene los datos de objeto binario grande (LOB) para esta tabla. Entre los ejemplos de tipos de datos LOB se incluyen **varbinary (Max)**, **VARCHAR (Max)**, **Geography** o **XML**.<br /><br /> 0 = La tabla no contiene datos LOB.|  
|filestream_data_space_id|**int**|Es el identificador del espacio de datos para un grupo de archivos FILESTREAM o un esquema de partición compuesto por grupos de archivos FILESTREAM.<br /><br /> Para notificar el nombre de un grupo de archivos FILESTREAM, ejecute la consulta `SELECT FILEGROUP_NAME (filestream_data_space_id) FROM sys.tables` .<br /><br /> sys.tables se puede combinar con las vistas siguientes en filestream_data_space_id = data_space_id.<br /><br /> -Sys. grupos de archivos<br /><br /> -sys.partition_schemes<br /><br /> -Sys. Indexes<br /><br /> -sys.allocation_units<br /><br /> -sys.fulltext_catalogs<br /><br /> -sys.data_spaces<br /><br /> -sys.destination_data_spaces<br /><br /> -sys.master_files<br /><br /> -sys.database_files<br /><br /> -backupfilegroup (combinación en filegroup_id)|  
|max_column_id_used|**int**|Identificador de columna máximo que se ha utilizado en esta tabla.|  
|lock_on_bulk_load|**bit**|La tabla está bloqueada en una carga masiva. Para obtener más información, vea [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|uses_ansi_nulls|**bit**|La tabla se creó con la opción de base de datos SET ANSI_NULLS establecida en ON.|  
|is_replicated|**bit**|1 = La tabla se publicó con la replicación de instantáneas o transaccional.|  
|has_replication_filter|**bit**|1 = La tabla tiene un filtro de replicación.|  
|is_merge_published|**bit**|1 = La tabla se publicó con la replicación de mezcla.|  
|is_sync_tran_subscribed|**bit**|1 = La tabla se suscribió con una suscripción de actualización inmediata.|  
|has_unchecked_assembly_data|**bit**|1 = La tabla contiene datos persistentes que dependen de un ensamblado cuya definición cambió durante el último ALTER ASSEMBLY. Se restablecerá en 0 tras la siguiente operación DBCC CHECKDB o DBCC CHECKTABLE correcta.|  
|text_in_row_limit|**int**|Número máximo de bytes permitido para text in row.<br /><br /> 0 = la opción text in row no está establecida. Para obtener más información, vea [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|large_value_types_out_of_row|**bit**|1 = Los tipos de valores grandes se guardan fuera de la fila. Para obtener más información, vea [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|is_tracked_by_cdc|**bit**|1 = La tabla está habilitada para la captura de datos modificados. Para obtener más información, vea [sys.sp_cdc_enable_table &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).|  
|lock_escalation|**tinyint**|Valor de la opción LOCK_ESCALATION para la tabla:<br /><br /> 0 = TABLE<br /><br /> 1 = DISABLE<br /><br /> 2 = AUTO|  
|lock_escalation_desc|**nvarchar(60)**|Descripción de texto de la opción lock_escalation para la tabla. Los valores posibles son: TABLE, AUTO y DISABLE.|  
|is_filetable|**bit**|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> 1 = La tabla es un objeto FileTable.<br /><br /> Para más información sobre FileTables, vea [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).|  
|durabilidad|**tinyint**|**Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores, y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> Estos son los valores posibles:<br /><br /> 0 = SCHEMA_AND_DATA<br /><br /> 1 = SCHEMA_ONLY<br /><br /> El valor 0 es el predeterminado.|  
|durability_desc|**nvarchar(60)**|**Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores, y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> Los posibles valores son los siguientes:<br /><br /> SCHEMA_ONLY<br /><br /> SCHEMA_AND_DATA<br /><br /> El valor de SCHEMA_AND_DATA indica que la tabla es una tabla en memoria perdurable. SCHEMA_AND_DATA es el valor predeterminado para las tablas optimizadas para memoria. El valor de SCHEMA_ONLY indica que los datos de la tabla no son persistentes tras reiniciar la base de datos con objetos optimizados para memoria.|  
|is_memory_optimized|**bit**|**Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores, y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> Los posibles valores son los siguientes:<br /><br /> 0 = no optimizado en memory.<br /><br /> 1 = está optimizado para memoria.<br /><br /> Un valor de 0 es el valor predeterminado.<br /><br /> Las tablas optimizadas para memoria se encuentran en las tablas de usuario de memoria, el esquema que se conserva en el disco similar a otras tablas de usuario. Se puede tener acceso a las tablas optimizadas para memoria desde procedimientos almacenados compilados de forma nativa.|  
|temporal_type|**tinyint**|**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> Valor numérico que representa el tipo de tabla:<br /><br /> 0 = NON_TEMPORAL_TABLE<br /><br /> 1 = HISTORY_TABLE<br /><br /> 2 = SYSTEM_VERSIONED_TEMPORAL_TABLE|  
|temporal_type_desc|**nvarchar(60)**|**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> La descripción de texto del tipo de tabla:<br /><br /> NON_TEMPORAL_TABLE<br /><br /> HISTORY_TABLE<br /><br /> SYSTEM_VERSIONED_TEMPORAL_TABLE|  
|history_table_id|**int**|**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> Cuando temporal_type en (2, 4) devuelve object_id de la tabla que mantiene los datos históricos; de lo contrario, devuelve NULL.|  
|is_remote_data_archive_enabled|**bit**|**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> Indica si la tabla está habilitada para Stretch.<br /><br /> 0 = la tabla no está habilitada para Stretch.<br /><br /> 1 = la tabla está habilitada para Stretch.<br /><br /> Para obtener más información, vea [Stretch Database](../../sql-server/stretch-database/stretch-database.md).|  
|is_external|**bit**|**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[sssdwfull](../../includes/sssdwfull-md.md)] .<br /><br /> Indica que la tabla es una tabla externa.<br /><br /> 0 = la tabla no es una tabla externa.<br /><br /> 1 = la tabla es una tabla externa.| 
|history_retention_period|**int**|**Se aplica a**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>Valor numérico que representa la duración del período de retención del historial temporal en unidades especificadas con history_retention_period_unit. |  
|history_retention_period_unit|**int**|**Se aplica a**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>Valor numérico que representa el tipo de unidad de período de retención de historial temporal. <br /><br />-1: INFINITO <br /><br />3: DÍA <br /><br />4: SEMANA <br /><br />5: MES <br /><br />6: AÑO |  
|history_retention_period_unit_desc|**nvarchar(10**|**Se aplica a**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>La descripción de texto del tipo de unidad de período de retención de historial temporal. <br /><br />INFINITE <br /><br />DAY <br /><br />WEEK <br /><br />MONTH <br /><br />YEAR |  
|is_node|**bit**|**Se aplica a**: [!INCLUDE[sssql17-md.md](../../includes/sssql17-md.md)] y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>1 = se trata de una tabla de nodo de gráfico. <br /><br />0 = no es una tabla de nodo de gráfico. |  
|is_edge|**bit**|**Se aplica a**: [!INCLUDE[sssql17-md.md](../../includes/sssql17-md.md)] y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>1 = esta es una tabla de borde del gráfico. <br /><br />0 = no es una tabla de borde de gráfico. |  

## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente devuelve todas las tablas de usuario que no tienen una clave principal.  
  
```  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasPrimaryKey') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
En el ejemplo siguiente se muestra cómo se pueden exponer los datos temporales relacionados.  
   
**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]
  
```  
SELECT T1.object_id, T1.name as TemporalTableName, SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,  
T2.name as HistoryTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,  
T1.temporal_type_desc  
FROM sys.tables T1  
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id  
ORDER BY T1.temporal_type desc  
```  

En el ejemplo siguiente se muestra cómo se puede exponer la información sobre la retención de historial temporal.  

**Se aplica a**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].  
  
```  
SELECT DB.is_temporal_history_retention_enabled, SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema, 
T1.name as TemporalTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema, T2.name as HistoryTableName,
T1.history_retention_period, T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases where name = DB_NAME()) DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2 
```  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
