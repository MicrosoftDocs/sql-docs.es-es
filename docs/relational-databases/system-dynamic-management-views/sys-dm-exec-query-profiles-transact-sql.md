---
description: sys.dm_exec_query_profiles (Transact-SQL)
title: sys.dm_exec_query_profiles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_query_profiles_TSQL
- sys.dm_exec_query_profiles_TSQL
- dm_exec_query_profiles
- sys.dm_exec_query_profiles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_profiles dynamic management view
ms.assetid: 54efc6cb-eea8-4f6d-a4d0-aa05eeb54081
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ac678e88eaa21a8bb0f074e1bd899108b390bd49
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342958"
---
# <a name="sysdm_exec_query_profiles-transact-sql"></a>sys.dm_exec_query_profiles (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

Supervisa el progreso de la consulta en tiempo real mientras la consulta está en ejecución. Por ejemplo, use esta DMV para determinar qué parte de la consulta se está ejecutando con lentitud. Combine esta DMV con otras DMV del sistema mediante las columnas identificadas en el campo de descripción. O bien, combine esta DMV con otros contadores de rendimiento (como el Monitor de rendimiento, xperf) mediante las columnas de marca de tiempo.  
  
## <a name="table-returned"></a>Tabla devuelta  
Los contadores devueltos son por operador y por subproceso. Los resultados son dinámicos y no coinciden con los resultados de las opciones existentes, como, por ejemplo, las `SET STATISTICS XML ON` que solo crean los resultados cuando finaliza la consulta.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Identifica la sesión en la que se ejecuta esta consulta. Hace referencia a dm_exec_sessions.session_id.|  
|request_id|**int**|Identifica la solicitud de destino. Hace referencia a dm_exec_sessions.request_id.|  
|sql_handle|**varbinary (64)**|Es un token que identifica de forma única el lote o el procedimiento almacenado del que forma parte la consulta. Hace referencia a dm_exec_query_stats.sql_handle.|  
|plan_handle|**varbinary (64)**|Es un token que identifica de forma única un plan de ejecución de consulta para un lote que se ha ejecutado y su plan reside en la caché del plan, o se está ejecutando actualmente. Hace referencia a dm_exec_query_stats. plan_handle.|  
|physical_operator_name|**nvarchar(256)**|Nombre del operador físico.|  
|node_id|**int**|Identifica un nodo de operador en el árbol de consulta.|  
|thread_id|**int**|Distingue los subprocesos (para una consulta en paralelo) que pertenecen al mismo nodo de operador de consulta.|  
|task_address|**varbinary(8**|Identifica la tarea de SQLOS que está utilizando este subproceso. Hace referencia a dm_os_tasks.task_address.|  
|row_count|**bigint**|Número de filas que ha devuelto hasta ahora el operador.|  
|rewind_count|**bigint**|Número de rebobinados hasta ahora.|  
|rebind_count|**bigint**|Número de reenlaces hasta ahora.|  
|end_of_scan_count|**bigint**|Número de finales de examen hasta ahora.|  
|estimate_row_count|**bigint**|Número de filas estimado. Puede ser útil comparar estimated_row_count con el row_count real.|  
|first_active_time|**bigint**|Hora, en milisegundos, a la que se llamó por primera vez al operador.|  
|last_active_time|**bigint**|Hora, en milisegundos, a la que se llamó por última vez al operador.|  
|open_time|**bigint**|Marca de tiempo al abrir (en milisegundos).|  
|first_row_time|**bigint**|Marca de tiempo en la que se abrió la primera fila (en milisegundos).|  
|last_row_time|**bigint**|Marca de tiempo en la que se abrió la última fila (en milisegundos).|  
|close_time|**bigint**|Marca de tiempo al cerrar (en milisegundos).|  
|elapsed_time_ms|**bigint**|Tiempo total transcurrido (en milisegundos) utilizado por las operaciones del nodo de destino hasta ahora.|  
|cpu_time_ms|**bigint**|Tiempo total de CPU (en milisegundos) utilizado por las operaciones del nodo de destino hasta ahora.|  
|database_id|**smallint**|Identificador de la base de datos que contiene el objeto en el que se efectúan las lecturas y escrituras.|  
|object_id|**int**|El identificador para el objeto en el que se efectúan las lecturas y escrituras. Hace referencia a sys.objects.object_id.|  
|index_id|**int**|El índice (si existe) en el que se abre el conjunto de filas.|  
|scan_count|**bigint**|Número de exámenes de índice o tabla hasta ahora.|  
|logical_read_count|**bigint**|Número de lecturas lógicas hasta ahora.|  
|physical_read_count|**bigint**|Número de lecturas físicas hasta ahora.|  
|read_ahead_count|**bigint**|Número de lecturas anticipadas hasta ahora.|  
|write_page_count|**bigint**|Número de escrituras en páginas hasta ahora debido al rebosamiento.|  
|lob_logical_read_count|**bigint**|Número de lecturas lógicas LOB hasta ahora.|  
|lob_physical_read_count|**bigint**|Número de lecturas físicas LOB hasta ahora.|  
|lob_read_ahead_count|**bigint**|Número de lecturas anticipadas LOB hasta ahora.|  
|segment_read_count|**int**|Número de lecturas anticipadas de segmento hasta ahora.|  
|segment_skip_count|**int**|Número de segmentos omitidos hasta ahora.| 
|actual_read_row_count|**bigint**|Número de filas leídas por un operador antes de que se aplicara el predicado residual.| 
|estimated_read_row_count|**bigint**|**Se aplica a:** A partir de [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] SP1. <br/>Número de filas que un operador debe leer antes de que se aplicara el predicado residual.|  
  
## <a name="general-remarks"></a>Notas generales  
 Si el nodo del plan de consulta no tiene ninguna e/s, todos los contadores relacionados con la e/s se establecen en NULL.  
  
 Los contadores relacionados con e/s indicados por esta DMV son más granulares que los indicados por `SET STATISTICS IO` de las dos maneras siguientes:  
  
-   `SET STATISTICS IO` agrupa los contadores para todas las operaciones de e/s en una tabla determinada. Con esta DMV obtendrá contadores independientes para cada nodo del plan de consulta que realiza la e/s a la tabla.  
  
-   Si se realizaran búsquedas en paralelo, esta DMV informa sobre los contadores para cada uno de los subprocesos paralelos que se ejecutan en la búsqueda.
 
A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, la *infraestructura de generación de perfiles de estadísticas de ejecución de consultas estándar* existe en paralelo con una *infraestructura ligera de generación de perfiles de estadísticas de ejecución de consultas*. `SET STATISTICS XML ON` y `SET STATISTICS PROFILE ON` usan siempre la *infraestructura de generación de perfiles de estadísticas de ejecución de consultas estándar*. Para `sys.dm_exec_query_profiles` que se rellene, una de las infraestructuras de generación de perfiles de consulta debe estar habilitada. Para obtener más información, vea [Infraestructura de generación de perfiles de consultas](../../relational-databases/performance/query-profiling-infrastructure.md).    

>[!NOTE]
> La consulta en investigación tiene que iniciarse **después** de que se haya habilitado la infraestructura de generación de perfiles de consulta, lo que lo habilitará después de iniciar la consulta no producirá resultados en `sys.dm_exec_query_profiles` . Para obtener más información sobre cómo habilitar las infraestructuras de generación de perfiles de consulta, consulte la [infraestructura de generación de perfiles de consulta](../../relational-databases/performance/query-profiling-infrastructure.md).

## <a name="permissions"></a>Permisos  
En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] y Azure SQL instancia administrada, requiere `VIEW DATABASE STATE` el permiso y la pertenencia al rol de la `db_owner` base de datos.   
En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] los niveles Premium, requiere el `VIEW DATABASE STATE` permiso en la base de datos. En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] SQL Database los objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
   
