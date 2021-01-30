---
description: sys.query_store_wait_stats (Transact-SQL)
title: sys.query_store_wait_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.query_store_wait_stats
- query_store_wait_stats
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_wait_stats catalog view
- sys.query_store_wait_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2517cd093bd840c8cf4cea34073acb8e03a98c8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171817"
---
# <a name="sysquery_store_wait_stats-transact-sql"></a>sys.query_store_wait_stats (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Contiene información sobre la información de espera de la consulta.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**wait_stats_id**|**bigint**|Identificador de la fila que representa las estadísticas de espera para el plan_id, runtime_stats_interval_id, execution_type y wait_category. Solo es único para los intervalos de estadísticas de tiempo de ejecución anteriores. Para el intervalo actualmente activo, puede haber varias filas que representen las estadísticas de espera del plan al que hace referencia plan_id, con el tipo de ejecución representado por execution_type y la categoría de espera representada por wait_category. Normalmente, una fila representa las estadísticas de espera que se vacían en el disco, mientras que otras representan el estado en memoria. Por lo tanto, para obtener el estado real de cada intervalo, debe agregar métricas, agrupación por plan_id, runtime_stats_interval_id, execution_type y wait_category. |  
|**plan_id**|**bigint**|Clave externa. Combina con [sys.query_store_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md).|  
|**runtime_stats_interval_id**|**bigint**|Clave externa. Combina con [sys.query_store_runtime_stats_interval &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md).|  
|**wait_category**|**tinyint**|Los tipos de espera se clasifican en la tabla siguiente y, a continuación, se agrega tiempo de espera en estas categorías de espera. Las distintas categorías de espera requieren un análisis de seguimiento diferente para resolver el problema, pero los tipos de espera de la misma categoría conducen a experiencias de solución de problemas similares, y proporcionar la consulta afectada además de las esperas es la pieza que falta para completar la mayoría de estas investigaciones correctamente.|
|**wait_category_desc**|**nvarchar(128)**|Para obtener una descripción textual del campo de la categoría de espera, revise la tabla siguiente.|
|**execution_type**|**tinyint**|Determina el tipo de ejecución de la consulta:<br /><br /> 0: ejecución normal (finalizada correctamente)<br /><br /> 3-ejecución de la anulada iniciada por el cliente<br /><br /> 4-ejecución de excepción anulada|  
|**execution_type_desc**|**nvarchar(128)**|Descripción textual del campo de tipo de ejecución:<br /><br /> 0: normal<br /><br /> 3-anulado<br /><br /> 4: excepción|  
|**total_query_wait_time_ms**|**bigint**|`CPU wait`Tiempo total para el plan de consulta en el intervalo de agregación y la categoría de espera (en milisegundos).|
|**avg_query_wait_time_ms**|**float**|Duración media de espera del plan de consulta por ejecución en el intervalo de agregación y la categoría de espera (en milisegundos).|
|**last_query_wait_time_ms**|**bigint**|Duración de la última espera del plan de consulta en el intervalo de agregación y la categoría de espera (en milisegundos).|
|**min_query_wait_time_ms**|**bigint**|`CPU wait`Tiempo mínimo para el plan de consulta dentro del intervalo de agregación y la categoría de espera (en milisegundos).|
|**max_query_wait_time_ms**|**bigint**|`CPU wait`Tiempo máximo para el plan de consulta dentro del intervalo de agregación y la categoría de espera (en milisegundos).|
|**stdev_query_wait_time_ms**|**float**|`Query wait` desviación estándar de la duración del plan de consulta en el intervalo de agregación y la categoría de espera (en milisegundos).|

## <a name="wait-categories-mapping-table"></a>Tabla de asignación de categorías de espera

"%" se utiliza como carácter comodín
  
