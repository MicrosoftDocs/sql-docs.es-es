---
title: Infraestructura de generación de perfiles de consultas | Microsoft Docs
description: Obtenga más información sobre cómo el Motor de base de datos de SQL Server accede a información del entorno de ejecución en planes de ejecución de consultas para comprender la carga de trabajo y cómo se controla el uso de recursos.
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query plans [SQL Server]
- execution plans [SQL Server]
- query profiling
- lightweight query profiling
- lightweight profiling
- lwp
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 39e32e8715c92a14faf68fd22464a50fed349997
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347693"
---
# <a name="query-profiling-infrastructure"></a>Infraestructura de generación de perfiles de consultas
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ofrece la posibilidad de acceder a información en tiempo de ejecución sobre los planes de ejecución de consultas. Una de las acciones más importantes cuando se produce un problema de rendimiento es obtener una descripción precisa de la carga de trabajo que se está ejecutando y de cómo se controla el uso de recursos. Por eso es importante el acceso al [plan de ejecución real](../../relational-databases/performance/display-an-actual-execution-plan.md).

Aunque la finalización de una consulta es un requisito previo para la disponibilidad de un plan de consulta real, las [estadísticas de consultas dinámicas](../../relational-databases/performance/live-query-statistics.md) pueden proporcionar información en tiempo real sobre el proceso de ejecución de las consultas a medida que los datos fluyen de un [operador de plan de consulta](../../relational-databases/showplan-logical-and-physical-operators-reference.md) a otro. El plan de consulta activa muestra el progreso general de las consultas, así como estadísticas de tiempo de ejecución de nivel de operador como el número de filas, las filas generadas, el tiempo transcurrido, el progreso del operador, etc. Puesto que estos datos están disponibles en tiempo real sin necesidad de esperar a la finalización de la consulta, estas estadísticas de ejecución resultan extremadamente útiles para depurar problemas de rendimiento de consultas, como consultas de larga ejecución y consultas que se ejecutan de forma indefinida y nunca terminan.

## <a name="the-standard-query-execution-statistics-profiling-infrastructure"></a>Infraestructura de generación de perfiles de estadísticas de ejecución de consultas estándar

La *infraestructura de generación de perfiles de estadísticas de ejecución de consultas*, o generación de perfiles estándar, debe habilitarse para recopilar información sobre los planes de ejecución, concretamente el recuento de filas y el uso de CPU y E/S. Los siguientes métodos de recopilación de información de planes de ejecución para una **sesión de destino** aprovechan la infraestructura de generación de perfiles estándar:

- [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md) 
- [SET STATISTICS PROFILE](../../t-sql/statements/set-statistics-profile-transact-sql.md)
- [Estadísticas de consultas activas](../../relational-databases/performance/live-query-statistics.md)

> [!NOTE]
> Al hacer clic en el botón *Incluir estadísticas de consultas activas* en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], se aprovecha la infraestructura de generación de perfiles estándar.    
> En versiones posteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si se habilita la [infraestructura de generación de perfiles ligera](#lwp), lo aprovechan las estadísticas de consultas activas en lugar de la generación de perfiles estándar cuando se ve a través del [Monitor de actividad](../../relational-databases/performance-monitor/activity-monitor.md) o se consulta directamente la DMV [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md). 

Los siguientes métodos de recopilación de información de planes de ejecución para **todas las sesiones** aprovechan la infraestructura de generación de perfiles estándar:

-  El evento extendido ***query_post_execution_showplan***. Para habilitar eventos extendidos, consulte [Monitor System Activity Using Extended Events](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md).  
- El evento de seguimiento **Showplan XML** de [Seguimiento de SQL](../../relational-databases/sql-trace/sql-trace.md) y [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md). Para obtener más información sobre este evento de seguimiento, vea [Showplan XML [clase de eventos]](../../relational-databases/event-classes/showplan-xml-event-class.md).

Cuando se ejecuta una sesión de eventos extendidos que usa el evento *query_post_execution_showplan*, la DMV [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md) también se rellena, lo que habilita las estadísticas de consulta dinámicas para todas las sesiones mediante [Monitor de actividad](../../relational-databases/performance-monitor/activity-monitor.md) o una consulta directa a la DMV. Para obtener más información, consulte [Live Query Statistics](../../relational-databases/performance/live-query-statistics.md).

## <a name="the-lightweight-query-execution-statistics-profiling-infrastructure"></a><a name="lwp"></a> Infraestructura de generación de perfiles de estadísticas de ejecución de consultas ligera

A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 y [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], se ha incorporado una nueva *infraestructura de generación de perfiles de estadísticas de ejecución de consultas ligera*, o **generación de perfiles ligera**. 

> [!NOTE]
> Concretamente, los procedimientos almacenados compilados de forma nativa no se admiten con la generación de perfiles ligera.  

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v1"></a>Infraestructura de generación de perfiles de estadísticas de ejecución de consultas ligera v1

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 hasta [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]). 
  
