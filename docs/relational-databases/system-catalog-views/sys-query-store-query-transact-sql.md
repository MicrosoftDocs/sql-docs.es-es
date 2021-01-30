---
description: sys.query_store_query (Transact-SQL)
title: sys.query_store_query (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- QUERY_STORE_QUERY
- SYS.QUERY_STORE_QUERY_TSQL
- SYS.QUERY_STORE_QUERY
- QUERY_STORE_QUERY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_query catalog view
- sys.query_store_query catalog view
ms.assetid: bdee149e-7556-4fc3-8242-925dd4b7b6ac
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d682d739c8f18a7a1f41b6abf23c62dd04f2c17f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208121"
---
# <a name="sysquery_store_query-transact-sql"></a>sys.query_store_query (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contiene información sobre la consulta y sus estadísticas de ejecución de tiempo de ejecución agregadas globales asociadas.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**query_id**|**bigint**|Clave principal.|  
|**query_text_id**|**bigint**|Clave externa. Combinaciones a [sys.query_store_query_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)|  
|**context_settings_id**|**bigint**|Clave externa. Combina con [sys.query_context_settings &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md).<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**object_id**|**bigint**|IDENTIFICADOR del objeto de base de datos del que forma parte la consulta (procedimiento almacenado, desencadenador, CLR UDF/UDAgg, etc.). 0 si la consulta no se ejecuta como parte de un objeto de base de datos (consulta ad hoc).<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**batch_sql_handle**|**varbinary (64)**|IDENTIFICADOR del lote de instrucciones del que forma parte la consulta. Solo se rellena si la consulta hace referencia a tablas temporales o variables de tabla.<br/>**Nota:** Azure Synapse Analytics siempre devolverá *null*.|  
|**query_hash**|**Binary(8**|Hash MD5 de la consulta individual, basado en el árbol de consulta lógico. Incluye sugerencias del optimizador.|  
|**is_internal_query**|**bit**|La consulta se ha generado internamente.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**query_parameterization_type**|**tinyint**|Tipo de parametrización:<br /><br /> 0 - Ninguno<br /><br /> 1: usuario<br /><br /> 2-simple<br /><br /> 3-forzado<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**query_parameterization_type_desc**|**nvarchar(60)**|Descripción textual del tipo de parametrización.<br/>**Nota:** Azure Synapse Analytics siempre devolverá *ninguno*.|  
|**initial_compile_start_time**|**datetimeoffset**|Hora de inicio de la compilación.|  
|**last_compile_start_time**|**datetimeoffset**|Hora de inicio de la compilación.|  
|**last_execution_time**|**datetimeoffset**|Última hora de ejecución hace referencia a la última hora de finalización de la consulta o el plan.|  
|**last_compile_batch_sql_handle**|**varbinary (64)**|Identificador del último lote de SQL en el que se usó la consulta por última vez. Se puede proporcionar como entrada para [sys.dm_exec_sql_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) para obtener el texto completo del lote.<br/>**Nota:** Azure Synapse Analytics siempre devolverá *null*.|  
|**last_compile_batch_offset_start**|**bigint**|Información que se puede proporcionar a sys.dm_exec_sql_text junto con last_compile_batch_sql_handle.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|
|**last_compile_batch_offset_end**|**bigint**|Información que se puede proporcionar a sys.dm_exec_sql_text junto con last_compile_batch_sql_handle.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**count_compiles**|**bigint**|Estadísticas de compilación.<br/>**Nota:** Azure Synapse Analytics siempre devolverá uno (1).|  
|**avg_compile_duration**|**float**|Estadísticas de compilación en microsegundos.|  
|**last_compile_duration**|**bigint**|Estadísticas de compilación en microsegundos.|  
|**avg_bind_duration**|**float**|Estadísticas de enlace en microsegundos.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**last_bind_duration**|**bigint**|Estadísticas de enlace.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**avg_bind_cpu_time**|**float**|Estadísticas de enlace.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**last_bind_cpu_time**|**bigint**|Estadísticas de enlace.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**avg_optimize_duration**|**float**|Estadísticas de optimización en microsegundos.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|
|**last_optimize_duration**|**bigint**|Estadísticas de optimización.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|
|**avg_optimize_cpu_time**|**float**|Estadísticas de optimización en microsegundos.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|
|**last_optimize_cpu_time**|**bigint**|Estadísticas de optimización.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|
|**avg_compile_memory_kb**|**float**|Estadísticas de memoria de compilación.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|
|**last_compile_memory_kb**|**bigint**|Estadísticas de memoria de compilación.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|
|**max_compile_memory_kb**|**bigint**|Estadísticas de memoria de compilación.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|
|**is_clouddb_internal_query**|**bit**|Siempre es 0 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el entorno local.<br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso **View Database State** .  
  
## <a name="see-also"></a>Consulte también  
 [sys.database_query_store_options &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Query Store Stored Procedures &#40;Transact-SQL&#41; (Procedimientos almacenados del Almacén de consultas &#40;Transact-SQL&#41;)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
