---
title: Supervisión del rendimiento de los procedimientos almacenados compilados de forma nativa
description: Obtenga información sobre cómo supervisar el rendimiento de los procedimientos almacenados compilados de forma nativa y de otros módulos de T-SQL compilados de forma nativa.
ms.custom: seo-dt-2019
ms.date: 04/03/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 55548cb2-77a8-4953-8b5a-f2778a4f13cf
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d87657183c53568781785cf2aa2e85ade81fbb18
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460408"
---
# <a name="monitoring-performance-of-natively-compiled-stored-procedures"></a>Supervisar el rendimiento de los procedimientos almacenados compilados de forma nativa

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este artículo se describe cómo supervisar el rendimiento de los procedimientos almacenados compilados de forma nativa y de otros módulos de T-SQL compilados de forma nativa.  
  
## <a name="using-extended-events"></a>Utilizar eventos extendidos  
 Use el evento extendido **sp_statement_completed** para realizar el seguimiento de la ejecución de una consulta. Cree una sesión de eventos extendidos con este evento, opcionalmente con un filtro en object_id para un procedimiento almacenado compilado de forma nativa específico. El evento extendido se genera después de la ejecución de cada consulta. El tiempo de CPU y la duración notificados por el evento extendido indican cuánta CPU utilizó la consulta y el tiempo de ejecución. Un procedimiento almacenado compilado de forma nativa que utiliza mucho tiempo de CPU puede tener problemas de rendimiento.  
  
Se puede usar **line_number**, junto con el valor **object_id** en el evento extendido para investigar la consulta. La siguiente consulta se puede utilizar para recuperar la definición del procedimiento. El número de línea se puede utilizar para identificar la consulta dentro de la definición:  
  
```sql  
SELECT [definition]
FROM sys.sql_modules
WHERE object_id=object_id;
```  
    
## <a name="using-data-management-views-and-query-store"></a>Usar vistas de administración de datos y el Almacén de consultas
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] admiten la recopilación de estadísticas de ejecución para los procedimientos almacenados compilados de forma nativa, tanto en el nivel de procedimiento como en el nivel de consulta. La recopilación de estadísticas de ejecución no está habilitada de forma predeterminada debido al impacto que tiene sobre el rendimiento.  

