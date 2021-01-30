---
description: sys.dm_resource_governor_resource_pools (Transact-SQL)
title: sys.dm_resource_governor_resource_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_resource_governor_resource_pools_TSQL
- dm_resource_governor_resource_pools_TSQL
- sys.dm_resource_governor_resource_pools
- dm_resource_governor_resource_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_resource_pools dynamic management view
ms.assetid: 9bfc926e-d8bc-40f8-9229-ab1f8a1e69c5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 81b2333e94a00c00c6743a51029d2b0e70cd9840
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99135221"
---
# <a name="sysdm_resource_governor_resource_pools-transact-sql"></a>sys.dm_resource_governor_resource_pools (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve información acerca del estado actual del grupo de recursos de servidor, la configuración actual de los grupos de recursos de servidor y estadísticas del grupo de recursos de servidor.  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_resource_governor_resource_pools**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|{1}pool_id{2}|**int**|Identificador del grupo de recursos. No admite valores NULL.|  
|name|**sysname**|Nombre del grupo de recursos. No admite valores NULL.|  
|statistics_start_time|**datetime**|La hora en que se restablecieron las estadísticas para este grupo. No admite valores NULL.|  
|total_cpu_usage_ms|**bigint**|El uso acumulado de la CPU en milisegundos desde que se restablecieron las estadísticas del regulador de recursos. No admite valores NULL.|  
|cache_memory_kb|**bigint**|El uso de la memoria caché total actual en kilobytes. No admite valores NULL.|  
|compile_memory_kb|**bigint**|El uso de memoria descartada total actual en kilobytes (kB). La mayoría de este uso sería para la compilación y optimización, pero también puede incluir otros usuarios de la memoria. No admite valores NULL.|  
|used_memgrant_kb|**bigint**|La memoria usada (descartada) total actual de las concesiones de memoria. No admite valores NULL.|  
|total_memgrant_count|**bigint**|El número acumulado de concesiones de memoria en este grupo de recursos de servidor. No admite valores NULL.|  
|total_memgrant_timeout_count|**bigint**|El número acumulado de tiempos de espera de concesiones de memoria en este grupo de recursos de servidor. No admite valores NULL.|  
|active_memgrant_count|**int**|El número actual de concesiones de memoria. No admite valores NULL.|  
|active_memgrant_kb|**bigint**|La suma, en kilobytes (kB), de las concesiones actuales de memoria. No admite valores NULL.|  
|memgrant_waiter_count|**int**|El número de consultas pendientes actualmente en concesiones de memoria. No admite valores NULL.|  
|max_memory_kb|**bigint**|La cantidad máxima de memoria, en kilobytes, que el grupo de recursos de servidor puede tener. Depende de la configuración actual y del estado del servidor. No admite valores NULL.|  
|used_memory_kb|**bigint**|La cantidad de memoria utilizada, en kilobytes, para el grupo de recursos de servidor. No admite valores NULL.|  
|target_memory_kb|**bigint**|La cantidad de memoria de destino, en kilobytes, que el grupo de recursos de servidor está intentando lograr. Depende de la configuración actual y del estado del servidor. No admite valores NULL.|  
|out_of_memory_count|**bigint**|El número de asignaciones de memoria con errores en el grupo desde que se restablecieron las estadísticas del regulador de recursos. No admite valores NULL.|  
|min_cpu_percent|**int**|La configuración actual del ancho banda de la CPU promedio garantizado para todas las solicitudes en el grupo de recursos de servidor cuando hay contención de CPU. No admite valores NULL.|  
|max_cpu_percent|**int**|La configuración actual del ancho banda de la CPU promedio máximo permitido para todas las solicitudes en el grupo de recursos de servidor cuando hay contención de CPU. No admite valores NULL.|  
|min_memory_percent|**int**|La configuración actual de la cantidad de memoria garantizada para todas las solicitudes en el grupo de recursos de servidor cuando hay contención de la memoria. No se comparte con otros grupos de recursos de servidor. No admite valores NULL.|  
|max_memory_percent|**int**|La configuración actual del porcentaje de memoria total del servidor que pueden utilizar las solicitudes en este grupo de recursos de servidor. No admite valores NULL.|  
|cap_cpu_percent|**int**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> El límite máximo de ancho de banda de la CPU que recibirán todas las solicitudes en el grupo de recursos. Limita el nivel de ancho de banda máximo de la CPU según el nivel especificado. El intervalo permitido para el valor es de 1 a 100. No admite valores NULL.|  
|min_iops_per_volume|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> La configuración mínima de operaciones de E/S por segundo (IOPS) por cada volumen de disco para este grupo. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, el MIN_IOPS_PER_VOLUME de grupo de recursos y la configuración de MAX_IOPS_PER_VOLUME son 0.|  
|max_iops_per_volume|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> La configuración máxima de operaciones de E/S por segundo (IOPS) por cada volumen de disco para este grupo. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, en el grupo de recursos de servidor, el valor de MIN_IOPS_PER_VOLUME y de MAX_IOPS_PER_VOLUME es 0.|  
|read_io_queued_total|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El total de operaciones de E/S de lectura puestas en cola desde el restablecimiento del regulador de recursos. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, el MIN_IOPS_PER_VOLUME de grupo de recursos y la configuración de MAX_IOPS_PER_VOLUME son 0.|  
|read_io_issued_total|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El total de operaciones de E/S de lectura emitidas desde que se restablecieron las estadísticas del regulador de recursos. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, el MIN_IOPS_PER_VOLUME de grupo de recursos y la configuración de MAX_IOPS_PER_VOLUME son 0.|  
|read_io_completed_total|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El total de operaciones de E/S de lectura completadas desde que se restablecieron las estadísticas del regulador de recursos. No admite valores NULL.|  
|read_io_throttled_total|**int**|El total de operaciones de E/S de lectura limitadas desde que se restablecieron las estadísticas del regulador de recursos. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, el MIN_IOPS_PER_VOLUME de grupo de recursos y la configuración de MAX_IOPS_PER_VOLUME son 0.|  
|read_bytes_total|**bigint**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El número total de bytes leídos desde que se restablecieron las estadísticas del regulador de recursos. No admite valores NULL.|  
|read_io_stall_total_ms|**bigint**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Tiempo total (en milisegundos) desde que llegó la operación de E/S de lectura hasta que se completó. No admite valores NULL. |  
|read_io_stall_queued_ms|**bigint**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Tiempo total (en milisegundos) desde que llegó la operación de E/S de lectura hasta que hubo el problema. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, el MIN_IOPS_PER_VOLUME de grupo de recursos y la configuración de MAX_IOPS_PER_VOLUME son 0.<br /><br /> Para determinar si la configuración de e/s del grupo está causando latencia, reste **read_io_stall_queued_ms** de **read_io_stall_total_ms**.|  
|write_io_queued_total|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El total de operaciones de E/S de escritura puestas en cola desde que se restablecieron las estadísticas del regulador de recursos. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, el MIN_IOPS_PER_VOLUME de grupo de recursos y la configuración de MAX_IOPS_PER_VOLUME son 0.|  
|write_io_issued_total|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El total de operaciones de E/S de escritura emitidas desde que se restablecieron las estadísticas del regulador de recursos. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, el MIN_IOPS_PER_VOLUME de grupo de recursos y la configuración de MAX_IOPS_PER_VOLUME son 0.|  
|write_io_completed_total|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El total de operaciones de E/S de escritura completadas desde que se restablecieron las estadísticas del regulador de recursos. No admite valores NULL|  
|write_io_throttled_total|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El total de operaciones de E/S de escritura limitadas desde que se restablecieron las estadísticas del regulador de recursos. No admite valores NULL|  
|write_bytes_total|**bigint**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> El número total de bytes escritos desde que se restablecieron las estadísticas del regulador de recursos. No admite valores NULL.|  
|write_io_stall_total_ms|**bigint**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Tiempo total (en milisegundos) desde que llegó la operación de E/S de escritura hasta que se completó. No admite valores NULL. |  
|write_io_stall_queued_ms|**bigint**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Tiempo total (en milisegundos) desde que llegó la operación de E/S de escritura hasta que hubo el problema. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, el MIN_IOPS_PER_VOLUME de grupo de recursos y la configuración de MAX_IOPS_PER_VOLUME son 0.<br /><br /> Este es el retraso que introduce la gobernanza de recursos de E/S.|  
|io_issue_violations_total|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Total de infracciones de emisiones de operaciones de E/S. Es decir, el número de veces en que la tasa de emisiones de operaciones de E/S ha sido inferior a la tasa reservada. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, el MIN_IOPS_PER_VOLUME de grupo de recursos y la configuración de MAX_IOPS_PER_VOLUME son 0.|  
|io_issue_delay_total_ms|**bigint**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Tiempo total (en milisegundos) entre la emisión programada y la emisión real de operaciones de E/S. Acepta valores NULL. Es NULL si el grupo de recursos de servidor no está regulado para las operaciones de E/S. Es decir, el MIN_IOPS_PER_VOLUME de grupo de recursos y la configuración de MAX_IOPS_PER_VOLUME son 0.|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="remarks"></a>Observaciones  
 Los grupos de cargas de trabajo y los grupos de recursos de servidor del regulador de recursos tienen una asignación de varios a uno. Como resultado, muchas de las estadísticas del grupo de recursos de servidor se derivan de las estadísticas del grupo de cargas de trabajo.  
  
 Esta vista de administración dinámica muestra la configuración en memoria. Para ver los metadatos de configuración almacenados, utilice la vista de catálogo sys.resource_governor_resource_pools.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW SERVER STATE.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [sys.resource_governor_resource_pools &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-resource-governor-resource-pools-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  



