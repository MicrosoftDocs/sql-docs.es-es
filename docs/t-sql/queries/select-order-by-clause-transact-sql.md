---
description: 'SELECT: Cláusula ORDER BY (Transact-SQL)'
title: Cláusula ORDER BY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ORDER_TSQL
- BY
- ORDER_BY_TSQL
- BY_TSQL
- ORDER
- ORDER BY
dev_langs:
- TSQL
helpviewer_keywords:
- ad-hoc query paging
- OFFSET clause
- SELECT statement [SQL Server], FETCH clause
- clauses [SQL Server], ORDER BY
- SELECT statement [SQL Server], limiting the rows returned
- data [SQL Server], limiting the rows returned
- data [SQL Server], ad-hoc query paging
- sort orders [SQL Server]
- SELECT statement [SQL Server], OFFSET clause
- ORDER BY clause [Transact-SQL]
- LIMIT
- limiting result sets
- SELECT statement [SQL Server], ORDER BY clause
- query paging
- data [SQL Server], sorting
- limiting data returned in a query
- sort orders [SQL Server], ORDER BY clause
- FETCH clause
ms.assetid: bb394abe-cae6-4905-b5c6-8daaded77742
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7903d3a208bd08c9ba22d251a0e4124e81725ea
ms.sourcegitcommit: 2bdf1f1ee88f4fe3e872227d025e965e95d1b2b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98711991"
---
# <a name="select---order-by-clause-transact-sql"></a>SELECT: Cláusula ORDER BY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Ordena los datos devueltos por una consulta en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Use esta cláusula para lo siguiente:  
  
-   Ordenar el conjunto de resultados de una consulta por la lista de columnas especificada y, opcionalmente, limitar las filas devueltas a un intervalo especificado. El orden en que se devuelven las filas en un conjunto de resultados no se puede garantizar, a menos que se especifique una cláusula ORDER BY.  
  
