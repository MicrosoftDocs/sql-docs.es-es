---
description: sys.query_store_runtime_stats (Transact-SQL)
title: sys.query_store_runtime_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.QUERY_STORE_RUNTIME_STATS_TSQL
- QUERY_STORE_RUNTIME_STATS_TSQL
- SYS.QUERY_STORE_RUNTIME_STATS
- QUERY_STORE_RUNTIME_STATS
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_runtime_stats catalog view
- sys.query_store_runtime_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 54bb057d2e53c38d7a37dbc0d9200513748eb8af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161404"
---
# <a name="sysquery_store_runtime_stats-transact-sql"></a>sys.query_store_runtime_stats (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contiene información sobre la información de estadísticas de ejecución en tiempo de ejecución de la consulta.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**runtime_stats_id**|**bigint**|Identificador de la fila que representa las estadísticas de ejecución en tiempo de ejecución para el **plan_id**, **execution_type** y **runtime_stats_interval_id**. Solo es único para los intervalos de estadísticas de tiempo de ejecución anteriores. Para el intervalo actualmente activo, puede haber varias filas que representen estadísticas de tiempo de ejecución para el plan al que hace referencia **plan_id**, con el tipo de ejecución representado por **execution_type**. Normalmente, una fila representa las estadísticas de tiempo de ejecución que se vacían en el disco, mientras que otras representan el estado en memoria. Por lo tanto, para obtener el estado real de cada intervalo, debe agregar métricas, agrupación por **plan_id**, **execution_type** y **runtime_stats_interval_id**.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**plan_id**|**bigint**|Clave externa. Combina con [sys.query_store_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md).|  
|**runtime_stats_interval_id**|**bigint**|Clave externa. Combina con [sys.query_store_runtime_stats_interval &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md).|  
|**execution_type**|**tinyint**|Determina el tipo de ejecución de la consulta:<br /><br /> 0: ejecución normal (finalizada correctamente)<br /><br /> 3-ejecución de la anulada iniciada por el cliente<br /><br /> 4-ejecución de excepción anulada|  
|**execution_type_desc**|**nvarchar(128)**|Descripción textual del campo de tipo de ejecución:<br /><br /> 0: normal<br /><br /> 3-anulado<br /><br /> 4: excepción|  
|**first_execution_time**|**datetimeoffset**|Hora de la primera ejecución del plan de consulta dentro del intervalo de agregación. Esto hace referencia a la hora de finalización de la ejecución de la consulta.|  
|**last_execution_time**|**datetimeoffset**|Hora de la última ejecución del plan de consulta dentro del intervalo de agregación. Esto hace referencia a la hora de finalización de la ejecución de la consulta.|  
|**count_executions**|**bigint**|Recuento total de ejecuciones del plan de consulta dentro del intervalo de agregación.|  
|**avg_duration**|**float**|Duración media del plan de consulta dentro del intervalo de agregación (en microsegundos).|  
|**last_duration**|**bigint**|Última duración del plan de consulta dentro del intervalo de agregación (en microsegundos).|  
|**min_duration**|**bigint**|Duración mínima del plan de consulta dentro del intervalo de agregación (en microsegundos).|  
|**max_duration**|**bigint**|Duración máxima del plan de consulta dentro del intervalo de agregación (en microsegundos).|  
|**stdev_duration**|**float**|Desviación estándar de la duración del plan de consulta dentro del intervalo de agregación (en microsegundos).|  
|**avg_cpu_time**|**float**|Promedio de tiempo de CPU para el plan de consulta dentro del intervalo de agregación (en microsegundos).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**last_cpu_time**|**bigint**|Último tiempo de CPU para el plan de consulta dentro del intervalo de agregación (en microsegundos).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**min_cpu_time**|**bigint**|Tiempo mínimo de CPU para el plan de consulta dentro del intervalo de agregación (en microsegundos).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**max_cpu_time**|**bigint**|Tiempo máximo de CPU para el plan de consulta dentro del intervalo de agregación (en microsegundos).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**stdev_cpu_time**|**float**|Desviación estándar de tiempo de CPU para el plan de consulta dentro del intervalo de agregación (en microsegundos).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**avg_logical_io_reads**|**float**|Número promedio de lecturas de e/s lógicas para el plan de consulta dentro del intervalo de agregación. (expresado como un número de 8 KB de lectura).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**last_logical_io_reads**|**bigint**|Último número de lecturas de e/s lógicas para el plan de consulta dentro del intervalo de agregación. (expresado como un número de 8 KB de lectura).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**min_logical_io_reads**|**bigint**|Número mínimo de lecturas de e/s lógicas para el plan de consulta dentro del intervalo de agregación. (expresado como un número de 8 KB de lectura).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**max_logical_io_reads**|**bigint**|Número máximo de lecturas de e/s lógicas para el plan de consulta dentro del intervalo de agregación. (expresado como un número de 8 KB de lectura).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**stdev_logical_io_reads**|**float**|El número de e/s lógicas lee la desviación estándar del plan de consulta dentro del intervalo de agregación. (expresado como un número de 8 KB de lectura).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**avg_logical_io_writes**|**float**|Número promedio de escrituras de e/s lógicas para el plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**last_logical_io_writes**|**bigint**|Último número de escrituras de e/s lógicas para el plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**min_logical_io_writes**|**bigint**|Número mínimo de escrituras de e/s lógicas para el plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**max_logical_io_writes**|**bigint**|Número máximo de escrituras de e/s lógicas para el plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**stdev_logical_io_writes**|**float**|El número de operaciones de e/s lógicas escribe la desviación estándar del plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**avg_physical_io_reads**|**float**|Número promedio de lecturas de e/s físicas para el plan de consulta dentro del intervalo de agregación (expresada como un número de 8 KB de lectura).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**last_physical_io_reads**|**bigint**|Último número de lecturas de e/s físicas para el plan de consulta dentro del intervalo de agregación (expresada como un número de 8 KB de lectura).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**min_physical_io_reads**|**bigint**|Número mínimo de lecturas de e/s físicas para el plan de consulta dentro del intervalo de agregación (expresada como un número de 8 KB de lectura).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**max_physical_io_reads**|**bigint**|Número máximo de lecturas de e/s físicas para el plan de consulta dentro del intervalo de agregación (expresada como un número de 8 KB de lectura).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**stdev_physical_io_reads**|**float**|El número de e/s física lee la desviación estándar del plan de consulta en el intervalo de agregación (expresado como un número de 8 KB de lectura).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**avg_clr_time**|**float**|Promedio de tiempo de CLR para el plan de consulta dentro del intervalo de agregación (en microsegundos).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**last_clr_time**|**bigint**|Última hora de CLR para el plan de consulta dentro del intervalo de agregación (en microsegundos).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**min_clr_time**|**bigint**|Tiempo de CLR mínimo para el plan de consulta dentro del intervalo de agregación (en microsegundos).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**max_clr_time**|**bigint**|Tiempo máximo de CLR para el plan de consulta dentro del intervalo de agregación (en microsegundos).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**stdev_clr_time**|**float**|Desviación estándar de tiempo de CLR para el plan de consulta dentro del intervalo de agregación (en microsegundos).<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**avg_dop**|**float**|Promedio de DOP (grado de paralelismo) para el plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**last_dop**|**bigint**|Último DOP (grado de paralelismo) del plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**min_dop**|**bigint**|Número mínimo de DOP (grado de paralelismo) para el plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**max_dop**|**bigint**|Máximo de DOP (grado de paralelismo) para el plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**stdev_dop**|**float**|Desviación estándar de DOP (grado de paralelismo) para el plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**avg_query_max_used_memory**|**float**|Promedio de concesión de memoria (informada como el número de páginas de 8 KB) para el plan de consulta dentro del intervalo de agregación. Siempre es 0 para las consultas que usan procedimientos optimizados para memoria compilados de forma nativa.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**last_query_max_used_memory**|**bigint**|Última concesión de memoria (indicada como el número de páginas de 8 KB) para el plan de consulta dentro del intervalo de agregación. Siempre es 0 para las consultas que usan procedimientos optimizados para memoria compilados de forma nativa.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**min_query_max_used_memory**|**bigint**|Concesión de memoria mínima (indicada como el número de páginas de 8 KB) para el plan de consulta dentro del intervalo de agregación. Siempre es 0 para las consultas que usan procedimientos optimizados para memoria compilados de forma nativa.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**max_query_max_used_memory**|**bigint**|Concesión de memoria máxima (indicada como el número de páginas de 8 KB) para el plan de consulta dentro del intervalo de agregación. Siempre es 0 para las consultas que usan procedimientos optimizados para memoria compilados de forma nativa.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**stdev_query_max_used_memory**|**float**|Desviación estándar de concesión de memoria (indicada como el número de páginas de 8 KB) para el plan de consulta dentro del intervalo de agregación. Siempre es 0 para las consultas que usan procedimientos optimizados para memoria compilados de forma nativa.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**avg_rowcount**|**float**|Promedio de filas devueltas para el plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**last_rowcount**|**bigint**|Número de filas devueltas por la última ejecución del plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**min_rowcount**|**bigint**|Número mínimo de filas devueltas para el plan de consulta dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**max_rowcount**|**bigint**|Número máximo de filas devueltas para el plan de consulta dentro del intervalo de agregación.|  
|**stdev_rowcount**|**float**|Número de filas devueltas desviación estándar para el plan de consulta dentro del intervalo de agregación.|
|**avg_log_bytes_used**|**float**|Número promedio de bytes en el registro de base de datos utilizado por el plan de consulta, dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**last_log_bytes_used**|**bigint**|Número de bytes en el registro de base de datos utilizado por la última ejecución del plan de consulta, dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**min_log_bytes_used**|**bigint**|Número mínimo de bytes en el registro de base de datos utilizado por el plan de consulta, dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**max_log_bytes_used**|**bigint**|Número máximo de bytes en el registro de base de datos utilizado por el plan de consulta, dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|
|**stdev_log_bytes_used**|**float**|Desviación estándar del número de bytes en el registro de base de datos utilizado por un plan de consulta, dentro del intervalo de agregación.<br/>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] siempre devolverá cero (0).|  
|**avg_tempdb_space_used**|**float**|Número medio de páginas usadas en tempdb para el plan de consulta dentro del intervalo de agregación (expresado como un número de 8 KB).<br><br/>**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**last_tempdb_space_used**|**bigint**|Último número de páginas usadas en tempdb para el plan de consulta dentro del intervalo de agregación (expresado como un número de 8 KB).<br><br/>**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**min_tempdb_space_used**|**bigint**|Número mínimo de páginas usadas en tempdb para el plan de consulta dentro del intervalo de agregación (expresado como un número de 8 KB).<br><br/>**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**max_tempdb_space_used**|**bigint**|Número máximo de páginas usadas en tempdb para el plan de consulta dentro del intervalo de agregación (expresado como un número de 8 KB).<br><br/>**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**stdev_tempdb_space_used**|**float**|Número de páginas usadas en la desviación estándar de tempdb para el plan de consulta dentro del intervalo de agregación (expresado como un número de 8 KB).<br><br/>**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|**avg_page_server_io_reads**|**float**|Número promedio de lecturas de e/s del servidor de páginas para el plan de consulta en el intervalo de agregación (expresado como un número de 8 KB de lectura).<br><br/>**Se aplica a:** Hiperescala Azure SQL Database</br>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (no hiperescala) siempre devolverá cero (0).|
|**last_page_server_io_reads**|**bigint**|Último número de lecturas de e/s del servidor de páginas para el plan de consulta dentro del intervalo de agregación (expresado como un número de 8 KB de lectura).<br><br/>**Se aplica a:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Hiperescala</br>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (no hiperescala) siempre devolverá cero (0).|
|**min_page_server_io_reads**|**bigint**|Número mínimo de lecturas de e/s del servidor de páginas para el plan de consulta dentro del intervalo de agregación (expresado como un número de 8 KB de lectura).<br><br/>**Se aplica a:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Hiperescala</br>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (no hiperescala) siempre devolverá cero (0).|
|**max_page_server_io_reads**|**bigint**|Número máximo de lecturas de e/s del servidor de páginas para el plan de consulta dentro del intervalo de agregación (expresado como un número de 8 KB de lectura).<br><br/>**Se aplica a:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Hiperescala</br>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (no hiperescala) siempre devolverá cero (0).|
|**stdev_page_server_io_reads**|**float**|Número de e/s del servidor de páginas lee la desviación estándar del plan de consulta en el intervalo de agregación (expresado como un número de 8 KB de lectura).<br><br/>**Se aplica a:** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Hiperescala</br>**Nota:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (no hiperescala) siempre devolverá cero (0).|
  
## <a name="permissions"></a>Permisos  
Requiere el permiso `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Consulte también  
 [sys.database_query_store_options &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Query Store Stored Procedures &#40;Transact-SQL&#41; (Procedimientos almacenados del Almacén de consultas &#40;Transact-SQL&#41;)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)    
 [Procedimiento recomendado con el Almacén de consultas](../../relational-databases/performance/best-practice-with-the-query-store.md)   
  