A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 y [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], se ha reducido la sobrecarga de rendimiento para recopilar información sobre los planes de ejecución gracias a la incorporación de la generación de perfiles ligera. A diferencia de la generación de perfiles estándar, la ligera no recopila información en tiempo de ejecución de CPU, aunque sigue recopilando la información de uso de E/S y de recuento de filas.

Además se ha incorporado un nuevo evento extendido ***query_thread_profile*** que aprovecha la generación de perfiles ligera. Este evento extendido expone estadísticas de ejecución por operador, lo que ofrece más información sobre el rendimiento de cada nodo y subproceso. Se puede configurar una sesión de ejemplo con este evento extendido como en el ejemplo siguiente:

```sql
CREATE EVENT SESSION [NodePerfStats] ON SERVER
ADD EVENT sqlserver.query_thread_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

> [!NOTE]
> Para más información sobre la sobrecarga de rendimiento del generación de perfiles de consulta, vea la entrada de blog [Developers Choice: Query progress - anytime, anywhere](/archive/blogs/sql_server_team/query-progress-anytime-anywhere) (Elección de los desarrolladores: progreso de la consulta, en cualquier momento y en cualquier lugar). 

Cuando se ejecuta una sesión de eventos extendidos que usa el evento *query_thread_profile*, la DMV [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md) también se rellena con la generación de perfiles ligera, lo que habilita las estadísticas de consulta dinámicas para todas las sesiones mediante [Monitor de actividad](../../relational-databases/performance-monitor/activity-monitor.md) o la consulta directa a la DMV.

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v2"></a>Infraestructura de generación de perfiles de estadísticas de ejecución de consultas ligera v2

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 hasta [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]). 

[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 incluye una versión revisada de la generación de perfiles ligera con sobrecarga mínima. La generación de perfiles ligera también puede habilitarse de forma global mediante la [marca de seguimiento 7412](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) para las versiones indicadas anteriormente en *Se aplica a*. Se ha incorporado una nueva DMF [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md) para devolver el plan de ejecución de consultas de las solicitudes en curso.

A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU3 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11, si la generación de perfiles ligera no está habilitada de forma global, se puede usar el nuevo argumento de [sugerencia de consulta USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint)**QUERY_PLAN_PROFILE** para habilitarla en el nivel de consulta y para cualquier sesión. Cuando una consulta que contiene esta nueva sugerencia finaliza, también se devuelve un nuevo evento extendido **_query_plan_profile_* que proporciona un archivo XML de plan de ejecución real similar al evento extendido query_post_execution_showplan*. 

> [!NOTE]
> El evento ampliado *query_plan_profile* también usa la generación de perfiles ligera, incluso si no se usa la sugerencia de consulta. 

Se puede configurar una sesión de ejemplo que use el evento ampliado *query_plan_profile*, como en el ejemplo siguiente:

```sql
CREATE EVENT SESSION [PerfStats_LWP_Plan] ON SERVER
ADD EVENT sqlserver.query_plan_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v3"></a>Infraestructura de generación de perfiles de estadísticas de ejecución de consultas ligera v3

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] incluyen una versión recién revisada de la generación de perfiles ligera que recopila información de recuento de filas para todas las ejecuciones. La generación de perfiles ligera está habilitada de forma predeterminada en [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], la marca de seguimiento 7412 no tiene ningún efecto. Se puede deshabilitar la generación de perfiles ligera en el nivel de base de datos mediante la [configuración de ámbito de base de datos](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) LIGHTWEIGHT_QUERY_PROFILING: `ALTER DATABASE SCOPED CONFIGURATION SET LIGHTWEIGHT_QUERY_PROFILING = OFF;`.

