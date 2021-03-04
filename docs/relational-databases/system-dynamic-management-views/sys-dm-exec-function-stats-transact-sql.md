---
description: sys.dm_exec_function_stats (Transact-SQL)
title: sys.dm_exec_function_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_function_stats
- sys.dm_exec_function_stats_tsql
- dm_exec_function_stats
- dm_exec_function_stats_tsql
helpviewer_keywords:
- sys.dm_exec_function_stats dynamic management view
ms.assetid: 4c3d6a02-08e4-414b-90be-36b89a0e5a3a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6acd8d9cdc09bcffc9e5ef95f6d3e7ed700ffafb
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836642"
---
# <a name="sysdm_exec_function_stats-transact-sql"></a>sys.dm_exec_function_stats (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Devuelve estadísticas de rendimiento agregadas para las funciones almacenadas en caché. La vista devuelve una fila por cada plan de funciones almacenadas en caché y la duración de la fila es siempre que la función permanezca almacenada en caché. Cuando se quita una función de la memoria caché, la fila correspondiente se elimina de esta vista. En ese momento, se genera un evento de Seguimiento de SQL de Estadísticas de rendimiento similar a **sys.dm_exec_query_stats**. Devuelve información sobre las funciones escalares, incluidas las funciones en memoria y las funciones escalares de CLR. No devuelve información acerca de las funciones con valores de tabla y sobre las funciones escalares que se insertan con la [inclusión de UDF escalar](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], las vistas de administración dinámica no pueden exponer información que impactaría a la contención de la base de datos ni acerca de otras bases de datos a las que el usuario tenga acceso. Para evitar exponer esta información, se filtran todas las filas que contienen datos que no pertenecen al inquilino conectado.  
  
> [!NOTE]
> Los resultados de **Sys.dm_exec_function_stats**  pueden variar en cada ejecución, ya que los datos solo reflejan las consultas finalizadas y no las que todavía están en curso. 

  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|IDENTIFICADOR de base de datos en el que reside la función.|  
|**object_id**|**int**|Número de identificación de objeto de la función.|  
|**type**|**char(2)**|Tipo del objeto: FN = funciones con valores escalares|  
|**type_desc**|**nvarchar(60)**|Descripción del tipo de objeto: SQL_SCALAR_FUNCTION|  
|**sql_handle**|**varbinary (64)**|Se puede utilizar para correlacionar con las consultas en **Sys.dm_exec_query_stats** que se ejecutaron desde esta función.|  
|**plan_handle**|**varbinary (64)**|Identificador del plan en memoria. Este identificador es transitorio y permanece constante solo mientras el plan permanece en la memoria caché. Este valor se puede usar con la vista de administración dinámica **Sys.dm_exec_cached_plans** .<br /><br /> Siempre se 0x000 cuando una función compilada de forma nativa consulta una tabla optimizada para memoria.|  
|**cached_time**|**datetime**|Hora a la que se agregó la función a la memoria caché.|  
|**last_execution_time**|**datetime**|Última vez que se ejecutó la función.|  
|**execution_count**|**bigint**|Número de veces que se ha ejecutado la función desde que se compiló por última vez.|  
|**total_worker_time**|**bigint**|Cantidad total de tiempo de CPU, en microsegundos, consumido por las ejecuciones de esta función desde que se compiló.<br /><br /> En el caso de las funciones compiladas de forma nativa, es posible que **total_worker_time** no sea preciso si muchas ejecuciones tardan menos de 1 milisegundo.|  
|**last_worker_time**|**bigint**|Tiempo de CPU, en microsegundos, consumido la última vez que se ejecutó la función. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Tiempo mínimo de CPU, en microsegundos, que esta función ha consumido alguna vez durante una ejecución. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Tiempo máximo de CPU, en microsegundos, que esta función ha consumido alguna vez durante una ejecución. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Número total de lecturas físicas realizadas por las ejecuciones de esta función desde que se compiló.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**last_physical_reads**|**bigint**|Número de lecturas físicas realizadas la última vez que se ejecutó la función.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**min_physical_reads**|**bigint**|Número mínimo de lecturas físicas que ha realizado esta función durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**max_physical_reads**|**bigint**|Número máximo de lecturas físicas que ha realizado esta función durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**total_logical_writes**|**bigint**|Número total de escrituras lógicas realizadas por las ejecuciones de esta función desde que se compiló.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**last_logical_writes**|**bigint**|Número de páginas del grupo de búferes desfasadas la última vez que se ejecutó el plan. Si una página ya está desfasada (modificada) no se cuenta ninguna escritura.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**min_logical_writes**|**bigint**|Número mínimo de escrituras lógicas realizadas por esta función durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**max_logical_writes**|**bigint**|Número máximo de escrituras lógicas realizadas por esta función durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**total_logical_reads**|**bigint**|Número total de lecturas lógicas realizadas por las ejecuciones de esta función desde que se compiló.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**last_logical_reads**|**bigint**|Número de lecturas lógicas realizadas la última vez que se ejecutó la función.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**min_logical_reads**|**bigint**|Número mínimo de lecturas lógicas que ha realizado esta función durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**max_logical_reads**|**bigint**|Número máximo de lecturas lógicas que ha realizado esta función durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**total_elapsed_time**|**bigint**|Tiempo total transcurrido, en microsegundos, para las ejecuciones completadas de esta función.|  
|**last_elapsed_time**|**bigint**|Tiempo transcurrido, en microsegundos, para la ejecución completada más recientemente de esta función.|  
|**min_elapsed_time**|**bigint**|Tiempo mínimo transcurrido, en microsegundos, para cualquier ejecución completada de esta función.|  
|**max_elapsed_time**|**bigint**|Tiempo máximo transcurrido, en microsegundos, para cualquier ejecución completada de esta función.|  
|**total_page_server_reads**|**bigint**|Número total de lecturas del servidor de páginas realizadas por las ejecuciones de esta función desde que se compiló.<br /><br /> **Se aplica a:** Hiperescala Azure SQL Database.|  
|**last_page_server_reads**|**bigint**|Número de lecturas del servidor de páginas realizadas la última vez que se ejecutó la función.<br /><br /> **Se aplica a:** Hiperescala Azure SQL Database.|  
|**min_page_server_reads**|**bigint**|Número mínimo de lecturas del servidor de páginas que esta función ha realizado durante una ejecución.<br /><br /> **Se aplica a:** Hiperescala Azure SQL Database.|  
|**max_page_server_reads**|**bigint**|Número máximo de lecturas de servidor de páginas que esta función ha realizado alguna vez durante una ejecución.<br /><br /> **Se aplica a:** Hiperescala Azure SQL Database.|
  
## <a name="permissions"></a>Permisos  

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve información sobre las diez funciones principales identificadas por el promedio de tiempo transcurrido.  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'function name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_function_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica relacionadas con la ejecución &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 
 [sys.dm_exec_trigger_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)   
 [sys.dm_exec_procedure_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  
  
