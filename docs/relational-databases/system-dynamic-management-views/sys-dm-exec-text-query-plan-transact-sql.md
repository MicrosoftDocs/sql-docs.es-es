---
description: sys.dm_exec_text_query_plan (Transact-SQL)
title: sys.dm_exec_text_query_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_text_query_plan
- sys.dm_exec_text_query_plan_TSQL
- dm_exec_text_query_plan_TSQL
- sys.dm_exec_text_query_plan
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_text_query_plan dynamic management function
ms.assetid: 9d5e5f59-6973-4df9-9eb2-9372f354ca57
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b1548ec0e134719e30dbc6a5ce85e1fec2395427
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342893"
---
# <a name="sysdm_exec_text_query_plan-transact-sql"></a>sys.dm_exec_text_query_plan (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve el plan de presentación en formato de texto para un lote [!INCLUDE[tsql](../../includes/tsql-md.md)] o para una instrucción concreta dentro del mismo. Este plan de consulta especificado por el identificador del plan puede estar almacenado en caché o ejecutándose. Esta función con valores de tabla es similar a [sys.dm_exec_query_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md), pero tiene las siguientes diferencias:  
  
-   El resultado del plan de consulta se devuelve en formato de texto.  
-   El resultado del plan de consulta no está limitado en tamaño.  
-   Se pueden especificar instrucciones individuales dentro del lote.  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sys.dm_exec_text_query_plan   
(   
    plan_handle   
    , { statement_start_offset | 0 | DEFAULT }  
        , { statement_end_offset | -1 | DEFAULT }  
)  
```  
  
## <a name="arguments"></a>Argumentos  
*plan_handle*  
Es un token que identifica de forma única un plan de ejecución de consulta para un lote que se ha ejecutado y su plan reside en la caché del plan, o se está ejecutando actualmente. *plan_handle* es **varbinary (64)**.   

El *plan_handle* se puede obtener de los siguientes objetos de administración dinámica: 
  
-   [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  
  
*statement_start_offset* | 0 | PREDETERMINADA  
Indica, en bytes, la posición inicial de la consulta que la fila describe en el texto del lote o del objeto permanente. *statement_start_offset* es de **tipo int**. Un valor de 0 indica el principio del lote. El valor predeterminado es 0.  
  
El desplazamiento inicial de la instrucción puede obtenerse de los siguientes objetos de administración dinámica:  
  
-  [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-  [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
*statement_end_offset* | -1 | PREDETERMINADA  
Indica, en bytes, la posición final de la consulta que la fila describe en el texto del lote o del objeto permanente.  
  
*statement_start_offset* es de **tipo int**.  
  
El valor -1 indica el final del lote. El valor predeterminado es -1.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DBID**|**smallint**|Identificador de la base de datos de contexto que estaba activa al compilarse la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondiente a este plan. En el caso de instrucciones SQL ad hoc y preparadas, identificador de la base de datos en que se compilaron las instrucciones.<br /><br /> Esta columna acepta valores NULL.|  
|**objectid**|**int**|Identificador del objeto (por ejemplo, procedimiento almacenado o función definida por el usuario) de este plan de consulta. Para lotes "ad hoc" y preparados, esta columna es **null**.<br /><br /> Esta columna acepta valores NULL.|  
|**número**|**smallint**|Entero de procedimiento almacenado numerado. Por ejemplo, un grupo de procedimientos de la aplicación **orders** puede llamarse **orderproc;1**, **orderproc;2**, etc.  Para lotes "ad hoc" y preparados, esta columna es **null**.<br /><br /> Esta columna acepta valores NULL.|  
|**cifra**|**bit**|Indica si el procedimiento almacenado correspondiente está cifrado.<br /><br /> 0 = no cifrado<br /><br /> 1 = cifrado<br /><br /> La columna no acepta valores NULL.|  
|**query_plan**|**nvarchar(max)**|Contiene la representación del plan de presentación de tiempo de compilación del plan de ejecución de consultas especificado con *plan_handle*. El plan de presentación está en formato de texto. Se genera un plan para cada lote que contiene, por ejemplo, instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] "ad hoc", llamadas a procedimientos almacenados y llamadas a funciones definidas por el usuario.<br /><br /> Esta columna acepta valores NULL.|  
  
## <a name="remarks"></a>Observaciones  
 En las siguientes condiciones, no se devuelve ningún plan de presentación en la columna **plan** de la tabla devuelta para **sys.dm_exec_text_query_plan**:  
  
-   Si el plan de consulta especificado mediante *plan_handle* se ha expulsado de la caché del plan, la columna **query_plan** de la tabla devuelta es NULL. Por ejemplo, esta condición puede producirse si hay un retraso entre el momento en que se captura el identificador del plan y el momento en que se utiliza con **sys.dm_exec_text_query_plan**.  
  
-   Algunas instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] no se almacenan en la caché, como, por ejemplo, instrucciones de operaciones masivas o instrucciones que contienen literales de cadenas de más de 8 KB. Los planes de presentación de dichas instrucciones no se pueden recuperar mediante **sys.dm_exec_text_query_plan** porque no existen en la caché.  
  
-   Si un [!INCLUDE[tsql](../../includes/tsql-md.md)] proceso por lotes o un procedimiento almacenado contiene una llamada a una función definida por el usuario o una llamada a SQL dinámico, por ejemplo, con exec (*String*), el plan de presentación XML compilado de la función definida por el usuario no se incluye en la tabla devuelta por **Sys.dm_exec_text_query_plan** para el lote o procedimiento almacenado. En su lugar, debe hacer una llamada independiente a **Sys.dm_exec_text_query_plan** para la *plan_handle* que corresponde a la función definida por el usuario.  
  
Cuando una consulta ad hoc usa [parametrización](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) [simple](../../relational-databases/query-processing-architecture-guide.md#SimpleParam) o forzada, la **query_plan** columna solo contendrá el texto de la instrucción y no el plan de consulta real. Para devolver el plan de consulta, llame a **sys.dm_exec_text_query_plan** con el identificador del plan de la consulta con parámetros preparada. Puede determinar si la consulta era con parámetros haciendo referencia a la columna **sql** de la vista [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) o a la columna de texto de la vista de administración dinámica [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar **sys.dm_exec_text_query_plan**, un usuario debe ser miembro del rol fijo de servidor **sysadmin** o disponer del permiso VIEW SERVER STATE en el servidor.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-retrieving-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>A. Recuperar el plan de consulta en la memoria caché de una consulta o proceso por lotes de Transact-SQL de ejecución lenta  
 Si una consulta o lote [!INCLUDE[tsql](../../includes/tsql-md.md)] se ejecuta durante mucho tiempo en una conexión determinada con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], recupere el plan de ejecución de dicha consulta o lote para averiguar la causa de la demora. En los siguientes ejemplos se muestra cómo recuperar el plan de presentación de una consulta o lote de ejecución lenta.  
  
> [!NOTE]  
> Para ejecutar este ejemplo, reemplace los valores de *session_id* y *plan_handle* por valores específicos de su servidor.  
  
 Primero, recupere el SPID (Id. de proceso del servidor) para el proceso que está ejecutando la consulta o proceso por lotes mediante el uso del procedimiento almacenado `sp_who`:  
  
```sql  
USE master;  
GO  
EXEC sp_who;  
GO  
```  
  
 El conjunto de resultados que devuelve `sp_who` indica que el SPID es `54`. Puede utilizar el SPID con la vista de administración dinámica `sys.dm_exec_requests` para recuperar el identificador del plan mediante la siguiente consulta:  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_requests  
WHERE session_id = 54;  
GO  
```  
  
 La tabla que devuelve **sys.dm_exec_requests** indica que el identificador del plan para la consulta o el lote de ejecución lenta es `0x06000100A27E7C1FA821B10600`. En el ejemplo siguiente se devuelve el plan de consulta del identificador del plan especificado y se usan los valores predeterminados 0 y -1 para devolver todas las instrucciones en la consulta o el lote.  
  
