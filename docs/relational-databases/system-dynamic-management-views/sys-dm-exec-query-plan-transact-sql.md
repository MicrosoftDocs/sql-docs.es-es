---
description: sys.dm_exec_query_plan (Transact-SQL)
title: sys.dm_exec_query_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_query_plan_TSQL
- sys.dm_exec_query_plan
- dm_exec_query_plan
- sys.dm_exec_query_plan_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_plan dynamic management function
ms.assetid: e26f0867-9be3-4b2e-969e-7f2840230770
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ca28e5540112af972ec84db2cb2a25a36a8ba832
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161385"
---
# <a name="sysdm_exec_query_plan-transact-sql"></a>sys.dm_exec_query_plan (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve el plan de presentación en formato XML para el lote especificado por el identificador del plan. Este plan especificado por el identificador del plan puede estar almacenado en caché o ejecutándose.  
  
El esquema XML del plan de presentación se publica y está disponible en [este sitio web de Microsoft](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409). También está disponible en el directorio de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sys.dm_exec_query_plan(plan_handle)  
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
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DBID**|**smallint**|Identificador de la base de datos de contexto que estaba activa al compilarse la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondiente a este plan. En el caso de instrucciones SQL ad hoc y preparadas, identificador de la base de datos en que se compilaron las instrucciones.<br /><br /> Esta columna acepta valores NULL.|  
|**objectid**|**int**|Identificador del objeto (por ejemplo, procedimiento almacenado o función definida por el usuario) de este plan de consulta. Para lotes "ad hoc" y preparados, esta columna es **null**.<br /><br /> Esta columna acepta valores NULL.|  
|**número**|**smallint**|Entero de procedimiento almacenado numerado. Por ejemplo, un grupo de procedimientos de la aplicación **orders** puede llamarse **orderproc;1**, **orderproc;2**, etc.  Para lotes "ad hoc" y preparados, esta columna es **null**.<br /><br /> Esta columna acepta valores NULL.|  
|**cifra**|**bit**|Indica si el procedimiento almacenado correspondiente está cifrado.<br /><br /> 0 = no cifrado<br /><br /> 1 = cifrado<br /><br /> La columna no acepta valores NULL.|  
|**query_plan**|**xml**|Contiene la representación del plan de presentación de tiempo de compilación del plan de ejecución de consultas especificado con *plan_handle*. El plan de presentación está en formato XML. Se genera un plan para cada lote que contiene, por ejemplo, instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] "ad hoc", llamadas a procedimientos almacenados y llamadas a funciones definidas por el usuario.<br /><br /> Esta columna acepta valores NULL.|  
  
## <a name="remarks"></a>Observaciones  
 En las siguientes condiciones, no se devuelve ningún plan de presentación en la columna **query_plan** de la tabla devuelta para **sys.dm_exec_query_plan**:  
  
-   Si el plan de consulta especificado mediante *plan_handle* se ha expulsado de la caché del plan, la columna **query_plan** de la tabla devuelta es NULL. Por ejemplo, esta condición puede producirse si hay un retraso entre el momento en que se captura el identificador del plan y el momento en que se utiliza con **sys.dm_exec_query_plan**.  
  
-   Algunas instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] no se almacenan en la caché, como, por ejemplo, instrucciones de operaciones masivas o instrucciones que contienen literales de cadenas de más de 8 KB. Los planes de presentación XML de dichas instrucciones no se pueden recuperar mediante **sys.dm_exec_query_plan** a menos que el lote esté ejecutándose en ese momento porque no existen en la caché.  
  
