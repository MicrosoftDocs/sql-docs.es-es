---
description: sys.dm_exec_sql_text (Transact-SQL)
title: sys.dm_exec_sql_text (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_sql_text
- sys.dm_exec_sql_text
- sys.dm_exec_sql_text_TSQL
- dm_exec_sql_text_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_sql_text dynamic management function
ms.assetid: 61b8ad6a-bf80-490c-92db-58dfdff22a24
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 089633aa2fb51a727d861d987bf9a628408b2540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192937"
---
# <a name="sysdm_exec_sql_text-transact-sql"></a>sys.dm_exec_sql_text (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el texto del lote SQL que se identifica mediante el *sql_handle* especificado. Esta función con valores de tabla reemplaza a la función del sistema **fn_get_sql**.  
  
 
## <a name="syntax"></a>Sintaxis  
  
```  
sys.dm_exec_sql_text(sql_handle | plan_handle)  
```  
  
## <a name="arguments"></a>Argumentos  
*sql_handle*  
Es un token que identifica de forma única un lote que ha ejecutado o que se está ejecutando actualmente. *sql_handle* es **varbinary (64)**. 

El *sql_handle* se puede obtener de los siguientes objetos de administración dinámica:  
  
-   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
-   [sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)  
  
-   [sys.dm_exec_xml_handles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)  
  
-   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
  
-   [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)  
  
*plan_handle*  
Es un token que identifica de forma única un plan de ejecución de consulta para un lote que se ha ejecutado y su plan reside en la caché del plan, o se está ejecutando actualmente. *plan_handle* es **varbinary (64)**.   

El *plan_handle* se puede obtener de los siguientes objetos de administración dinámica:    
  
-   [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)   
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DBID**|**smallint**|Identificador de la base de datos.<br /><br /> En el caso de instrucciones SQL ad hoc y preparadas, identificador de la base de datos en que se compilaron las instrucciones.|  
|**objectid**|**int**|Identificador del objeto.<br /><br /> Este valor es NULL para las instrucciones SQL ad hoc y preparadas.|  
|**número**|**smallint**|En un procedimiento almacenado numerado, esta columna devuelve el número del procedimiento almacenado. Para obtener más información, vea [sys.numbered_procedures &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql.md).<br /><br /> Este valor es NULL para las instrucciones SQL ad hoc y preparadas.|  
|**cifra**|**bit**|1 = El texto SQL está cifrado.<br /><br /> 0 = El texto SQL no está cifrado.|  
|**text**|**nvarchar (Max** **)**|Texto de la consulta de SQL.<br /><br /> Este valor es NULL para objetos cifrados.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW SERVER STATE` en el servidor.  
  
## <a name="remarks"></a>Observaciones  
En el caso de las consultas ad hoc, los identificadores SQL son valores hash basados en el texto SQL que se envía al servidor y pueden proceder de cualquier base de datos. 

Para los objetos de base de datos, como procedimientos almacenados, desencadenadores o funciones, los identificadores SQL se derivan del identificador de base de datos, del identificador de objeto y del número de objeto. 

El identificador del plan es un valor hash derivado del plan compilado del lote completo. 

> [!NOTE]
> no se puede determinar **DBID** a partir de *sql_handle* para consultas ad hoc. Para determinar el **DBID** para las consultas ad hoc, use *plan_handle* en su lugar.
  
## <a name="examples"></a>Ejemplos 

### <a name="a-conceptual-example"></a>A. Ejemplo conceptual
A continuación se muestra un ejemplo básico para ilustrar el paso de un **sql_handle** directamente o con **Cross Apply**.
  1.  Crear actividad.  
Ejecute el siguiente T-SQL en una nueva ventana de consulta de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .   
      ```sql
      -- Identify current spid (session_id)
      SELECT @@SPID;
      GO
  
      -- Create activity
        WAITFOR DELAY '00:02:00';
      ```
      
  2.  Usar **Cross Apply**.  
    La sql_handle de **Sys.dm_exec_requests** se pasará a **Sys.DM_EXEC_SQL_TEXT** mediante **Cross Apply**. Abra una nueva ventana de consulta y pase el SPID identificado en el paso 1. En este ejemplo, el SPID es `59` .

        ```sql
        SELECT t.*
        FROM sys.dm_exec_requests AS r
        CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) AS t
        WHERE session_id = 59 -- modify this value with your actual spid
         ```      
 
  2.  Pasar **sql_handle** directamente.  
Adquiera el **sql_handle** de **Sys.dm_exec_requests**. A continuación, pase el **sql_handle** directamente a **Sys.dm_exec_sql_text**. Abra una nueva ventana de consulta y pase el SPID identificado en el paso 1 a **Sys.dm_exec_requests**. En este ejemplo, el SPID es `59` . A continuación, pase el **sql_handle** devuelto como argumento a **Sys.dm_exec_sql_text**.

        ```sql
        -- acquire sql_handle
        SELECT sql_handle FROM sys.dm_exec_requests WHERE session_id = 59  -- modify this value with your actual spid
        
        -- pass sql_handle to sys.dm_exec_sql_text
        SELECT * FROM sys.dm_exec_sql_text(0x01000600B74C2A1300D2582A2100000000000000000000000000000000000000000000000000000000000000) -- modify this value with your actual sql_handle
         ```      
    
  
### <a name="b-obtain-information-about-the-top-five-queries-by-average-cpu-time"></a>B. Obtener información acerca de las cinco primeras consultas por promedio de tiempo de CPU  
 El ejemplo siguiente devuelve el texto de la instrucción SQL y el promedio de tiempo de CPU de las cinco mejores consultas.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
    SUBSTRING(st.text, (qs.statement_start_offset/2)+1,   
        ((CASE qs.statement_end_offset  
          WHEN -1 THEN DATALENGTH(st.text)  
         ELSE qs.statement_end_offset  
         END - qs.statement_start_offset)/2) + 1) AS statement_text  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
ORDER BY total_worker_time/execution_count DESC;  
```  
  
### <a name="c-provide-batch-execution-statistics"></a>C. Proporcionar estadísticas de ejecución por lotes  
 El ejemplo siguiente devuelve el texto de consultas SQL que se están ejecutando por lotes y proporciona información estadística sobre ellas.  
  
```sql  
SELECT s2.dbid,   
    s1.sql_handle,    
    (SELECT TOP 1 SUBSTRING(s2.text,statement_start_offset / 2+1 ,   
      ( (CASE WHEN statement_end_offset = -1   
         THEN (LEN(CONVERT(nvarchar(max),s2.text)) * 2)   
         ELSE statement_end_offset END)  - statement_start_offset) / 2+1))  AS sql_statement,  
    execution_count,   
    plan_generation_num,   
    last_execution_time,     
    total_worker_time,   
    last_worker_time,   
    min_worker_time,   
    max_worker_time,  
    total_physical_reads,   
    last_physical_reads,   
    min_physical_reads,    
    max_physical_reads,    
    total_logical_writes,   
    last_logical_writes,   
    min_logical_writes,   
    max_logical_writes    
FROM sys.dm_exec_query_stats AS s1   
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS s2    
WHERE s2.objectid is null   
ORDER BY s1.sql_handle, s1.statement_start_offset, s1.statement_end_offset;  
```  
  
## <a name="see-also"></a>Vea también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con la ejecución &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_cursors &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)   
 [sys.dm_exec_xml_handles &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [Usar APPLY](../../t-sql/queries/from-transact-sql.md#using-apply)   
 [sys.dm_exec_text_query_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  

