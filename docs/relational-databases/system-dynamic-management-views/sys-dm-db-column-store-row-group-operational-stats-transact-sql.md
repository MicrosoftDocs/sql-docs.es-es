---
description: sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)
title: sys.dm_db_column_store_row_group_operational_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 31b71c68-50a0-4fd8-a7fe-2d2292be1163
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4adcc52df1d3039f18ca63cbe93bd355f8df4544
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180659"
---
# <a name="sysdm_db_column_store_row_group_operational_stats-transact-sql"></a>sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Devuelve la actividad actual de e/s de nivel de fila, el bloqueo y el método de acceso para filas comprimidos en un índice de almacén de columnas. Use **Sys.dm_db_column_store_row_group_operational_stats** para realizar un seguimiento de la cantidad de tiempo que una consulta de usuario debe esperar para leer o escribir en un filas o partición comprimidos de un índice de almacén de columnas e identificar filas que detectan actividades de e/s y zonas activas significativas.  
  
 Los índices de almacén de columnas en memoria no aparecen en esta DMV.  
 
 
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|IDENTIFICADOR de la tabla con el índice de almacén de columnas.|  
|**id_de_índice**|**int**|Identificador del índice de almacén de columnas.|  
|**partition_number**|**int**|Número de partición en base 1 en el índice o montón.|  
|**row_group_id**|**int**|IDENTIFICADOR del filas en el índice de almacén de columnas. Es único en una partición.|  
|**scan_count**|**int**|Número de recorridos a través de filas desde el último reinicio de SQL.|  
|**delete_buffer_scan_count**|**int**|Número de veces que se usó el búfer de eliminación para determinar las filas eliminadas en este filas. Esto incluye el acceso a la tabla hash en memoria y el árbol b subyacente.|  
|**index_scan_count**|**int**|Número de veces que se examinó la partición de índice de almacén de columnas. Es el mismo para todos los filas de la partición.|  
|**rowgroup_lock_count**|**bigint**|Recuento acumulado de solicitudes de bloqueo para este filas desde el último reinicio de SQL.|  
|**rowgroup_lock_wait_count**|**bigint**|Número acumulado de veces que el motor de base de datos ha esperado en este bloqueo de filas desde el último reinicio de SQL.|  
|**rowgroup_lock_wait_in_ms**|**bigint**|Número acumulado de milisegundos que el motor de base de datos ha esperado en este bloqueo de filas desde el último reinicio de SQL.|  
  
## <a name="permissions"></a>Permisos  
 Necesita los siguientes permisos:  
  
-   El permiso CONTROL en la tabla especificada por object_id.  
  
-   Permiso VIEW DATABASE STATE para devolver información sobre todos los objetos de la base de datos, mediante el uso del comodín de objeto @*object_id* = null  
  
 El permiso VIEW DATABASE STATE permite devolver todos los objetos de la base de datos, independientemente de los permisos CONTROL denegados en objetos específicos.  
  
 Si se deniega el permiso VIEW DATABASE STATE, no se puede devolver ningún objeto de la base de datos, independientemente de que se hayan concedido permisos CONTROL a objetos específicos. Además, cuando se especifica el carácter comodín de base de datos @*database_id*= null, se omite la base de datos.  
  
 Para obtener más información, vea [funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con índices &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_usage_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)   
 [sys.dm_os_latch_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)   
 [sys.dm_db_partition_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  

