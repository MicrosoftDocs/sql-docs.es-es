---
description: sys.dm_os_workers (Transact-SQL)
title: sys.dm_os_workers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_workers_TSQL
- sys.dm_os_workers_TSQL
- dm_os_workers
- sys.dm_os_workers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_workers dynamic management view
ms.assetid: 4d5d1e52-a574-4bdd-87ae-b932527235e8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5fdbf16415df10f596ae9cf636d943fff3c3c4ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347627"
---
# <a name="sysdm_os_workers-transact-sql"></a>sys.dm_os_workers (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada trabajador del sistema. Para obtener más información sobre los trabajos, consulte la [Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md). 
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_workers**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|worker_address|**varbinary(8**|Dirección de memoria del trabajador.|  
|status|**int**|Solo para uso interno.|  
|is_preemptive|**bit**|1 = El trabajador está trabajando con un programa preferente. Un trabajador que ejecuta código externo trabaja con un programa preferente.|  
|is_fiber|**bit**|1 = El trabajador está trabajando con la opción de agrupación ligera. Para obtener más información, vea [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).|  
|is_sick|**bit**|1 = El trabajador está atascado intentando obtener un bloqueo por subproceso. Si este bit está establecido, podría indicar un problema con la contención de un objeto al que se tiene acceso con frecuencia.|  
|is_in_cc_exception|**bit**|1 = El trabajador está controlando actualmente una excepción que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|is_fatal_exception|**bit**|Especifica si este trabajador recibió una excepción grave.|  
|is_inside_catch|**bit**|1 = El trabajador está controlando actualmente una excepción.|  
|is_in_polling_io_completion_routine|**bit**|1 = El trabajador está ejecutando actualmente una rutina de finalización de E/S de una E/S pendiente. Para obtener más información, vea [sys.dm_io_pending_io_requests &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-io-pending-io-requests-transact-sql.md).|  
|context_switch_count|**int**|Número de cambios de contexto del programador realizados por este trabajador.|  
|pending_io_count|**int**|Número de E/S físicas realizadas por este trabajador.|  
|pending_io_byte_count|**bigint**|Número total de bytes de todas las E/S físicas pendientes de este trabajador.|  
|pending_io_byte_average|**int**|Número promedio de bytes de las E/S físicas de este trabajador.|  
|wait_started_ms_ticks|**bigint**|Punto en el tiempo, en [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), cuando este trabajador entró en estado suspendido. Si se resta este valor de ms_ticks en [Sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) , se devuelve el número de milisegundos que el trabajador ha estado esperando.|  
|wait_resumed_ms_ticks|**bigint**|Punto en el tiempo, en [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), cuando este trabajador entró en el estado RUNNABLE. Restar este valor de ms_ticks en [Sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) devuelve el número de milisegundos que el trabajador ha estado en la cola de ejecutables.|  
|task_bound_ms_ticks|**bigint**|Punto en el tiempo, en [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), cuando una tarea se enlaza a este trabajador.|  
|worker_created_ms_ticks|**bigint**|Punto en el tiempo, en [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), cuando se crea un trabajador.|  
|exception_num|**int**|Número de error de la última excepción que encontró este trabajador.|  
|exception_severity|**int**|Gravedad de la última excepción que encontró este trabajador.|  
|exception_address|**varbinary(8**|Dirección del código que produjo la excepción|  
|affinity|**bigint**|La afinidad de subprocesos del trabajador. Coincide con la afinidad del subproceso en [sys.dm_os_threads &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md).|  
|state|**nvarchar(60)**|Estado del trabajador. Puede ser uno de los siguientes valores:<br /><br /> INIT = El trabajador se está inicializando actualmente.<br /><br /> RUNNING = El trabajador está trabajando de forma preferente o no preferente.<br /><br /> RUNNABLE = El trabajador está preparado para ejecutarse en el programador.<br /><br /> SUSPENDED = El trabajador está suspendido actualmente, esperando por un evento para enviarle una señal.|  
|start_quantum|**bigint**|Tiempo en milisegundos al inicio de la ejecución actual de este trabajador.|  
|end_quantum|**bigint**|Tiempo en milisegundos al final de la ejecución actual de este trabajador.|  
|last_wait_type|**nvarchar(60)**|Tipo de la última espera. Para obtener una lista de tipos de espera, vea [sys.dm_os_wait_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|return_code|**int**|Valor devuelto de la última espera. Puede ser uno de los siguientes valores:<br /><br /> 0 =SUCCESS<br /><br /> 3 = DEADLOCK<br /><br /> 4 = PREMATURE_WAKEUP<br /><br /> 258 = TIMEOUT|  
|quantum_used|**bigint**|Solo para uso interno.|  
|max_quantum|**bigint**|Solo para uso interno.|  
|boost_count|**int**|Solo para uso interno.|  
|tasks_processed_count|**int**|Número de tareas procesadas por este trabajador.|  
|fiber_address|**varbinary(8**|Dirección de memoria de la fibra con la que está asociado este trabajador.<br /><br /> NULL = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no está configurado para agrupación ligera.|  
|task_address|**varbinary(8**|Dirección de memoria de la tarea actual. Para obtener más información, vea [sys.dm_os_tasks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md).|  
|memory_object_address|**varbinary(8**|Dirección de memoria del objeto de memoria del trabajador. Para obtener más información, vea [sys.dm_os_memory_objects &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).|  
|thread_address|**varbinary(8**|Dirección de memoria del subproceso asociado con este trabajador. Para obtener más información, vea [sys.dm_os_threads &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md).|  
|signal_worker_address|**varbinary(8**|Dirección de memoria del último trabajador que indicó este objeto. Para obtener más información, vea [Sys.dm_os_workers](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|scheduler_address|**varbinary(8**|Dirección de memoria del programador. Para obtener más información, vea [sys.dm_os_schedulers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|processor_group|**smallint**|Almacena el identificador de grupo de procesadores que está asignado a este subproceso.|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="remarks"></a>Observaciones  
 Si el estado de trabajador es RUNNING y se está ejecutando de forma no preferente, la dirección del trabajador coincide con active_worker_address en sys.dm_os_schedulers.  
  
 Cuando se señala a un trabajador que espera en un evento, el trabajador se coloca al principio de la cola de ejecutables. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permite que suceda esto 1.000 veces en una fila; después, el trabajador se coloca al final de la cola. Mover un trabajador al final de la cola tiene algunas implicaciones sobre el rendimiento.  
  
## <a name="permissions"></a>Permisos
En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] los niveles Premium, requiere el `VIEW DATABASE STATE` permiso en la base de datos. En [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] los niveles estándar y básico, requiere la  `Server Admin` pertenencia al rol o una `Azure Active Directory admin` cuenta.   

## <a name="examples"></a>Ejemplos  
 Puede utilizar la siguiente consulta para saber cuánto tiempo se ha estado ejecutando un trabajador en un estado SUSPENDED o RUNNABLE.  
  
```sql
SELECT   
    t1.session_id,  
    CONVERT(varchar(10), t1.status) AS status,  
    CONVERT(varchar(15), t1.command) AS command,  
    CONVERT(varchar(10), t2.state) AS worker_state,  
    w_suspended =   
      CASE t2.wait_started_ms_ticks  
        WHEN 0 THEN 0  
        ELSE   
          t3.ms_ticks - t2.wait_started_ms_ticks  
      END,  
    w_runnable =   
      CASE t2.wait_resumed_ms_ticks  
        WHEN 0 THEN 0  
        ELSE   
          t3.ms_ticks - t2.wait_resumed_ms_ticks  
      END  
  FROM sys.dm_exec_requests AS t1  
  INNER JOIN sys.dm_os_workers AS t2  
    ON t2.task_address = t1.task_address  
  CROSS JOIN sys.dm_os_sys_info AS t3  
  WHERE t1.scheduler_id IS NOT NULL;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
 session_id status     command         worker_state w_suspended w_runnable  
 ---------- ---------- --------------- ------------ ----------- --------------------  
 4          background LAZY WRITER     SUSPENDED    688         688  
 6          background LOCK MONITOR    SUSPENDED    4657        4657
 19         background BRKR TASK       SUSPENDED    603820344   603820344  
 14         background BRKR EVENT HNDL SUSPENDED    63583641    63583641  
 51         running    SELECT          RUNNING      0           0  
 2          background RESOURCE MONITO RUNNING      0           603825954  
 3          background LAZY WRITER     SUSPENDED    422         422  
 7          background SIGNAL HANDLER  SUSPENDED    603820485   603820485  
 13         background TASK MANAGER    SUSPENDED    603824704   603824704  
 18         background BRKR TASK       SUSPENDED    603820407   603820407  
 9          background TRACE QUEUE TAS SUSPENDED    454         454  
 52         suspended  SELECT          SUSPENDED    35094       35094  
 1          background RESOURCE MONITO RUNNING      0           603825954  
```

 En la salida, si `w_runnable` y `w_suspended` son iguales, indica el tiempo que el trabajador está en el estado SUSPENDED. De lo contrario, `w_runnable` representa el tiempo que ha pasado el trabajador en el estado RUNNABLE. En la salida, la sesión `52` está en estado `SUSPENDED` durante `35,094` milisegundos.  
  
## <a name="see-also"></a>Consulte también  
[SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)       
[Query Processing Architecture Guide](../../relational-databases/query-processing-architecture-guide.md#DOP)      (Guía de arquitectura de procesamiento de consultas)  
[Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md)    
