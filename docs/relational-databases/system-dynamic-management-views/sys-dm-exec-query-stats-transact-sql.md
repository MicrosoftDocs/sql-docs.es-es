---
description: sys.dm_exec_query_stats (Transact-SQL)
title: sys.dm_exec_query_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_query_stats_TSQL
- dm_exec_query_stats
- sys.dm_exec_query_stats
- sys.dm_exec_query_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_stats dynamic management view
ms.assetid: eb7b58b8-3508-4114-97c2-d877bcb12964
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: de35914f80e73cd193460476a3c9fc9c1c7330e6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342902"
---
# <a name="sysdm_exec_query_stats-transact-sql"></a>sys.dm_exec_query_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve estadísticas de rendimiento de agregado para planes de consulta en caché en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La vista contiene una fila por cada instrucción de consulta dentro del plan en caché, y la duración de las filas está ligada al propio plan. Cuando se quita un plan de la caché, se eliminan las filas correspondientes de esta vista.  
  
> [!NOTE]
> - Los resultados de **Sys.dm_exec_query_stats**  pueden variar en cada ejecución, ya que los datos solo reflejan las consultas finalizadas y no las que todavía están en curso.
> - Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_exec_query_stats**.    

  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**sql_handle**|**varbinary (64)**  |Es un token que identifica de forma única el lote o el procedimiento almacenado del que forma parte la consulta.<br /><br /> **sql_handle**, junto con **statement_start_offset** y **statement_end_offset**, se pueden usar para recuperar el texto SQL de la consulta llamando a la función de administración dinámica **sys.dm_exec_sql_text**.|  
