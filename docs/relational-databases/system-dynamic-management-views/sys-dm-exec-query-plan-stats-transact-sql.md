---
description: sys.dm_exec_query_plan_stats (Transact-SQL)
title: sys.dm_exec_query_plan_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_query_plan_stats
- sys.dm_exec_query_plan_stats_TSQL
- dm_exec_query_plan_stats_TSQL
- dm_exec_query_plan_stats
helpviewer_keywords:
- sys.dm_exec_query_plan_stats management view
ms.assetid: fdc7659e-df41-488e-b2b5-0d79734dfacb
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: c1f833351fd342629b858b19c113015e81ec3db1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072031"
---
# <a name="sysdm_exec_query_plan_stats-transact-sql"></a>sys.dm_exec_query_plan_stats (Transact-SQL)
[!INCLUDE[SQL Server 2019](../../includes/tsql-appliesto-ssver15-asdb-xxxx-xxx.md)]

Devuelve el equivalente del último plan de ejecución real conocido para un plan de consulta almacenado en caché previamente.

## <a name="syntax"></a>Sintaxis

```
sys.dm_exec_query_plan_stats(plan_handle)  
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

|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|
|**DBID**|**smallint**|Identificador de la base de datos de contexto que estaba activa al compilarse la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondiente a este plan. En el caso de instrucciones SQL ad hoc y preparadas, identificador de la base de datos en que se compilaron las instrucciones.<br /><br /> Esta columna acepta valores NULL.|  
|**objectid**|**int**|Identificador del objeto (por ejemplo, procedimiento almacenado o función definida por el usuario) de este plan de consulta. Para lotes "ad hoc" y preparados, esta columna es **null**.<br /><br /> Esta columna acepta valores NULL.|  
|**número**|**smallint**|Entero de procedimiento almacenado numerado. Por ejemplo, un grupo de procedimientos de la aplicación **orders** puede llamarse **orderproc;1**, **orderproc;2**, etc.  Para lotes "ad hoc" y preparados, esta columna es **null**.<br /><br /> Esta columna acepta valores NULL.|  
|**cifra**|**bit**|Indica si el procedimiento almacenado correspondiente está cifrado.<br /><br /> 0 = no cifrado<br /><br /> 1 = cifrado<br /><br /> La columna no acepta valores NULL.|  
|**query_plan**|**xml**|Contiene la última representación de SHOWPLAN en tiempo de ejecución conocida del plan de ejecución de consulta real que se especifica con *plan_handle*. El plan de presentación está en formato XML. Se genera un plan para cada lote que contiene, por ejemplo, instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] "ad hoc", llamadas a procedimientos almacenados y llamadas a funciones definidas por el usuario.<br /><br /> Esta columna acepta valores NULL.| 

## <a name="remarks"></a>Observaciones
Esta característica es opcional. Para habilitar en el nivel de servidor, use la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 2451. Para habilitar en el nivel de base de datos, use la opción LAST_QUERY_PLAN_STATS en [ALTER DATABASE scoped CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

Esta función del sistema funciona en la infraestructura de generación de perfiles de estadísticas de ejecución de consultas **ligeras** . Para obtener más información, vea [Infraestructura de generación de perfiles de consultas](../../relational-databases/performance/query-profiling-infrastructure.md).  

La salida del plan de presentación `sys.dm_exec_query_plan_stats` contiene la siguiente información:
-  Toda la información de tiempo de compilación que se encuentra en el plan almacenado en caché
-  Información en tiempo de ejecución, como el número real de filas por operador, el tiempo de la CPU de consulta total y el tiempo de ejecución, las advertencias de derrame, el DOP real, la memoria máxima usada y la memoria concedida

En las siguientes condiciones, se devuelve un resultado de SHOWPLAN **equivalente a un plan de ejecución real** en la columna **query_plan** de la tabla devuelta para `sys.dm_exec_query_plan_stats` :  

-   El plan se puede encontrar en [Sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md).     
    **AND**    
-   La consulta que se ejecuta es compleja o de consumo de recursos.

En las siguientes condiciones, se devuelve una salida del plan de presentación **simplificada <sup></sup>** en la **query_plan** columna de la tabla devuelta para `sys.dm_exec_query_plan_stats` :  

-   El plan se puede encontrar en [Sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md).     
    **AND**    
-   La consulta es lo suficientemente sencilla, normalmente clasificada como parte de una carga de trabajo OLTP.

<sup>1</sup> hace referencia a un plan de presentación que solo contiene el operador de nodo raíz (Select).

En las siguientes condiciones, **no se devuelve ninguna salida** de `sys.dm_exec_query_plan_stats` :

-   El plan de consulta especificado mediante `plan_handle` se ha expulsado de la caché del plan.     
    **OR**    
-   No se pudo almacenar en caché el plan de consulta en primer lugar. Para obtener más información, consulte [almacenamiento en caché y reutilización del plan de ejecución ](../../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse).
  
> [!NOTE] 
> Debido a una limitación en el número de niveles anidados permitidos en el tipo de datos **XML** , `sys.dm_exec_query_plan` no puede devolver planes de consulta que cumplan o superen los 128 niveles de elementos anidados. En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esta condición impedía la devolución del plan de consulta y generaba el [error 6335](../../relational-databases/errors-events/database-engine-events-and-errors.md#errors-6000-to-6999). En [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 y versiones posteriores, la `query_plan` columna devuelve NULL.  

## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW SERVER STATE` en el servidor.  

## <a name="examples"></a>Ejemplos  
  
### <a name="a-looking-at-last-known-actual-query-execution-plan-for-a-specific-cached-plan"></a>A. Examinando el último plan de ejecución de consultas real conocido para un plan almacenado en caché específico  
 En el ejemplo siguiente se realiza una consulta **Sys.dm_exec_cached_plans** para buscar el plan interesante y copiar su `plan_handle` desde la salida.  
  
```sql  
SELECT * FROM sys.dm_exec_cached_plans;  
GO  
```  
  
A continuación, para obtener el último plan de ejecución de consulta real conocido, use el `plan_handle` **Sys.dm_exec_query_plan_stats** copiado con la función del sistema.  
  
```sql  
SELECT * FROM sys.dm_exec_query_plan_stats(< copied plan_handle >);  
GO  
```   

### <a name="b-looking-at-last-known-actual-query-execution-plan-for-all-cached-plans"></a>B. Examinando el último plan de ejecución de consultas real conocido para todos los planes almacenados en caché
  
```sql  
SELECT *   
FROM sys.dm_exec_cached_plans AS cp
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS st
CROSS APPLY sys.dm_exec_query_plan_stats(plan_handle) AS qps;  
GO  
```   

### <a name="c-looking-at-last-known-actual-query-execution-plan-for-a-specific-cached-plan-and-query-text"></a>C. Examinando el último plan de ejecución de consulta real conocido para un plan y un texto de consulta específicos almacenados en caché

```sql  
SELECT *   
FROM sys.dm_exec_cached_plans AS cp
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS st
CROSS APPLY sys.dm_exec_query_plan_stats(plan_handle) AS qps
WHERE st.text LIKE 'SELECT * FROM Person.Person%';  
GO  
```   

### <a name="d-look-at-cached-events-for-trigger"></a>D. Examinar eventos en caché para desencadenador

```sql
SELECT *
FROM sys.dm_exec_cached_plans
CROSS APPLY sys.dm_exec_query_plan_stats(plan_handle)
WHERE objtype ='Trigger';
GO
```

## <a name="see-also"></a>Consulte también
  [Marcas de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con la ejecución &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  