Las estadísticas de ejecución se reflejan en las vistas del sistema [sys.dm_exec_procedure_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) y [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md), así como en el [Almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

## <a name="procedure-level-execution-statistics"></a>Estadísticas de ejecución a nivel de procedimiento

**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : habilite o deshabilite la recopilación de estadísticas en los procedimientos almacenados compilados de forma nativa a nivel de procedimiento mediante [sys.sp_xtp_control_proc_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md).  La siguiente instrucción habilita la recopilación de estadísticas de ejecución a nivel de procedimiento de todos los módulos de T-SQL compilados de forma nativa en la instancia actual:

```sql
EXEC sys.sp_xtp_control_proc_exec_stats 1
```

**[!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]** y **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : habilite o deshabilite la recopilación de estadísticas en los procedimientos almacenados compilados de forma nativa a nivel de procedimiento usando la opción de [configuración con ámbito de base de datos](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`XTP_PROCEDURE_EXECUTION_STATISTICS`. La siguiente instrucción habilita la recopilación de estadísticas de ejecución a nivel de procedimiento de todos los módulos de T-SQL compilados de forma nativa en la base de datos actual:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET XTP_PROCEDURE_EXECUTION_STATISTICS = ON;
```

## <a name="query-level-execution-statistics"></a>Estadísticas de ejecución a nivel de consulta

**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : habilite o deshabilite la recopilación de estadísticas en los procedimientos almacenados compilados de forma nativa a nivel de consulta mediante [sys.sp_xtp_control_query_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).  La siguiente instrucción habilita la recopilación de estadísticas de ejecución a nivel de consulta de todos los módulos de T-SQL compilados de forma nativa en la instancia actual:

```sql
EXEC sys.sp_xtp_control_query_exec_stats 1
```

**[!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]** y **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : habilite o deshabilite la recopilación de estadísticas en los procedimientos almacenados compilados de forma nativa a nivel de instrucción usando la opción de [configuración con ámbito de base de datos](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)`XTP_QUERY_EXECUTION_STATISTICS`. La siguiente instrucción habilita la recopilación de estadísticas de ejecución a nivel de consulta de todos los módulos de T-SQL compilados de forma nativa en la base de datos actual:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET XTP_QUERY_EXECUTION_STATISTICS = ON;
```

## <a name="sample-queries"></a>Consultas de ejemplo

 Después de recopilar las estadísticas, se pueden consultar las estadísticas de ejecución de los procedimientos almacenados compilados de forma nativa de un procedimiento con [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) y de las consultas con [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md).  
 
  
 La consulta siguiente devuelve los nombres de los procedimientos y las estadísticas de ejecución para los procedimientos almacenados compilados de forma nativa de la base de datos actual, después de la recopilación de estadísticas:  

```sql
SELECT object_id, object_name(object_id) AS 'object name',
       cached_time, last_execution_time, execution_count,
       total_worker_time, last_worker_time,
       min_worker_time, max_worker_time,
       total_elapsed_time, last_elapsed_time,
       min_elapsed_time, max_elapsed_time
FROM sys.dm_exec_procedure_stats
WHERE database_id = DB_ID()
      AND object_id IN (SELECT object_id FROM sys.sql_modules WHERE uses_native_compilation = 1)
ORDER BY total_worker_time desc;
```

La consulta siguiente devuelve el texto de la consulta y las estadísticas de ejecución para todas las consultas de procedimientos almacenados compilados de forma nativa de la base de datos actual para la que se han recopilado estadísticas, ordenadas por tiempo total de trabajo, en orden descendente:  

```sql
SELECT st.objectid,
        OBJECT_NAME(st.objectid) AS 'object name',
        SUBSTRING(
            st.text,
            (qs.statement_start_offset/2) + 1,
            ((qs.statement_end_offset-qs.statement_start_offset)/2) + 1
            ) AS 'query text',
        qs.creation_time, qs.last_execution_time, qs.execution_count,
        qs.total_worker_time, qs.last_worker_time, qs.min_worker_time, 
        qs.max_worker_time, qs.total_elapsed_time, qs.last_elapsed_time,
        qs.min_elapsed_time, qs.max_elapsed_time
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(sql_handle) st
WHERE database_id = DB_ID()
      AND object_id IN (SELECT object_id FROM sys.sql_modules WHERE uses_native_compilation = 1)
ORDER BY total_worker_time desc;
```

## <a name="query-execution-plans"></a>Planes de ejecución de consulta

 Los procedimientos almacenados compilados de forma nativa admiten SHOWPLAN_XML (plan de ejecución estimado). El plan de ejecución estimado se puede utilizar para inspeccionar el plan de consulta con el fin de detectar cualquier problema de plan incorrecto. Los motivos más frecuentes de los planes no válidos son:  
  
-   Las estadísticas no se actualizaron antes de que se creara el procedimiento.  
  
-   Faltan índices  
  
 Showplan XML se obtiene ejecutando la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]siguiente:  
  
```sql  
SET SHOWPLAN_XML ON  
GO  
EXEC my_proc   
GO  
SET SHOWPLAN_XML OFF  
GO  
```  
  
 O bien, en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], seleccione el nombre del procedimiento y haga clic en **Mostrar plan de ejecución estimado**.  
  
 El plan de ejecución estimado para los procedimientos almacenados compilados de forma nativa muestra los operadores y las expresiones de consulta para las consultas del procedimiento. [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] no admite todos los atributos SHOWPLAN_XML para los procedimientos almacenados compilados de forma nativa. Por ejemplo, los atributos relacionados con el costo del optimizador de consultas no forman parte de SHOWPLAN_XML para el procedimiento.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados compilados de forma nativa](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
