---
description: sys.dm_exec_requests (Transact-SQL)
title: sys.dm_exec_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_requests_TSQL
- sys.dm_exec_requests
- dm_exec_requests_TSQL
- dm_exec_requests
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_requests dynamic management view
ms.assetid: 4161dc57-f3e7-4492-8972-8cfb77b29643
author: pmasl
ms.author: pelopes
ms.reviewer: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b3ff96f67611b41db3e1cf1e827ff2577305a24d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100342933"
---
# <a name="sysdm_exec_requests-transact-sql"></a>sys.dm_exec_requests (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve información sobre cada una de las solicitudes que se ejecutan en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información sobre las solicitudes, vea la [Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md).
   
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Identificador de la sesión con la que está relacionada esta solicitud. No admite valores NULL.|  
|request_id|**int**|Id. de la solicitud. Es único en el contexto de la sesión. No admite valores NULL.|  
|start_time|**datetime**|Marca de tiempo de la llegada de la solicitud. No admite valores NULL.|  
|status|**nvarchar(30)**|Estado de la solicitud. Este puede ser uno de los siguientes:<br /><br /> Información previa<br />En ejecución<br />Ejecutable<br />En espera<br />Suspended<br /><br /> No admite valores NULL.|  
|command|**nvarchar(32)**|Identifica el tipo de comando actual que se está procesando. Los tipos de comandos comunes incluyen lo siguiente:<br /><br /> SELECT<br />INSERT<br />UPDATE<br />Delete<br />BACKUP LOG<br />BACKUP DATABASE<br />DBCC<br />FOR<br /><br /> El texto de la solicitud se puede recuperar mediante sys.dm_exec_sql_text con el correspondiente sql_handle para la solicitud. Los procesos internos del sistema establecen el comando según el tipo de tarea que realizan. Las tareas pueden incluir las siguientes:<br /><br /> LOCK MONITOR<br />CHECKPOINTLAZY<br />WRITER<br /><br /> No admite valores NULL.|  
|sql_handle|**varbinary (64)**|Es un token que identifica de forma única el lote o el procedimiento almacenado del que forma parte la consulta. Acepta valores NULL.| 
|statement_start_offset|**int**|Indica, en bytes, empezando por 0, la posición inicial de la instrucción que se está ejecutando actualmente para el lote que se está ejecutando actualmente o el objeto almacenado. Se puede usar junto con `sql_handle` , `statement_end_offset` y la `sys.dm_exec_sql_text` función de administración dinámica para recuperar la instrucción que se está ejecutando actualmente para la solicitud. Acepta valores NULL.|  
|statement_end_offset|**int**|Indica, en bytes, empezando por 0, la posición final de la instrucción que se está ejecutando actualmente para el lote que se está ejecutando actualmente o el objeto almacenado. Se puede usar junto con `sql_handle` , `statement_start_offset` y la `sys.dm_exec_sql_text` función de administración dinámica para recuperar la instrucción que se está ejecutando actualmente para la solicitud. Acepta valores NULL.|  
|plan_handle|**varbinary (64)**|Es un token que identifica de forma única un plan de ejecución de consulta para un lote que se está ejecutando actualmente. Acepta valores NULL.|  
|database_id|**smallint**|Identificador de la base de datos en la que se ejecuta la solicitud. No admite valores NULL.|  
|user_id|**int**|Identificador del usuario que envió la solicitud. No admite valores NULL.|  
|connection_id|**uniqueidentifier**|Identificador de la conexión a la que ha llegado la solicitud. Acepta valores NULL.|  
|blocking_session_id|**smallint**|Id. de la sesión que bloquea la solicitud. Si esta columna es NULL o es igual a 0, la solicitud no está bloqueada o la información de sesión de la sesión de bloqueo no está disponible (o no se puede identificar).<br /><br /> -2 = El recurso de bloqueo es propiedad de una transacción distribuida huérfana.<br /><br /> -3 = El recurso de bloqueo es propiedad de una transacción de recuperación diferida.<br /><br /> -4 = No se pudo determinar el identificador de sesión del propietario del bloqueo temporal a causa de transiciones internas de estado de dicho bloqueo.|  
|wait_type|**nvarchar(60)**|Si la solicitud está actualmente bloqueada, esta columna devuelve el tipo de espera. Acepta valores NULL.<br /><br /> Para obtener información sobre los tipos de esperas, vea [sys.dm_os_wait_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|wait_time|**int**|Si la solicitud está actualmente bloqueada, esta columna devuelve la duración en milisegundos de la espera actual. No admite valores NULL.|  
|last_wait_type|**nvarchar(60)**|Si esta solicitud se ha bloqueado anteriormente, esta columna devuelve el tipo de la última espera. No admite valores NULL.|  
|wait_resource|**nvarchar(256)**|Si la solicitud está actualmente bloqueada, esta columna devuelve el recurso por el que está esperando la solicitud actualmente. No admite valores NULL.|  
|open_transaction_count|**int**|Número de transacciones abiertas para esta solicitud. No admite valores NULL.|  
|open_resultset_count|**int**|Número de conjuntos de resultados abiertos para esta solicitud. No admite valores NULL.|  
|transaction_id|**bigint**|Identificador de la transacción donde se ejecuta esta solicitud. No admite valores NULL.|  
|context_info|**varbinary(128)**|Valor CONTEXT_INFO de la sesión. Acepta valores NULL.|  
|percent_complete|**real**|Porcentaje de trabajo completado en los siguientes comandos:<br /><br /> ALTER INDEX REORGANIZE<br />Opción AUTO_SHRINK con ALTER DATABASE<br />BACKUP DATABASE<br />DBCC CHECKDB<br />DBCC CHECKFILEGROUP<br />DBCC CHECKTABLE<br />DBCC INDEXDEFRAG<br />DBCC SHRINKDATABASE<br />DBCC SHRINKFILE<br />RECOVERY<br />RESTORE DATABASE<br />ROLLBACK<br />TDE ENCRYPTION<br /><br /> No admite valores NULL.|  
|estimated_completion_time|**bigint**|Solo interno. No admite valores NULL.|  
|cpu_time|**int**|Tiempo de CPU en milisegundos utilizado por la solicitud. No admite valores NULL.|  
|total_elapsed_time|**int**|Tiempo total transcurrido en milisegundos desde que llegó la solicitud. No admite valores NULL.|  
|scheduler_id|**int**|Identificador del programador que programa esta solicitud. No admite valores NULL.|  
|task_address|**varbinary(8**|Dirección de memoria asignada a la tarea asociada con esta solicitud. Acepta valores NULL.|  
|Lecturas|**bigint**|Número de lecturas realizadas por esta solicitud. No admite valores NULL.|  
|Escrituras|**bigint**|Número de escrituras realizadas por esta solicitud. No admite valores NULL.|  
|logical_reads|**bigint**|Número de lecturas lógicas realizadas por la solicitud. No admite valores NULL.|  
|text_size|**int**|Valor de TEXTSIZE para esta solicitud. No admite valores NULL.|  
|language|**nvarchar(128)**|Configuración de idioma para esta solicitud. Acepta valores NULL.|  
|date_format|**nvarchar (3)**|Valor de DATEFORMAT para esta solicitud. Acepta valores NULL.|  
|date_first|**smallint**|Valor de DATEFIRST para esta solicitud. No admite valores NULL.|  
|quoted_identifier|**bit**|1 = El valor de QUOTED_IDENTIFIER es ON para la solicitud. De lo contrario, es 0.<br /><br /> No admite valores NULL.|  
|arithabort|**bit**|1 = El valor de ARITHABORT es ON para la solicitud. De lo contrario, es 0.<br /><br /> No admite valores NULL.|  
|ansi_null_dflt_on|**bit**|1 = El valor de ANSI_NULL_DFLT_ON es ON para la solicitud. De lo contrario, es 0.<br /><br /> No admite valores NULL.|  
|ansi_defaults|**bit**|1 = El valor de ANSI_DEFAULTS es ON para la solicitud. De lo contrario, es 0.<br /><br /> No admite valores NULL.|  
|ansi_warnings|**bit**|1 = El valor de ANSI_WARNINGS es ON para la solicitud. De lo contrario, es 0.<br /><br /> No admite valores NULL.|  
|ansi_padding|**bit**|1 = El valor de ANSI_PADDING es ON para la solicitud.<br /><br /> De lo contrario, es 0.<br /><br /> No admite valores NULL.|  
|ansi_nulls|**bit**|1 = El valor de ANSI_NULLS es ON para la solicitud. De lo contrario, es 0.<br /><br /> No admite valores NULL.|  
|concat_null_yields_null|**bit**|1 = El valor de CONCAT_NULL_YIELDS_NULL es ON para la solicitud. De lo contrario, es 0.<br /><br /> No admite valores NULL.|  
|transaction_isolation_level|**smallint**|Nivel de aislamiento con el que se creó la transacción para esta solicitud. No admite valores NULL.<br /> 0 = Unspecified<br /> 1 = ReadUncomitted<br /> 2 = ReadCommitted<br /> 3 = Repeatable<br /> 4 = Serializable<br /> 5 = Snapshot|  
|lock_timeout|**int**|Tiempo de espera de bloqueo en milisegundos para esta solicitud. No admite valores NULL.|  
|deadlock_priority|**int**|Valor de DEADLOCK_PRIORITY para la solicitud. No admite valores NULL.|  
|row_count|**bigint**|Número de filas que esta solicitud ha devuelto al cliente. No admite valores NULL.|  
|prev_error|**int**|Último error que se ha producido durante la ejecución de la solicitud. No admite valores NULL.|  
|nest_level|**int**|Nivel de anidamiento actual del código que se está ejecutando en la solicitud. No admite valores NULL.|  
|granted_query_memory|**int**|Número de páginas asignadas a la ejecución de una consulta en la solicitud. No admite valores NULL.|  
|executing_managed_code|**bit**|Indica si una solicitud concreta está ejecutando actualmente objetos Common Language Runtime, como rutinas, tipos y desencadenadores. Se establece para todo el tiempo que un objeto Common Language Runtime esté en la pila, incluso mientras se ejecuta [!INCLUDE[tsql](../../includes/tsql-md.md)] desde Common Language Runtime. No admite valores NULL.|  
|group_id|**int**|Identificador del grupo de cargas de trabajo al que pertenece esta consulta. No admite valores NULL.|  
|query_hash|**Binary(8**|Valor hash binario que se calcula en la consulta y que se usa para identificar consultas con una lógica similar. Puede usar el hash de consulta para determinar el uso de recursos agregados para las consultas que solo se diferencian en los valores literales.|  
|query_plan_hash|**Binary(8**|Valor hash binario que se calcula en el plan de ejecución de consulta y que se usa para identificar planes de ejecución de consulta similares. Puede usar el hash del plan de consulta para buscar el costo acumulativo de las consultas con planes de ejecución similares.|  
|statement_sql_handle|**varbinary (64)**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Identificador SQL de la consulta individual.<br /><br />Esta columna es NULL si Almacén de consultas no está habilitada para la base de datos. |  
|statement_context_id|**bigint**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Clave externa opcional que se va a sys.query_context_settings.<br /><br />Esta columna es NULL si Almacén de consultas no está habilitada para la base de datos. |  
|dop |**int** |**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> Grado de paralelismo de la consulta. |  
|parallel_worker_count |**int** |**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> El número de trabajadores paralelos reservados si se trata de una consulta en paralelo.  |  
|external_script_request_id |**uniqueidentifier** |**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> IDENTIFICADOR de solicitud de script externo asociado a la solicitud actual. |  
|is_resumable |**bit** |**Válido para** : [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] y versiones posteriores.<br /><br /> Indica si la solicitud es una operación de índice reanudable. |  
|page_resource |**Binary(8** |**Se aplica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]<br /><br /> Representación hexadecimal de 8 bytes del recurso de página si la `wait_resource` columna contiene una página. Para obtener más información, vea [Sys.fn_PageResCracker](../../relational-databases/system-functions/sys-fn-pagerescracker-transact-sql.md). |  
|page_server_reads|**bigint**|**Se aplica a**: hiperescala Azure SQL Database<br /><br /> Número de lecturas del servidor de páginas realizadas por esta solicitud. No admite valores NULL.|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="remarks"></a>Observaciones 
Para ejecutar código situado fuera de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (por ejemplo, en procedimientos almacenados extendidos y consultas distribuidas), se tiene que ejecutar un subproceso fuera del control del programador no preferente. Para hacerlo, un trabajador se cambia al modo preferente. Los valores de tiempo que devuelve esta vista de administración dinámica no incluyen el tiempo transcurrido en modo preferente.

Al ejecutar solicitudes paralelas en [modo de fila](../../relational-databases/query-processing-architecture-guide.md#row-mode-execution), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna un subproceso de trabajo para coordinar los subprocesos de trabajo responsables de completar las tareas asignadas. En esta DMV solo el subproceso de coordinador es visible para la solicitud. Las columnas **Read**, **writes**, **logical_reads** y **ROW_COUNT** **no se actualizan** para el subproceso de coordinador. Las columnas **wait_type**, **wait_time**, **last_wait_type**, **wait_resource** y **granted_query_memory** solo se **actualizan** para el subproceso de coordinador. Para más información, consulte la [guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md).

## <a name="permissions"></a>Permisos
Si el usuario tiene `VIEW SERVER STATE` permiso en el servidor, el usuario verá todas las sesiones en ejecución en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; de lo contrario, el usuario solo verá la sesión actual. `VIEW SERVER STATE` no se puede conceder en Azure SQL Database por lo que `sys.dm_exec_requests` siempre está limitado a la conexión actual.

En Always-On escenarios, si la réplica secundaria se establece en **solo intención de lectura**, la conexión a la secundaria debe especificar su intención de aplicación en los parámetros de la cadena de conexión agregando `applicationintent=readonly` . De lo contrario, la comprobación de acceso para `sys.dm_exec_requests` no pasará para las bases de datos del grupo de disponibilidad, aunque el `VIEW SERVER STATE` permiso esté presente.

  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-finding-the-query-text-for-a-running-batch"></a>A. Buscar el texto de consulta para un lote en ejecución

 En el siguiente ejemplo se consulta `sys.dm_exec_requests` para buscar la consulta de interés y copiar su `sql_handle` desde la salida.  

```sql
SELECT * FROM sys.dm_exec_requests;  
GO  
```  

Después, para obtener el texto de la instrucción, use el `sql_handle` copiado con la función del sistema `sys.dm_exec_sql_text(sql_handle)`.  

```sql
SELECT * FROM sys.dm_exec_sql_text(< copied sql_handle >);  
GO  
```

### <a name="b-finding-all-locks-that-a-running-batch-is-holding"></a>B. Buscar todos los bloqueos que mantiene un lote en ejecución

En el ejemplo siguiente se realiza una consulta **Sys.dm_exec_requests** para buscar el lote interesante y copiar su `transaction_id` desde la salida.

```sql
SELECT * FROM sys.dm_exec_requests;  
GO
```

A continuación, para buscar información de bloqueo, use el copiado `transaction_id` con la función del sistema **Sys.dm_tran_locks**.  

```sql
SELECT * FROM sys.dm_tran_locks
WHERE request_owner_type = N'TRANSACTION'
    AND request_owner_id = < copied transaction_id >;
GO  
```

### <a name="c-finding-all-currently-blocked-requests"></a>C. Buscar todas las solicitudes bloqueadas actualmente

En el ejemplo siguiente se consulta **Sys.dm_exec_requests** para buscar información acerca de las solicitudes bloqueadas.  

```sql
SELECT session_id ,status ,blocking_session_id  
    ,wait_type ,wait_time ,wait_resource
    ,transaction_id
FROM sys.dm_exec_requests
WHERE status = N'suspended';  
GO  
```  

### <a name="d-ordering-existing-requests-by-cpu"></a>D. Ordenación de las solicitudes existentes por CPU

```sql
SELECT 
   req.session_id
   , req.start_time
   , cpu_time 'cpu_time_ms'
   , object_name(st.objectid,st.dbid) 'ObjectName' 
   , substring
      (REPLACE
        (REPLACE
          (SUBSTRING
            (ST.text
            , (req.statement_start_offset/2) + 1
            , (
               (CASE statement_end_offset
                  WHEN -1
                  THEN DATALENGTH(ST.text)  
                  ELSE req.statement_end_offset
                  END
                    - req.statement_start_offset)/2) + 1)
       , CHAR(10), ' '), CHAR(13), ' '), 1, 512)  AS statement_text  
FROM sys.dm_exec_requests AS req  
   CROSS APPLY sys.dm_exec_sql_text(req.sql_handle) as ST
   ORDER BY cpu_time desc;
GO
```

## <a name="see-also"></a>Consulte también
[Funciones y vistas de administración dinámica](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
[Funciones y vistas de administración dinámica relacionadas con la ejecución](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)      
[sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)     
[sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)     
[sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)    
[sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)      
[SQL Server, objeto SQL Statistics](../../relational-databases/performance-monitor/sql-server-sql-statistics-object.md)     
[Query Processing Architecture Guide](../../relational-databases/query-processing-architecture-guide.md#DOP)      (Guía de arquitectura de procesamiento de consultas)  
[Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md)    