Se ha introducido una nueva DMF [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md) para devolver el equivalente del último plan de ejecución real conocido para la mayoría de las consultas, y se llama *últimas estadísticas de plan de consulta*. Las últimas estadísticas de plan de consulta se pueden habilitar en el nivel de base de datos mediante la [configuración de ámbito de base de datos](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) LAST_QUERY_PLAN_STATS: `ALTER DATABASE SCOPED CONFIGURATION SET LAST_QUERY_PLAN_STATS = ON;`.

Un nuevo evento extendido *query_post_execution_plan_profile* recopila el equivalente de un plan de ejecución real en función de la generación de perfiles ligera, a diferencia de *query_post_execution_showplan* que utiliza la generación de perfiles estándar. [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] también ofrece este evento a partir de CU14. Se puede configurar una sesión de ejemplo que use el evento ampliado *query_post_execution_plan_profile*, como en el ejemplo siguiente:

```sql
CREATE EVENT SESSION [PerfStats_LWP_All_Plans] ON SERVER
ADD EVENT sqlserver.query_post_execution_plan_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

#### <a name="example-1---extended-event-session-using-standard-profiling"></a>Ejemplo 1: Sesión de eventos extendidos mediante la generación de perfiles estándar

```sql
CREATE EVENT SESSION [QueryPlanOld] ON SERVER 
ADD EVENT sqlserver.query_post_execution_showplan(
    ACTION(sqlos.task_time, sqlserver.database_id, 
    sqlserver.database_name, sqlserver.query_hash_signed, 
    sqlserver.query_plan_hash_signed, sqlserver.sql_text))
ADD TARGET package0.event_file(SET filename = N'C:\Temp\QueryPlanStd.xel')
WITH (MAX_MEMORY=4096 KB, EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS, 
    MAX_DISPATCH_LATENCY=30 SECONDS, MAX_EVENT_SIZE=0 KB, 
    MEMORY_PARTITION_MODE=NONE, TRACK_CAUSALITY=OFF, STARTUP_STATE=OFF);
```

#### <a name="example-2---extended-event-session-using-lightweight-profiling"></a>Ejemplo 2: Sesión de eventos extendidos mediante la generación de perfiles ligera

```sql
CREATE EVENT SESSION [QueryPlanLWP] ON SERVER 
ADD EVENT sqlserver.query_post_execution_plan_profile(
    ACTION(sqlos.task_time, sqlserver.database_id, 
    sqlserver.database_name, sqlserver.query_hash_signed, 
    sqlserver.query_plan_hash_signed, sqlserver.sql_text))
ADD TARGET package0.event_file(SET filename=N'C:\Temp\QueryPlanLWP.xel')
WITH (MAX_MEMORY=4096 KB, EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS, 
    MAX_DISPATCH_LATENCY=30 SECONDS, MAX_EVENT_SIZE=0 KB, 
    MEMORY_PARTITION_MODE=NONE, TRACK_CAUSALITY=OFF, STARTUP_STATE=OFF);
