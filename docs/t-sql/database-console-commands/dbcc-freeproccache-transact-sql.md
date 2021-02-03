---
description: DBCC FREEPROCCACHE (Transact-SQL)
title: DBCC FREEPROCCACHE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/13/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FREEPROCCACHE_TSQL
- FREEPROCCACHE
- DBCC_FREEPROCCACHE_TSQL
- DBCC FREEPROCCACHE
dev_langs:
- TSQL
helpviewer_keywords:
- freeing procedure cache
- removing procedure cache elements
- deleting procedure cache elements
- DBCC FREEPROCCACHE statement
- procedure cache [SQL Server]
- clearing procedure cache
ms.assetid: 0e09d210-6f23-4129-aedb-3d56b2980683
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1efc7b38c4f68a37c9c43b7dd00cfbf09c6dc277
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233773"
---
# <a name="dbcc-freeproccache-transact-sql"></a>DBCC FREEPROCCACHE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Quita todos los elementos de la memoria caché del plan, quita un plan concreto de la misma especificando un identificador de plan o un identificador SQL, o quita todas las entradas de caché asociadas a un grupo de recursos de servidor especificado.

> [!NOTE]
> DBCC FREEPROCCACHE no borra las estadísticas de ejecución para los procedimientos almacenados compilados de forma nativa. La memoria caché de procedimientos no contiene información sobre los procedimientos almacenados compilados de forma nativa. Las estadísticas de ejecución recopiladas de ejecuciones de procedimientos aparecerán en las DMV de estadísticas de ejecución: [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) y [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md).  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
Sintaxis para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

```sql
DBCC FREEPROCCACHE [ ( { plan_handle | sql_handle | pool_name } ) ] [ WITH NO_INFOMSGS ]  
```  

Sintaxis para [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:
  
```sql
DBCC FREEPROCCACHE [ ( COMPUTE | ALL ) ] 
     [ WITH NO_INFOMSGS ]   
[;]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 ( { *plan_handle* | *sql_handle* | *pool_name* } )  
*plan_handle* identifica de forma exclusiva un plan de consulta de un lote que se ha ejecutado y cuyo plan reside en la memoria caché del plan. *plan_handle* es **varbinary(64)** y se puede obtener de los siguientes objetos de administración dinámica:  
 -   [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  

*sql_handle* es el identificador SQL del lote que se va a borrar. *sql_handle* es **varbinary(64)** y se puede obtener de los siguientes objetos de administración dinámica:  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)  
 -   [sys.dm_exec_xml_handles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  

*pool_name* es el nombre de un grupo de recursos del regulador de recursos. *pool_name* es **sysname** y se puede obtener consultando la vista de administración dinámica [sys.dm_resource_governor_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
 Para asociar un grupo de cargas de trabajo del regulador de recursos a un grupo de recursos, consulte la vista de administración dinámica [sys.dm_resource_governor_workload_groups](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md). Para más información sobre el grupo de cargas de trabajo de una sesión, consulte la vista de administración dinámica [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md).  

  
 WITH NO_INFOMSGS  
 Suprime todos los mensajes de información.  
  
 COMPUTE  
 Purga la memoria caché del plan de consulta de cada nodo de ejecución. Este es el valor predeterminado.  
  
 ALL  
 Purga la memoria caché del plan de consulta de cada nodo de ejecución y del nodo de control.  

> [!NOTE]
> Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE` borra la memoria caché (de plan) de procedimientos de la base de datos en ámbito.

## <a name="remarks"></a>Comentarios  
Use DBCC FREEPROCCACHE con precaución para borrar la caché del plan. Borrar la memoria caché (de plan) de procedimientos hace que todos los planes se expulsen y las ejecuciones de consultas entrantes compilarán un nuevo plan, en lugar de volver a usar alguno de los planes previamente almacenados en caché. 

Como consecuencia, el rendimiento de las consultas puede disminuir de manera repentina y temporal a medida que el número de compilaciones nuevas vaya aumentando. Para cada almacén de caché borrado de la caché de planes, el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contendrá el siguiente mensaje informativo:

> `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to 'DBCC FREEPROCCACHE' or 'DBCC FREESYSTEMCACHE' operations.` 

Este mensaje se registra cada cinco minutos siempre que se vacíe la memoria caché dentro de ese intervalo de tiempo.

Las siguientes operaciones de reconfiguración también borran la caché de procedimientos:
-   access check cache bucket count  
-   access check cache quota  
-   clr enabled  
-   cost threshold for parallelism  
-   cross db ownership chaining  
-   index create memory  
-   max degree of parallelism  
-   memoria de servidor máxima  
-   max text repl size  
-   max worker threads  
-   memoria mínima por consulta  
-   memoria de servidor mínima  
-   query governor cost limit  
-   query wait  
-   remote query timeout  
-   user options  
  
## <a name="result-sets"></a>Conjuntos de resultados  
Si no se especifica la cláusula WITH NO_INFOMSGS, DBCC FREEPROCCACHE devuelve: "Ejecución de DBCC completada. Si DBCC imprime algún mensaje de error, póngase en contacto con su administrador del sistema."
  
## <a name="permissions"></a>Permisos  
Se aplica a: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 
- Requiere el permiso ALTER SERVER STATE en el servidor.  

Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]
- Requiere pertenencia al rol fijo de servidor DB_OWNER.  

