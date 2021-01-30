---
description: sys.dm_resource_governor_workload_groups_history_ex (Azure SQL Database)
title: sys.dm_resource_governor_workload_groups_history_ex (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 01/05/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_resource_governor_workload_groups_history_ex_TSQL
- sys.dm_resource_governor_workload_groups_history_ex
- dm_resource_governor_workload_groups_history_ex
- dm_resource_governor_workload_groups_history_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_resource_governor_workload_groups_history_ex dynamic management view
author: joesackmsft
ms.author: josack
ms.openlocfilehash: ac8bc675c5a059c211e6ecfbed063d54daf6ee92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203306"
---
# <a name="sysdm_resource_governor_workload_groups_history_ex-azure-sql-database"></a>sys.dm_resource_governor_workload_groups_history_ex (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Cada fila representa una instantánea periódica de las estadísticas del grupo de cargas de trabajo en Azure SQL Database. Cuando el motor de base de datos se inicia y cada pocos segundos después, se toma una instantánea. El intervalo entre la instantánea actual y la anterior puede variar, y se proporciona en la `duration_ms` columna. Se devuelven las instantáneas disponibles más recientes, hasta 128 instantáneas para cada grupo de cargas de trabajo.
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**pool_id**| int |Id. del grupo de recursos de servidor. No admite valores NULL.|
|**group_id**| int |Id. del grupo de cargas de trabajo No admite valores NULL.|
|**name**| nvarchar(256) |Nombre del grupo de cargas de trabajo No admite valores NULL.|
|**snapshot_time**| datetime |Fecha y hora de la instantánea de estadísticas del grupo de recursos tomada.|
|**duration_ms**| int |Duración entre la instantánea actual y la anterior.|
|**active_worker_count**| int |Total de trabajos en la instantánea actual.|
|**active_request_count**| int |Recuento actual de solicitudes. No admite valores NULL.|
|**active_session_count**| int |Total de sesiones activas en la instantánea actual.|
|**total_request_count**| bigint |El recuento acumulado de solicitudes completadas en el grupo de cargas de trabajo. No admite valores NULL.|
|**delta_request_count**| int |Recuento de solicitudes completadas en el grupo de cargas de trabajo desde la última instantánea. No admite valores NULL.|
|**total_cpu_usage_ms**| bigint |Uso acumulado de la CPU en milisegundos de este grupo de cargas de trabajo. No admite valores NULL.|
|**delta_cpu_usage_ms**| int |Uso de CPU en milisegundos desde la última instantánea. No admite valores NULL.|
|**delta_cpu_usage_preemptive_ms**| int |Las llamadas preferentes de Win32 no rigen por el RG de CPU de SQL, desde la última instantánea.|
|**delta_reads_reduced_memgrant_count**| int |El recuento de concesiones de memoria que alcanzaron el límite máximo de tamaño de consulta desde la última instantánea. No admite valores NULL.|
|**reads_throttled**| int |Número total de lecturas limitadas.|
|**delta_reads_queued**| int |Número total de e/s de lectura en cola desde la última instantánea. Acepta valores NULL. Es NULL si no se rige el grupo de recursos para la e/s.|
|**delta_reads_issued**| int |Número total de e/s de lectura emitidos desde la última instantánea. Acepta valores NULL. Es NULL si no se rige el grupo de recursos para la e/s.|
|**delta_reads_completed**| int |Número total de e/s de lectura completadas desde la última instantánea. No admite valores NULL.|
|**delta_read_bytes**| bigint |Número total de bytes leídos desde la última instantánea. No admite valores NULL.|
|**delta_read_stall_ms**| int |Tiempo total (en milisegundos) entre la llegada y finalización de e/s de lectura desde la última instantánea. No admite valores NULL.|
|**delta_read_stall_queued_ms**| int |Tiempo total (en milisegundos) entre la llegada de e/s de lectura y el problema desde la última instantánea. Acepta valores NULL. Es NULL si no se rige el grupo de recursos para la e/s. Un delta_read_stall_queued_ms distinto de cero significa que la e/s se ve afectada por RG.|
|**delta_writes_queued**| int |Número total de e/s de escritura en cola desde la última instantánea. Acepta valores NULL. Es NULL si no se rige el grupo de recursos para la e/s.|
|**delta_writes_issued**| int |Número total de e/s de escritura emitidas desde la última instantánea. Acepta valores NULL. Es NULL si no se rige el grupo de recursos para la e/s.|
|**delta_writes_completed**| int |Número total de e/s de escritura completadas desde la última instantánea. No admite valores NULL.|
|**delta_writes_bytes**| bigint |Número total de bytes escritos desde la última instantánea. No admite valores NULL.|
|**delta_write_stall_ms**| int |Tiempo total (en milisegundos) entre la llegada y finalización de e/s de escritura desde la última instantánea. No admite valores NULL.|
|**delta_background_writes**| int |Las escrituras totales realizadas por las tareas en segundo plano desde la última instantánea.|
|**delta_background_write_bytes**| bigint |Tamaño total de escritura realizado por las tareas en segundo plano desde la última instantánea, en bytes.|
|**delta_log_bytes_used**| bigint |Registro usado desde la última instantánea en bytes.|
|**delta_log_temp_db_bytes_used**| bigint |Registro de tempdb usado desde la última instantánea en bytes.|
|**delta_query_optimizations**| bigint |El número de optimizaciones de consultas en este grupo de cargas de trabajo desde la última instantánea. No admite valores NULL.|
|**delta_suboptimal_plan_generations**| bigint |Recuento de generaciones de planes no óptimas que se produjeron en este grupo de cargas de trabajo debido a la presión de memoria desde la última instantánea. No admite valores NULL.
|**max_memory_grant_kb**| bigint |Concesión de memoria máxima para el grupo en KB.|
|**max_request_cpu_msec**| bigint |Uso máximo de CPU, en milisegundos, para una única solicitud. No admite valores NULL.|
|**max_concurrent_request**| int |Valor actual del número máximo de solicitudes simultáneas. No admite valores NULL.|
|**max_io**| int |Límite máximo de e/s para el grupo.|
|**max_global_io**| int |Solamente se identifica con fines informativos. No se admite. La compatibilidad con versiones posteriores no está garantizada.
|**max_queued_io**| int |Solamente se identifica con fines informativos. No se admite. La compatibilidad con versiones posteriores no está garantizada.|
|**max_log_rate_kb**| bigint |Velocidad de registro máxima (kilobytes por segundo) en el nivel de grupo de recursos.|
|**max_session**| int |Límite de sesión para el grupo.|
|**max_worker**| int |Límite de trabajo para el grupo.|
|||

## <a name="permissions"></a>Permisos

Esta vista requiere el permiso VIEW SERVER STATE.

## <a name="remarks"></a>Observaciones

Los usuarios pueden tener acceso a esta vista de administración dinámica para supervisar el consumo de recursos casi en tiempo real para el grupo de cargas de trabajo de usuario, así como los grupos internos del sistema de la instancia de Azure SQL Database.

> [!IMPORTANT]
> La mayoría de los datos expuestos por esta DMV están destinados al consumo interno y están sujetos a cambios.

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se devuelven los datos de velocidad de registro máxima y el consumo en cada instantánea por grupo de usuarios:

```sql
SELECT snapshot_time,
       name,
       max_log_rate_kb,
       delta_log_bytes_used
FROM sys.dm_resource_governor_workload_groups_history_ex
WHERE name LIKE 'User%'
ORDER BY snapshot_time DESC;
```

## <a name="see-also"></a>Consulte también

- [Gobierno de velocidad de registro de traducción](/azure/sql-database/sql-database-resource-limits-database-server#transaction-log-rate-governance)
- [Límites de recursos de DTU de grupos elásticos](/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- [Límites de recursos de núcleo virtual de grupos elásticos](/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)