```

## <a name="query-profiling-infrastruture-usage-guidance"></a>Instrucciones de uso de la infraestructura de generación de perfiles de consultas
En la tabla siguiente se resumen las acciones para habilitar la generación de perfiles estándar o la generación de perfiles ligera, tanto globalmente (en el nivel de servidor) como en una sola sesión. También incluye la versión más antigua para la que está disponible la acción. 

|Ámbito|Generación de perfiles estándar|Generación de perfiles ligera|
|---------------|---------------|---------------|
|Global|Sesión de xEvent con el XE `query_post_execution_showplan`; a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|Marca de seguimiento 7412; a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1|
|Global|Seguimiento de SQL y SQL Server Profiler con el evento de seguimiento `Showplan XML`, a partir de SQL Server 2000|Sesión de xEvent con el XE `query_thread_profile`; a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2|
|Global|-|Sesión de xEvent con el XE `query_post_execution_plan_profile`; a partir de CU14 de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|
|Sesión|Use `SET STATISTICS XML ON`, a partir de SQL Server 2000.|Use la sugerencia de consulta `QUERY_PLAN_PROFILE` con una sesión xEvent con el XE `query_plan_profile`, a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU3 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11.|
|Sesión|Use `SET STATISTICS PROFILE ON`, a partir de SQL Server 2000.|-|
|Sesión|Haga clic en el botón [Estadísticas de consultas activas](../../relational-databases/performance/live-query-statistics.md) en SSMS, a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2.|-|

## <a name="remarks"></a>Observaciones

> [!IMPORTANT]
> Debido a un posible análisis AV aleatorio al ejecutar un procedimiento almacenado de supervisión que haga referencia a [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md), asegúrese de que [KB 4078596](https://support.microsoft.com/help/4078596) esté instalado en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].

A partir de la generación de perfiles ligera v2 y su baja sobrecarga, cualquier servidor que aún no esté enlazado a CPU puede ejecutar la generación de perfiles ligera **continuamente** y permitir que los profesionales de bases de datos pulsen en cualquier ejecución activa en cualquier momento, por ejemplo mediante Monitor de actividad o directamente al consultar a `sys.dm_exec_query_profiles`, y obtengan el plan de consulta con estadísticas en tiempo de ejecución.

Para más información sobre la sobrecarga de rendimiento del generación de perfiles de consulta, vea la entrada de blog [Developers Choice: Query progress - anytime, anywhere](https://techcommunity.microsoft.com/t5/SQL-Server/Developers-Choice-Query-progress-anytime-anywhere/ba-p/385004) (Elección de los desarrolladores: progreso de la consulta, en cualquier momento y en cualquier lugar). 

> [!NOTE]
> Los eventos extendidos que aprovechan la generación de perfiles ligera usarán información de la generación de perfiles estándar en el caso de que la infraestructura correspondiente ya esté habilitada. Por ejemplo, se está ejecutando una sesión de evento extendido mediante `query_post_execution_showplan` y se inicia otra mediante `query_post_execution_plan_profile`. La segunda sesión seguirá utilizando la información de la generación de perfiles estándar.

> [!NOTE]
> En [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], la Generación de perfiles ligera está desactivada de forma predeterminada, pero se activa cuando se inicia un seguimiento de XEvent basado en `query_post_execution_plan_profile` y, después, se desactiva de nuevo cuando este seguimiento se detiene. Como consecuencia, si los seguimientos de XEvent basados en `query_post_execution_plan_profile` se inician y detienen con frecuencia en una instancia de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], se recomienda encarecidamente activar la Generación de perfiles ligera en el nivel global con la marca de seguimiento 7412, para evitar la sobrecarga derivada de una activación y desactivación constante. 

## <a name="see-also"></a>Consulte también  
 [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Herramientas de supervisión y optimización del rendimiento](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Abrir el Monitor de actividad &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Monitor de actividad](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [Supervisar la actividad del sistema mediante eventos extendidos](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)      
 [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
 [Marcas de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
 [Referencia de operadores lógicos y físicos del plan de presentación](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
 [Mostrar un plan de ejecución real](../../relational-databases/performance/display-an-actual-execution-plan.md)    
 [Estadísticas de consultas activas](../../relational-databases/performance/live-query-statistics.md)