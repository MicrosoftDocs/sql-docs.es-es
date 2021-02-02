---
description: sys.dm_exec_query_memory_grants (Transact-SQL)
title: sys.dm_exec_query_memory_grants (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_query_memory_grants_TSQL
- sys.dm_exec_query_memory_grants
- sys.dm_exec_query_memory_grants_TSQL
- dm_exec_query_memory_grants
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_memory_grants dynamic management view
ms.assetid: 2c417747-2edd-4e0d-8a9c-e5f445985c1a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f61f84416e76e6a03c12efe7336a7d978585736c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235923"
---
# <a name="sysdm_exec_query_memory_grants-transact-sql"></a>sys.dm_exec_query_memory_grants (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información acerca de todas las consultas que han solicitado y están esperando una concesión de memoria o a los que se ha dado una concesión de memoria. Las consultas que no requieran una concesión de memoria no aparecerán en esta vista. Por ejemplo, las operaciones de ordenación y combinación hash tienen concesiones de memoria para la ejecución de la consulta, mientras que las consultas sin una cláusula **order by** no tendrán una concesión de memoria.  
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], las vistas de administración dinámica no pueden exponer información que impactaría a la contención de la base de datos ni acerca de otras bases de datos a las que el usuario tenga acceso. Para evitar exponer esta información, se filtran todas las filas que contienen datos que no pertenecen al inquilino conectado. Además, se filtran los valores de las columnas **scheduler_id**, **wait_order**, **pool_id** **group_id** . el valor de la columna se establece en NULL.  
  
> [!NOTE]  
> Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_exec_query_memory_grants**.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**session_id**|**smallint**|Id. (SPID) de la sesión en la que se está ejecutando esta consulta.|  
|**id_de_solicitud**|**int**|Id. de la solicitud. Es único en el contexto de la sesión.|  
|**scheduler_id**|**int**|Id. del programador que programa esta consulta.|  
|**DOP**|**smallint**|Grado de paralelismo de esta consulta.|  
|**request_time**|**datetime**|Fecha y hora a la que esta consulta solicitó la concesión de memoria.|  
|**grant_time**|**datetime**|Fecha y hora a la que se concedió la memoria para esta consulta. Es NULL si aún no se ha concedido la memoria.|  
|**requested_memory_kb**|**bigint**|Memoria solicitada total en kilobytes.|  
|**granted_memory_kb**|**bigint**|Memoria total realmente otorgada en kilobytes. Puede ser NULL si aún no se ha concedido la memoria. En una situación típica, este valor debe ser igual a **requested_memory_kb**. En la creación de índices, el servidor puede permitir memoria adicional a petición además de la memoria concedida inicialmente.|  
|**required_memory_kb**|**bigint**|Memoria mínima necesaria para ejecutar esta consulta en kilobytes. **requested_memory_kb** es igual o mayor que esta cantidad.|  
|**used_memory_kb**|**bigint**|Memoria física usada en este momento en kilobytes.|  
|**max_used_memory_kb**|**bigint**|Memoria física máxima usada hasta este momento en kilobytes.|  
|**query_cost**|**float**|Costo estimado de la consulta.|  
|**timeout_sec**|**int**|Tiempo de espera en segundos antes de que esta consulta abandone la solicitud de concesión de memoria.|  
|**resource_semaphore_id**|**smallint**|Identificador no único del semáforo de recursos al que está esperando esta consulta.<br /><br /> **Nota:** Este identificador es único en las versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anteriores a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] . Este cambio puede afectar a la solución de problemas de ejecución de consultas. Para obtener más información, vea la sección “Comentarios” más adelante en este tema.|  
|**queue_id**|**smallint**|Id. de la cola de espera en la que esta consulta espera las concesiones de memoria. Es NULL si ya se ha concedido la memoria.|  
|**wait_order**|**int**|Orden secuencial de las consultas en espera en el **queue_id** especificado. Este valor puede cambiar para una consulta determinada si otras consultas obtienen concesiones de memoria o agotan el tiempo de espera. Es NULL si ya se ha concedido la memoria.|  
|**is_next_candidate**|**bit**|Candidata para la siguiente concesión de memoria.<br /><br /> 1 = Sí<br /><br /> 0 = No<br /><br /> NULL = Ya se ha concedido la memoria.|  
|**wait_time_ms**|**bigint**|Tiempo de espera en milisegundos. Es NULL si ya se ha concedido la memoria.|  
|**plan_handle**|**varbinary (64)**|Identificador de este plan de consulta. Use **sys.dm_exec_query_plan** para extraer el plan XML real.|  
|**sql_handle**|**varbinary (64)**|Identificador del texto de [!INCLUDE[tsql](../../includes/tsql-md.md)] de esta consulta. Use **sys.dm_exec_sql_text** para obtener el texto de [!INCLUDE[tsql](../../includes/tsql-md.md)] real.|  
|**group_id**|**int**|Id. para el grupo de cargas de trabajo donde se está ejecutando la consulta.|  
|**pool_id**|**int**|Id. del grupo de recursos de servidor al que pertenece este grupo de cargas de trabajo.|  
|**is_small**|**tinyint**|Cuando se establece en 1, indica que esta concesión utiliza el semáforo de recursos pequeño. Cuando se establece en 0, indica que se utiliza un semáforo normal.|  
|**ideal_memory_kb**|**bigint**|Tamaño, en kilobytes (KB), de la concesión de memoria para ajustar todo en la memoria física. Está basado en la estimación de la cardinalidad.|  
|**pdw_node_id**|**int**|Identificador del nodo en el que se encuentra esta distribución.<br /><br /> **Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
|**reserved_worker_count**|**bigint**|Número de [subprocesos de trabajo](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling)reservados.<br /><br />**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |  
|**used_worker_count**|**bigint**|Número de [subprocesos de trabajo](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) usados en este momento.<br /><br />**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**max_used_worker_count**|**bigint**|Número máximo de [subprocesos de trabajo](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) usados hasta este momento.<br /><br />**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**reserved_node_bitmap**|**bigint**|Mapa de bits de nodos NUMA en los que se reservan los [subprocesos de trabajo](../../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling) .<br /><br />**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
  
