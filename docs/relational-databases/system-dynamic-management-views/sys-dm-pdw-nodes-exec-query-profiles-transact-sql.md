---
title: sys.dm_pdw_nodes_exec_query_profiles (Transact-SQL) | Microsoft Docs
description: Vista de administración dinámica que se puede usar para supervisar el progreso de las consultas de almacenamiento de datos en tiempo real mientras la consulta está en ejecución.
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 9158ede7f526fe5ce4cbce7b9e6741c03e170849
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99140933"
---
# <a name="sysdm_pdw_nodes_exec_query_profiles-transact-sql"></a>sys.dm_pdw_nodes_exec_query_profiles (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Supervisa el progreso de las consultas de almacenamiento de datos en tiempo real mientras la consulta está en ejecución.   
  
## <a name="table-returned"></a>Tabla devuelta
  
Los contadores devueltos son por operador y por subproceso. Los resultados son dinámicos y no coinciden con los resultados de las opciones existentes, como, por ejemplo, las `SET STATISTICS XML ON` que solo crean los resultados cuando finaliza la consulta.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|pdw_node_id|**int**|IDENTIFICADOR numérico único asociado al nodo.|
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
  
## <a name="remarks"></a>Observaciones

Se aplican los mismos comentarios en [Sys.dm_exec_query_profiles](./sys-dm-exec-query-profiles-transact-sql.md) .  

## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW SERVER STATE` en el servidor.  

## <a name="see-also"></a>Vea también

 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
   

 ## <a name="next-steps"></a>Pasos siguientes 

Información general sobre el desarrollo de Azure Synapse Analytics] (/Azure/SQL-Data-Warehouse/SQL-Data-Warehouse-Overview-develop).