|**statement_start_offset**|**int**|Indica (en bytes y empezando por 0) la posición inicial de la consulta que la fila describe en el texto del lote o del objeto persistente.|  
|**statement_end_offset**|**int**|Indica (en bytes y empezando por 0) la posición final de la consulta que la fila describe en el texto del lote o del objeto persistente. En el caso de las versiones anteriores a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , un valor de-1 indica el final del lote. Los comentarios finales ya no están incluidos.|  
|**plan_generation_num**|**bigint**|Número de secuencia que se puede usar para distinguir entre instancias de los planes después de una nueva compilación.|  
|**plan_handle**|**varbinary (64)**|Es un token que identifica de forma única un plan de ejecución de consulta para un lote que se ha ejecutado y su plan reside en la caché del plan, o se está ejecutando actualmente. Este valor se puede pasar a la función de administración dinámica [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md) para obtener el plan de consulta.<br /><br /> Será siempre 0x000 cuando un procedimiento almacenado nativo consulte una tabla optimizada para memoria.|  
|**creation_time**|**datetime**|Hora en que se compiló el plan.|  
|**last_execution_time**|**datetime**|Hora a la que se inició la ejecución del plan por última vez.|  
|**execution_count**|**bigint**|Número de veces que se ha ejecutado el plan desde que se compiló por última vez.|  
|**total_worker_time**|**bigint**|Tiempo total de CPU, notificado en microsegundos (pero solo con precisión de milisegundos), consumido por las ejecuciones de este plan desde su compilación.<br /><br /> Para los procedimientos almacenados compilados de forma nativa, **total_worker_time** puede no ser exacto si varias ejecuciones tardan menos de 1 milisegundo.|  
|**last_worker_time**|**bigint**|Tiempo de CPU, notificado en microsegundos (pero solo con precisión de milisegundos), que se consumió la última vez que se ejecutó el plan. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Tiempo de CPU mínimo, notificado en microsegundos (pero solo con precisión de milisegundos), que este plan ha consumido alguna vez durante una sola ejecución. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Tiempo de CPU máximo, notificado en microsegundos (pero solo con precisión de milisegundos), que este plan ha consumido alguna vez durante una sola ejecución. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Número total de lecturas físicas realizadas por las ejecuciones de este plan desde su compilación.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**last_physical_reads**|**bigint**|Número de lecturas físicas realizadas la última vez que se ejecutó el plan.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**min_physical_reads**|**bigint**|Número mínimo de lecturas físicas que ha realizado este plan durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**max_physical_reads**|**bigint**|Número máximo de lecturas físicas que ha realizado este plan durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**total_logical_writes**|**bigint**|Número total de escrituras lógicas realizadas por las ejecuciones de este plan desde su compilación.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**last_logical_writes**|**bigint**|Número de páginas del grupo de búferes sucias durante la ejecución completada más recientemente del plan.<br /><br />Después de leer una página, la página se vuelve desfasada solo la primera vez que se modifica. Cuando se modifica una página, se incrementa este número. Las modificaciones posteriores de una página ya desfasada no afectan a este número.<br /><br />Este número siempre será 0 al consultar una tabla optimizada para memoria.|  
|**min_logical_writes**|**bigint**|Número mínimo de escrituras lógicas que ha realizado este plan durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**max_logical_writes**|**bigint**|Número máximo de escrituras lógicas que ha realizado este plan durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**total_logical_reads**|**bigint**|Número total de lecturas lógicas realizadas por las ejecuciones de este plan desde su compilación.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**last_logical_reads**|**bigint**|Número de lecturas lógicas realizadas la última vez que se ejecutó el plan.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**min_logical_reads**|**bigint**|Número mínimo de lecturas lógicas que ha realizado este plan durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**max_logical_reads**|**bigint**|Número máximo de lecturas lógicas que ha realizado este plan durante una ejecución.<br /><br /> Será siempre 0 al consultar una tabla optimizada para memoria.|  
|**total_clr_time**|**bigint**|Tiempo, notificado en microsegundos (pero solo con precisión de milisegundos), consumido dentro [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Common Language Runtime objetos (CLR) por las ejecuciones de este plan desde su compilación. Los objetos CLR pueden ser procedimientos almacenados, funciones, desencadenadores, tipos y agregados.|  
|**last_clr_time**|**bigint**|Tiempo, notificado en microsegundos (pero solo con precisión de milisegundos), consumido por la ejecución dentro de los objetos CLR de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] durante la última ejecución de este plan. Los objetos CLR pueden ser procedimientos almacenados, funciones, desencadenadores, tipos y agregados.|  
|**min_clr_time**|**bigint**|Tiempo de CPU mínimo, notificado en microsegundos (pero solo con precisión de milisegundos), que este plan ha consumido alguna vez dentro de objetos CLR de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] durante una sola ejecución. Los objetos CLR pueden ser procedimientos almacenados, funciones, desencadenadores, tipos y agregados.|  
|**max_clr_time**|**bigint**|Tiempo de CPU máximo, notificado en microsegundos (pero solo con precisión de milisegundos), que este plan ha consumido alguna vez dentro del CLR de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] durante una sola ejecución. Los objetos CLR pueden ser procedimientos almacenados, funciones, desencadenadores, tipos y agregados.|  
|**total_elapsed_time**|**bigint**|Tiempo total transcurrido, notificado en microsegundos (pero solo con precisión de milisegundos), para las ejecuciones completadas de este plan.|  
|**last_elapsed_time**|**bigint**|Tiempo transcurrido, notificado en microsegundos (pero solo con precisión de milisegundos), para la ejecución completada más recientemente de este plan.|  
|**min_elapsed_time**|**bigint**|Tiempo mínimo transcurrido, notificado en microsegundos (pero solo con precisión de milisegundos), para cualquier ejecución completada de este plan.|  
|**max_elapsed_time**|**bigint**|Tiempo máximo transcurrido, notificado en microsegundos (pero solo con precisión de milisegundos), para cualquier ejecución completada de este plan.|  
|**query_hash**|**Binario (8)**|Valor hash binario que se calcula en la consulta y que se usa para identificar consultas con una lógica similar. Puede usar el hash de consulta para determinar el uso de recursos agregados para las consultas que solo se diferencian en los valores literales.|  
|**query_plan_hash**|**Binary(8**|Valor hash binario que se calcula en el plan de ejecución de consulta y que se usa para identificar planes de ejecución de consulta similares. Puede usar el hash del plan de consulta para buscar el costo acumulativo de las consultas con planes de ejecución similares.<br /><br /> Será siempre 0x000 cuando un procedimiento almacenado nativo consulte una tabla optimizada para memoria.|  
|**total_rows**|**bigint**|Número total de filas devueltas por la consulta. No puede ser NULL.<br /><br /> Será siempre 0 cuando un procedimiento almacenado nativo consulte una tabla optimizada para memoria.|  
|**last_rows**|**bigint**|Número de filas devueltas por la última ejecución de la consulta. No puede ser NULL.<br /><br /> Será siempre 0 cuando un procedimiento almacenado nativo consulte una tabla optimizada para memoria.|  
|**min_rows**|**bigint**|Número mínimo de filas devueltas por la consulta durante una ejecución. No puede ser NULL.<br /><br /> Será siempre 0 cuando un procedimiento almacenado nativo consulte una tabla optimizada para memoria.|  
|**max_rows**|**bigint**|Número máximo de filas devueltas por la consulta durante una ejecución. No puede ser NULL.<br /><br /> Será siempre 0 cuando un procedimiento almacenado nativo consulte una tabla optimizada para memoria.|  
|**statement_sql_handle**|**varbinary (64)**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Se rellena con valores no NULL solo si Almacén de consultas está activado y recopila las estadísticas para esa consulta concreta.|  
|**statement_context_id**|**bigint**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Se rellena con valores no NULL solo si Almacén de consultas está activado y recopila las estadísticas para esa consulta concreta.|  
|**total_dop**|**bigint**|Suma total del grado de paralelismo que ha utilizado este plan desde su compilación. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**last_dop**|**bigint**|Grado de paralelismo que se ha ejecutado por última vez en este plan. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**min_dop**|**bigint**|Grado mínimo de paralelismo que ha usado este plan durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**max_dop**|**bigint**|El grado máximo de paralelismo que ha utilizado este plan durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**total_grant_kb**|**bigint**|La cantidad total de concesiones de memoria reservada en KB que ha recibido este plan desde su compilación. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**last_grant_kb**|**bigint**|La cantidad de concesiones de memoria reservada en KB cuando se ejecutó este plan por última vez. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**min_grant_kb**|**bigint**|La cantidad mínima de concesión de memoria reservada en KB que este plan ha recibido alguna vez durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**max_grant_kb**|**bigint**|Cantidad máxima de concesión de memoria reservada en KB que este plan ha recibido alguna vez durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**total_used_grant_kb**|**bigint**|La cantidad total de concesiones de memoria reservada en KB usada por este plan desde su compilación. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**last_used_grant_kb**|**bigint**|La cantidad de concesión de memoria usada en KB cuando se ejecutó este plan por última vez. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**min_used_grant_kb**|**bigint**|La cantidad mínima de concesión de memoria usada en KB que este plan ha usado alguna vez durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**max_used_grant_kb**|**bigint**|Cantidad máxima de concesión de memoria usada en KB en este plan que se ha usado alguna vez durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**total_ideal_grant_kb**|**bigint**|La cantidad total de concesión de memoria ideal en KB que ha estimado este plan desde su compilación. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**last_ideal_grant_kb**|**bigint**|La cantidad de concesión de memoria ideal en KB cuando se ejecutó este plan por última vez. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**min_ideal_grant_kb**|**bigint**|La cantidad mínima de concesión de memoria ideal en KB que este plan ha estimado alguna vez durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**max_ideal_grant_kb**|**bigint**|La cantidad máxima de concesión de memoria ideal en KB que este plan ha estimado alguna vez durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**total_reserved_threads**|**bigint**|La suma total de los subprocesos paralelos reservados que este plan ha usado alguna vez desde que se compiló. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**last_reserved_threads**|**bigint**|El número de subprocesos paralelos reservados cuando este plan se ejecutó por última vez. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**min_reserved_threads**|**bigint**|Número mínimo de subprocesos paralelos reservados que este plan ha usado durante una ejecución.  Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**max_reserved_threads**|**bigint**|Número máximo de subprocesos paralelos reservados que este plan ha usado durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**total_used_threads**|**bigint**|Suma total de los subprocesos paralelos usados que ha usado este plan desde su compilación. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**last_used_threads**|**bigint**|El número de subprocesos paralelos usados cuando este plan se ejecutó por última vez. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**min_used_threads**|**bigint**|Número mínimo de subprocesos paralelos usados que este plan ha usado durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**max_used_threads**|**bigint**|Número máximo de subprocesos paralelos usados que este plan ha usado durante una ejecución. Siempre será 0 para consultar una tabla optimizada para memoria.<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
|**total_columnstore_segment_reads**|**bigint**|Suma total de los segmentos de almacén de columnas leídos por la consulta. No puede ser NULL.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**last_columnstore_segment_reads**|**bigint**|El número de segmentos de almacén de columnas leídos por la última ejecución de la consulta. No puede ser NULL.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**min_columnstore_segment_reads**|**bigint**|Número mínimo de segmentos de almacén de columnas leídos por la consulta durante una ejecución. No puede ser NULL.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**max_columnstore_segment_reads**|**bigint**|Número máximo de segmentos de almacén de columnas leídos por la consulta durante una ejecución. No puede ser NULL.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**total_columnstore_segment_skips**|**bigint**|Suma total de los segmentos de almacén de columnas omitidos por la consulta. No puede ser NULL.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**last_columnstore_segment_skips**|**bigint**|Número de segmentos de almacén de columnas omitidos por la última ejecución de la consulta. No puede ser NULL.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**min_columnstore_segment_skips**|**bigint**|Número mínimo de segmentos de almacén de columnas omitidos por la consulta durante una ejecución. No puede ser NULL.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|    
|**max_columnstore_segment_skips**|**bigint**|Número máximo de segmentos de almacén de columnas omitidos por la consulta durante una ejecución. No puede ser NULL.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|
|**total_spills**|**bigint**|Número total de páginas desbordadas por la ejecución de esta consulta desde su compilación.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Número de páginas desbordadas la última vez que se ejecutó la consulta.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Número mínimo de páginas que esta consulta ha sobrevertido durante una ejecución.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Número máximo de páginas que esta consulta ha sobrevertido durante una ejecución.<br /><br /> **Se aplica a: a** partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**pdw_node_id**|**int**|Identificador del nodo en el que se encuentra esta distribución.<br /><br /> **Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]| 
|**total_page_server_reads**|**bigint**|Número total de lecturas del servidor de páginas remotas realizadas por las ejecuciones de este plan desde su compilación.<br /><br /> **Se aplica a:** Hiperescala Azure SQL Database |  
|**last_page_server_reads**|**bigint**|Número de lecturas del servidor de páginas remotas realizadas la última vez que se ejecutó el plan.<br /><br /> **Se aplica a:** Hiperescala Azure SQL Database |  
|**min_page_server_reads**|**bigint**|Número mínimo de lecturas del servidor de páginas remotas que ha realizado este plan durante una ejecución.<br /><br /> **Se aplica a:** Hiperescala Azure SQL Database |  
|**max_page_server_reads**|**bigint**|Número máximo de lecturas del servidor de páginas remotas que ha realizado este plan durante una ejecución.<br /><br /> **Se aplica a:** Hiperescala Azure SQL Database |  
> [!NOTE]
> <sup>1</sup> para los procedimientos almacenados compilados de forma nativa cuando la recopilación de estadísticas está habilitada, el tiempo de trabajo se recopila en milisegundos. Si la consulta se ejecuta en menos de un milisegundo, el valor será 0.  
  
