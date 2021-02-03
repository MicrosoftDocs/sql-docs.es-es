---
title: SELECT (Transact-SQL)
description: SELECT (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SELECT_TSQL
- SELECT
dev_langs:
- TSQL
helpviewer_keywords:
- retrieving rows
- SELECT statement [SQL Server]
- SELECT statement [SQL Server], about SELECT statement
- row retrieval [SQL Server], SELECT statement
- DML [SQL Server], SELECT statement
- data manipulation language [SQL Server], SELECT statement
- row retrieval [SQL Server]
- queries [SQL Server], results
author: VanMSFT
ms.author: vanto
ms.custom: ''
ms.date: 10/24/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5bc947add5b9413d4935cd3e6ddc517dfbc30cd5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204347"
---
# <a name="select-transact-sql"></a>SELECT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Recupera filas de la base de datos y habilita la selección de una o varias filas o columnas de una o varias tablas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La sintaxis completa de la instrucción SELECT es compleja, aunque las cláusulas principales se pueden resumir del modo siguiente:  
  
[ WITH { [[ XMLNAMESPACES ,]](../../t-sql/xml/with-xmlnamespaces.md) [[ \<common_table_expression> ]](../../t-sql/queries/with-common-table-expression-transact-sql.md) } ]
  
 [SELECT *select_list*](../../t-sql/queries/select-clause-transact-sql.md) [ [INTO *new_table*](../../t-sql/queries/select-into-clause-transact-sql.md) ]  
  
 [ [FROM *table_source*](../../t-sql/queries/from-transact-sql.md) ] [ [WHERE *search_condition*](../../t-sql/queries/where-transact-sql.md) ]  
  
 [ [GROUP BY *group_by_expression*](../../t-sql/queries/select-group-by-transact-sql.md) ]  
  
 [ [HAVING *search_condition*](../../t-sql/queries/select-having-transact-sql.md) ]  
  
 [ [ORDER BY *order_expression* [ ASC | DESC ]](../../t-sql/queries/select-order-by-clause-transact-sql.md)]  
  
 Los operadores [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md), [EXCEPT e INTERSECT](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md) se pueden usar entre consultas para combinar o comparar resultados en un conjunto de resultados.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
<SELECT statement> ::=    
    [ WITH { [ XMLNAMESPACES ,] [ <common_table_expression> [,...n] ] } ]  
    <query_expression>   
    [ ORDER BY <order_by_expression> ] 
    [ <FOR Clause>]   
    [ OPTION ( <query_hint> [ ,...n ] ) ]   
<query_expression> ::=   
    { <query_specification> | ( <query_expression> ) }   
    [  { UNION [ ALL ] | EXCEPT | INTERSECT }  
        <query_specification> | ( <query_expression> ) [...n ] ]   
<query_specification> ::=   
SELECT [ ALL | DISTINCT ]   
    [TOP ( expression ) [PERCENT] [ WITH TIES ] ]   
    < select_list >   
    [ INTO new_table ]   
    [ FROM { <table_source> } [ ,...n ] ]   
    [ WHERE <search_condition> ]   
    [ <GROUP BY> ]   
    [ HAVING < search_condition > ]   
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ WITH <common_table_expression> [ ,...n ] ]  
SELECT <select_criteria>  
[;]  
  
<select_criteria> ::=  
    [ TOP ( top_expression ) ]   
    [ ALL | DISTINCT ]   
    { * | column_name | expression } [ ,...n ]   
    [ FROM { table_source } [ ,...n ] ]  
    [ WHERE <search_condition> ]   
    [ GROUP BY <group_by_clause> ]   
    [ HAVING <search_condition> ]   
    [ ORDER BY <order_by_expression> ]  
    [ OPTION ( <query_option> [ ,...n ] ) ]  
  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Observaciones  
 Debido a la complejidad de la instrucción SELECT, se muestran elementos y argumentos detallados de la sintaxis de cada cláusula:  

