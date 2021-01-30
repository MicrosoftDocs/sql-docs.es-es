---
description: sys.pdw_nodes_column_store_dictionaries (Transact-SQL)
title: sys.pdw_nodes_column_store_dictionaries (Transact-SQL)
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.custom: seo-dt-2019
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 7ae1c2e4-45c0-4880-a692-1f299fbcfd19
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: e4c29b811f6a208b0e6bdfc42a28b83ca6f0d852
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186587"
---
# <a name="syspdw_nodes_column_store_dictionaries-transact-sql"></a>sys.pdw_nodes_column_store_dictionaries (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene una fila para cada diccionario utilizado en los índices de almacén de columnas. Los diccionarios se usan para codificar algunos tipos de datos (no todos), por tanto no todas las columnas en un índice de almacén de columnas tienen diccionarios. Un diccionario puede ser un diccionario primario, para todos los segmentos, y posiblemente para otros diccionarios que se usan para un subconjunto de los segmentos de la columna.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|Indica el identificador de partición. Es único en una base de datos.|  
|**hobt_id**|**bigint**|IDENTIFICADOR del índice de montículo o árbol B (HoBT) de la tabla que tiene este índice de almacén de columnas.|  
|**column_id**|**int**|Identificador de la columna de almacén de columnas.|  
|**dictionary_id**|**int**|Identificador del diccionario.|  
|**version**|**int**|Versión del formato de diccionario.|  
|**type**|**int**|Tipo de diccionario:<br /><br /> 1: Diccionario hash que contiene valores **int**<br /><br /> 2: no se usa<br /><br /> 3: Diccionario hash que contiene valores de cadena<br /><br /> 4: Diccionario hash que contiene valores **float**|  
|**last_id**|**int**|El último identificador de datos del diccionario.|  
|**entry_count**|**bigint**|Número de entradas en el diccionario.|  
|**on_disc_size**|**bigint**|Tamaño del diccionario en bytes.|  
|**pdw_node_id**|**int**|Identificador único de un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nodo.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW SERVER STATE`.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [CREAR índice de almacén de columnas &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)   
 [sys.pdw_nodes_column_store_segments &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-segments-transact-sql.md)   
 [sys.pdw_nodes_column_store_row_groups &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-row-groups-transact-sql.md)  
  
  
