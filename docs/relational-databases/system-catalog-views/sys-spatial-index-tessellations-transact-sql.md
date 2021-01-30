---
description: sys.spatial_index_tessellations (Transact-SQL)
title: sys.spatial_index_tessellations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- spatial_index_tessellations
- sys.spatial_index_tessellations_TSQL
- spatial_index_tessellations_TSQL
- sys.spatial_index_tessellations
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_index_tessellations catalog view
ms.assetid: 8b17a9a4-b57f-4220-8138-fc73581b1670
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8f4d10efa7a0f3c04ac1864086d703c8fd4a0844
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195573"
---
# <a name="sysspatial_index_tessellations-transact-sql"></a>sys.spatial_index_tessellations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 Representa la información acerca del esquema de teselación y los parámetros de cada uno de los índices espaciales.  
  
> [!NOTE]  
>  Para más información sobre la teselación, vea [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md).  
  

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Identificador del objeto en el que se define el índice. Cada par (object_id, index_id) tiene una entrada correspondiente en [Sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md).|  
|index_id|**int**|Identificador del índice espacial en el que se define la columna indizada.|  
|tessellation_scheme|**sysname**|Nombre del esquema de teselación, uno de los siguientes: GEOMETRY_GRID, GEOGRAPHY_GRID|  
|bounding_box_xmin|**float(53)**|Coordenada x de la esquina inferior izquierda del cuadro de límite; uno de los siguientes: NULL = no es aplicable para un esquema de teselación determinado (como GEOGRAPHY_GRID) *n* = si tessellation_scheme es GEOMETRY_GRID, el valor de la coordenada x mín.                     **Nota:** Las coordenadas definidas por los parámetros del cuadro de límite se interpretan para cada objeto según su [identificador de referencia espacial (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).|  
|bounding_box_ymin|**float(53)**|Coordenada y de la esquina inferior izquierda del cuadro de límite; uno de los siguientes: NULL = no es aplicable para un esquema de teselación determinado (como GEOGRAPHY_GRID) *n* = si tessellation_scheme es GEOMETRY_GRID, el valor de la coordenada y mínima|  
|bounding_box_xmax|**float(53)**|Coordenada x de la esquina superior derecha del cuadro de límite; uno de los siguientes: NULL = no es aplicable para un esquema de teselación determinado (como GEOGRAPHY_GRID) *n* = si tessellation_scheme es GEOMETRY_GRID, el valor de la coordenada x máxima|  
|bounding_box_ymax|**float(53)**|Coordenada Y de la esquina superior derecha del cuadro de límite; uno de los siguientes: NULL = no es aplicable para un esquema de teselación determinado (como GEOGRAPHY_GRID) *n* = si tessellation_scheme es GEOMETRY_GRID, el valor de la coordenada y máxima|  
|level_1_grid|**smallint**|Densidad de la cuadrícula para la cuadrícula de nivel superior. Si tessellation_scheme es GEOMETRY_GRID o GEOGRAPHY_GRID, uno de los siguientes: 16 = 4 por 4 Grid (LOW) 64 = 8 por 8 cuadrícula (media) 256 = 16 por 16 Grid (HIGH) NULL = no aplicable para el tipo de índice espacial o el esquema de teselación especificados.  Se devuelve NULL cuando se utiliza la nueva teselación de SQL Server 11.|  
|level_1_grid_desc|**nvarchar(60)**|Densidad de la cuadrícula de nivel superior; uno de los siguientes: LOW MEDIUM HIGH NULL = no aplicable para el tipo de índice espacial o esquema de teselación determinados.  Se devuelve NULL cuando se utiliza la nueva teselación de SQL Server 11.|  
|level_2_grid|**smallint**|Densidad de la cuadrícula de segundo nivel. Si tessellation_scheme es GEOMETRY_GRID o GEOGRAPHY_GRID, uno de los siguientes: 16 = 4 por 4 Grid (LOW) 64 = 8 por 8 cuadrícula (media) 256 = 16 por 16 Grid (HIGH) NULL = no aplicable para el tipo de índice espacial o el esquema de teselación especificados.  Se devuelve NULL cuando se utiliza la nueva teselación de SQL Server 11.|  
|level_2_grid_desc|**nvarchar(60)**|Densidad de la cuadrícula de segundo nivel; uno de los siguientes: LOW MEDIUM HIGH NULL = no aplicable para el tipo de índice espacial o esquema de teselación determinados.  Se devuelve NULL cuando se utiliza la nueva teselación de SQL Server 11.|  
|level_3_grid|**smallint**|Densidad de la cuadrícula de tercer nivel.   Si tessellation_scheme es GEOMETRY_GRID o GEOGRAPHY_GRID, uno de los siguientes: 16 = 4 por 4 Grid (LOW) 64 = 8 por 8 cuadrícula (media) 256 = 16 por 16 Grid (HIGH) NULL = no aplicable para el tipo de índice espacial o el esquema de teselación especificados.  Se devuelve NULL cuando se utiliza la nueva teselación de SQL Server 11.|  
|level_3_grid_desc|**nvarchar(60)**|Densidad de la cuadrícula de tercer nivel; uno de los siguientes: LOW MEDIUM HIGH NULL = no aplicable para el tipo de índice espacial o esquema de teselación determinados.  Se devuelve NULL cuando se utiliza la nueva teselación de SQL Server 11.|  
|level_4_grid|**smallint**|Densidad de la cuadrícula de cuarto nivel. Si tessellation_scheme es GEOMETRY_GRID o GEOGRAPHY_GRID, uno de los siguientes: 16 = 4 por 4 Grid (LOW) 64 = 8 por 8 Grid (media) 256 = 16 por 16 Grid (HIGH) NULL = no aplicable para el tipo de índice espacial o el esquema de teselación especificados.  Se devuelve NULL cuando se utiliza la nueva teselación de SQL Server 11.|  
|level_4_grid_desc|**nvarchar(60)**|Densidad de la cuadrícula de cuarto nivel; uno de los siguientes: < LOW MEDIUM HIGH NULL = no es aplicable para el esquema de teselación o tipo de índice espacial determinado.  Se devuelve NULL cuando se utiliza la nueva teselación de SQL Server 11.|  
|cells_per_object|**int**|Número de celdas por objeto espacial, uno de los siguientes: si tessellation_scheme es GEOMETRY_GRID o GEOGRAPHY_GRID, *n* = número de celdas por objeto NULL = no aplicable para el tipo de índice espacial o esquema de teselación determinados|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.spatial_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
