---
description: sys.dm_os_tasks (Transact-SQL)
title: sys.dm_os_tasks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_tasks
- sys.dm_os_tasks_TSQL
- dm_os_tasks_TSQL
- dm_os_tasks
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_tasks dynamic management view
ms.assetid: 180a3c41-e71b-4670-819d-85ea7ef98bac
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bd21dd5cce8a794b6a0a909f62a8b223f274cb76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193780"
---
# <a name="sysdm_os_tasks-transact-sql"></a>sys.dm_os_tasks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada tarea activa en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Una tarea es la unidad básica de ejecución en SQL Server. Entre los ejemplos de tareas se incluyen una consulta, un inicio de sesión, un cierre de sesión y tareas del sistema como la actividad de limpieza fantasma, la actividad de punto de control, el escritor de registros y la actividad de puesta al día Para obtener más información acerca de las tareas, vea la [Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md).
  
> [!NOTE]  
> Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_tasks**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**task_address**|**varbinary(8**|Dirección de memoria del objeto.|  
|**task_state**|**nvarchar(60)**|Estado de la tarea. Este puede ser uno de los siguientes:<br /><br /> PENDING: esperando un subproceso de trabajo.<br /><br /> RUNNABLE: se puede ejecutar, pero está esperando a recibir un cuanto.<br /><br /> RUNNING: ejecutándose actualmente en el programador.<br /><br /> SUSPENDED: tiene un trabajador, pero está esperando un evento.<br /><br /> DONE: completado.<br /><br /> SPINLOOP: atrapado en un subproceso.|  
|**context_switches_count**|**int**|Número de cambios de contexto del programador que esta tarea ha completado.|  
|**pending_io_count**|**int**|Número de entradas y salidas físicas realizadas por esta tarea.|  
|**pending_io_byte_count**|**bigint**|Recuento total de bytes de las entradas y salidas realizadas por esta tarea.|  
|**pending_io_byte_average**|**int**|Recuento promedio de bytes de las entradas y salidas realizadas por esta tarea.|  
|**scheduler_id**|**int**|Id. del programador primario. Es un identificador de la información del programador para esta tarea. Para obtener más información, vea [sys.dm_os_schedulers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|**session_id**|**smallint**|Id. de la sesión que está asociada a la tarea.|  
|**exec_context_id**|**int**|Id. del contexto de ejecución que está asociado a la tarea.|  
|**id_de_solicitud**|**int**|Id. de la solicitud de la tarea. Para obtener más información, vea [sys.dm_exec_requests &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|**worker_address**|**varbinary(8**|Dirección de memoria del trabajador que ejecuta la tarea.<br /><br /> NULL = La tarea espera un trabajador que pueda ejecutarla o la tarea acaba de finalizar la ejecución.<br /><br /> Para obtener más información, vea [sys.dm_os_workers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|**host_address**|**varbinary(8**|Dirección de memoria del host.<br /><br /> 0 = No se ha usado el hospedaje para crear la tarea. Esto ayuda a identificar el host que se ha utilizado para crear esta tarea.<br /><br /> Para obtener más información, vea [sys.dm_os_hosts &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md).|  
|**parent_task_address**|**varbinary(8**|Dirección de memoria de la tarea que es el elemento primario del objeto.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos
En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="examples"></a>Ejemplos  
  
### <a name="a-monitoring-parallel-requests"></a>A. Supervisar solicitudes paralelas  
 En el caso de las solicitudes que se ejecutan en paralelo, verá varias filas para la misma combinación de ( \<**session_id**> , \<**request_id**> ). Utilice la siguiente consulta para encontrar la [opción de configuración del servidor grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) para todas las solicitudes activas.  
  
> [!NOTE]  
>  Un **request_id** es único dentro de una sesión.  
  
```sql  
SELECT  
    task_address,  
    task_state,  
    context_switches_count,  
    pending_io_count,  
    pending_io_byte_count,  
    pending_io_byte_average,  
    scheduler_id,  
    session_id,  
    exec_context_id,  
    request_id,  
    worker_address,  
    host_address  
  FROM sys.dm_os_tasks  
  ORDER BY session_id, request_id;  
```  
  
### <a name="b-associating-session-ids-with-windows-threads"></a>B. Asociar identificadores de sesión con subprocesos de Windows  
 Puede utilizar la siguiente consulta para asociar un valor de identificador de sesión a un identificador de subproceso de Windows. A continuación, puede supervisar el rendimiento del subproceso en el Monitor de rendimiento de Windows. La siguiente consulta no devuelve información para sesiones en espera.  
  
```sql  
SELECT STasks.session_id, SThreads.os_thread_id  
  FROM sys.dm_os_tasks AS STasks  
  INNER JOIN sys.dm_os_threads AS SThreads  
    ON STasks.worker_address = SThreads.worker_address  
  WHERE STasks.session_id IS NOT NULL  
  ORDER BY STasks.session_id;  
GO  
```  

## <a name="see-also"></a>Consulte también  
[SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)    
[Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md)     
  


