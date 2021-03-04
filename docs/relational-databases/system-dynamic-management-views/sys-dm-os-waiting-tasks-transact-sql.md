---
description: sys.dm_os_waiting_tasks (Transact-SQL)
title: sys.dm_os_waiting_tasks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_waiting_tasks
- sys.dm_os_waiting_tasks_TSQL
- dm_os_waiting_tasks_TSQL
- sys.dm_os_waiting_tasks
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_waiting_tasks dynamic management view
ms.assetid: ca5e6844-368c-42e2-b187-6e5f5afc8df3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b0e751d000219fd585288cf5765b520a89fcbc2a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837922"
---
# <a name="sysdm_os_waiting_tasks-transact-sql"></a>sys.dm_os_waiting_tasks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve información sobre la cola de tareas que están esperando en algún recurso. Para obtener más información acerca de las tareas, vea la [Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md).
   
> [!NOTE]  
> Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_waiting_tasks**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**waiting_task_address**|**varbinary(8**|Dirección de la tarea a la espera.|  
|**session_id**|**smallint**|Id. de la sesión asociada a la tarea.|  
|**exec_context_id**|**int**|Id. del contexto de ejecución asociado a la tarea.|  
|**wait_duration_ms**|**bigint**|Tiempo de espera total para este tipo de espera, en milisegundos. Esta hora es inclusiva de **signal_wait_time**.|  
|**wait_type**|**nvarchar(60)**|Nombre del tipo de espera.|  
|**resource_address**|**varbinary(8**|Dirección del recurso por el que la tarea está esperando.|  
|**blocking_task_address**|**varbinary(8**|Tarea que alberga actualmente este recurso.|  
|**blocking_session_id**|**smallint**|Id. de la sesión que bloquea la solicitud. Si esta columna es NULL, la solicitud no está bloqueada o la información de la sesión de bloqueo no está disponible (o no puede ser identificada).<br /><br /> -2 = El recurso de bloqueo es propiedad de una transacción distribuida huérfana.<br /><br /> -3 = El recurso de bloqueo es propiedad de una transacción de recuperación diferida.<br /><br /> -4 = No se pudo determinar el Id. de sesión del propietario del bloqueo temporal a causa de transiciones internas de estado del bloqueo temporal.|  
|**blocking_exec_context_id**|**int**|Id. del contexto de ejecución de la tarea de bloqueo.|  
|**resource_description**|**nvarchar (a.**|Descripción del recurso utilizado. Para obtener más información, vea la siguiente lista:|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="resource_description-column"></a>Columna resource_description  
 La columna resource_description tiene los siguientes posibles valores.  
  
 **Propietario del recurso de grupo de subprocesos:**  
  
-   ThreadPool ID = Scheduler\<hex-address>  
  
 **Propietario del recurso de consulta en paralelo:**  
  
-   exchangeEvent ID = {puerto | Pipe} \<hex-address> WaitType = \<exchange-wait-type> nodeId =\<exchange-node-id>  
  
 **Exchange-wait-type:**  
  
-   e_waitNone  
  
-   e_waitPipeNewRow  
  
-   e_waitPipeGetRow  
  
-   e_waitSynchronizeConsumerOpen  
  
-   e_waitPortOpen  
  
-   e_waitPortClose  
  
-   e_waitRange  
  
 **Propietario de recurso de bloqueo:**  
  
-   \<type-specific-description> ID = Lock \<lock-hex-address> Mode = \<mode> associatedObjectId =\<associated-obj-id>  
  
     **\<type-specific-description> puede ser:**  
  
    -   Para la base de datos: databaselock subresource = \<databaselock-subresource> DBID =\<db-id>  
  
    -   En FILE: FileLock fileid = \<file-id> subresource = \<filelock-subresource> DBID =\<db-id>  
  
    -   Para objeto: objectlock lockPartition = \<lock-partition-id> objid = \<obj-id> subresource = \<objectlock-subresource> DBID =\<db-id>  
  
    -   En PAGE: pagelock fileid = \<file-id> pageId = \<page-id> DBID = \<db-id> subresource =\<pagelock-subresource>  
  
    -   En Key: Keylock hobtid = \<hobt-id> DBID =\<db-id>  
  
    -   Para extent: extentlock fileid = \<file-id> pageId = \<page-id> DBID =\<db-id>  
  
    -   En RID: ridlock fileid = \<file-id> pageId = \<page-id> DBID =\<db-id>  
  
    -   Para la aplicación: applicationlock hash = \<hash> databasePrincipalId = \<role-id> DBID =\<db-id>  
  
    -   En el caso de metadatos: metadatalock subresource = \<metadata-subresource> ClassID = \<metadatalock-description> DBID =\<db-id>  
  
    -   Para HOBT: hobtlock hobtid = \<hobt-id> subresource = \<hobt-subresource> DBID =\<db-id>  
  
    -   Por ALLOCATION_UNIT: allocunitlock hobtid = \<hobt-id> subresource = \<alloc-unit-subresource> DBID =\<db-id>  
  
     **\<mode> puede ser:**  
  
     Sch-S, Sch-M, S, U, X, IS, IU, IX, SIU, SIX, UIX, BU, RangeS-S, RangeS-U, RangeI-N, RangeI-S, RangeI-U, RangeI-X, RangeX-, RangeX-U, RangeX-X  
  
 **Propietario de recurso externo:**  
  
-   ExternalResource externo =\<wait-type>  
  
 **Propietario de recurso genérico:**  
  
-   Área de trabajo de TransactionMutex TransactionInfo =\<workspace-id>  
  
-   Mutex  
  
-   CLRTaskJoin  
  
-   CLRMonitorEvent  
  
-   CLRRWLockEvent  
  
-   resourceWait  
  
 **Propietario de recurso de bloqueo temporal:**  
  
-   \<db-id>:\<file-id>:\<page-in-file>  
  
-   \<GUID>  
  
-   \<latch-class> (\<latch-address>)  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
 
## <a name="example"></a>Ejemplo
### <a name="a-identify-tasks-from-blocked-sessions"></a>A. Identificar tareas de sesiones bloqueadas. 

```sql
SELECT * FROM sys.dm_os_waiting_tasks 
WHERE blocking_session_id IS NOT NULL; 
```   

### <a name="b-view-waiting-tasks-per-connection"></a>B. Ver tareas en espera por conexión

```sql
SELECT st.text AS [SQL Text], c.connection_id, w.session_id, 
  w.wait_duration_ms, w.wait_type, w.resource_address, 
  w.blocking_session_id, w.resource_description, c.client_net_address, c.connect_time
FROM sys.dm_os_waiting_tasks AS w
INNER JOIN sys.dm_exec_connections AS c ON w.session_id = c.session_id 
CROSS APPLY (SELECT * FROM sys.dm_exec_sql_text(c.most_recent_sql_handle)) AS st 
              WHERE w.session_id > 50 AND w.wait_duration_ms > 0
ORDER BY c.connection_id, w.session_id
GO
```

### <a name="c-view-waiting-tasks-for-all-user-processes-with-additional-information"></a>C. Ver las tareas en espera para todos los procesos de usuario con información adicional

```sql
SELECT 'Waiting_tasks' AS [Information], owt.session_id,
    owt.wait_duration_ms, owt.wait_type, owt.blocking_session_id,
    owt.resource_description, es.program_name, est.text,
    est.dbid, eqp.query_plan, er.database_id, es.cpu_time,
    es.memory_usage*8 AS memory_usage_KB
FROM sys.dm_os_waiting_tasks owt
INNER JOIN sys.dm_exec_sessions es ON owt.session_id = es.session_id
INNER JOIN sys.dm_exec_requests er ON es.session_id = er.session_id
OUTER APPLY sys.dm_exec_sql_text (er.sql_handle) est
OUTER APPLY sys.dm_exec_query_plan (er.plan_handle) eqp
WHERE es.is_user_process = 1
ORDER BY owt.session_id;
GO
```
  
## <a name="see-also"></a>Consulte también  
[SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)      
[Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md)     
   