## <a name="general-remarks-for-sssdw-and-sspdw"></a>Notas generales sobre [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Se pueden ejecutar varios comandos DBCC FREEPROCCACHE a la vez.
En [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], borrar la memoria caché de plan puede hacer que el rendimiento de las consultas disminuya temporalmente debido a que las consultas compilan un nuevo plan, en lugar de volver a usar alguno de los planes previamente almacenados en caché. 

DBCC FREEPROCCACHE (COMPUTE) solo hace que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuelva a compilar las consultas cuando se ejecuten en los nodos de ejecución. No hace que [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] vuelvan a compilar el plan de consulta paralelo que se genera en el nodo de control.
DBCC FREEPROCCACHE se puede cancelar durante su ejecución.
  
## <a name="limitations-and-restrictions-for-sssdw-and-sspdw"></a>Limitaciones y restricciones de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
DBCC FREEPROCCACHE no se puede ejecutar dentro de una transacción.
DBCC FREEPROCCACHE no se admite en una instrucción EXPLAIN.
  
## <a name="metadata-for-sssdw-and-sspdw"></a>Metadatos de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Cuando DBCC FREEPROCCACHE se ejecuta, se agrega una fila nueva a la vista del sistema sys.pdw_exec_requests.

## <a name="examples-ssnoversion"></a>Ejemplos: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
### <a name="a-clearing-a-query-plan-from-the-plan-cache"></a>A. Borrar un plan de consulta de la memoria caché del plan  
En el ejemplo siguiente se especifica el identificador de plan de consulta para borrar el plan de consulta de la caché del plan. Para asegurarse de que la consulta del ejemplo está en la caché del plan, la consulta se ejecuta primero. Se consultan las vistas de administración dinámicas `sys.dm_exec_cached_plans` y `sys.dm_exec_sql_text` para obtener el identificador de plan de la consulta. 

A continuación, el valor del identificador de plan del conjunto de resultados se inserta en la instrucción `DBCC FREEPROCACHE` para borrar únicamente dicho plan de la memoria caché del plan.
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM Person.Address;  
GO  
SELECT plan_handle, st.text  
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS st  
WHERE text LIKE N'SELECT * FROM Person.Address%';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
plan_handle                                         text  
--------------------------------------------------  -----------------------------  
0x060006001ECA270EC0215D05000000000000000000000000  SELECT * FROM Person.Address;  
  
(1 row(s) affected)
 ```
 
```sql  
-- Remove the specific plan from the cache.  
DBCC FREEPROCCACHE (0x060006001ECA270EC0215D05000000000000000000000000);  
GO  
```  
  
### <a name="b-clearing-all-plans-from-the-plan-cache"></a>B. Borrar todos los planes de la memoria caché del plan  
En el ejemplo siguiente se borran todos los elementos de la memoria caché del plan. La cláusula WITH `NO_INFOMSGS` se especifica para que no se muestre el mensaje informativo.
  
```sql  
DBCC FREEPROCCACHE WITH NO_INFOMSGS;  
```  
  
### <a name="c-clearing-all-cache-entries-associated-with-a-resource-pool"></a>C. Borrar todas las entradas de caché asociadas a un grupo de recursos  
En el ejemplo siguiente se borran todas las entradas de caché asociadas a un grupo de recursos de servidor especificado. Primero, se consulta la vista `sys.dm_resource_governor_resource_pools` para obtener el valor de *pool_name*.
  
```sql  
SELECT * FROM sys.dm_resource_governor_resource_pools;  
GO  
DBCC FREEPROCCACHE ('default');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-dbcc-freeproccache-basic-syntax-examples"></a>D. Ejemplos de sintaxis básica de DBCC FREEPROCCACHE  
En el siguiente ejemplo se quitan todas las memorias caché de plan de consulta existentes de los nodos de ejecución. Aunque el contexto está establecido en UserDbSales, se quitarán las memorias caché de plan de consulta de nodo de ejecución de todas las bases de datos. La cláusula WITH NO_INFOMSGS impide que se muestren mensajes informativos en los resultados.  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE) WITH NO_INFOMSGS;
```  
  
 El siguiente ejemplo tiene los mismos resultados que el ejemplo anterior, salvo por el hecho de que se muestran mensajes informativos en los resultados.  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE);  
```  
  
Cuando se solicitan mensajes informativos y la ejecución finaliza correctamente, los resultados de la consulta tendrán una línea por cada nodo de ejecución.
  
### <a name="e-granting-permission-to-run-dbcc-freeproccache"></a>E. Conceder permiso para ejecutar DBCC FREEPROCCACHE  
En el siguiente ejemplo se da permiso al usuario David para ejecutar DBCC FREEPROCCACHE.  
  
```sql
GRANT ALTER SERVER STATE TO David; 
GO
```  
  
## <a name="see-also"></a>Consulte también  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[Regulador de recursos](../../relational-databases/resource-governor/resource-governor.md)  
[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)
  
  
