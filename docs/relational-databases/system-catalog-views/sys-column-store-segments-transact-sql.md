---
description: sys.column_store_segments (Transact-SQL)
title: sys.column_store_segments (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- column_store_segments
- sys.column_store_segments_TSQL
- sys.column_store_segments
- column_store_segments_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_segments catalog view
ms.assetid: 1253448c-2ec9-4900-ae9f-461d6b51b2ea
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 51c5e31a97a52e94a8a6d164c6d43425451494b1
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102463973"
---
# <a name="syscolumn_store_segments-transact-sql"></a>sys.column_store_segments (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Devuelve una fila por cada segmento de columna de un índice de almacén de columnas. Hay un segmento de columna por columna por cada filas. Por ejemplo, una tabla con 10 filas y 34 columnas devuelve 340 filas. 
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|Indica el identificador de partición. Es único en una base de datos.|  
|**hobt_id**|**bigint**|IDENTIFICADOR del índice de montículo o árbol B (HoBT) de la tabla que tiene este índice de almacén de columnas.|  
|**column_id**|**int**|Identificador de la columna de almacén de columnas.|  
|**segment_id**|**int**|IDENTIFICADOR de filas. Por compatibilidad con versiones anteriores, el nombre de columna sigue siendo llamado segment_id aunque se trata del identificador de filas. Puede identificar de forma única un segmento mediante \<hobt_id, partition_id, column_id> <segment_id>.|  
|**version**|**int**|Versión del formato de segmento de columna.|  
|**encoding_type**|**int**|Tipo de codificación que se usa para ese segmento:<br /><br /> 1 = VALUE_BASED-no cadena/binaria sin diccionario (similar a 4 con algunas variaciones internas)<br /><br /> 2 = VALUE_HASH_BASED una columna no cadena/binaria con valores comunes en el Diccionario<br /><br /> 3 = STRING_HASH_BASED-cadena/columna binaria con valores comunes en el Diccionario<br /><br /> 4 = STORE_BY_VALUE_BASED-no cadena/binaria sin Diccionario<br /><br /> 5 = STRING_STORE_BY_VALUE_BASED-cadena/binario sin Diccionario<br /><br /> Para más información, vea la sección [Comentarios](#remarks).|  
|**row_count**|**int**|Número de filas del grupo de filas.|  
|**has_nulls**|**int**|1 si el segmento de la columna tiene valores NULL.|  
|**base_id**|**bigint**|IDENTIFICADOR del valor base si se está utilizando el tipo de codificación 1. Si no se usa el tipo de codificación 1, base_id se establece en-1.|  
|**magnitude**|**float**|Magnitud si se usa el tipo de codificación 1. Si no se usa el tipo de codificación 1, Magnitude se establece en-1.|  
|**primary_dictionary_id**|**int**|Un valor de 0 representa el Diccionario global. Un valor de-1 indica que no hay ningún diccionario global creado para esta columna.|  
|**secondary_dictionary_id**|**int**|Un valor distinto de cero apunta al diccionario local para esta columna en el segmento actual (es decir, filas). Un valor de-1 indica que no hay ningún diccionario local para este segmento.|  
|**min_data_id**|**bigint**|IDENTIFICADOR de datos mínimo en el segmento de columna.|  
|**max_data_id**|**bigint**|IDENTIFICADOR de datos máximo en el segmento de columna.|  
|**null_value**|**bigint**|Valor usado para representar valores NULL.|  
|**on_disk_size**|**bigint**|Tamaño del segmento en bytes.|  
  
## <a name="remarks"></a>Observaciones  
El tipo de codificación de segmentos de almacén de columnas lo selecciona el [!INCLUDE[ssde_md](../../includes/ssde_md.md)] con el objetivo de lograr el costo de almacenamiento más bajo mediante el análisis de los datos del segmento. Si los datos son principalmente distintos, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] utiliza la codificación basada en valores. Si los datos no son principalmente distintos, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] utiliza la codificación basada en hash. La opción entre la codificación basada en cadena y la codificación basada en valores se relaciona con el tipo de datos que se almacenan, ya sea datos de cadena o datos binarios. Todas las codificaciones aprovechan el empaquetado de bits y la codificación de longitud de ejecución cuando sea posible.
 
## <a name="permissions"></a>Permisos  
 Todas las columnas necesitan al menos `VIEW DEFINITION` el permiso en la tabla. Las columnas siguientes devuelven null a menos que el usuario también tenga el `SELECT` permiso: `has_nulls` , `base_id` , `magnitude` , `min_data_id` , `max_data_id` y `null_value` .  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="examples"></a>Ejemplos

### <a name="the-following-query-returns-information-about-segments-of-a-columnstore-index"></a>La consulta siguiente devuelve información acerca de los segmentos de un índice de almacén de columnas.  
  
```sql  
SELECT i.name, p.object_id, p.index_id, i.type_desc,   
    COUNT(*) AS number_of_segments  
FROM sys.column_store_segments AS s   
INNER JOIN sys.partitions AS p   
    ON s.hobt_id = p.hobt_id   
INNER JOIN sys.indexes AS i   
    ON p.object_id = i.object_id  
WHERE i.type = 5 OR i.type = 6  
GROUP BY i.name, p.object_id, p.index_id, i.type_desc ;  
GO  
```  

## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [Guía de índices de almacén de columnas](~/relational-databases/indexes/columnstore-indexes-overview.md)    
 [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
  
 
