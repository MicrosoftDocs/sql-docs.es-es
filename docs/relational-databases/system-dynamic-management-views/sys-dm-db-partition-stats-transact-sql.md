---
description: sys.dm_db_partition_stats (Transact-SQL)
title: sys.dm_db_partition_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_partition_stats
- dm_db_partition_stats_TSQL
- sys.dm_db_partition_stats_TSQL
- sys.dm_db_partition_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_partition_stats dynamic management view
ms.assetid: 9db9d184-b3a2-421e-a804-b18ebcb099b7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0838f88916f2fb1b7847ab1ee27e7f9f13a594ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179327"
---
# <a name="sysdm_db_partition_stats-transact-sql"></a>sys.dm_db_partition_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve información de página y recuento de filas de cada partición en la base de datos actual.  
  
> [!NOTE]  
> Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_db_partition_stats**. La partition_id de sys.dm_pdw_nodes_db_partition_stats difiere de la partition_id en la vista de catálogo sys. partitions de Azure Synapse Analytics.
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|Id. de la partición. Es único en la base de datos. Este es el mismo valor que el **partition_id** en la vista de catálogo **Sys. partitions** , excepto Azure Synapse Analytics.|  
|**object_id**|**int**|Id. de objeto de la tabla o vista indizada de la que esta partición forma parte.|  
|**id_de_índice**|**int**|Id. del montón o índice del que esta partición forma parte.<br /><br /> 0 = Montón<br /><br /> 1 = Índice clúster.<br /><br /> > 1 = Índice no clúster|  
|**partition_number**|**int**|Número de partición en base 1 en el índice o montón.|  
|**in_row_data_page_count**|**bigint**|Número de páginas en uso para almacenar datos consecutivos en esta partición. Si la partición forma parte de un montón, el valor es el número de páginas de datos en el montón. Si la partición forma parte de un índice, el valor es el número de páginas en el nivel hoja. (Las páginas no hoja del árbol B no se incluyen en el recuento). En cualquier caso, no se incluyen las páginas IAM (Mapa de asignación de índices). Siempre es 0 para un índice de almacén de columnas optimizado de memoria xVelocity.|  
|**in_row_used_page_count**|**bigint**|Número total de páginas en uso para almacenar y administrar datos consecutivos en esta partición. Este recuento incluye páginas de árbol B no hoja, páginas IAM y todas las páginas incluidas en la columna **in_row_data_page_count**. Siempre es 0 para un índice de almacén de columnas.|  
|**in_row_reserved_page_count**|**bigint**|Número total de páginas reservadas para almacenar y administrar datos consecutivos en esta partición, independientemente de si las páginas están en uso o no. Siempre es 0 para un índice de almacén de columnas.|  
|**lob_used_page_count**|**bigint**|Número de páginas en uso para almacenar y administrar columnas **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** y **xml** no consecutivas en la partición. Las páginas IAM están incluidas.<br /><br /> Número total de LOBs utilizados para almacenar y administrar el índice de almacén de columnas en la partición.|  
|**lob_reserved_page_count**|**bigint**|Número total de páginas reservadas para almacenar y administrar columnas **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** y **xml** no consecutivas en la partición, independientemente de si las páginas están en uso o no. Las páginas IAM están incluidas.<br /><br /> Número total de LOBs reservados para almacenar y administrar un índice de almacén de columnas en la partición.|  
|**row_overflow_used_page_count**|**bigint**|Número de páginas en uso para almacenar y administrar columnas **varchar**, **nvarchar**, **varbinary** y **sql_variant** de desbordamiento de fila en la partición. Las páginas IAM están incluidas.<br /><br /> Siempre es 0 para un índice de almacén de columnas.|  
|**row_overflow_reserved_page_count**|**bigint**|Número total de páginas reservadas para almacenar y administrar columnas **varchar**, **nvarchar**, **varbinary** y **sql_variant** de desbordamiento de fila en la partición, independientemente de si las páginas están en uso o no. Las páginas IAM están incluidas.<br /><br /> Siempre es 0 para un índice de almacén de columnas.|  
|**used_page_count**|**bigint**|Número total de páginas usadas para la partición. Se calcula como **in_row_used_page_count**  +  **lob_used_page_count**  +  **row_overflow_used_page_count**.|  
|**reserved_page_count**|**bigint**|Número total de páginas reservadas para la partición. Se calcula como **in_row_reserved_page_count**  +  **lob_reserved_page_count**  +  **row_overflow_reserved_page_count**.|  
|**row_count**|**bigint**|Número aproximado de filas de la partición.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
|**distribution_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador numérico único asociado a la distribución.|  
  
## <a name="remarks"></a>Observaciones  
 **sys.dm_db_partition_stats** muestra información acerca del espacio usado para almacenar y administrar datos LOB de datos consecutivos y datos de desbordamiento de fila para todas las particiones en una base de datos. Se muestra una fila por partición.  
  
 Los recuentos en los que se basa el resultado se almacenan en caché en memoria o se almacenan en disco en varias tablas del sistema.  
  
 Los datos consecutivos, datos LOB y datos de desbordamiento de fila representan las tres unidades de asignación que forman una partición. La vista de catálogo [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) se puede consultar para encontrar metadatos acerca de cada unidad de asignación en la base de datos.  
  
 Si un montón o un índice no tiene particiones, entonces consta de una partición (con el número de partición = 1); por tanto, solo se devuelve una fila para ese montón o índice. La vista de catálogo [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) se puede consultar para encontrar metadatos acerca de cada partición de todas las tablas e índices en una base de datos.  
  
 El recuento total de cada tabla o índice se puede obtener agregando los recuentos de todas las particiones relacionadas.  
  
## <a name="permissions"></a>Permisos  
 Requiere `VIEW DATABASE STATE` `VIEW DEFINITION` los permisos y para consultar la vista de administración dinámica **Sys.dm_db_partition_stats** . Para obtener más información acerca de los permisos en las vistas de administración dinámica, vea [funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-all-counts-for-all-partitions-of-all-indexes-and-heaps-in-a-database"></a>A. Devolver todos los recuentos de todas las particiones de todos los índices y montones en una base de datos  
 En el siguiente ejemplo se muestran todos los recuentos de todas las particiones de todos los índices y montones en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_partition_stats;  
GO  
```  
  
### <a name="b-returning-all-counts-for-all-partitions-of-a-table-and-its-indexes"></a>B. Devolver todos los recuentos de todas las particiones de una tabla y sus índices  
 En el siguiente ejemplo se muestran todos los recuentos de todas las particiones de la tabla `HumanResources.Employee` y sus índices.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_partition_stats   
WHERE object_id = OBJECT_ID('HumanResources.Employee');  
GO  
```  
  
### <a name="c-returning-total-used-pages-and-total-number-of-rows-for-a-heap-or-clustered-index"></a>C. Devolver el número total de páginas usadas y el número total de filas de un montón o índice clúster  
 En el siguiente ejemplo se devuelve el número total de páginas usadas y el número total de filas del montón o índice clúster de la tabla `HumanResources.Employee`. Puesto que la tabla `Employee` no tiene particiones de forma predeterminada, observe que la suma solo incluye una partición.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SUM(used_page_count) AS total_number_of_used_pages,   
    SUM (row_count) AS total_number_of_rows   
FROM sys.dm_db_partition_stats  
WHERE object_id=OBJECT_ID('HumanResources.Employee')    AND (index_id=0 or index_id=1);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
  


