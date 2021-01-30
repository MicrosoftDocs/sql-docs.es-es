---
description: sys.column_store_row_groups (Transact-SQL)
title: sys.column_store_row_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.column_store_row_groups_TSQL
- column_store_row_groups
- sys.column_store_row_groups
- column_store_row_groups_TSQL
- deleted bitmap
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_row_groups catalog view
ms.assetid: 76e7fef2-d1a4-4272-a2bb-5f5dcd84aedc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e2b9faf192f19df1cf402d6fc1850fb4acd855c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205357"
---
# <a name="syscolumn_store_row_groups-transact-sql"></a>sys.column_store_row_groups (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información del índice clúster de almacén de columnas una vez por segmento para ayudar al administrador a tomar decisiones de administración del sistema. **Sys.column_store_row_groups** tiene una columna para el número total de filas almacenadas físicamente (incluidas las marcadas como eliminadas) y una columna para el número de filas marcadas como eliminadas. Use **Sys.column_store_row_groups** para determinar qué grupos de filas tienen un alto porcentaje de filas eliminadas y deben volver a generarse.  
   
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|int|El identificador de la tabla en el que se define este índice.|  
|**id_de_índice**|int|Identificador del índice de la tabla que contiene este índice de almacén de columnas.|  
|**partition_number**|int|Identificador de la partición de la tabla que contiene el grupo de filas row_group_id. Puede utilizar partition_number para unir esta DMV a sys.partitions.|  
|**row_group_id**|int|El número de grupo de filas asociado con este grupo de filas. Es único en la partición.<br /><br /> -1 = final de una tabla en memoria.|  
|**delta_store_hobt_id**|bigint|El hobt_id para el grupo de filas abierto en el almacén Delta.<br /><br /> Es NULL si el grupo de filas no está en el almacén Delta.<br /><br /> NULL para la cola de una tabla en memoria.|  
|**state**|TINYINT|Número de identificación asociado con el state_description.<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED <br /><br /> 4 = MARCADORES DE EXCLUSIÓN|  
|**state_description**|nvarchar(60)|Descripción del estado persistente del grupo de filas:<br /><br /> INVISIBLE: segmento comprimido oculto en el proceso de compilación a partir de los datos de un almacén Delta. Las acciones de lectura utilizarán el almacén delta hasta que se complete el segmento comprimido invisible. Después, se hará visible el nuevo segmento y se quitará el almacén delta de origen.<br /><br /> ABRIR: un grupo de filas de lectura/escritura que acepta nuevos registros. Un grupo de filas abierto está todavía en formato de almacén de filas y no se ha comprimido al formato de almacén de columnas.<br /><br /> CLOSED: Grupo de filas que se ha rellenado, pero que aún no se ha comprimido mediante el proceso de la tupla.<br /><br /> COMPRIMIDO: Grupo de filas que se ha rellenado y comprimido.|  
|**total_rows**|bigint|Total de filas almacenadas físicamente en el grupo de filas. Es posible que se hayan eliminado algunas, pero estas se siguen almacenando. El número máximo de filas en un grupo de filas es 1.048.576 (hexadecimal FFFFF).|  
|**deleted_rows**|bigint|Total de filas del grupo de filas marcadas como eliminadas. Esto es siempre 0 para los grupos de filas DELTA.|  
|**size_in_bytes**|bigint|Tamaño en bytes de todos los datos de este grupo de filas (sin incluir metadatos o diccionarios compartidos), tanto para los grupos de filas DELTA como COLUMNSTORE.|  
  
## <a name="remarks"></a>Observaciones  
 Devuelve una fila para cada grupo de filas del almacén de columnas de cada tabla que tenga un índice clúster o no clúster de almacén de columnas.  
  
 Utilice **Sys.column_store_row_groups** para determinar el número de filas incluidas en el grupo de filas y el tamaño del grupo de filas.  
  
 Cuando el número de filas eliminadas de un grupo de filas alcanza un alto porcentaje de las filas totales, la tabla pierde eficiencia. Vuelva a generar el índice de almacén de columnas para reducir el tamaño de la tabla, reduciendo así la E/S de disco necesaria para leer la tabla. Para volver a generar el índice de almacén de columnas, use la opción **rebuild** de la instrucción **ALTER index** .  
  
 En primer lugar, el almacén de columnas actualizable inserta nuevos datos en un filas **abierto** , que está en formato almacén, y a veces también se denomina tabla Delta.  Una vez que un filas abierto está lleno, su estado cambia a **cerrado**. Un filas cerrado se comprime en el formato de almacén de columnas por el motor de tupla y el estado cambia a **comprimido**.  La tupla motriz es un proceso en segundo plano que de forma periódica se despierta y comprueba si hay grupos de filas cerrados listos para comprimirse en un grupo de filas de almacén de columnas.  La tupla motriz también cancela la asignación de los grupos de filas en los que se han borrado todas las filas. Los filas desasignados se marcan como **marcadores de exclusión**. Para ejecutar el motor de tupla inmediatamente, use la opción **REorganize** de la instrucción **ALTER index** .  
  
 Cuando se ha rellenado un grupo de filas de almacén de columnas, se comprime y ya no se aceptan filas nuevas. Cuando se eliminan filas de un grupo comprimido, siguen estando allí pero están marcadas como eliminadas. Las actualizaciones de un grupo comprimido se implementan como una eliminación del grupo comprimido, y como una inserción en un grupo abierto.  
  
## <a name="permissions"></a>Permisos  
 Devuelve información de una tabla si el usuario tiene el `VIEW DEFINITION` permiso en la tabla.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se combina la tabla **Sys.column_store_row_groups** con otras tablas del sistema para devolver información acerca de tablas específicas. La columna `PercentFull` calculada es una estimación de la eficacia del grupo de filas. Para buscar información en una sola tabla, quite los guiones de comentario delante de la cláusula **Where** y proporcione un nombre de tabla.  
  
```sql  
SELECT i.object_id, object_name(i.object_id) AS TableName,   
i.name AS IndexName, i.index_id, i.type_desc,   
CSRowGroups.*,   
100*(total_rows - ISNULL(deleted_rows,0))/total_rows AS PercentFull    
FROM sys.indexes AS i  
JOIN sys.column_store_row_groups AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id  
AND i.index_id = CSRowGroups.index_id   
--WHERE object_name(i.object_id) = '<table_name>'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
```  
  
## <a name="see-also"></a>Vea también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [Guía de índices de almacén de columnas](~/relational-databases/indexes/columnstore-indexes-overview.md)     
 [sys.column_store_dictionaries &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   
 [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
  

