---
description: sys.pdw_nodes_column_store_segments (Transact-SQL)
title: sys.pdw_nodes_column_store_segments (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 03/28/2018
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: e2fdf8e9-1b74-4682-b2d4-c62aca053d7f
author: julieMSFT
ms.author: jrasnick
manager: jrj
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 08e99e71c4086f60b869b9d25c6b0d0f4deec35d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186563"
---
# <a name="syspdw_nodes_column_store_segments-transact-sql"></a>sys.pdw_nodes_column_store_segments (Transact-SQL)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

Contiene una fila para cada columna de un índice de almacén de columnas.

| Nombre de la columna                 | Tipo de datos  | Descripción                                                  |
| :-------------------------- | :--------- | :----------------------------------------------------------- |
| **partition_id**            | **bigint** | Indica el identificador de partición. Es único en una base de datos.     |
| **hobt_id**                 | **bigint** | Identificador del montón o el índice de árbol b (hobt) para la tabla que contiene este índice de almacén de columnas. |
| **column_id**               | **int**    | Identificador de la columna de almacén de columnas.                                |
| **segment_id**              | **int**    | Identificador del segmento de columna. Por compatibilidad con versiones anteriores, el nombre de columna sigue siendo llamado segment_id aunque se trata del identificador de filas. Puede identificar de forma única un segmento mediante <hobt_id, partition_id, column_id> <segment_id>. |
| **version**                 | **int**    | Versión del formato de segmento de columna.                        |
| **encoding_type**           | **int**    | Tipo de codificación que se usa para ese segmento:<br /><br /> 1 = VALUE_BASED-no cadena/binaria sin diccionario (similar a 4 con algunas variaciones internas)<br /><br /> 2 = VALUE_HASH_BASED una columna no cadena/binaria con valores comunes en el Diccionario<br /><br /> 3 = STRING_HASH_BASED-cadena/columna binaria con valores comunes en el Diccionario<br /><br /> 4 = STORE_BY_VALUE_BASED-no cadena/binaria sin Diccionario<br /><br /> 5 = STRING_STORE_BY_VALUE_BASED-cadena/binario sin Diccionario<br /><br /> Todas las codificaciones aprovechan el empaquetado de bits y la codificación de longitud de ejecución cuando sea posible. |
| **row_count**               | **int**    | Número de filas del grupo de filas.                             |
| **has_nulls**               | **int**    | 1 si el segmento de la columna tiene valores NULL.                     |
| **base_id**                 | **bigint** | IDENTIFICADOR del valor base si se está utilizando el tipo de codificación 1.  Si no se usa el tipo de codificación 1, base_id se establece en 1. |
| **magnitude**               | **float**  | Magnitud si se usa el tipo de codificación 1.  Si no se usa el tipo de codificación 1, Magnitude se establece en 1. |
| **primary__dictionary_id**  | **int**    | IDENTIFICADOR del diccionario principal. Un valor distinto de cero apunta al diccionario local para esta columna en el segmento actual (es decir, filas). Un valor de-1 indica que no hay ningún diccionario local para este segmento. |
| **secondary_dictionary_id** | **int**    | IDENTIFICADOR del diccionario secundario. Un valor distinto de cero apunta al diccionario local para esta columna en el segmento actual (es decir, filas). Un valor de-1 indica que no hay ningún diccionario local para este segmento. |
| **min_data_id**             | **bigint** | IDENTIFICADOR de datos mínimo en el segmento de columna.                       |
| **max_data_id**             | **bigint** | IDENTIFICADOR de datos máximo en el segmento de columna.                       |
| **null_value**              | **bigint** | Valor usado para representar valores NULL.                               |
| **on_disk_size**            | **bigint** | Tamaño del segmento en bytes.                                    |
| **pdw_node_id**             | **int**    | Identificador único de un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nodo. |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Combine sys.pdw_nodes_column_store_segments con otras tablas del sistema para determinar el número de segmentos de almacén de columnas por tabla lógica.

```sql
SELECT  sm.name           as schema_nm
,       tb.name           as table_nm
,       nc.name           as col_nm
,       nc.column_id
,       COUNT(*)          as segment_count
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb                   ON  sm.[schema_id]          = tb.[schema_id]
JOIN    sys.[pdw_table_mappings] mp       ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt         ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[pdw_nodes_partitions] np     ON  np.[object_id]          = nt.[object_id]
                                          AND np.[pdw_node_id]        = nt.[pdw_node_id]
                                          AND np.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_nodes_columns] nc        ON  np.[object_id]          = nc.[object_id]
                                          AND np.[pdw_node_id]        = nc.[pdw_node_id]
                                          AND np.[distribution_id]    = nc.[distribution_id]
JOIN    sys.[pdw_nodes_column_store_segments] rg  ON  rg.[partition_id]         = np.[partition_id]
                                                      AND rg.[pdw_node_id]      = np.[pdw_node_id]
                                                      AND rg.[distribution_id]  = np.[distribution_id]
                                                      AND rg.[column_id]        = nc.[column_id]
GROUP BY    sm.name
,           tb.name
,           nc.name
,           nc.column_id  
ORDER BY    table_nm
,           nc.column_id
,           sm.name ;
```

## <a name="permissions"></a>Permisos

Requiere el permiso **VIEW SERVER STATE**.

## <a name="see-also"></a>Consulte también

[Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)  
[sys.pdw_nodes_column_store_row_groups &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-row-groups-transact-sql.md)  
[sys.pdw_nodes_column_store_dictionaries &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql.md)
