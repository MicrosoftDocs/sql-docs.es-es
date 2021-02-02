---
description: sys.dm_resource_governor_workload_groups (Transact-SQL)
title: sys.dm_resource_governor_workload_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_resource_governor_workload_groups
- sys.dm_resource_governor_workload_groups_TSQL
- dm_resource_governor_workload_groups
- dm_resource_governor_workload_groups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_workload_groups dynamic management view
ms.assetid: f63c4914-1272-43ef-b135-fe1aabd953e0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 20ad2ca8797dd9e3f094028422edef5b935cd2ae
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237525"
---
# <a name="sysdm_resource_governor_workload_groups-transact-sql"></a>sys.dm_resource_governor_workload_groups (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve las estadísticas del grupo de cargas de trabajo y la configuración actual en memoria del grupo de cargas de trabajo. Puede unirse esta vista con sys.dm_resource_governor_resource_pools para obtener el nombre del grupo de recursos de servidor.  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_resource_governor_workload_groups**.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|group_id|**int**|Id. del grupo de cargas de trabajo No admite valores NULL.|  
|name|**sysname**|Nombre del grupo de cargas de trabajo No admite valores NULL.|  
|{1}pool_id{2}|**int**|Id. del grupo de recursos de servidor. No admite valores NULL.|  
|external_pool_id|**int**|**Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] .<br /><br /> IDENTIFICADOR del grupo de recursos externos. No admite valores NULL.|  
|statistics_start_time|**datetime**|Tiempo en que se restableció la colección de estadísticas para el grupo de cargas de trabajo. No admite valores NULL.|  
|total_request_count|**bigint**|El recuento acumulado de solicitudes completadas en el grupo de cargas de trabajo. No admite valores NULL.|  
|total_queued_request_count|**bigint**|El recuento acumulado de solicitudes en cola una vez alcanzado el límite de GROUP_MAX_REQUESTS. No admite valores NULL.|  
|active_request_count|**int**|Recuento actual de solicitudes. No admite valores NULL.|  
|queued_request_count|**int**|Recuento actual de solicitudes en cola. No admite valores NULL.|  
|total_cpu_limit_violation_count|**bigint**|Recuento acumulado de solicitudes que superan el límite de CPU. No admite valores NULL.|  
|total_cpu_usage_ms|**bigint**|Uso acumulado de la CPU en milisegundos de este grupo de cargas de trabajo. No admite valores NULL.|  
|max_request_cpu_time_ms|**bigint**|Uso máximo de CPU, en milisegundos, para una única solicitud. No admite valores NULL.<br /><br /> **Nota:** Se trata de un valor medido, a diferencia de request_max_cpu_time_sec, que es un valor configurable. Para obtener más información, vea [Clase de eventos Umbral de la CPU superado](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).|  
|blocked_task_count|**int**|Contador actual de tareas bloqueadas. No admite valores NULL.|  
|total_lock_wait_count|**bigint**|Recuento acumulado de esperas del bloqueo producidas. No admite valores NULL.|  
|total_lock_wait_time_ms|**bigint**|Suma acumulada de tiempo transcurrido, en milisegundos, que se mantiene un bloqueo. No admite valores NULL.|  
|total_query_optimization_count|**bigint**|El recuento acumulado de optimizaciones de consultas en este grupo de cargas de trabajo. No admite valores NULL.|  
|total_suboptimal_plan_generation_count|**bigint**|Recuento acumulado de generaciones de planes poco óptimos producidas en este grupo de cargas de trabajo debido a la presión de memoria. No admite valores NULL.|  
|total_reduced_memgrant_count|**bigint**|Recuento acumulado de concesiones de memoria que alcanzaron el límite máximo de tamaño de consulta. No admite valores NULL.|  
|max_request_grant_memory_kb|**bigint**|El tamaño máximo de la concesión de memoria, en kilobytes, de una única solicitud desde que se restablecieron las estadísticas. No admite valores NULL.|  
|active_parallel_thread_count|**bigint**|Recuento actual de uso del subproceso paralelo. No admite valores NULL.|  
|importance|**sysname**|Valor de la configuración actual de la importancia relativa de una solicitud en este grupo de cargas de trabajo. Importance es uno de los siguientes, siendo Medium el valor predeterminado: Low, Medium o High.<br /><br /> No admite valores NULL.|  
|request_max_memory_grant_percent|**int**|Valor actual de la concesión máxima de memoria, en porcentaje, para una única solicitud. No admite valores NULL.|  
|request_max_cpu_time_sec|**int**|Valor actual máximo de uso de CPU, en segundos, para una única solicitud. No admite valores NULL.|  
|request_memory_grant_timeout_sec|**int**|Valor actual del tiempo de espera de concesiones de memoria, en segundos, para una única solicitud. No admite valores NULL.|  
|group_max_requests|**int**|Valor actual del número máximo de solicitudes simultáneas. No admite valores NULL.|  
|max_dop|**int**|Grado máximo de paralelismo configurado para el grupo de cargas de trabajo. El valor predeterminado, 0, utiliza la configuración global. No admite valores NULL.| 
|effective_max_dop|**int**|**Se aplica a: a** partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] .<br /><br />Grado máximo efectivo de paralelismo para el grupo de cargas de trabajo. No admite valores NULL.| 
|total_cpu_usage_preemptive_ms|**bigint**|**Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] .<br /><br />Tiempo total de CPU usado durante la programación de modo preferente para el grupo de cargas de trabajo, medido en MS. No admite valores NULL.<br /><br />Para ejecutar código situado fuera de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (por ejemplo, en procedimientos almacenados extendidos y consultas distribuidas), se tiene que ejecutar un subproceso fuera del control del programador no preferente. Para hacerlo, un trabajador se cambia al modo preferente.| 
|request_max_memory_grant_percent_numeric|**float**|**Se aplica a: a** partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] .<br /><br />Valor actual de la concesión máxima de memoria, en porcentaje, para una única solicitud. No admite valores NULL.| 
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="remarks"></a>Notas  
 Esta vista de administración dinámica muestra la configuración en memoria. Para ver los metadatos de la configuración almacenada, use el sys.resource_governor_workload_groups &#40;vista de catálogo [de Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-workload-groups-transact-sql.md) .  
  
 Cuando `ALTER RESOURCE GOVERNOR RESET STATISTICS` se ejecuta correctamente, se restablecen los contadores siguientes `statistics_start_time` : `total_request_count` , `total_queued_request_count` ,, `total_cpu_limit_violation_count` , `total_cpu_usage_ms` , `max_request_cpu_time_ms` ,,,,, `total_lock_wait_count` `total_lock_wait_time_ms` `total_query_optimization_count` `total_suboptimal_plan_generation_count` `total_reduced_memgrant_count` y `max_request_grant_memory_kb` . El contador `statistics_start_time` se establece en la fecha y hora actuales del sistema, y los otros contadores se establecen en cero (0).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW SERVER STATE`.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_resource_governor_resource_pools &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)   
 [sys.resource_governor_workload_groups &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-resource-governor-workload-groups-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