|Valor entero|Categoría de espera|Los tipos de espera se incluyen en la categoría|  
|-----------------|---------------|-----------------|  
|**0**|**Unknown**|Unknown |  
|**1**|**CPU**|SOS_SCHEDULER_YIELD|
|**2**|**Subproceso de trabajo**|THREADPOOL|
|**3**|**Bloquear**|LCK_M_%|
|**4**|**Bloqueo temporal**|LATCH_%|
|**5**|**Bloqueo temporal de búfer**|PAGELATCH_%|
|**6**|**E/s de búfer**|PAGEIOLATCH_%|
|**7**|**Compilación** _|RESOURCE_SEMAPHORE_QUERY_COMPILE|
|_ *8**|**SQL CLR**|CLR%, SQLCLR%|
|**9**|**Creación de reflejo**|DBMIRROR%|
|**10**|**Transacción**|% DE TRANSACCIONES%, DTC%, TRAN_MARKLATCH_%, MSQL_XACT_% TRANSACTION_MUTEX|
|**11**|**Activos**|SLEEP_%, LAZYWRITER_SLEEP, SQLTRACE_BUFFER_FLUSH, SQLTRACE_INCREMENTAL_FLUSH_SLEEP, SQLTRACE_WAIT_ENTRIES, FT_IFTS_SCHEDULER_IDLE_WAIT, XE_DISPATCHER_WAIT, REQUEST_FOR_DEADLOCK_SEARCH, LOGMGR_QUEUE, ONDEMAND_TASK_QUEUE, CHECKPOINT_QUEUE XE_TIMER_EVENT|
|**12**|**Preemptive**|PREEMPTIVE_%|
|**13**|**Service Broker**|BROKER_% **(pero no BROKER_RECEIVE_WAITFOR)**|
|**14**|**E/s de registro de Tran**|LOGMGR, LOGBUFFER, LOGMGR_RESERVE_APPEND, LOGMGR_FLUSH, LOGMGR_PMM_LOG, CHKPT, WRITELOG|
|**15**|**E/s de red**|ASYNC_NETWORK_IO, NET_WAITFOR_PACKET, PROXY_NETWORK_IO, EXTERNAL_SCRIPT_NETWORK_IOF|
|**16**|**Paralelismo**|ESPERAS CXPACKET, EXCHANGE, HT%, BMP%, BP%|
|**17**|**Memoria**|RESOURCE_SEMAPHORE, CMEMTHREAD, CMEMPARTITIONED, EE_PMOLOCK, MEMORY_ALLOCATION_EXT, RESERVED_MEMORY_ALLOCATION_EXT, MEMORY_GRANT_UPDATE|
|**18**|**Espera de usuario**|WAITFOR, WAIT_FOR_RESULTS BROKER_RECEIVE_WAITFOR|
|**19**|**Seguimiento**|TRACEWRITE, SQLTRACE_LOCK, SQLTRACE_FILE_BUFFER, SQLTRACE_FILE_WRITE_IO_COMPLETION, SQLTRACE_FILE_READ_IO_COMPLETION, SQLTRACE_PENDING_BUFFER_WRITERS, SQLTRACE_SHUTDOWN, QUERY_TRACEOUT TRACE_EVTNOTIFF|
|**20**|**Búsqueda de texto completo**|FT_RESTART_CRAWL, RECOPILADOR DE TEXTO COMPLETO, MSSEARCH, FT_METADATA_MUTEX, FT_IFTSHC_MUTEX, FT_IFTSISM_MUTEX, FT_IFTS_RWLOCK, FT_COMPROWSET_RWLOCK, FT_MASTER_MERGE, FT_PROPERTYLIST_CACHE, FT_MASTER_MERGE_COORDINATOR, PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC|
|**21**|**Otra e/s de disco**|ASYNC_IO_COMPLETION, IO_COMPLETION, BACKUPIO, WRITE_COMPLETION, IO_QUEUE_LIMIT, IO_RETRY|
|**22**|**Replicación**|SE_REPL_%, REPL_%, HADR_% **(pero no HADR_THROTTLE_LOG_RATE_GOVERNOR)**, PWAIT_HADR_%, REPLICA_WRITES, FCB_REPLICA_WRITE, FCB_REPLICA_READ PWAIT_HADRSIM|
|**23**|**Regulador de velocidad de registro**|LOG_RATE_GOVERNOR, POOL_LOG_RATE_GOVERNOR, HADR_THROTTLE_LOG_RATE_GOVERNOR, INSTANCE_LOG_RATE_GOVERNOR|

La categoría de espera de **compilación** no se admite actualmente.

## <a name="permissions"></a>Permisos

 Requiere el permiso `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Consulte también

- [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
- [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)
- [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)
- [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)
- [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)
- [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)
- [Supervisar el rendimiento mediante el Almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Procedimientos almacenados del almacén de consultas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
