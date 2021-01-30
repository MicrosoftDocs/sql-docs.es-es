---
description: sys.dm_fts_population_ranges (Transact-SQL)
title: sys.dm_fts_population_ranges (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_population_ranges
- sys.dm_fts_population_ranges_TSQL
- dm_fts_population_ranges_TSQL
- dm_fts_population_ranges
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_population_ranges dynamic management view
ms.assetid: 58d8564b-9c43-4965-a31c-2893890334ef
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 31465b18c9395a961a08a607648424b62c840c6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99106416"
---
# <a name="sysdm_fts_population_ranges-transact-sql"></a>sys.dm_fts_population_ranges (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información acerca de los intervalos específicos relacionados con un rellenado de índices de texto completo actualmente en progreso.  
   
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**memory_address**|**varbinary(8**|Dirección de búferes de memoria asignados para la actividad relacionada con este subintervalo de un rellenado de índices de texto completo.|  
|**parent_memory_address**|**varbinary(8**|Dirección de búferes de memoria que representa el objeto primario de todos los intervalos de rellenado relacionados con un índice de texto completo.|  
|**is_retry**|**bit**|Si el valor es 1, este subintervalo es responsable de reintentar las filas con errores.|  
|**session_id**|**smallint**|Id. de la sesión que procesa actualmente esta tarea.|  
|**processed_row_count**|**int**|Número de filas que se han procesado en este intervalo. El progreso se conserva y se cuenta cada 5 minutos, en lugar de con cada confirmación del lote.|  
|**error_count**|**int**|Número de filas que han tenido errores en este intervalo. El progreso se conserva y se cuenta cada 5 minutos, en lugar de con cada confirmación del lote.|  
  
## <a name="permissions"></a>Permisos  

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
 
## <a name="physical-joins"></a>Combinaciones físicas  
 ![Combinaciones significativas de esta vista de administración dinámica](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-population-ranges-1.gif "Combinaciones significativas de esta vista de administración dinámica")  
  
## <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Relación|  
|----------|--------|------------------|  
|dm_fts_population_ranges.parent_memory_address|dm_fts_index_population.memory_address|Varios a uno|  
  
## <a name="see-also"></a>Consulte también  
  [Funciones y vistas de administración dinámica de la búsqueda de texto completo y la búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  

