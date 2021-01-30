---
description: sys.pdw_nodes_column_store_row_groups (Transact-SQL)
title: sys.pdw_nodes_column_store_row_groups (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 08/05/2020
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 17a4c925-d4b5-46ee-9cd6-044f714e6f0e
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: d4b9ea267b0dbf23ff304dc7632a2fdc742d37dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186570"
---
# <a name="syspdw_nodes_column_store_row_groups-transact-sql"></a>sys.pdw_nodes_column_store_row_groups (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Proporciona información del índice de almacén de columnas agrupado por segmento para ayudar al administrador a tomar decisiones de administración del sistema en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . **Sys.pdw_nodes_column_store_row_groups** tiene una columna para el número total de filas almacenadas físicamente (incluidas las marcadas como eliminadas) y una columna para el número de filas marcadas como eliminadas. Use **Sys.pdw_nodes_column_store_row_groups** para determinar qué grupos de filas tienen un alto porcentaje de filas eliminadas y deben volver a generarse.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|IDENTIFICADOR de la tabla subyacente. Esta es la tabla física del nodo de proceso, no la object_id de la tabla lógica del nodo de control. Por ejemplo, object_id no coincide con el object_id de sys. tables.<br /><br /> Para combinar con sys. Tables, use sys.pdw_index_mappings.|  
|**id_de_índice**|**int**|IDENTIFICADOR del índice de almacén de columnas agrupado en *object_id* tabla.|  
|**partition_number**|**int**|IDENTIFICADOR de la partición de tabla que contiene el *row_group_id* de grupo de filas. Puede usar *partition_number* para unir esta DMV a sys. partitions.|  
|**row_group_id**|**int**|IDENTIFICADOR de este grupo de filas. Es único en la partición.|  
|**dellta_store_hobt_id**|**bigint**|El hobt_id para los grupos de filas delta, o NULL si el tipo del grupo de filas no es delta. Un grupo de filas delta es un grupo de filas de lectura/escritura que acepta nuevos registros. Un grupo de filas Delta tiene el estado **Open** . Un grupo de filas delta está todavía en formato de almacén de filas y no se ha comprimido al formato de almacén de columnas.|  
|**state**|**tinyint**|Número de identificación asociado con el state_description.<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED|  
|**state_desccription**|**nvarchar(60)**|Descripción del estado persistente del grupo de filas:<br /><br /> ABRIR: un grupo de filas de lectura/escritura que acepta nuevos registros. Un grupo de filas abierto está todavía en formato de almacén de filas y no se ha comprimido al formato de almacén de columnas.<br /><br /> CLOSED: Grupo de filas que se ha rellenado, pero que aún no se ha comprimido mediante el proceso de la tupla.<br /><br /> COMPRIMIDO: Grupo de filas que se ha rellenado y comprimido.|  
|**total_rows**|**bigint**|Total de filas almacenadas físicamente en el grupo de filas. Es posible que se hayan eliminado algunas, pero estas se siguen almacenando. El número máximo de filas en un grupo de filas es 1.048.576 (hexadecimal FFFFF).|  
|**deleted_rows**|**bigint**|Número de filas almacenadas físicamente en el grupo de filas que se han marcado para su eliminación.<br /><br /> Siempre es 0 para los grupos de filas DELTA.|  
|**size_in_bytes**|**int**|Tamaño combinado, en bytes, de todas las páginas de este grupo de filas. Este tamaño no incluye el tamaño necesario para almacenar metadatos o diccionarios compartidos.|  
|**pdw_node_id**|**int**|IDENTIFICADOR único de un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nodo.|  
|**distribution_id**|**int**|IDENTIFICADOR único de la distribución.|
  
## <a name="remarks"></a>Observaciones  
 Devuelve una fila para cada grupo de filas del almacén de columnas de cada tabla que tenga un índice clúster o no clúster de almacén de columnas.  
  
 Utilice **Sys.pdw_nodes_column_store_row_groups** para determinar el número de filas incluidas en el grupo de filas y el tamaño del grupo de filas.  
  
 Cuando el número de filas eliminadas de un grupo de filas alcanza un alto porcentaje de las filas totales, la tabla pierde eficiencia. Vuelva a generar el índice de almacén de columnas para reducir el tamaño de la tabla, reduciendo así la E/S de disco necesaria para leer la tabla. Para volver a generar el índice de almacén de columnas, use la opción **rebuild** de la instrucción **ALTER index** .  
  
 En primer lugar, el almacén de columnas actualizable inserta nuevos datos en un filas **abierto** , que está en formato almacén, y a veces también se denomina tabla Delta.  Una vez que un filas abierto está lleno, su estado cambia a **cerrado**. Un filas cerrado se comprime en el formato de almacén de columnas por el motor de tupla y el estado cambia a **comprimido**.  La tupla motriz es un proceso en segundo plano que de forma periódica se despierta y comprueba si hay grupos de filas cerrados listos para comprimirse en un grupo de filas de almacén de columnas.  La tupla motriz también cancela la asignación de los grupos de filas en los que se han borrado todas las filas. Los filas desasignados se marcan como **retirados**. Para ejecutar el motor de tupla inmediatamente, use la opción **REorganize** de la instrucción **ALTER index** .  
  
 Cuando se ha rellenado un grupo de filas de almacén de columnas, se comprime y ya no se aceptan filas nuevas. Cuando se eliminan filas de un grupo comprimido, siguen estando allí pero están marcadas como eliminadas. Las actualizaciones de un grupo comprimido se implementan como una eliminación del grupo comprimido, y como una inserción en un grupo abierto.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso **VIEW SERVER STATE**.  
  
## <a name="examples-sssdw-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el ejemplo siguiente se combina la tabla **Sys.pdw_nodes_column_store_row_groups** con otras tablas del sistema para devolver información acerca de tablas específicas. La columna `PercentFull` calculada es una estimación de la eficacia del grupo de filas. Para buscar información en una sola tabla, quite los guiones de comentario delante de la cláusula WHERE y proporcione un nombre de tabla.  
  
```sql
SELECT IndexMap.object_id,   
  object_name(IndexMap.object_id) AS LogicalTableName,   
  i.name AS LogicalIndexName, IndexMap.index_id, NI.type_desc,   
  IndexMap.physical_name AS PhyIndexNameFromIMap,   
  CSRowGroups.*,  
  100*(ISNULL(deleted_rows,0))/total_rows AS PercentDeletedRows   
FROM sys.tables AS t  
JOIN sys.indexes AS i  
    ON t.object_id = i.object_id  
JOIN sys.pdw_index_mappings AS IndexMap  
    ON i.object_id = IndexMap.object_id  
    AND i.index_id = IndexMap.index_id  
JOIN sys.pdw_nodes_indexes AS NI  
    ON IndexMap.physical_name = NI.name  
    AND IndexMap.index_id = NI.index_id  
JOIN sys.pdw_nodes_column_store_row_groups AS CSRowGroups  
    ON CSRowGroups.object_id = NI.object_id   
    AND CSRowGroups.pdw_node_id = NI.pdw_node_id  
    AND CSRowGroups.distribution_id = NI.distribution_id
    AND CSRowGroups.index_id = NI.index_id      
WHERE total_rows > 0
--WHERE t.name = '<table_name>'   
ORDER BY object_name(i.object_id), i.name, IndexMap.physical_name, pdw_node_id;  
```  

En el [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] ejemplo siguiente se cuentan las filas por partición para almacenes de columnas agrupados, así como el número de filas que hay en grupos de filas abiertos, cerrados o comprimidos:  

```sql
SELECT
    s.name AS [Schema Name]
    ,t.name AS [Table Name]
    ,rg.partition_number AS [Partition Number]
    ,SUM(rg.total_rows) AS [Total Rows]
    ,SUM(CASE WHEN rg.State = 1 THEN rg.Total_rows Else 0 END) AS [Rows in OPEN Row Groups]
    ,SUM(CASE WHEN rg.State = 2 THEN rg.Total_Rows ELSE 0 END) AS [Rows in Closed Row Groups]
    ,SUM(CASE WHEN rg.State = 3 THEN rg.Total_Rows ELSE 0 END) AS [Rows in COMPRESSED Row Groups]
FROM sys.pdw_nodes_column_store_row_groups rg
  JOIN sys.pdw_nodes_tables pt
    ON rg.object_id = pt.object_id
    AND rg.pdw_node_id = pt.pdw_node_id
    AND pt.distribution_id = rg.distribution_id
  JOIN sys.pdw_table_mappings tm
    ON pt.name = tm.physical_name
  INNER JOIN sys.tables t
    ON tm.object_id = t.object_id
  INNER JOIN sys.schemas s
    ON t.schema_id = s.schema_id
GROUP BY s.name, t.name, rg.partition_number
ORDER BY 1, 2
```
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [CREAR índice de almacén de columnas &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)   
 [sys.pdw_nodes_column_store_segments &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-segments-transact-sql.md)   
 [sys.pdw_nodes_column_store_dictionaries &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql.md)  
  
  