## <a name="examples"></a>Ejemplos  
 Paso 1: inicie sesión en una sesión en la que vaya a ejecutar la consulta que va a analizar `sys.dm_exec_query_profiles` . Para configurar la consulta para el uso de la generación de perfiles `SET STATISTICS PROFILE ON` . Ejecute la consulta en esta misma sesión.  
  
```sql  
--Configure query for profiling with sys.dm_exec_query_profiles  
SET STATISTICS PROFILE ON;  
GO  

--Or enable query profiling globally under SQL Server 2016 SP1 or above (not needed in SQL Server 2019)  
DBCC TRACEON (7412, -1);  
GO 
  
--Next, run your query in this session, or in any other session if query profiling has been enabled globally 
```  
  
 Paso 2: iniciar sesión en una segunda sesión diferente de la sesión en la que se ejecuta la consulta.  
  
 La siguiente instrucción resume el progreso que ha realizado la consulta que se ejecutaba de forma simultánea en la sesión 54. Para ello, calcula el número total de filas resultantes de todos los subprocesos para cada nodo y lo compara con el número estimado de filas resultantes para ese nodo.  
  
```sql  
--Run this in a different session than the session in which your query is running. 
--Note that you may need to change session id 54 below with the session id you want to monitor.
SELECT node_id,physical_operator_name, SUM(row_count) row_count, 
  SUM(estimate_row_count) AS estimate_row_count, 
  CAST(SUM(row_count)*100 AS float)/SUM(estimate_row_count)  
FROM sys.dm_exec_query_profiles   
WHERE session_id=54
GROUP BY node_id,physical_operator_name  
ORDER BY node_id;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con ejecuciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
 