-   Si un [!INCLUDE[tsql](../../includes/tsql-md.md)] proceso por lotes o un procedimiento almacenado contiene una llamada a una función definida por el usuario o una llamada a SQL dinámico, por ejemplo, con exec (*String*), el plan de presentación XML compilado de la función definida por el usuario no se incluye en la tabla devuelta por **Sys.dm_exec_query_plan** para el lote o procedimiento almacenado. En su lugar, debe realizar una llamada independiente a **Sys.dm_exec_query_plan** para el identificador de plan que corresponda a la función definida por el usuario.  
  
 Cuando una consulta "ad hoc" usa parametrización simple o forzada, la columna **query_plan** contendrá únicamente el texto de la instrucción y no el plan de consulta real. Para devolver el plan de consulta, llame a **sys.dm_exec_query_plan** con el identificador del plan de la consulta con parámetros preparada. Puede determinar si la consulta era con parámetros haciendo referencia a la columna **sql** de la vista [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) o a la columna de texto de la vista de administración dinámica [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
> [!NOTE] 
> Debido a una limitación en el número de niveles anidados permitidos en el tipo de datos **XML** , **Sys.dm_exec_query_plan** no pueden devolver planes de consulta que cumplan o superen los 128 niveles de elementos anidados. En las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta condición impedía la devolución del plan de consulta y generaba el error 6335. En [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 y versiones posteriores, la columna **QUERY_PLAN** devuelve NULL.   
> Puede usar la sys.dm_exec_text_query_plan &#40;función de administración dinámica [&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md) para devolver la salida del plan de consulta en formato de texto.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar **Sys.dm_exec_query_plan**, un usuario debe ser miembro del rol fijo de servidor **sysadmin** o tener el `VIEW SERVER STATE` permiso en el servidor.  
  
## <a name="examples"></a>Ejemplos  
 En los siguientes ejemplos se muestra cómo usar la vista de administración dinámica **sys.dm_exec_query_plan**.  
  
 Para ver los planes de presentación XML, ejecute las siguientes consultas en el Editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y, a continuación, haga clic en **ShowPlanXML** en la columna **query_plan** de la tabla devuelta por **sys.dm_exec_query_plan**. El plan de presentación XML se muestra en el panel de resumen de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Para guardar el plan de presentación XML en un archivo, haga clic con el botón secundario en **ShowPlanXML** en la columna **query_plan** , haga clic en **Guardar resultados como**, asigne al archivo el nombre format \<*file_name*> . sqlplan; por ejemplo, miplandepresentaciónxml. sqlplan.  
  
### <a name="a-retrieve-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>A. Recuperar el plan de consulta en la caché de una consulta o proceso por lotes de Transact-SQL de ejecución lenta  
 Los planes de consulta de varios tipos de lotes [!INCLUDE[tsql](../../includes/tsql-md.md)], como los lotes "ad hoc", procedimientos almacenados y funciones definidas por el usuario, se almacenan en un área de la memoria caché denominada caché del plan. Cada plan de consulta almacenado en la caché está identificado mediante un identificador exclusivo denominado identificador del plan. Puede especificar este identificador del plan mediante la vista de administración dinámica **sys.dm_exec_query_plan** para recuperar el plan de ejecución de una consulta o lote [!INCLUDE[tsql](../../includes/tsql-md.md)] determinado.  
  
 Si una consulta o lote [!INCLUDE[tsql](../../includes/tsql-md.md)] se ejecuta durante mucho tiempo en una conexión determinada con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], recupere el plan de ejecución de dicha consulta o lote para averiguar la causa de la demora. En los siguientes ejemplos se muestra cómo recuperar el plan de presentación XML de una consulta o proceso por lotes de ejecución lenta.  
  
> [!NOTE]  
>  Para ejecutar este ejemplo, reemplace los valores de *session_id* y *plan_handle* por valores específicos de su servidor.  
  
 Primero, recupere el SPID (Id. de proceso del servidor) para el proceso que está ejecutando la consulta o proceso por lotes mediante el uso del procedimiento almacenado `sp_who`:  
  
```sql  
USE master;  
GO  
exec sp_who;  
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
  
 La tabla devuelta por **Sys.dm_exec_requests** indica que el identificador del plan para la consulta o el lote de ejecución lenta es `0x06000100A27E7C1FA821B10600` , que puede especificar como el *plan_handle* argumento con `sys.dm_exec_query_plan` para recuperar el plan de ejecución en formato XML como se indica a continuación. El plan de ejecución en formato XML de la consulta o proceso por lotes de ejecución lenta se muestra en la columna **query_plan** de la tabla devuelta por `sys.dm_exec_query_plan`.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_plan (0x06000100A27E7C1FA821B10600);  
GO  
```  
  
### <a name="b-retrieve-every-query-plan-from-the-plan-cache"></a>B. Recuperar todos los planes de consulta de la memoria caché del plan  
 Para recuperar una instantánea de todos los planes de consulta que residen en la caché del plan, recupere los identificadores de todos los planes de consulta de la caché; para ello, consulte la vista de administración dinámica `sys.dm_exec_cached_plans`. Los identificadores del plan se almacenan en la columna `plan_handle` de `sys.dm_exec_cached_plans`. Acto seguido, utilice el operador CROSS APPLY para pasar los identificadores del plan a `sys.dm_exec_query_plan` como se indica a continuación. La salida del plan de presentación XML de todos los planes almacenados actualmente en la caché del plan se muestra en la columna `query_plan` de la tabla devuelta.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_cached_plans AS cp 
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
GO  
```  
  
### <a name="c-retrieve-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>C. Recuperar todos los planes de consulta para los que el servidor ha reunido estadísticas de consultas procedentes de la memoria caché del plan  
 Para recuperar una instantánea de todos los planes de consulta para los que el servidor ha reunido estadísticas que residen actualmente en la caché del plan, recupere los identificadores de estos planes de la caché; para ello, consulte la vista de administración dinámica `sys.dm_exec_query_stats`. Los identificadores del plan se almacenan en la columna `plan_handle` de `sys.dm_exec_query_stats`. Acto seguido, utilice el operador CROSS APPLY para pasar los identificadores del plan a `sys.dm_exec_query_plan` como se indica a continuación. La salida del plan de presentación XML de todos los planes para los que el servidor ha reunido estadísticas almacenadas actualmente en la caché del plan se muestra en la columna `query_plan` de la tabla devuelta.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_stats AS qs 
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle);  
GO  
```  
  
### <a name="d-retrieve-information-about-the-top-five-queries-by-average-cpu-time"></a>D. Obtener información acerca de las cinco mejores consultas por promedio de tiempo de CPU  
 En el ejemplo siguiente se devuelven los planes y el promedio de tiempo de CPU de las cinco mejores consultas.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
   plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Referencia de operadores lógicos y físicos del plan de presentación](../../relational-databases/showplan-logical-and-physical-operators-reference.md)   
 [sys.dm_exec_text_query_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  
  
  
