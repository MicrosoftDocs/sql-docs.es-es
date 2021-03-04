---
description: sys.dm_os_latch_stats (Transact-SQL)
title: sys.dm_os_latch_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_latch_stats_TSQL
- dm_os_latch_stats_TSQL
- dm_os_latch_stats
- sys.dm_os_latch_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_latch_stats dynamic management view
ms.assetid: 2085d9fc-828c-453e-82ec-b54ed8347ae5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d1a8d34673b28e1da4fb15b0f469740aab378391
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839243"
---
# <a name="sysdm_os_latch_stats-transact-sql"></a>sys.dm_os_latch_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve información acerca de todas las esperas de bloqueos temporales organizadas por clase. 
  
> [!NOTE]  
> Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_latch_stats**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|latch_class|**nvarchar(60)**|Nombre de la clase de bloqueo temporal.|  
|waiting_requests_count|**bigint**|Número de esperas en bloqueos temporales en esta clase Este recuento se incrementa al inicio de una espera de bloqueo temporal.|  
|wait_time_ms|**bigint**|Tiempo total de espera, en milisegundos, en bloqueos temporales en esta clase<br /><br /> **Nota:** Esta columna se actualiza cada cinco minutos durante una espera de bloqueo temporal y al final de una espera de bloqueo temporal.|  
|max_wait_time_ms|**bigint**|Tiempo máximo que un objeto de memoria ha esperado en este bloqueo temporal. Si este valor es extraordinariamente alto, puede indicar un bloqueo interno.|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos  
En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="remarks"></a>Observaciones  
 sys.dm_os_latch_stats se puede utilizar para identificar el origen de la contención del bloqueo temporal examinando los tiempos y números de esperas relativos en las diferentes clases de bloqueos temporales. En algunas situaciones, se puede resolver o reducir la contención de bloqueos temporales. No obstante, puede haber situaciones que requerirán ponerse en contacto con los servicios de soporte al cliente de [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
Puede restablecer el contenido de sys.dm_os_latch_stats utilizando `DBCC SQLPERF` de la forma siguiente:  
  
```sql  
DBCC SQLPERF ('sys.dm_os_latch_stats', CLEAR);  
GO  
```  
  
 Esto restablece todos los recuentos en 0.  
  
> [!NOTE]  
>  Estas estadísticas no permanecen si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se reinicia. Todos los datos se acumulan desde la última vez que se restablecieron las estadísticas o desde que se inició [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="latches"></a><a name="latches"></a> Bloqueos temporales  
 Un bloqueo temporal es un objeto de sincronización ligero interno similar a un bloqueo, que se usa en varios [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] componentes. Un bloqueo temporal se usa principalmente para sincronizar páginas de base de datos durante operaciones como el acceso a archivos o búferes. Cada bloqueo temporal está asociado a una sola unidad de asignación. 
  
 Una espera de bloqueo temporal se produce cuando no se puede conceder una solicitud de bloqueo temporal inmediatamente, porque otro subproceso mantiene el bloqueo temporal en un modo de conflictos. A diferencia de los bloqueos, un bloqueo temporal se libera inmediatamente después de la operación, incluso en operaciones de escritura.  
  
 Los bloqueos temporales se agrupan en clases basándose en componentes y usos. En una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden existir cero o más bloqueos temporales de una determinada clase en cualquier momento.  
  
> [!NOTE]  
> `sys.dm_os_latch_stats` no realiza el seguimiento de solicitudes de bloqueos temporales que se concedieron inmediatamente o que tuvieron errores sin esperar.  
  
 En la siguiente tabla se ofrecen descripciones breves de las diversas clases de bloqueos temporales.  
  
|Clase de bloqueo temporal|Descripción|  
|-----------------|-----------------|  
|ALLOC_CREATE_RINGBUF|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo utiliza internamente para inicializar la sincronización de la creación de un búfer de anillo de asignación.|  
|ALLOC_CREATE_FREESPACE_CACHE|Se utiliza para inicializar la sincronización de cachés de espacio libre interno para montones.|  
|ALLOC_CACHE_MANAGER|Se utiliza para sincronizar pruebas de coherencia internas.|  
|ALLOC_FREESPACE_CACHE|Se utiliza para sincronizar el acceso a una caché de páginas con espacio disponible para montones y objetos binarios grandes (BLOB). La contención de bloqueos temporales de esta clase puede producirse cuando varias conexiones intentan insertar filas en un montón o BLOB simultáneamente. Puede reducir la contención si particiona el objeto. Cada partición tiene su propio bloqueo temporal. La creación de particiones distribuirá las inserciones en varios bloqueos temporales.|  
|ALLOC_EXTENT_CACHE|Se utiliza para sincronizar el acceso a una caché de extensiones que contiene páginas que no están asignadas. La contención de bloqueos temporales de esta clase puede producirse cuando varias conexiones intentan asignar páginas de datos en la misma unidad de asignación simultáneamente. Esta contención se puede reducir particionando el objeto de la que forma parte esta unidad de asignación.|  
|ACCESS_METHODS_DATASET_PARENT|Se utiliza para sincronizar el acceso de un conjunto de datos secundario al conjunto de datos primario durante operaciones en paralelo.|  
|ACCESS_METHODS_HOBT_FACTORY|Se utiliza para sincronizar el acceso a una tabla de hash interna.|  
|ACCESS_METHODS_HOBT|Se utiliza para sincronizar el acceso a la representación en memoria de entrada de un HoBt.|  
|ACCESS_METHODS_HOBT_COUNT|Se utiliza para sincronizar el acceso una página HoBt y recuentos de fila.|  
|ACCESS_METHODS_HOBT_VIRTUAL_ROOT|Se utiliza para sincronizar el acceso a la abstracción de la página raíz de un árbol b interno.|  
|ACCESS_METHODS_CACHE_ONLY_HOBT_ALLOC|Se utiliza para sincronizar el acceso a la tabla de trabajo.|  
|ACCESS_METHODS_BULK_ALLOC|Se utiliza para sincronizar el acceso en asignadores masivos.|  
|ACCESS_METHODS_SCAN_RANGE_GENERATOR|Se utiliza para sincronizar el acceso a un generador de intervalos durante búsquedas en paralelo.|  
|ACCESS_METHODS_KEY_RANGE_GENERATOR|Se utiliza para sincronizar el acceso a operaciones de lectura anticipada durante búsquedas en paralelo de intervalos de claves.|  
|APPEND_ONLY_STORAGE_INSERT_POINT|Se utiliza para sincronizar el acceso inserciones en unidades de almacenamiento solo de adición rápida.|  
|APPEND_ONLY_STORAGE_FIRST_ALLOC|Se utiliza para sincronizar la primera asignación de una unidad de almacenamiento solo de adición.|  
|APPEND_ONLY_STORAGE_UNIT_MANAGER|Se utiliza para sincronizar el acceso de estructuras de datos internas en el administrador de unidades de almacenamiento solo de adición rápida.|  
|APPEND_ONLY_STORAGE_MANAGER|Se utiliza para sincronizar operaciones de reducción en el administrador de unidades de almacenamiento solo de adición rápida.|  
|BACKUP_RESULT_SET|Se utiliza para sincronizar conjuntos de resultados de copia de seguridad en paralelo.|  
|BACKUP_TAPE_POOL|Se utiliza para sincronizar grupos de cintas de copia de seguridad.|  
|BACKUP_LOG_REDO|Se utiliza para sincronizar operaciones de rehacer el registro de copia de seguridad.|  
|BACKUP_INSTANCE_ID|Se utiliza para sincronizar la generación de identificadores de instancia de contadores de supervisión de rendimiento de copia de seguridad.|  
|BACKUP_MANAGER|Se utiliza para sincronizar el administrador de copia de seguridad interno.|  
|BACKUP_MANAGER_DIFFERENTIAL|Se utiliza para sincronizar operaciones de copia de seguridad diferencial con DBCC.|  
|BACKUP_OPERATION|Se utiliza para sincronizar estructuras de datos internas en una operación de copia de seguridad, como base de datos, registro o copia de seguridad de archivos.|  
|BACKUP_FILE_HANDLE|Se utiliza para sincronizar operaciones de apertura de archivos durante una operación de restauración.|  
|BUFFER|Se utiliza para sincronizar el acceso a corto plazo a páginas de la base de datos. Se requiere un bloqueo temporal de búfer antes de leer o modificar una página de la base de datos. La contención de bloqueos temporales de búfer puede indicar varios problemas, incluidas páginas activas y operaciones de E/S lentas.<br /><br /> Esta clase de bloqueo temporal cubre todos los usos posibles de bloqueos temporales de página. sys.dm_os_wait_stats crea una diferencia entre las esperas de bloqueos temporales de página causadas por operaciones de e/s y las operaciones de lectura y escritura en la página.|  
|BUFFER_POOL_GROW|Se utiliza para sincronizar el administrador del búfer interno durante operaciones de ampliación del grupo de búferes.|  
|DATABASE_CHECKPOINT|Se utiliza para serializar puntos de comprobación en una base de datos.|  
|CLR_PROCEDURE_HASHTABLE|Solo para uso interno.|  
|CLR_UDX_STORE|Solo para uso interno.|  
|CLR_DATAT_ACCESS|Solo para uso interno.|  
|CLR_XVAR_PROXY_LIST|Solo para uso interno.|  
|DBCC_CHECK_AGGREGATE|Solo para uso interno.|  
|DBCC_CHECK_RESULTSET|Solo para uso interno.|  
|DBCC_CHECK_TABLE|Solo para uso interno.|  
|DBCC_CHECK_TABLE_INIT|Solo para uso interno.|  
|DBCC_CHECK_TRACE_LIST|Solo para uso interno.|  
|DBCC_FILE_CHECK_OBJECT|Solo para uso interno.|  
|DBCC_PERF|Se utiliza para sincronizar contadores de supervisión de rendimiento internos.|  
|DBCC_PFS_STATUS|Solo para uso interno.|  
|DBCC_OBJECT_METADATA|Solo para uso interno.|  
|DBCC_HASH_DLL|Solo para uso interno.|  
|EVENTING_CACHE|Solo para uso interno.|  
|FCB|Se utiliza para sincronizar el acceso a un bloque de control de archivos.|  
|FCB_REPLICA|Solo para uso interno.|  
|FGCB_ALLOC|Se utiliza para sincronizar el acceso a información de asignación por turnos en un grupo de archivos.|  
|FGCB_ADD_REMOVE|Se usa para sincronizar el acceso a los grupos de archivos para las operaciones de archivo agregar, quitar, aumentar y reducir.|  
|FILEGROUP_MANAGER|Solo para uso interno.|  
|FILE_MANAGER|Solo para uso interno.|  
|FILESTREAM_FCB|Solo para uso interno.|  
|FILESTREAM_FILE_MANAGER|Solo para uso interno.|  
|FILESTREAM_GHOST_FILES|Solo para uso interno.|  
|FILESTREAM_DFS_ROOT|Solo para uso interno.|  
|LOG_MANAGER|Solo para uso interno.|  
|FULLTEXT_DOCUMENT_ID|Solo para uso interno.|  
|FULLTEXT_DOCUMENT_ID_TRANSACTION|Solo para uso interno.|  
|FULLTEXT_DOCUMENT_ID_NOTIFY|Solo para uso interno.|  
|FULLTEXT_LOGS|Solo para uso interno.|  
|FULLTEXT_CRAWL_LOG|Solo para uso interno.|  
|FULLTEXT_ADMIN|Solo para uso interno.|  
|FULLTEXT_AMDIN_COMMAND_CACHE|Solo para uso interno.|  
|FULLTEXT_LANGUAGE_TABLE|Solo para uso interno.|  
|FULLTEXT_CRAWL_DM_LIST|Solo para uso interno.|  
|FULLTEXT_CRAWL_CATALOG|Solo para uso interno.|  
|FULLTEXT_FILE_MANAGER|Solo para uso interno.|  
|DATABASE_MIRRORING_REDO|Solo para uso interno.|  
|DATABASE_MIRRORING_SERVER|Solo para uso interno.|  
|DATABASE_MIRRORING_CONNECTION|Solo para uso interno.|  
|DATABASE_MIRRORING_STREAM|Solo para uso interno.|  
|QUERY_OPTIMIZER_VD_MANAGER|Solo para uso interno.|  
|QUERY_OPTIMIZER_ID_MANAGER|Solo para uso interno.|  
|QUERY_OPTIMIZER_VIEW_REP|Solo para uso interno.|  
|RECOVERY_BAD_PAGE_TABLE|Solo para uso interno.|  
|RECOVERY_MANAGER|Solo para uso interno.|  
|SECURITY_OPERATION_RULE_TABLE|Solo para uso interno.|  
|SECURITY_OBJPERM_CACHE|Solo para uso interno.|  
|SECURITY_CRYPTO|Solo para uso interno.|  
|SECURITY_KEY_RING|Solo para uso interno.|  
|SECURITY_KEY_LIST|Solo para uso interno.|  
|SERVICE_BROKER_CONNECTION_RECEIVE|Solo para uso interno.|  
|SERVICE_BROKER_TRANSMISSION|Solo para uso interno.|  
|SERVICE_BROKER_TRANSMISSION_UPDATE|Solo para uso interno.|  
|SERVICE_BROKER_TRANSMISSION_STATE|Solo para uso interno.|  
|SERVICE_BROKER_TRANSMISSION_ERRORS|Solo para uso interno.|  
|SSBXmitWork|Solo para uso interno.|  
|SERVICE_BROKER_MESSAGE_TRANSMISSION|Solo para uso interno.|  
|SERVICE_BROKER_MAP_MANAGER|Solo para uso interno.|  
|SERVICE_BROKER_HOST_NAME|Solo para uso interno.|  
|SERVICE_BROKER_READ_CACHE|Solo para uso interno.|  
|SERVICE_BROKER_WAITFOR_MANAGER| Se utiliza para sincronizar un mapa de nivel de instancia de colas de espera. Existe una cola por el identificador de base de datos, la versión de base de datos y la tupla de ID. de cola. La contención en bloqueos temporales de esta clase puede producirse cuando hay muchas conexiones: en un estado de espera de WAITFOR (RECEIVE); llamar a WAITFOR (RECEIVE); supera el tiempo de espera de WAITFOR; recibir un mensaje; confirmar o revertir la transacción que contiene WAITFOR (RECEIVE); Puede reducir la contención reduciendo el número de subprocesos en un estado de espera de WAITFOR (RECEIVE). |  
|SERVICE_BROKER_WAITFOR_TRANSACTION_DATA|Solo para uso interno.|  
|SERVICE_BROKER_TRANSMISSION_TRANSACTION_DATA|Solo para uso interno.|  
|SERVICE_BROKER_TRANSPORT|Solo para uso interno.|  
|SERVICE_BROKER_MIRROR_ROUTE|Solo para uso interno.|  
|TRACE_ID|Solo para uso interno.|  
|TRACE_AUDIT_ID|Solo para uso interno.|  
|TRACE|Solo para uso interno.|  
|TRACE_CONTROLLER|Solo para uso interno.|  
|TRACE_EVENT_QUEUE|Solo para uso interno.|  
|TRANSACTION_DISTRIBUTED_MARK|Solo para uso interno.|  
|TRANSACTION_OUTCOME|Solo para uso interno.|  
|NESTING_TRANSACTION_READONLY|Solo para uso interno.|  
|NESTING_TRANSACTION_FULL|Solo para uso interno.|  
|MSQL_TRANSACTION_MANAGER|Solo para uso interno.|  
|DATABASE_AUTONAME_MANAGER|Solo para uso interno.|  
|UTILITY_DYNAMIC_VECTOR|Solo para uso interno.|  
|UTILITY_SPARSE_BITMAP|Solo para uso interno.|  
|UTILITY_DATABASE_DROP|Solo para uso interno.|  
|UTILITY_DYNAMIC_MANAGER_VIEW|Solo para uso interno.|  
|UTILITY_DEBUG_FILESTREAM|Solo para uso interno.|  
|UTILITY_LOCK_INFORMATION|Solo para uso interno.|  
|VERSIONING_TRANSACTION|Solo para uso interno.|  
|VERSIONING_TRANSACTION_LIST|Solo para uso interno.|  
|VERSIONING_TRANSACTION_CHAIN|Solo para uso interno.|  
|VERSIONING_STATE|Solo para uso interno.|  
|VERSIONING_STATE_CHANGE|Solo para uso interno.|  
|KTM_VIRTUAL_CLOCK|Solo para uso interno.|  
  
## <a name="see-also"></a>Consulte también  
[DBCC SQLPERF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)       
[SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)       
[Latches (objeto de SQL Server)](../../relational-databases/performance-monitor/sql-server-latches-object.md)