## <a name="permissions"></a>Permisos  

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], requiere el permiso `VIEW DATABASE STATE` en la base de datos.   
   
## <a name="remarks"></a>Notas  
 Un escenario de depuración típico para un tiempo de espera de consulta puede tener el siguiente aspecto:  
  
-   Compruebe el estado de la memoria del sistema global con [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md), [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) y diversos contadores de rendimiento.  
  
-   Compruebe las reservas de memoria para la ejecución de consultas en **sys.dm_os_memory_clerks**, donde `type = 'MEMORYCLERK_SQLQERESERVATIONS'`.  
  
-   Busque las consultas en espera <sup>1</sup> para las concesiones mediante **Sys.dm_exec_query_memory_grants**.  
  
    ```sql  
    --Find all queries waiting in the memory queue  
    SELECT * FROM sys.dm_exec_query_memory_grants where grant_time is null  
    ```  
    
    <sup>1</sup> En este caso, el tipo de espera normalmente es RESOURCE_SEMAPHORE. Para obtener más información, vea [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md). 
  
-   Almacenar en caché las consultas con concesiones de memoria mediante [sys.dm_exec_cached_plans &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md) y [Sys.dm_exec_query_plan &#40;transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)  
  
    ```sql  
    -- retrieve every query plan from the plan cache  
    USE master;  
    GO  
    SELECT * FROM sys.dm_exec_cached_plans cp CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
    GO  
    ```  
  
-   Examine más las consultas que utilizan mucha memoria con [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).  
  
    ```sql  
    --Find top 5 queries by average CPU time  
    SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
      plan_handle, query_plan   
    FROM sys.dm_exec_query_stats AS qs  
    CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
    ORDER BY total_worker_time/execution_count DESC;  
    GO  
    ```  
  
-   Si sospecha que hay una consulta descontrolada, examine el plan de presentación en [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) y el texto del lote en [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 Las consultas que utilizan vistas de administración dinámica que incluyen `ORDER BY` o agregados pueden aumentar el consumo de memoria y, por tanto, contribuir al problema que están solucionando.  
  
 La característica del regulador de recursos permite que un administrador de bases de datos distribuya los recursos del servidor entre los grupos de recursos de servidor, hasta un máximo de 64 fondos. A partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , cada grupo se comporta como una pequeña instancia de servidor independiente y requiere 2 semáforos. El número de filas que se devuelven desde **Sys.dm_exec_query_resource_semaphores** puede ser hasta 20 veces mayor que las filas devueltas en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_exec_query_resource_semaphores &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-resource-semaphores-transact-sql.md)     
 [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)     
 [Funciones y vistas de administración dinámica relacionadas con la ejecución &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
 [Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md)   
  
