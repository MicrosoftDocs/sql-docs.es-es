---
description: sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)
title: sys.dm_db_column_store_row_group_physical_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_column_store_row_group_physical_stats_TSQL
- sys.dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_column_store_row_group_physical_stats
- sys.dm_db_column_store_row_group_physical_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b6a1b4c7a8bdead03561989fbe17f5a28b4f0621
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237513"
---
# <a name="sysdm_db_column_store_row_group_physical_stats-transact-sql"></a>sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Proporciona información de nivel de filas actual sobre todos los índices de almacén de columnas en la base de datos actual.  

Esto extiende la vista de catálogo [sys.column_store_row_groups &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md).  

|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|IDENTIFICADOR de la tabla subyacente.|  
|**id_de_índice**|**int**|IDENTIFICADOR de este índice de almacén de columnas en *object_id* tabla.|  
|**partition_number**|**int**|IDENTIFICADOR de la partición de tabla que contiene *row_group_id*. Puede utilizar partition_number para unir esta DMV a sys.partitions.|  
|**row_group_id**|**int**|IDENTIFICADOR de este grupo de filas. En el caso de las tablas con particiones, el valor es único dentro de la partición.<br /><br /> -1 para una cola en memoria.|  
|**delta_store_hobt_id**|**bigint**|Hobt_id para un grupo de filas en el almacén Delta.<br /><br /> Es NULL si el grupo de filas no está en el almacén Delta.<br /><br /> NULL para la cola de una tabla en memoria.|  
|**state**|**tinyint**|Número de identificación asociado *state_description*.<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED<br /><br /> 4 = MARCADORES DE EXCLUSIÓN<br /><br /> Compressed es el único Estado que se aplica a las tablas en memoria.|  
|**state_desc**|**nvarchar(60)**|Descripción del estado del grupo de filas:<br /><br /> 0-INVISIBLE: un grupo de filas que se está compilando. Por ejemplo: <br />Un grupo de filas del almacén de columnas es INVISIBLE mientras se comprimen los datos. Una vez finalizada la compresión, un cambio de metadatos cambia el estado del grupo de filas de almacén de columnas de INVISIBLE a comprimido, y el estado del grupo de filas almacén Delta de cerrado a EXTINGUIdo.<br /><br /> 1-abrir-un grupo de filas de almacén Delta que acepta nuevas filas. Un grupo de filas abierto está todavía en formato de almacén de filas y no se ha comprimido al formato de almacén de columnas.<br /><br /> 2-cerrado: un grupo de filas en el almacén Delta que contiene el número máximo de filas y está esperando a que el proceso de motor de tupla lo comprimirá en el almacén de columnas.<br /><br /> 3-comprimido: Grupo de filas que se comprime con la compresión de almacén de columnas y se almacena en el almacén de columnas.<br /><br /> 4-marcadores de exclusión: un grupo de filas que se encontraba anteriormente en el almacén Delta y ya no se usa.|  
|**total_rows**|**bigint**|Número de filas almacenadas físicamente en el grupo de filas. Para grupos de filas comprimidos. Incluye las filas marcadas como eliminadas.|  
|**deleted_rows**|**bigint**|Número de filas almacenadas físicamente en un grupo de filas comprimido que se han marcado para su eliminación.<br /><br /> Es 0 en el caso de los grupos de filas que se encuentran en el almacén delta.|  
|**size_in_bytes**|**bigint**|Tamaño combinado, en bytes, de todas las páginas de este grupo de filas. Este tamaño no incluye el tamaño necesario para almacenar metadatos o diccionarios compartidos.|  
|**trim_reason**|**tinyint**|Motivo por el que desencadenó el grupo de filas COMPRIMIDOs para que tenga menos del número máximo de filas.<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION<br /><br /> 1-NO_TRIM<br /><br /> 2-CARGA MASIVA<br /><br /> 3-REORG<br /><br /> 4-DICTIONARY_SIZE<br /><br /> 5-MEMORY_LIMITATION<br /><br /> 6-RESIDUAL_ROW_GROUP<br /><br /> 7-STATS_MISMATCH<br /><br /> 8-SOBRANTE<br /><br /> 9-AUTO_MERGE|  
|**trim_reason_desc**|**nvarchar(60)**|Descripción de *trim_reason*.<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION: se produjo al actualizar desde la versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> 1-NO_TRIM: no se recortó el grupo de filas. El grupo de filas se comprimió con el máximo de 1.048.576 filas.  El número de filas puede ser menor si un subconjunto de filas se eliminó después de que se cerrara Delta filas<br /><br /> 2-bulk-Bulk: el tamaño del lote de carga masiva limitó el número de filas.<br /><br /> 3-REORG: compresión forzada como parte del comando REORG.<br /><br /> 4-DICTIONARY_SIZE: el tamaño del diccionario creció demasiado para comprimir todas las filas juntas.<br /><br /> 5-MEMORY_LIMITATION: no hay suficiente memoria disponible para comprimir todas las filas juntas.<br /><br /> 6-RESIDUAL_ROW_GROUP: cerrado como parte del último grupo de filas con filas < 1 millón durante la operación de generación de índice<br /><br /> 7-STATS_MISMATCH: solo para el almacén de columnas en la tabla en memoria. Si las estadísticas se indican incorrectamente >= 1 millón filas calificadas en la cola pero encontramos menos, el filas comprimido tendrá < 1 millón filas.<br /><br /> 8-sobrante: solo para el almacén de columnas en la tabla en memoria. Si el final de la cola tiene > 1 millón filas completas, el último lote restante filas se comprimen si el recuento está entre 100 y 1 millón<br /><br /> 9-AUTO_MERGE: una operación de combinación del motor de tupla que se ejecuta en segundo plano consolida una o más filas en este filas.|  
|**transition_to_compressed_state**|TINYINT|Muestra cómo se ha pasado este filas de almacén Delta a un estado comprimido en el almacén de columnas.<br /><br /> 1-NOT_APPLICABLE<br /><br /> 2-INDEX_BUILD<br /><br /> 3-TUPLE_MOVER<br /><br /> 4-REORG_NORMAL<br /><br /> 5-REORG_FORCED<br /><br /> 6-CARGA MASIVA<br /><br /> 7-FUSIONAR MEDIANTE COMBINACIÓN|  
|**transition_to_compressed_state_desc**|nvarchar(60)| 1-NOT_APPLICABLE: la operación no se aplica a almacén Delta. O bien, el filas se comprimió antes de actualizar a, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] en cuyo caso no se conserva el historial.<br /><br /> 2-INDEX_BUILD-una creación de índice o una regeneración de índice comprimida el filas.<br /><br /> 3-TUPLE_MOVER: el Movedor de tupla que se ejecuta en el fondo ha comprimido el filas. El motor de tupla se produce después de que el filas cambie el estado de abierto a cerrado.<br /><br /> 4-REORG_NORMAL: la operación de reorganización, ALTER INDEX... REORG, se mueve el filas cerrado del almacén delta al almacén de columnas. Esto ocurrió antes de que el motor de tupla tuviera tiempo para mover el filas.<br /><br /> 5-REORG_FORCED: este filas estaba abierto en almacén Delta y se forzó en el almacén de columnas antes de que tuviera un número completo de filas.<br /><br /> 6-bulk-Bulk: operación de carga masiva comprimida de filas directamente sin usar almacén Delta.<br /><br /> 7-MERGE: una operación de combinación consolidada uno o más filas en este filas y, a continuación, realizó la compresión del almacén de columnas.|  
|**has_vertipaq_optimization**|bit|La optimización de VertiPaq mejora la compresión del almacén de columnas reorganizando el orden de las filas en filas para lograr una mayor compresión. Esta optimización se produce automáticamente en la mayoría de los casos. Hay dos casos en los que no se usa la optimización de VertiPaq:<br/>  a. Cuando un filas Delta se mueve al almacén de columnas y hay uno o varios índices no clúster en el índice de almacén de columnas, en este caso se omite la optimización de VertiPaq para minimizar los cambios en el índice de asignación.<br/> b. para los índices de almacén de columnas en tablas optimizadas para memoria. <br /><br /> 0 = No<br /><br /> 1 = Sí|  
|**última**|bigint|Generación de grupos de filas asociada a este grupo de filas.|  
|**created_time**|datetime2|Hora de reloj en la que se creó este filas.<br /><br /> NULL: para un índice de almacén de columnas en una tabla en memoria.|  
|**closed_time**|datetime2|Hora de reloj en la que se cerró este filas.<br /><br /> NULL: para un índice de almacén de columnas en una tabla en memoria.|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="results"></a>Resultados  
 Devuelve una fila por cada filas en la base de datos actual.  
  