:::row:::
    :::column:::
        [WITH XMLNAMESPACES](../../t-sql/xml/with-xmlnamespaces.md)
    :::column-end:::
    :::column:::
        [HAVING](../../t-sql/queries/select-having-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [WITH common_table_expression](../../t-sql/queries/with-common-table-expression-transact-sql.md)
    :::column-end:::
    :::column:::
        [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [Cláusula SELECT](../../t-sql/queries/select-clause-transact-sql.md)
    :::column-end:::
    :::column:::
        [EXCEPT e INTERSECT](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [Cláusula INTO](../../t-sql/queries/select-into-clause-transact-sql.md)
    :::column-end:::
    :::column:::
        [ORDER BY](../../t-sql/queries/select-order-by-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [FROM](../../t-sql/queries/from-transact-sql.md)
    :::column-end:::
    :::column:::
        [Cláusula FOR](../../t-sql/queries/select-for-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [WHERE](../../t-sql/queries/where-transact-sql.md)
    :::column-end:::
    :::column:::
        [Cláusula OPTION](../../t-sql/queries/option-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

 El orden de las cláusulas en la instrucción SELECT es importante. Se puede omitir cualquiera de las cláusulas opcionales pero, cuando se utilizan, deben aparecer en el orden apropiado.  
  
 Las instrucciones SELECT se permiten en las funciones definidas por el usuario solo si las listas de selección de estas instrucciones contienen expresiones que asignan valores a variables locales de las funciones.  
  
 Un nombre de cuatro partes creado con la función OPENDATASOURCE como la parte de nombre de servidor se puede usar como origen de tabla en todos los lugares de una instrucción SELECT donde pueda aparecer un nombre de tabla. No se puede especificar un nombre de cuatro partes para [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Existen algunas restricciones sintácticas en las instrucciones SELECT relacionadas con las tablas remotas.  
  
## <a name="logical-processing-order-of-the-select-statement"></a>Orden de procesamiento lógico de la instrucción SELECT  
 Los pasos siguientes muestran el orden de procesamiento lógico, u orden de enlaces, de una instrucción SELECT. Este orden determina cuándo los objetos definidos en un paso están disponibles para las cláusulas en pasos posteriores. Por ejemplo, si el procesador de consultas puede enlazar (obtener acceso) a las tablas o las vistas definidas en la cláusula FROM, estos objetos y sus columnas están disponibles para todos los pasos siguientes. Por el contrario, dado que la cláusula SELECT es el paso 8, las cláusulas anteriores no pueden hacer referencia a los alias de columna o columnas derivadas definidas en esa cláusula. Sin embargo, las cláusulas siguientes, tales como la cláusula ORDER BY, sí pueden hacer referencia. La ejecución física real de la instrucción está determinada por el procesador de consultas y el orden puede variar en esta lista.  
  
1.  FROM  
2.  ACTIVAR  
3.  JOIN  
4.  WHERE  
5.  GROUP BY  
6.  WITH CUBE o WITH ROLLUP  
7.  HAVING  
8.  SELECT  
9. DISTINCT  
10. ORDER BY  
11. TOP  

> [!WARNING]
> La secuencia anterior suele ser la habitual, pero hay casos poco comunes en los que puede ser distinta.
>
> Por ejemplo, imagine que tiene un índice agrupado en una vista y esa vista excluye algunas filas de la tabla, mientras que la lista de columnas SELECT de la vista usa una instrucción CONVERT que cambia un tipo de datos de *varchar* a *integer*. En esta situación, la función CONVERT se puede ejecutar antes de la cláusula WHERE, lo cual es verdaderamente poco habitual. A menudo, suele haber una manera de modificar la vista que evita que la secuencia sea diferente, si esto es importante en su caso. 

## <a name="permissions"></a>Permisos  
 La selección de datos necesita el permiso **SELECT** en la tabla o en la vista, que se puede heredar de un ámbito superior como el permiso **SELECT** en el esquema o el permiso **CONTROL** en la tabla. Debe pertenecer a los roles fijos de base de datos **db_datareader** o **db_owner** o al rol fijo de servidor **sysadmin**. La creación de una nueva tabla con **SELECT INTO** también requiere los permisos **CREATE TABLE** y **ALTER SCHEMA** en el esquema al que pertenece la nueva tabla.  
  
## <a name="examples"></a>Ejemplos:   
Los ejemplos siguientes usan la base de datos [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].
  
### <a name="a-using-select-to-retrieve-rows-and-columns"></a>A. Usar SELECT para recuperar filas y columnas  
 En esta sección se muestran tres códigos de ejemplo. En el primero, se devuelven todas las filas (no se especifica la cláusula WHERE) y todas las columnas (con `*`) de la tabla `DimEmployee`.  
  
```sql  
SELECT *  
FROM DimEmployee  
ORDER BY LastName;  
```  
  
 El siguiente ejemplo usa alias de tabla para lograr el mismo resultado.  
  
```sql  
SELECT e.*  
FROM DimEmployee AS e  
ORDER BY LastName;  
```  
  
 En este ejemplo se devuelven todas las filas (no se ha especificado la cláusula WHERE) y un subconjunto de las columnas (`FirstName`, `LastName`, `StartDate`) de la tabla `DimEmployee` de la base de datos `AdventureWorksPDW2012`. El nombre del tercer encabezado de columna cambia a `FirstDay`.  
  
```sql  
SELECT FirstName, LastName, StartDate AS FirstDay  
FROM DimEmployee   
ORDER BY LastName;  
```  
  
 En este ejemplo, solo se devuelven las filas de `DimEmployee` que tengan un valor de `EndDate` distinto de NULL y un valor de `MaritalStatus` de “M” (que se corresponde con “casado”).  
  
```sql  
SELECT FirstName, LastName, StartDate AS FirstDay  
FROM DimEmployee   
WHERE EndDate IS NOT NULL   
AND MaritalStatus = 'M'  
ORDER BY LastName;  
```  
  
### <a name="b-using-select-with-column-headings-and-calculations"></a>B. Usar SELECT con encabezados de columna y cálculos  
 El siguiente ejemplo devuelve todas las filas de la tabla `DimEmployee` y calcula el sueldo bruto de cada empleado partiendo de su `BaseRate` y de una semana laboral de 40 horas.  
  
```sql  
SELECT FirstName, LastName, BaseRate, BaseRate * 40 AS GrossPay  
FROM DimEmployee  
ORDER BY LastName;  
```  
  
### <a name="c-using-distinct-with-select"></a>C. Usar DISTINCT con SELECT  
 En el siguiente ejemplo se usa `DISTINCT` para generar una lista de todos los puestos únicos de la tabla `DimEmployee`.  
  
```sql  
SELECT DISTINCT Title  
FROM DimEmployee  
ORDER BY Title;  
```  
  
### <a name="d-using-group-by"></a>D. Usar GROUP BY  
 En el ejemplo siguiente se busca la cantidad total de todas las ventas de cada día.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
 Debido a la cláusula `GROUP BY`, solo se devuelve una fila que contiene la suma de todas las ventas de cada día.  
  
### <a name="e-using-group-by-with-multiple-groups"></a>E. Usar GROUP BY con varios grupos  
 En el siguiente ejemplo se busca el precio medio y la suma de ventas por Internet de cada día, agrupados por fecha de pedido y por clave de promoción.  
  
```sql  

SELECT OrderDateKey, PromotionKey, AVG(SalesAmount) AS AvgSales, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey, PromotionKey  
ORDER BY OrderDateKey;   
```  
  
### <a name="f-using-group-by-and-where"></a>F. Usar GROUP BY y WHERE  
 En el siguiente ejemplo, los resultados se colocan en grupos después de recuperar únicamente las filas con fechas de pedido posteriores al 1 de agosto de 2002.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
WHERE OrderDateKey > '20020801'  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
### <a name="g-using-group-by-with-an-expression"></a>G. Usar GROUP BY con una expresión  
 En este ejemplo se agrupa por una expresión. Puede agrupar por una expresión si ésta no incluye funciones de agregado.  
  
```sql  
SELECT SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY (OrderDateKey * 10);  
```  
  
### <a name="h-using-group-by-with-order-by"></a>H. Usar GROUP BY con ORDER BY  
 En el siguiente ejemplo se busca la suma de las ventas por día y los pedidos por día.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
### <a name="i-using-the-having-clause"></a>I. Usar la cláusula HAVING  
 En esta consulta se usa la cláusula `HAVING` para acotar los resultados.  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
HAVING OrderDateKey > 20010000  
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplos de SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)  
 [Sugerencias &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)
  

