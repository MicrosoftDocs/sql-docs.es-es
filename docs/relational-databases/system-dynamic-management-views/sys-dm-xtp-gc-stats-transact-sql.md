---
description: sys.dm_xtp_gc_stats (Transact-SQL)
title: sys.dm_xtp_gc_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_xtp_gc_stats
- dm_xtp_gc_stats_TSQL
- sys.dm_xtp_gc_stats_TSQL
- sys.dm_xtp_gc_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xtp_gc_stats dynamic management view
ms.assetid: 8287d611-50e3-43e1-ba8d-3e3793d3ba0e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: afe3d85f7763359a485eeb5b3939ae4444ddf38a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99126940"
---
# <a name="sysdm_xtp_gc_stats-transact-sql"></a>sys.dm_xtp_gc_stats (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Proporciona información (las estadísticas generales) sobre el comportamiento actual del proceso de recopilación de elementos no utilizados de [!INCLUDE[hek_2](../../includes/hek-2-md.md)].  
  
 La recopilación de elementos no utilizados de filas forma parte del procesamiento normal de transacciones o del subproceso principal de recopilación de elementos no utilizados, que se conoce como trabajador inactivo. Cuando se confirma una transacción de usuario, se quita de la cola un elemento de trabajo de la cola de recolección de elementos no utilizados ([sys.dm_xtp_gc_queue_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-gc-queue-stats-transact-sql.md)). Como parte del recorrido de esquinas sucias (un recorrido en búsqueda de áreas del índice a las que se tiene menos acceso), el trabajador inactivo recopila todas las filas que se pueden recopilar pero a las que no ha accedido la transacción de usuario principal.  
  
 Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nombre de la columna|Tipo|Descripción|  
|-----------------|----------|-----------------|  
|rows_examined|**bigint**|El número de filas que ha examinado el subsistema de recolección de elementos no utilizados desde que se inició el servidor.|  
|rows_no_sweep_needed|**bigint**|Número de filas quitadas sin un recorrido de esquinas sucias.|  
|rows_first_in_bucket|**bigint**|Número de filas que ha examinado la recolección de elementos no utilizados que eran la primera fila del depósito de hash.|  
|rows_first_in_bucket_removed|**bigint**|El número de filas que ha examinado la recolección de elementos no utilizados que eran la primera fila del cubo de hash y que se han quitado.|  
|rows_marked_for_unlink|**bigint**|El número de filas que ha examinado la recolección de elementos no utilizados que ya estaban marcadas como desvinculadas en sus índices con ref count =0.|  
|parallel_assist_count|**bigint**|Número de filas procesadas por transacciones de usuario.|  
|idle_worker_count|**bigint**|Número de filas no utilizadas procesadas por el trabajador inactivo.|  
|sweep_scans_started|**bigint**|Número de recorridos de esquinas sucias realizados por el subsistema de recopilación de elementos no utilizados.|  
|sweep_scans_retries|**bigint**|Número de recorridos de esquinas sucias realizados por el subsistema de recopilación de elementos no utilizados.|  
|sweep_rows_touched|**bigint**|Filas leídas por el procesamiento de esquinas sucias.|  
|sweep_rows_expiring|**bigint**|Filas que van a expirar leídas por el procesamiento de esquinas sucias.|  
|sweep_rows_expired|**bigint**|Filas expiradas leídas por el procesamiento de esquinas sucias.|  
|sweep_rows_expired_removed|**bigint**|Filas expiradas quitadas por el procesamiento de esquinas sucias.|  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW SERVER STATE en la instancia.  
  
## <a name="usage-scenario"></a>Escenario de uso  
 A continuación, se muestra un resultado de ejemplo:  
  
```  
rows_examined        rows_no_sweep_needed rows_first_in_bucket rows_first_in_bucket_removed  
280085               209512               69905  
rows_first_in_bucket_removed rows_marked_for_unlink parallel_assist_count idle_worker_count  
69905                        0                      8953  
  
idle_worker_count    sweep_scans_started  sweep_scan_retries   sweep_rows_touched  
10306473             670                  0                    1343  
  
sweep_rows_expiring  sweep_rows_expired   sweep_rows_expired_removed  
               0                 673673  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de tablas optimizadas para memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
