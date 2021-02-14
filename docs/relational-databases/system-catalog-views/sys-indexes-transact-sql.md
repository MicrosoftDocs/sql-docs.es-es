---
description: sys.indexes (Transact-SQL)
title: Sys. Indexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/12/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.indexes
- indexes
- sys.indexes_TSQL
- indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.indexes catalog view
ms.assetid: 066bd9ac-6554-4297-88fe-d740de1f94a8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8f3c695d222f538682670bae4f5dcbe2dad4888b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341340"
---
# <a name="sysindexes-transact-sql"></a>sys.indexes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila por índice o montón de un objeto tabular, como una tabla, una vista o una función con valores de tabla.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Id. del objeto al que pertenece este índice.|  
|**name**|**sysname**|Nombre del índice. **Name** es único solo dentro del objeto.<br /><br /> NULL = Montón|  
|**id_de_índice**|**int**|Id. del índice. **index_id** es único solo dentro del objeto.<br /><br /> 0 = Montón<br /><br /> 1 o 5 = índice agrupado (árbol B, almacén de columnas)<br /><br /> > 1 y <> 5 = índice no clúster|  
|**type**|**tinyint**|Tipo de índice:<br /><br /> 0 = Montón<br /><br /> 1 = Clúster<br /><br /> 2 = No clúster<br /><br /> 3 = XML<br /><br /> 4 = Espacial<br /><br /> 5 = índice clúster de almacén de columnas. **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> 6 = índice de almacén de columnas no agrupado. **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> 7 = Índice de hash no clúster. **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.|  
|**type_desc**|**nvarchar(60)**|Descripción del tipo de índice:<br /><br /> HEAP<br /><br /> CLUSTERED<br /><br /> NONCLUSTERED<br /><br /> XML<br /><br /> SPATIAL<br /><br /> ALMACÉN de columnas en clúster: **se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> ALMACÉN de columnas no AGRUPAdo: **se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> HASH no AGRUPAdo: los índices de HASH no clúster solo se admiten en tablas optimizadas para memoria. La vista sys.hash_indexes muestra los índices hash actuales y las propiedades hash. Para obtener más información, vea [sys.hash_indexes &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-hash-indexes-transact-sql.md). **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.|  
|**is_unique**|**bit**|1 = El índice es exclusivo.<br /><br /> 0 = El índice no es exclusivo.<br /><br /> Siempre es 0 para los índices clúster de almacén de columnas.|  
|**data_space_id**|**int**|Id. del espacio de datos para este índice. El espacio de datos es un grupo de archivos o un esquema de partición.<br /><br /> 0 = **object_id** es una función con valores de tabla o un índice en memoria.|  
|**ignore_dup_key**|**bit**|1 = IGNORE_DUP_KEY está ON.<br /><br /> 0 = IGNORE_DUP_KEY está OFF.|  
|**is_primary_key**|**bit**|1 = El índice forma parte de una restricción PRIMARY KEY.<br /><br /> Siempre es 0 para los índices clúster de almacén de columnas.|  
|**is_unique_constraint**|**bit**|1 = El índice forma parte de una restricción UNIQUE.<br /><br /> Siempre es 0 para los índices clúster de almacén de columnas.|  
|**fill_factor**|**tinyint**|> 0 = porcentaje de FILLFACTOR utilizado al crear o volver a generar el índice.<br /><br /> 0 = Valor predeterminado<br /><br /> Siempre es 0 para los índices clúster de almacén de columnas.|  
|**is_padded**|**bit**|1 = PADINDEX está ON.<br /><br /> 0 = PADINDEX está OFF.<br /><br /> Siempre es 0 para los índices clúster de almacén de columnas.|  
|**is_disabled**|**bit**|1 = El índice está deshabilitado.<br /><br /> 0 = El índice no está deshabilitado.|  
|**is_hypothetical**|**bit**|1 = El índice es hipotético y no se puede utilizar directamente como ruta de acceso a datos. Los índices hipotéticos contienen estadísticas de nivel de columna.<br /><br /> 0 = El índice no es hipotético.|  
|**allow_row_locks**|**bit**|1 = El índice admite bloqueos de fila.<br /><br /> 0 = El índice no admite bloqueos de fila.<br /><br /> Siempre es 0 para los índices clúster de almacén de columnas.|  
|**allow_page_locks**|**bit**|1 = El índice admite bloqueos de página.<br /><br /> 0 = El índice no admite bloqueos de página.<br /><br /> Siempre es 0 para los índices clúster de almacén de columnas.|  
|**has_filter**|**bit**|1 = El índice tiene un filtro y solo contiene filas que cumplen con la definición del filtro.<br /><br /> 0 = El índice no tiene un filtro.|  
|**filter_definition**|**nvarchar(max)**|Expresión para el subconjunto de filas incluido en el índice filtrado.<br /><br /> NULL para el montón, el índice no filtrado o los permisos insuficientes en la tabla.|  
|**auto_created**|**bit**|1 = el ajuste automático creó el índice.<br /><br />0 = el usuario creó el índice.
|**optimize_for_sequential_key**|**bit**|1 = el índice tiene habilitada la optimización de la última página.<br><br>0 = valor predeterminado. El índice tiene deshabilitada la optimización de la última página.|

> [!NOTE]
> El bit de **optimize_for_sequential_key** solo se admite en las versiones SQL Server 2019 CTP 3,1 y versiones posteriores.
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelven todos los índices de la tabla `Production.Product` en la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] base de datos.  
  
```  
  
SELECT i.name AS index_name  
    ,i.type_desc  
    ,is_unique  
    ,ds.type_desc AS filegroup_or_partition_scheme  
    ,ds.name AS filegroup_or_partition_scheme_name  
    ,ignore_dup_key  
    ,is_primary_key  
    ,is_unique_constraint  
    ,fill_factor  
    ,is_padded  
    ,is_disabled  
    ,allow_row_locks  
    ,allow_page_locks  
FROM sys.indexes AS i  
INNER JOIN sys.data_spaces AS ds ON i.data_space_id = ds.data_space_id  
WHERE is_hypothetical = 0 AND i.index_id <> 0   
AND i.object_id = OBJECT_ID('Production.Product');  
GO  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.xml_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.key_constraints &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
