---
description: sys.dm_db_missing_index_group_stats_query (Transact-SQL)
title: sys.dm_db_missing_index_group_stats_query (Transact-SQL)
ms.custom: ''
ms.date: 02/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_group_stats_query_TSQL
- sys.dm_db_missing_index_group_stats_query
- dm_db_missing_index_group_stats_query_TSQL
- dm_db_missing_index_group_stats_query
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_group_stats_query dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_group_stats_query dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current
ms.openlocfilehash: f54143b965847c83cd07ee07543873fcd7359900
ms.sourcegitcommit: c6cc0b669b175ae290cf5b08952010661ebd03c3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100531058"
---
# <a name="sysdm_db_missing_index_group_stats_query-transact-sql"></a>sys.dm_db_missing_index_group_stats_query (Transact-SQL)
[!INCLUDE [SQL Server 2019 SQL Database](../../includes/applies-to-version/sqlserver2019-asdb-asdbmi.md)]

  Devuelve información sobre las consultas que necesitan un índice que falta de los grupos de índices que faltan, excluidos los índices espaciales. Se puede devolver más de una consulta por cada grupo de índices que faltan. Un grupo de índices que faltan puede tener varias consultas que requieran el mismo índice.
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], las vistas de administración dinámica no pueden exponer información que impactaría a la contención de la base de datos ni acerca de otras bases de datos a las que el usuario tenga acceso. Para evitar exponer esta información, se filtran todas las filas que contienen datos que no pertenecen al inquilino conectado.  
    
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**group_handle**|**int**|Identifica un grupo de índices que faltan. Este identificador es único en todo el servidor.<br /><br /> Las otras columnas proporcionan información sobre todas las consultas para las que se considera que falta el índice del grupo.<br /><br /> Un grupo de índices solo contiene un índice.<BR><BR>Se puede combinar con **index_group_handle** en [Sys.dm_db_missing_index_groups](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md).|  
|**query_hash**|**Binary(8**|Valor hash binario que se calcula en la consulta y que se usa para identificar consultas con una lógica similar. Puede usar el hash de consulta para determinar el uso de recursos agregados para las consultas que solo se diferencian en los valores literales.|  
|**query_plan_hash**|**Binary(8**|Valor hash binario que se calcula en el plan de ejecución de consulta y que se usa para identificar planes de ejecución de consulta similares. Puede usar el hash del plan de consulta para buscar el costo acumulativo de las consultas con planes de ejecución similares.<br /><br /> Será siempre 0x000 cuando un procedimiento almacenado nativo consulte una tabla optimizada para memoria.|  
|**last_sql_handle**|**varbinary (64)**|Es un token que identifica de forma única el lote o procedimiento almacenado de la última instrucción compilada que necesitaba este índice.<BR><BR>**last_sql_handle** se puede usar para recuperar el texto SQL de la consulta llamando a la función de administración dinámica [Sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) .|
|**last_statement_start_offset**|**int**|Indica, en bytes, empezando por 0, la posición inicial de la consulta que la fila describe en el texto del lote o el objeto almacenado para la última instrucción compilada que necesitaba este índice en su lote de SQL.|
|**last_statement_end_offset**|**int**|Indica, en bytes, empezando por 0, la posición final de la consulta que la fila describe en el texto del lote o el objeto almacenado para la última instrucción compilada que necesitaba este índice en su lote de SQL.|
|**last_statement_sql_handle**|**varbinary (64)**|Es un token que identifica de forma única el lote o procedimiento almacenado de la última instrucción compilada que necesitaba este índice.<BR><BR>**last_statement_sql_handle**, junto con **last_statement_start_offset** y **last_statement_end_offset**, se pueden usar para recuperar el texto SQL de la consulta llamando a la función de administración dinámica de [Sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) .<BR><BR>Si no se ha habilitado Almacén de consultas al compilar la consulta, devuelve 0.|
|**user_seeks**|**bigint**|Número de búsquedas iniciadas por consultas de usuario para las que se podría haber utilizado el índice recomendado del grupo.|  
|**user_scans**|**bigint**|Número de recorridos iniciados por consultas de usuario para los que se podría haber utilizado el índice recomendado del grupo.|  
|**last_user_seek**|**datetime**|Fecha y hora de la última búsqueda iniciada por consultas de usuario para la que se podría haber utilizado el índice recomendado del grupo.|  
|**last_user_scan**|**datetime**|Fecha y hora del último recorrido iniciado por consultas de usuario para el que se podría haber utilizado el índice recomendado del grupo.|  
|**avg_total_user_cost**|**float**|Costo medio de las consultas de usuario que podría reducirse mediante el índice del grupo.|  
|**avg_user_impact**|**float**|Beneficio porcentual medio que podrían obtener las consultas de usuario si se implementara este grupo de índices que faltan. El valor significa que el costo de las consultas se reduciría este porcentaje como promedio si se implementara este grupo de índices que faltan.|  
|**system_seeks**|**bigint**|Número de búsquedas iniciadas por consultas del sistema, como consultas de estadísticas automáticas, para las que se podría haber utilizado el índice recomendado del grupo. Para obtener más información, consulte [auto stats (clase de eventos](../../relational-databases/event-classes/auto-stats-event-class.md)).|  
|**system_scans**|**bigint**|Número de recorridos iniciados por consultas del sistema para los que se podría haber utilizado el índice recomendado del grupo.|  
|**last_system_seek**|**datetime**|Fecha y hora de la última búsqueda en el sistema iniciada por consultas del sistema para la que se podría haber utilizado el índice recomendado del grupo.|  
|**last_system_scan**|**datetime**|Fecha y hora del último recorrido en el sistema iniciado por consultas del sistema para el que se podría haber utilizado el índice recomendado del grupo.|  
|**avg_total_system_cost**|**float**|Costo medio de las consultas del sistema que podría reducirse mediante el índice del grupo.|  
|**avg_system_impact**|**float**|Beneficio porcentual medio que podrían obtener las consultas del sistema si se implementara este grupo de índices que faltan. El valor significa que el costo de las consultas se reduciría este porcentaje como promedio si se implementara este grupo de índices que faltan.|  
  
## <a name="remarks"></a>Observaciones  
 La información devuelta por **Sys.dm_db_missing_index_group_stats_query** se actualiza por cada ejecución de la consulta, no por cada compilación o recompilación de la consulta. Las estadísticas de uso no se guardan; solo se conservan hasta que se reinicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los administradores de bases de datos deben realizar periódicamente una copia de seguridad de la información de los índices que faltan si desean conservar las estadísticas de uso después del reciclaje del servidor.  
 
  >[!NOTE]
  >El conjunto de resultados de esta DMV está limitado a 600 filas. Cada fila contiene un índice que falta. Si tiene más de 600 índices que faltan, debe tratar los índices que faltan existentes para que pueda ver los más recientes.

## <a name="permissions"></a>Permisos  
 Para consultar esta vista de administración dinámica, se debe conceder a los usuarios el permiso VIEW SERVER STATE o cualquier permiso que implique el permiso VIEW SERVER STATE.  
  
## <a name="examples"></a>Ejemplos  
 En los siguientes ejemplos se muestra cómo usar la vista de administración dinámica **Sys.dm_db_missing_index_group_stats_query** .  
  
  
### <a name="a-find-the-latest-query-text-for-the-top-10-highest-anticipated-improvements-for-user-queries"></a>A. Buscar el texto de consulta más reciente de las 10 mejores mejoras previstas para las consultas de usuario 
 La consulta siguiente devuelve el texto de la última consulta grabada para los 10 índices que faltan y que generarían la mejora acumulativa más alta prevista, en orden descendente.  

```sql
SELECT TOP 10 
    SUBSTRING
    (
            sql_text.text,
            misq.last_statement_start_offset / 2 + 1,
            (
            CASE misq.last_statement_Start_offset
                WHEN -1 THEN datalength(sql_text.text)
                ELSE misq.last_statement_end_offset
            END - misq.last_statement_start_offset
            ) / 2 + 1
    ),
    misq.*
FROM sys.dm_db_missing_index_group_stats_query AS misq
CROSS APPLY sys.dm_exec_sql_text(last_sql_handle) AS sql_text
ORDER BY avg_total_user_cost * avg_user_impact * (user_seeks + user_scans) DESC; 
```
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_db_missing_index_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