## <a name="permissions"></a>Permisos  
Requiere `CONTROL` el permiso en la tabla y el `VIEW DATABASE STATE` permiso en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-calculate-fragmentation-to-decide-when-to-reorganize-or-rebuild-a-columnstore-index"></a>A. Calcular la fragmentación para decidir cuándo reorganizar o volver a generar un índice de almacén de columnas.  
 En el caso de los índices de almacén de columnas, el porcentaje de filas eliminadas es una buena medida para la fragmentación en un filas. Si la fragmentación es del 20% o más, quite las filas eliminadas. Para obtener más ejemplos, vea [reorganizar y volver a generar índices](~/relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 En este ejemplo se combina **Sys.dm_db_column_store_row_group_physical_stats** con otras tablas del sistema y, a continuación, se calcula la `Fragmentation` columna como una estimación de la eficacia de cada grupo de filas en la base de datos actual. Para buscar información en una sola tabla, quite los guiones de comentario delante de la cláusula **Where** y proporcione un nombre de tabla.  
  
```sql  
SELECT i.object_id,   
    object_name(i.object_id) AS TableName,   
    i.name AS IndexName,   
    i.index_id,   
    i.type_desc,   
    CSRowGroups.*,  
    100*(ISNULL(deleted_rows,0))/NULLIF(total_rows,0) AS 'Fragmentation'
FROM sys.indexes AS i  
JOIN sys.dm_db_column_store_row_group_physical_stats AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id AND i.index_id = CSRowGroups.index_id   
-- WHERE object_name(i.object_id) = 'table_name'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)      
 [Diseño de los índices de almacén de columnas](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)         
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
 [sys.column_store_dictionaries &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   
 [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