-   Determinar el orden en que se aplican los valores de la [función de categoría](../../t-sql/functions/ranking-functions-transact-sql.md) al conjunto de resultados.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!NOTE]  
>  ORDER BY no se admite en instrucciones SELECT/INTO o CREATE TABLE AS SELECT (CTAS) en [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ORDER BY order_by_expression  
    [ COLLATE collation_name ]   
    [ ASC | DESC ]   
    [ ,...n ]   
[ <offset_fetch> ]  
  
<offset_fetch> ::=  
{   
    OFFSET { integer_constant | offset_row_count_expression } { ROW | ROWS }  
    [  
      FETCH { FIRST | NEXT } {integer_constant | fetch_row_count_expression } { ROW | ROWS } ONLY  
    ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ ORDER BY   
    {  
    order_by_expression   
    [ ASC | DESC ]   
    } [ ,...n ]   
]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *order_by_expression*  
 Especifica una columna o expresión según la que se ordenará el conjunto de resultados de la consulta. Una columna de ordenación se puede especificar como un nombre o un alias de columna, o un entero no negativo que representa la posición de la columna en la lista de selección.  
  
 Es posible especificar varias columnas de ordenación. Los nombres de columna tienen que ser únicos. La secuencia de las columnas de ordenación de la cláusula ORDER BY define la organización del conjunto de resultados ordenado. Es decir, el conjunto de resultados se ordena conforme a la primera columna y, a continuación, esa lista ordenada se ordena según la segunda columna, y así sucesivamente.  
  
 Los nombres de columna a los que se hace referencia en la cláusula ORDER BY deben corresponderse con una columna o un alias de columna de la lista de selección, o bien con una columna definida en la tabla especificada en la cláusula FROM sin ambigüedades. Si la cláusula ORDER BY hace referencia a un alias de columna de la lista de selección, el alias de columna se debe usar de forma independiente y no como parte de una expresión en la cláusula ORDER BY, por ejemplo:
 
```sql
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName; -- correct 
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName + ''; -- wrong
```
  
 COLLATE *collation_name*  
 Especifica que la operación ORDER BY debe realizarse conforme a la intercalación especificada en *collation_name*, y no conforme a la intercalación de la columna definida en la tabla o vista. *collation_name* puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL. Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md). COLLATE se aplica a las columnas de tipo **char**, **varchar**, **nchar** y **nvarchar**.  
  
 **ASC** | DESC  
 Indica que los valores de la columna especificada se deben ordenar en sentido ascendente o descendente. ASC ordena del valor mínimo al valor máximo. DESC ordena del valor máximo al valor mínimo. ASC es el criterio de ordenación predeterminado. Los valores NULL se tratan como los valores más bajos posibles.  
  
 OFFSET { *integer_constant* | *offset_row_count_expression* } { ROW | ROWS }  
 Especifica el número de filas que se deben omitir antes de comenzar a devolver filas de la expresión de consulta. El valor puede ser una expresión o constante entera mayor o igual que cero.  
  
**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 *offset_row_count_expression* puede ser una variable, un parámetro o una subconsulta escalar constante. Cuando se utiliza una subconsulta, no puede hacer referencia a ninguna columna definida en el ámbito de la consulta externa. Es decir, no se puede poner en correlación con la consulta externa.  
  
 ROW y ROWS son sinónimos y se proporcionan para ofrecer compatibilidad con ANSI.  
  
 En los planes de ejecución de consultas, el valor de recuento de filas de desplazamiento se muestra en el atributo **Offset** del operador de consulta TOP.  
  
 FETCH { FIRST \| NEXT } { *integer_constant* \| *fetch_row_count_expression* } { ROW \| ROWS } ONLY  
 Especifica el número de filas que se devolverán después de procesar la cláusula OFFSET. El valor puede ser una expresión o constante entera mayor o igual que uno.  
  
**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 *fetch_row_count_expression* puede ser una variable, un parámetro o una subconsulta escalar constante. Cuando se utiliza una subconsulta, no puede hacer referencia a ninguna columna definida en el ámbito de la consulta externa. Es decir, no se puede poner en correlación con la consulta externa.  
  
 FIRST y NEXT son sinónimos y se proporcionan para ofrecer compatibilidad con ANSI.  
  
 ROW y ROWS son sinónimos y se proporcionan para ofrecer compatibilidad con ANSI.  
  
 En los planes de ejecución de consultas, el valor de recuento de filas de desplazamiento se muestra en el atributo **Rows** o **Top** del operador de consulta TOP.  
  
## <a name="best-practices"></a>Prácticas recomendadas  
 Evite especificar enteros en la cláusula ORDER BY como representaciones posicionales de las columnas en la lista de selección. Por ejemplo, aunque una instrucción como `SELECT ProductID, Name FROM Production.Production ORDER BY 2` es válida, otros usuarios no la entenderán tan bien como si especificase el nombre de la columna real. Además, para realizar cambios en la lista de selección, como modificar el orden de las columnas o agregar otras nuevas, es necesario modificar la cláusula ORDER BY a fin de evitar resultados inesperados.  
  
 En una instrucción SELECT TOP (*N*), use siempre una cláusula ORDER BY. Esta es la única manera de indicar previsiblemente a qué filas afecta TOP. Para más información, vea [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
## <a name="interoperability"></a>Interoperabilidad  
 Cuando se usa con una instrucción SELECT...INTO o INSERT...SELECT para insertar filas de otro origen, la cláusula ORDER BY no garantiza la inserción de las filas en el orden especificado.  
  
 Al usar OFFSET y FETCH en una vista no se cambia la propiedad Updateability de la vista.  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 No hay ningún límite en cuanto al número de columnas de la cláusula ORDER BY; sin embargo, el tamaño total de las columnas especificadas en una cláusula ORDER BY no puede superar los 8.060 bytes.  
  
 Las columnas de tipo **ntext**, **text**, **image**, **geography**, **geometry** y **xml** no se pueden usar en una cláusula ORDER BY.  
  
 No puede especificarse un entero o una constante cuando en una función de categoría aparece *order_by_expression*. Para más información, vea [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 Si los nombres de tabla están asociados a un alias en la cláusula FROM, solo pueden usarse los nombres de alias para calificar sus columnas en la cláusula ORDER BY.  
  
 Los nombres y alias de columna especificados en la cláusula ORDER BY deben estar definidos en la lista de selección si la instrucción SELECT contiene uno de los operadores o cláusulas siguientes:  
  
-   UNION, operador  
  
-   EXCEPT, operador  
  
-   INTERSECT, operador  
  
-   SELECT DISTINCT  
  
 Además, cuando la instrucción incluye un operador UNION, EXCEPT o INTERSECT, los nombres o los alias de columna deben especificarse en la lista de selección de la primera consulta (lado izquierdo).  
  
 En una consulta que utiliza los operadores UNION, INTERSECT o EXCEPT, ORDER BY se permite únicamente al final de la instrucción. Esta restricción se aplica únicamente cuando se especifica UNION, EXCEPT e INTERSECT en una consulta de nivel superior, y no en una subconsulta. Vea la sección Ejemplos que aparece más adelante.  
  
 La cláusula ORDER BY no es válida en vistas, funciones insertadas, tablas derivadas y subconsultas, a menos que se especifiquen también las cláusulas TOP u OFFSET y FETCH. Cuando ORDER BY se utiliza en estos objetos, la cláusula únicamente se utiliza para determinar las filas devueltas por la cláusula TOP o las cláusulas OFFSET Y FETCH. La cláusula ORDER BY no garantiza resultados ordenados cuando se consulten estos constructores, a menos que también se especifique ORDER BY en la misma consulta.  
  
 OFFSET y FETCH no se admiten en vistas indizadas ni en vistas definidas mediante la cláusula CHECK OPTION.  
  
 OFFSET y FETCH se pueden utilizar en cualquier consulta que permita TOP y ORDER BY con las siguientes limitaciones:  
  
-   La cláusula OVER no admite OFFSET ni FETCH.  
  
-   OFFSET y FETCH no se pueden especificar directamente en las instrucciones INSERT, UPDATE, MERGE ni DELETE, pero sí en una subconsulta definida en ellas. Por ejemplo, en la instrucción INSERT INTO SELECT, se pueden especificar OFFSET y FETCH en la instrucción SELECT.  
  
-   En una consulta que utiliza los operadores UNION, EXCEPT o INTERSECT, OFFSET y FETCH únicamente se pueden utilizar en la consulta final que especifica el orden de los resultados de la consulta.  
  
-   TOP no se puede combinar con OFFSET y FETCH en la misma expresión de consulta (en el mismo ámbito de la consulta).  
  
## <a name="using-offset-and-fetch-to-limit-the-rows-returned"></a>Utilizar OFFSET y FETCH para limitar las filas devueltas  
 Recomendamos utilizar las cláusulas OFFSET y FETCH en lugar de la cláusula TOP para implementar una solución de paginación de consulta y limitar el número de filas enviadas a una aplicación cliente.  
  
 Para utilizar OFFSET y FETCH como solución de paginación, es preciso ejecutar la consulta una vez por cada "página" de datos devuelta a la aplicación cliente. Por ejemplo, para devolver los resultados de una consulta en incrementos de 10 filas, se debe ejecutar la consulta una vez para devolver las filas de 1 a 10, después otra vez para devolver las filas de 11 a 20, y así sucesivamente. Cada consulta es independiente y no está relacionada con las demás de forma alguna. Esto significa que, a diferencia de cuando se usa un cursor en que la consulta se ejecuta una vez y su estado se mantiene en el servidor, en este caso es la aplicación cliente la responsable de realizar el seguimiento del estado. Para lograr resultados estables entre las solicitudes de consultas donde se utilicen OFFSET y FETCH, se deben cumplir las siguientes condiciones:  
  
1.  Los datos subyacentes que la consulta utilice no deben cambiar. Es decir, o bien las filas afectadas por la consulta no se actualizarán, o bien todas las solicitudes correspondientes a las páginas de la consulta se ejecutarán en una transacción única utilizando el aislamiento de transacción serializable o de instantánea. Para más información sobre los niveles de aislamiento de estas transacciones, vea [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
2.  Debe garantizarse que la columna o combinación de columnas contenidas en la cláusula ORDER BY sean únicas.  
  
 Vea el ejemplo que "Ejecutar varias consultas en una sola transacción" en la sección Ejemplos que aparece más adelante en este tema.  
  
 Si el hecho de que los planes de ejecución sean coherentes es importante para su solución de paginación, puede ser conveniente utilizar la sugerencia de consulta OPTIMIZE FOR para los parámetros de OFFSET y FETCH. Vea "Especificar expresiones para valores de OFFSET y FETCH" en la sección Ejemplos que aparece más adelante en este tema. Para más información sobre OPTIMIZE FOR, vea [Sugerencias de consulta &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="examples"></a>Ejemplos  
  
|Category|Elementos de sintaxis ofrecidos|  
|--------------|------------------------------|  
|[Sintaxis básica](#BasicSyntax)|ORDER BY|  
|[Especificar orden ascendente y descendente](#SortOrder)|DESC • ASC|  
|[Especificar una intercalación](#Collation)|COLLATE|  
|[Especificar un orden condicional](#Case)|CASE, expresión|  
|[Usar ORDER BY en una función de categoría](#Rank)|Funciones de categoría|  
|[Limitar el número de filas devueltas](#Offset)|OFFSET • FETCH|  
|[Usar ORDER BY con UNION, EXCEPT e INTERSECT](#Union)|UNION|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Sintaxis básica  
 En los ejemplos de esta sección se muestra la funcionalidad básica de la cláusula ORDER BY con la sintaxis mínima requerida.  
  
#### <a name="a-specifying-a-single-column-defined-in-the-select-list"></a>A. Especificar una sola columna definida en la lista de selección  
 En el siguiente ejemplo se ordena el conjunto de resultados por la columna numérica `ProductID`. Dado que no se especifica un criterio de ordenación concreto, se utiliza el valor predeterminado (orden ascendente).  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID;  
```  
  
#### <a name="b-specifying-a-column-that-is-not-defined-in-the-select-list"></a>B. Especificar una columna que no está definida en la lista de selección  
 En el siguiente ejemplo se ordena el conjunto de resultados por una columna que no está incluida en la lista de selección, pero sí definida en la tabla especificada en la cláusula FROM.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name, Color  
FROM Production.Product  
ORDER BY ListPrice;  
```  
  
#### <a name="c-specifying-an-alias-as-the-sort-column"></a>C. Especificar un alias como columna de ordenación  
 En el ejemplo siguiente se especifica el alias de columna `SchemaName` como columna de criterio de ordenación.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT name, SCHEMA_NAME(schema_id) AS SchemaName  
FROM sys.objects  
WHERE type = 'U'  
ORDER BY SchemaName;  
```  
  
#### <a name="d-specifying-an-expression-as-the-sort-column"></a>D. Especificar una expresión como columna de ordenación  
 En el ejemplo siguiente se utiliza una expresión como columna de ordenación. La expresión se define mediante la función DATEPART para ordenar el conjunto de resultados según el año de contratación de los empleados.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY DATEPART(year, HireDate);  
```  
  
###  <a name="specifying-ascending-and-descending-sort-order"></a><a name="SortOrder"></a> Especificar un criterio de ordenación ascendente y descendente  
  
#### <a name="a-specifying-a-descending-order"></a>A. Especificar un orden descendente  
 En el siguiente ejemplo se ordena el conjunto de resultados en sentido descendente según la columna numérica `ProductID`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID DESC;  
```  
  
#### <a name="b-specifying-an-ascending-order"></a>B. Especificar un orden ascendente  
 En el siguiente ejemplo se ordena el conjunto de resultados en orden ascendente según la columna `Name`. Los caracteres están ordenados alfabéticamente, no numéricamente. Es decir, 10 se ordena antes que 2.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY Name ASC ;  
```  
  
#### <a name="c-specifying-both-ascending-and-descending-order"></a>C. Especificar orden ascendente y también descendente  
 En el siguiente ejemplo se ordena el conjunto de resultados según dos columnas. El conjunto de resultados se ordena en primer lugar en sentido ascendente según la columna `FirstName` y, a continuación, en orden descendente según la columna `LastName`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT LastName, FirstName FROM Person.Person  
WHERE LastName LIKE 'R%'  
ORDER BY FirstName ASC, LastName DESC ;  
```  
  
###  <a name="specifying-a-collation"></a><a name="Collation"></a> Especificar una intercalación  
 En el siguiente ejemplo se muestra cómo especificar una intercalación en la cláusula ORDER BY puede cambiar el orden en que se devuelven los resultados de la consulta. Se crea una tabla que contiene una columna definida mediante una intercalación que no distingue entre mayúsculas y minúsculas, ni las tildes. Los valores se insertan con diversas diferencias de uso de mayúsculas, minúsculas y tildes. Dado que no se especifica ninguna intercalación en la cláusula ORDER BY, la primera consulta utiliza la intercalación de la columna al ordenar los valores. En la segunda consulta, se especifica una intercalación que distingue entre mayúsculas y minúsculas y las tildes; en consecuencia, cambia el orden en el que se devuelven las filas.  
  
```sql
USE tempdb;  
GO  
CREATE TABLE #t1 (name NVARCHAR(15) COLLATE Latin1_General_CI_AI)  
GO  
INSERT INTO #t1 VALUES(N'Sánchez'),(N'Sanchez'),(N'sánchez'),(N'sanchez');  
  
-- This query uses the collation specified for the column 'name' for sorting.  
SELECT name  
FROM #t1  
ORDER BY name;  
-- This query uses the collation specified in the ORDER BY clause for sorting.  
SELECT name  
FROM #t1  
ORDER BY name COLLATE Latin1_General_CS_AS;  
```  
  
###  <a name="specifying-a-conditional-order"></a><a name="Case"></a> Especificar un orden condicional  
 En los ejemplos siguientes se usa la expresión CASE en una cláusula ORDER BY para determinar de manera condicional el criterio de ordenación de las filas según el valor de una columna dada. En el primer ejemplo se evalúe el valor de la columna `SalariedFlag` de la tabla `HumanResources.Employee`. Los empleados que tienen la columna `SalariedFlag` establecida en 1 se devuelven en orden descendente según el `BusinessEntityID`. Los empleados que tienen la columna `SalariedFlag` establecida en 0 se devuelven en orden ascendente según el `BusinessEntityID`. En el segundo ejemplo, el conjunto de resultados se ordena según la columna `TerritoryName` cuando la columna `CountryRegionName` es igual a 'Estados Unidos' y según la columna `CountryRegionName` en las demás filas.  
  
```sql
SELECT BusinessEntityID, SalariedFlag  
FROM HumanResources.Employee  
ORDER BY CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC  
        ,CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END;  
GO  
```  
  
```sql
SELECT BusinessEntityID, LastName, TerritoryName, CountryRegionName  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL  
ORDER BY CASE CountryRegionName WHEN 'United States' THEN TerritoryName  
         ELSE CountryRegionName END;  
```  
  
###  <a name="using-order-by-in-a-ranking-function"></a><a name="Rank"></a> Usar ORDER BY en una función de categoría  
 En el siguiente ejemplo se utiliza la cláusula ORDER BY en las funciones de categoría ROW_NUMBER, RANK, DENSE_RANK y NTILE.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS "Rank"  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS "Quartile"  
    ,s.SalesYTD, a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
###  <a name="limiting-the-number-of-rows-returned"></a><a name="Offset"></a> Limitar el número de filas devueltas  
 En los siguientes ejemplos se utiliza OFFSET y FETCH para limitar el número de filas devueltas por una consulta.  
  
**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
#### <a name="a-specifying-integer-constants-for-offset-and-fetch-values"></a>A. Especificar constantes enteras para los valores de OFFSET y FETCH  
 En el siguiente ejemplo se especifica una constante entera como valor para las cláusulas OFFSET y FETCH. La primera consulta devuelve todas las filas ordenadas según la columna `DepartmentID`. Compare los resultados devueltos por esta consulta con los de las dos consultas siguientes. La consulta siguiente utiliza la cláusula `OFFSET 5 ROWS` para omitir las primeras 5 filas y devolver todas las restantes. La última consulta utiliza la cláusula `OFFSET 0 ROWS` para comenzar por la primera fila y, a continuación, utiliza `FETCH NEXT 10 ROWS ONLY` para limitar las filas devueltas a 10 filas del conjunto de resultados ordenado.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Return all rows sorted by the column DepartmentID.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID;  
  
-- Skip the first 5 rows from the sorted result set and return all remaining rows.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID OFFSET 5 ROWS;  
  
-- Skip 0 rows and return only the first 10 rows from the sorted result set.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID   
    OFFSET 0 ROWS  
    FETCH NEXT 10 ROWS ONLY;  
```  
  
#### <a name="b-specifying-variables-for-offset-and-fetch-values"></a>B. Especificar variables para los valores de OFFSET y FETCH  
 En el siguiente ejemplo se declaran las variables `@RowsToSkip` y `@FetchRows`, y se especifican estas variables en las cláusulas OFFSET y FETCH.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Specifying variables for OFFSET and FETCH values    
DECLARE @RowsToSkip TINYINT = 2
      , @FetchRows TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @RowsToSkip ROWS   
    FETCH NEXT @FetchRows ROWS ONLY;  
```  
  
#### <a name="c-specifying-expressions-for-offset-and-fetch-values"></a>C. Especificar expresiones para los valores de OFFSET y FETCH  
 En el siguiente ejemplo se utiliza la expresión `@StartingRowNumber - 1` para especificar el valor de OFFSET y la expresión `@EndingRowNumber - @StartingRowNumber + 1` para especificar el valor de FETCH. Además, se especifica la sugerencia de consulta OPTIMIZE FOR. Esta sugerencia se puede usar para que se utilice un valor concreto para una variable local al compilar y optimizar la consulta. El valor se utiliza solo durante la optimización de la consulta y no durante la ejecución de la misma. Para obtener más información, vea [Sugerencias de consulta &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Specifying expressions for OFFSET and FETCH values      
DECLARE @StartingRowNumber TINYINT = 1  
      , @EndingRowNumber TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @EndingRowNumber - @StartingRowNumber + 1 ROWS ONLY  
OPTION ( OPTIMIZE FOR (@StartingRowNumber = 1, @EndingRowNumber = 20) );  
```  
  
#### <a name="d-specifying-a-constant-scalar-subquery-for-offset-and-fetch-values"></a>D. Especificar una subconsulta escalar constante para los valores de OFFSET y FETCH  
 En el siguiente ejemplo se utiliza una subconsulta escalar constante a fin de definir el valor para la cláusula FETCH. La subconsulta devuelve un valor único de la columna `PageSize` de la tabla `dbo.AppSettings`.  
  
```sql
-- Specifying a constant scalar subquery  
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.AppSettings (AppSettingID INT NOT NULL, PageSize INT NOT NULL);  
GO  
INSERT INTO dbo.AppSettings VALUES(1, 10);  
GO  
DECLARE @StartingRowNumber TINYINT = 1;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber ROWS   
    FETCH NEXT (SELECT PageSize FROM dbo.AppSettings WHERE AppSettingID = 1) ROWS ONLY;  
```  
  
#### <a name="e-running-multiple-queries-in-a-single-transaction"></a>E. Ejecutar varias consultas en una sola transacción  
 En el siguiente ejemplo se muestra un método de implementar una solución de paginación que permite asegurarse de la devolución de resultados estables en todas las solicitudes de la consulta. La consulta se ejecuta en una sola transacción utilizando el nivel de aislamiento de instantánea, mientras que la columna especificada en la cláusula ORDER BY asegura la singularidad de la columna.  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Ensure the database can support the snapshot isolation level set for the query.  
IF (SELECT snapshot_isolation_state FROM sys.databases WHERE name = N'AdventureWorks2012') = 0  
    ALTER DATABASE AdventureWorks2012 SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Set the transaction isolation level  to SNAPSHOT for this query.  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
-- Beginning the transaction.
BEGIN TRANSACTION;  
GO  
-- Declare and set the variables for the OFFSET and FETCH values.  
DECLARE @StartingRowNumber INT = 1  
      , @RowCountPerPage INT = 3;  
  
-- Create the condition to stop the transaction after all rows have been returned.  
WHILE (SELECT COUNT(*) FROM HumanResources.Department) >= @StartingRowNumber  
BEGIN  
  
-- Run the query until the stop condition is met.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @RowCountPerPage ROWS ONLY;  
  
-- Increment @StartingRowNumber value.  
SET @StartingRowNumber = @StartingRowNumber + @RowCountPerPage;  
CONTINUE  
END;  
GO  
COMMIT TRANSACTION;  
GO  
```  
  
###  <a name="using-order-by-with-union-except-and-intersect"></a><a name="Union"></a> Usar ORDER BY con UNION, EXCEPT e INTERSECT  
 Cuando una consulta utiliza los operadores UNION, EXCEPT o INTERSECT, la cláusula ORDER BY se debe especificar al final de la instrucción y se ordenan los resultados de las consultas combinadas. En el siguiente ejemplo se devuelven todos los productos que son rojos o amarillos y la lista combinada se ordena según la columna `ListPrice`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Red'  
-- ORDER BY cannot be specified here.  
UNION ALL  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Yellow'  
ORDER BY ListPrice ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el ejemplo siguiente se muestra la ordenación de un conjunto de resultados en sentido ascendente según la columna numérica `EmployeeKey`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey;  
```  
  
 En el ejemplo siguiente se ordena un conjunto de resultados en sentido descendente según la columna numérica `EmployeeKey`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey DESC;  
```  
  
 En el ejemplo siguiente se ordena un conjunto de resultados según la columna `LastName`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName;  
```  
  
 En el ejemplo siguiente se ordena según dos columnas. Esta consulta ordena primero en sentido ascendente según la columna `FirstName` y, después, ordena valores comunes `FirstName` en sentido descendente según la columna `LastName`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName, FirstName;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [Funciones de categoría &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md)   
 [Sugerencias de consulta &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)   
 [EXCEPT e INTERSECT &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [UNION &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-union-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)  
  
  