```sql  
USE master;  
GO  
SELECT query_plan   
FROM sys.dm_exec_text_query_plan (0x06000100A27E7C1FA821B10600,0,-1);  
GO  
```  
  
### <a name="b-retrieving-every-query-plan-from-the-plan-cache"></a>B. Recuperar todos los planes de consulta de caché del plan  
 Para recuperar una instantánea de todos los planes de consulta que residen en la caché del plan, recupere los identificadores de todos los planes de consulta de la caché; para ello, consulte la vista de administración dinámica `sys.dm_exec_cached_plans`. Los identificadores del plan se almacenan en la columna `plan_handle` de `sys.dm_exec_cached_plans`. Acto seguido, utilice el operador CROSS APPLY para pasar los identificadores del plan a `sys.dm_exec_text_query_plan` como se indica a continuación. La salida del plan de presentación de cada plan que se encuentra actualmente en la caché del plan está en la `query_plan` columna de la tabla que se devuelve.  
  
```sql  
USE master;  
GO  
SELECT *   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_text_query_plan(cp.plan_handle, DEFAULT, DEFAULT);  
GO  
```  
  
### <a name="c-retrieving-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>C. Recuperar todos los planes de consulta para los que el servidor ha reunido estadísticas de consultas procedentes de la memoria caché de plan  
 Para recuperar una instantánea de todos los planes de consulta para los que el servidor ha reunido estadísticas que residen actualmente en la caché del plan, recupere los identificadores de estos planes de la caché; para ello, consulte la vista de administración dinámica `sys.dm_exec_query_stats`. Los identificadores del plan se almacenan en la columna `plan_handle` de `sys.dm_exec_query_stats`. Acto seguido, utilice el operador CROSS APPLY para pasar los identificadores del plan a `sys.dm_exec_text_query_plan` como se indica a continuación. La salida del plan de presentación de cada plan está en la columna `query_plan` de la tabla que se devuelve.  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_query_stats AS qs   
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset);  
GO  
```  
  
### <a name="d-retrieving-information-about-the-top-five-queries-by-average-cpu-time"></a>D. Recuperación de la información sobre las cinco mejores consultas por promedio de tiempo de CPU  
 En el ejemplo siguiente se devuelven los planes de consulta y el promedio de tiempo de CPU de las cinco mejores consultas. La función **sys.dm_exec_text_query_plan** especifica los valores predeterminados 0 y -1 para devolver todas las instrucciones del lote del plan de consulta.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
Plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, 0, -1)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_exec_query_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)  
