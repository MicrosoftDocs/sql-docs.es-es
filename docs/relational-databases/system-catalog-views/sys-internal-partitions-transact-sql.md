---
description: sys.internal_partitions (Transact-SQL)
title: sys.internal_partitions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0262df2b-5ba7-4715-b17b-3d9ce470a38e
author: ronortloff
ms.author: rortloff
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 402e8ff5793ad0ea5abd11cad60f93b6ae963df2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191406"
---
# <a name="sysinternal_partitions-transact-sql"></a>sys.internal_partitions (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Devuelve una fila por cada conjunto de filas que realiza un seguimiento de los datos internos de los índices de almacén de columnas en las tablas basadas en disco. Estos conjuntos de filas son internos a los índices de almacén de columnas y realizan un seguimiento de las filas eliminadas, las asignaciones de filas y el filas de almacenamiento Delta. Realizan un seguimiento de los datos para cada partición de tabla; cada tabla tiene al menos una partición. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuelve a crear los conjuntos de filas cada vez que vuelve a generar el índice de almacén de columnas.   
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|IDENTIFICADOR de partición para esta partición. Es único en la base de datos.|  
|object_id|**int**|IDENTIFICADOR de objeto de la tabla que contiene la partición.|  
|index_id|**int**|IDENTIFICADOR de índice para el índice de almacén de columnas definido en la tabla.<br /><br /> 1 = Índice clúster de almacén de columnas<br /><br /> 2 = Índice de almacén de columnas no agrupado|  
|partition_number|**int**|Número de partición.<br /><br /> 1 = primera partición de una tabla con particiones o una sola partición de una tabla sin particiones.<br /><br /> 2 = segunda partición, etc.|  
|internal_object_type|**tinyint**|Objetos de conjunto de filas que realizan el seguimiento de datos internos para el índice de almacén de columnas.<br /><br /> 2 = COLUMN_STORE_DELETE_BITMAP<br /><br /> 3 = COLUMN_STORE_DELTA_STORE<br /><br /> 4 = COLUMN_STORE_DELETE_BUFFER<br /><br /> 5 = COLUMN_STORE_MAPPING_INDEX|  
|internal_object_type_desc|**nvarchar(60)**|COLUMN_STORE_DELETE_BITMAP: este índice de mapa de bits realiza un seguimiento de las filas marcadas como eliminadas del almacén de columnas. El mapa de bits es para cada filas, ya que las particiones pueden tener filas en varios filas. Las filas siguen estando presentes físicamente y ocupando espacio en el almacén de columnas.<br /><br /> COLUMN_STORE_DELTA_STORE: almacena grupos de filas, denominados filas, que no se han comprimido en el almacenamiento en columnas. Cada partición de tabla puede tener cero o más almacén Delta filas.<br /><br /> COLUMN_STORE_DELETE_BUFFER: para mantener las eliminaciones en los índices de almacén de columnas no agrupados actualizables. Cuando una consulta elimina una fila de la tabla almacén subyacente, el búfer de eliminación realiza un seguimiento de la eliminación del almacén de columnas. Cuando el número de filas eliminadas supera 1048576, se vuelven a combinar en el subproceso de eliminación de mapa de bits por segundo de la tupla o mediante un comando reorganize explícito.  En un momento dado, la Unión del mapa de bits de eliminación y el búfer de eliminación representa todas las filas eliminadas.<br /><br /> COLUMN_STORE_MAPPING_INDEX: solo se usa cuando el índice de almacén de columnas agrupado tiene un índice no clúster secundario. Esto asigna las claves de índice no clúster al filas y el identificador de fila correctos en el almacén de columnas. Solo almacena las claves para las filas que se mueven a otro filas; Esto sucede cuando se comprime un filas Delta en el almacén de columnas y cuando una operación de combinación combina filas de dos filas diferentes.|  
|Row_group_id|**int**|IDENTIFICADOR de almacén Delta filas. Cada partición de tabla puede tener cero o más almacén Delta filas.|  
|hobt_id|**bigint**|IDENTIFICADOR del objeto de conjunto de filas interno (HoBT). Se trata de una buena clave para combinar con otras DMV para obtener más información sobre las características físicas del conjunto de filas interno.|  
|rows|**bigint**|Número aproximado de filas de esta partición.|  
|data_compression|**tinyint**|El estado de compresión del conjunto de filas:<br /><br /> 0 = NONE<br /><br /> 1 = ROW<br /><br /> 2 = PAGE|  
|data_compression_desc|**nvarchar(60)**|El estado de compresión para cada partición. Los valores posibles para las tablas de almacén de filas son NONE, ROW y PAGE. Los valores posibles para tablas de almacén de columnas son COLUMNSTORE y COLUMNSTORE_ARCHIVE.|  
|optimize_for_sequential_key|**bit**|1 = la partición tiene habilitada la optimización de la última página.<br><br>0 = valor predeterminado. La partición tiene deshabilitada la optimización de la última página.|
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol `public`. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="general-remarks"></a>Notas generales  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuelve a crear nuevos índices internos de almacén de columnas cada vez que crea o vuelve a generar un índice de almacén de columnas.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-view-all-of-the-internal-rowsets-for-a-table"></a>A. Ver todos los conjuntos de filas internos de una tabla  
 En este ejemplo se devuelven todos los conjuntos de filas de almacén de columnas internos de una tabla. También puede usar el hobt_id para obtener más información sobre el conjunto de filas específico.  
  
```sql  
SELECT i.object_id, i.index_id, i.name, p.hobt_id, p.internal_object_type_id, p.internal_object_type_desc  
FROM sys.internal_partitions AS p  
JOIN sys.indexes AS i  
on i.object_id = p.object_id  
WHERE p.object_id = OBJECT_ID ( '<table name' ) ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