## <a name="permissions"></a>Permisos  

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
   
## <a name="remarks"></a>Comentarios  
 Cuando se completa una consulta, se actualizan las estadísticas en la vista.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-finding-the-top-n-queries"></a>A. Buscar las consultas TOP N  
 El siguiente ejemplo devuelve información acerca de las cinco consultas principales clasificadas en función del tiempo promedio de CPU. Este ejemplo agrega las consultas según su hash de consulta para que las consultas lógicamente equivalentes se agrupen según su consumo acumulado de los recursos.  
  
```sql  
SELECT TOP 5 query_stats.query_hash AS "Query Hash",   
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",  
    MIN(query_stats.statement_text) AS "Statement Text"  
FROM   
    (SELECT QS.*,   
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,  
    ((CASE statement_end_offset   
        WHEN -1 THEN DATALENGTH(ST.text)  
        ELSE QS.statement_end_offset END   
            - QS.statement_start_offset)/2) + 1) AS statement_text  
     FROM sys.dm_exec_query_stats AS QS  
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats  
GROUP BY query_stats.query_hash  
ORDER BY 2 DESC;  
```  
  
### <a name="b-returning-row-count-aggregates-for-a-query"></a>B. Devolver agregados de recuentos de filas para una consulta  
 En el ejemplo siguiente se devuelve información de agregado de recuento de filas (filas totales, filas mínimas, filas máximas y últimas filas) para las consultas.  
  
```sql  
SELECT qs.execution_count,  
    SUBSTRING(qt.text,qs.statement_start_offset/2 +1,   
                 (CASE WHEN qs.statement_end_offset = -1   
                       THEN LEN(CONVERT(nvarchar(max), qt.text)) * 2   
                       ELSE qs.statement_end_offset end -  
                            qs.statement_start_offset  
                 )/2  
             ) AS query_text,   
     qt.dbid, dbname= DB_NAME (qt.dbid), qt.objectid,   
     qs.total_rows, qs.last_rows, qs.min_rows, qs.max_rows  
FROM sys.dm_exec_query_stats AS qs   
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS qt   
WHERE qt.text like '%SELECT%'   
ORDER BY qs.execution_count DESC;  
```  
  
## <a name="see-also"></a>Vea también  
[Funciones y vistas de administración dinámica relacionadas con la ejecución &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)    
[sys.dm_exec_sql_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)    
[sys.dm_exec_query_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_procedure_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)     
[sys.dm_exec_trigger_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)     
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    
  


