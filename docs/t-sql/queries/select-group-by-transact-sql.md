---
description: 'SELECT: GROUP BY (Transact-SQL)'
title: GROUP BY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUP
- CUBE
- ROLLUP
- GROUPING_SETS_TSQL
- GROUP BY
- GROUPING SETS
- GROUP_BY_TSQL
- GROUP_TSQL
- CUBE_TSQL
- ROLLUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GROUP BY clause, about GROUP BY clause
- dividing tables into groups
- GROUPING SETS
- GROUP BY clause
- table groups [SQL Server]
- groups [SQL Server], tables divided into groups
- summary values [SQL Server]
ms.assetid: 40075914-6385-4692-b4a5-62fe44ae6cb6
author: shkale-msft
ms.author: shkale
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4bbf0df92834b3bec1e7314edb8dfba305f734d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207573"
---
# <a name="select---group-by--transact-sql"></a>SELECT: GROUP BY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cláusula de la instrucción SELECT que divide el resultado de la consulta en grupos de filas, normalmente con el fin de realizar una o varias agregaciones en cada grupo. La instrucción SELECT devuelve una fila por grupo.  
  
## <a name="syntax"></a>Sintaxis  

 ![Icono de vínculo a temas](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database   
-- ISO-Compliant Syntax  
  
GROUP BY {
      column-expression  
    | ROLLUP ( <group_by_expression> [ ,...n ] )  
    | CUBE ( <group_by_expression> [ ,...n ] )  
    | GROUPING SETS ( <grouping_set> [ ,...n ]  )  
    | () --calculates the grand total 
} [ ,...n ] 
 
<group_by_expression> ::=  
      column-expression  
    | ( column-expression [ ,...n ] )    
   
<grouping_set> ::=  
      () --calculates the grand total  
    | <grouping_set_item>  
    | ( <grouping_set_item> [ ,...n ] )  
  
<grouping_set_item> ::=  
      <group_by_expression>  
    | ROLLUP ( <group_by_expression> [ ,...n ] )  
    | CUBE ( <group_by_expression> [ ,...n ] )  
  

-- For backward compatibility only.
-- Non-ISO-Compliant Syntax for SQL Server and Azure SQL Database 
  
GROUP BY 
      [ ALL ] column-expression [ ,...n ] 
    | column-expression [ ,...n ] [ WITH { CUBE | ROLLUP } ]   

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics 
  
GROUP BY {
      column-name [ WITH (DISTRIBUTED_AGG) ]  
    | column-expression
    | ROLLUP ( <group_by_expression> [ ,...n ] ) 
} [ ,...n ]

```

```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
GROUP BY {
      column-name [ WITH (DISTRIBUTED_AGG) ]  
    | column-expression
} [ ,...n ]

```  
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 
### <a name="column-expression"></a>*column-expression*  
Especifica una columna o un cálculo no agregado en una columna. Esta columna puede pertenecer a una tabla, una tabla derivada o una vista. La columna debe aparecer en la cláusula FROM de la instrucción SELECT, pero no es necesario que aparezca en la lista SELECT. 

Para conocer las expresiones válidas, vea [Expresiones](~/t-sql/language-elements/expressions-transact-sql.md).    

La columna debe aparecer en la cláusula FROM de la instrucción SELECT, pero no es necesario que aparezca en la lista SELECT. Pero todas las columnas de la tabla o vista de cualquier expresión no agregada de la lista de \<select> se deben incluir en la lista GROUP BY:  
  
Están permitidas las siguientes instrucciones:  
  
```sql  
SELECT ColumnA, ColumnB FROM T GROUP BY ColumnA, ColumnB;  
SELECT ColumnA + ColumnB FROM T GROUP BY ColumnA, ColumnB;  
SELECT ColumnA + ColumnB FROM T GROUP BY ColumnA + ColumnB;  
SELECT ColumnA + ColumnB + constant FROM T GROUP BY ColumnA, ColumnB;  
```  
  
No están permitidas las siguientes instrucciones:  
  
```sql  
SELECT ColumnA, ColumnB FROM T GROUP BY ColumnA + ColumnB;  
SELECT ColumnA + constant + ColumnB FROM T GROUP BY ColumnA + ColumnB;  
```  

La expresión de columna no puede contener:

- Un alias de columna que esté definido en la lista SELECT. Puede usar un alias de columna para una tabla derivada que esté definida en la cláusula FROM.
- Una columna de tipo **text**, **ntext** o **image**. Aun así, puede usar una columna de tipo text, ntext o image como argumento para una función que devuelva un valor de un tipo de datos válido. Por ejemplo, la expresión puede usar SUBSTRING() y CAST(). Esto también se aplica a las expresiones de la cláusula HAVING.
- Métodos de tipo de datos xml. Puede incluir una función definida por el usuario que use métodos de tipo de datos xml. Puede incluir una columna calculada que use métodos de tipo de datos xml. 
- Una subconsulta. Se devuelve el error 144. 
- Una columna de una vista indexada. 
 
### <a name="group-by-column-expression--n-"></a>GROUP BY *column-expression* [ ,...n ] 

Agrupa los resultados de la instrucción SELECT según los valores en una lista con una o varias expresiones de columna. 

Por ejemplo, esta consulta crea una tabla de ventas con columnas para el país, la región y las ventas. Inserta cuatro filas, y dos de las filas tienen valores coincidentes para el país y la región.  

```sql
CREATE TABLE Sales ( Country VARCHAR(50), Region VARCHAR(50), Sales INT );

INSERT INTO sales VALUES (N'Canada', N'Alberta', 100);
INSERT INTO sales VALUES (N'Canada', N'British Columbia', 200);
INSERT INTO sales VALUES (N'Canada', N'British Columbia', 300);
INSERT INTO sales VALUES (N'United States', N'Montana', 100);
```
La tabla de ventas contiene estas filas:

| Country | Region | Sales |
|---------|--------|-------|
| Canadá | Alberta | 100 |
| Canadá | Columbia Británica | 200 |
| Canadá | Columbia Británica | 300 |
| Estados Unidos | Montana | 100 |

La siguiente consulta agrupa el país y la región y devuelve la suma de agregados de cada combinación de valores.  
 
```sql 
SELECT Country, Region, SUM(sales) AS TotalSales
FROM Sales
GROUP BY Country, Region;
```
El resultado de la consulta tiene tres filas, ya que hay tres combinaciones de valores para el país y la región. El valor de las ventas totales para Canadá y Columbia Británica es la suma de dos filas. 

| Country | Region | Ventas totales |
|---------|--------|-------|
| Canadá | Alberta | 100 |
| Canadá | Columbia Británica | 500 |
| Estados Unidos | Montana | 100 |

### <a name="group-by-rollup"></a>GROUP BY ROLLUP

Crea un grupo para cada combinación de expresiones de columna. Además, "acumula" los resultados en subtotales y totales generales. Para ello, mueve de derecha a izquierda reduciendo el número de expresiones de columna para las que crea grupos y agregaciones. 

El orden de las columnas afecta a la salida de ROLLUP y puede afectar al número de filas del conjunto de resultados.  

Por ejemplo, `GROUP BY ROLLUP (col1, col2, col3, col4)` crea grupos para cada combinación de expresiones de columna en las listas siguientes.  

- col1, col2, col3, col4 
- col1, col2, col3, NULL
- col1, col2, NULL, NULL
- col1, NULL, NULL, NULL
- NULL, NULL, NULL, NULL (este es el total general)

Con la tabla del ejemplo anterior, este código ejecuta una operación GROUP BY ROLLUP en lugar de una operación GROUP BY simple.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY ROLLUP (Country, Region);
```

El resultado de la consulta tiene las mismas agregaciones que la operación GROUP BY simple sin ROLLUP. Además, crea subtotales para cada valor de país. Por último, proporciona un total general para todas las filas. El resultado tiene el aspecto siguiente:

| Country | Region | Ventas totales |
| :------ | :----- | ---------: |
| Canadá | Alberta | 100 |
| Canadá | Columbia Británica | 500 |
| Canadá | NULL | 600 |
| Estados Unidos | Montana | 100 |
| Estados Unidos | NULL | 100 |
| NULL | NULL | 700 |

### <a name="group-by-cube--"></a>GROUP BY CUBE ( )  

GROUP BY CUBE crea grupos para todas las combinaciones posibles de columnas. Para GROUP BY CUBE (a, b), el resultado tiene grupos de valores únicos de (a, b), (NULL, b), (a, NULL) y (NULL, NULL).

Con la tabla de los ejemplos anteriores, este código ejecuta una operación GROUP BY CUBE en el país y la región. 

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY CUBE (Country, Region);
```

El resultado de la consulta tiene grupos para valores únicos de (Country, Region), (NULL, Region), (Country, NULL) y (NULL, NULL). El resultado tiene el aspecto siguiente:

| Country | Region | Ventas totales |
|---------|--------|-------|
| Canadá | Alberta | 100 |
| NULL | Alberta | 100 |
| Canadá | Columbia Británica | 500 |
| NULL | Columbia Británica | 500 |
| Estados Unidos | Montana | 100 |
| NULL | Montana | 100 |
| NULL | NULL | 700
| Canadá | NULL | 600 |
| Estados Unidos | NULL | 100 |
   
 ### <a name="group-by-grouping-sets--"></a>GROUP BY GROUPING SETS ( )  
 
La opción GROUPING SETS permite combinar varias cláusulas GROUP BY en una cláusula GROUP BY. Los resultados son equivalentes a usar la instrucción UNION ALL en los grupos especificados. 

Por ejemplo, `GROUP BY ROLLUP (Country, Region)` y `GROUP BY GROUPING SETS ( ROLLUP (Country, Region) )` devuelven los mismos resultados. 

Cuando GROUPING SETS tiene dos o más elementos, los resultados son la unión de los elementos. En este ejemplo se devuelve la unión de los resultados ROLLUP y CUBE para el país y la región.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY GROUPING SETS ( ROLLUP (Country, Region), CUBE (Country, Region) );
```

Los resultados son los mismos que los de esta consulta que devuelve la unión de las dos instrucciones GROUP BY.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY ROLLUP (Country, Region)
UNION ALL
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY CUBE (Country, Region);
```

SQL no consolida los grupos duplicados generados para una lista GROUPING SETS. Por ejemplo, en `GROUP BY ( (), CUBE (Country, Region) )`, ambos elementos devuelven una fila para el total general y ambas filas se mostrarán en los resultados. 

 ### <a name="group-by-"></a>GROUP BY ()  
Especifica el grupo vacío que genera el total general. Esto resulta útil como uno de los elementos de GROUPING SET. Por ejemplo, esta instrucción proporciona el total de ventas de cada país y, después, el total general para todos los países.

```sql
SELECT Country, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY GROUPING SETS ( Country, () );
```

### <a name="group-by--all--column-expression--n-"></a>GROUP BY [ ALL ] column-expression [ ,...n ] 

Se aplica a: SQL Server y Azure SQL Database

NOTA:  Esta sintaxis se proporciona únicamente por motivos de compatibilidad con versiones anteriores. Se quitará en una versión futura. Evite usar esta sintaxis en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la usan.

Especifica que se incluyen todos los grupos en los resultados independientemente de que cumplan los criterios de búsqueda en la cláusula WHERE. Los grupos que no cumplen los criterios de búsqueda tienen el valor NULL para la agregación. 

GROUP BY ALL:
- No se admite en consultas que tienen acceso a tablas remotas si también hay una cláusula WHERE en la consulta.
- Generará un error en las columnas que tengan el atributo FILESTREAM.
  
### <a name="with-distributed_agg"></a>WITH (DISTRIBUTED_AGG)
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

La sugerencia de consulta DISTRIBUTED_AGG obliga al sistema de procesamiento paralelo masivo (MPP) a redistribuir una tabla en una columna específica antes de realizar una agregación. Solo una columna de la cláusula GROUP BY puede tener una sugerencia de consulta DISTRIBUTED_AGG. Una vez finalizada la consulta, se quita la tabla redistribuida. La tabla original no se cambia.  

NOTA:  La sugerencia de consulta DISTRIBUTED_AGG se proporciona para ofrecer compatibilidad con versiones anteriores de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] y no mejorará el rendimiento de la mayoría de las consultas. De forma predeterminada, MPP redistribuye los datos según sea necesario para mejorar el rendimiento de las agregaciones. 
  
## <a name="general-remarks"></a>Notas generales

### <a name="how-group-by-interacts-with-the-select-statement"></a>Cómo interactúa GROUP BY con la instrucción SELECT
Lista SELECT:
- Agregados vectoriales. Si se incluyen funciones de agregado en la lista SELECT, GROUP BY calcula un valor de resumen para cada grupo. Se conocen como agregados vectoriales. 
- Agregados distintos. Los agregados AVG (DISTINCT *column_name*), COUNT (DISTINCT *column_name*) y SUM (DISTINCT *column_name*) se admiten con ROLLUP, CUBE, and GROUPING SETS.
  
Cláusula WHERE:
- SQL quita las filas que no cumplen las condiciones especificadas en la cláusula WHERE antes de realizar ninguna operación de agrupación.  
  
Cláusula HAVING:
- SQL usa la cláusula HAVING para filtrar los grupos en el conjunto de resultados. 
  
Cláusula ORDER BY:
- En su lugar, use la cláusula ORDER BY para ordenarlo. La cláusula GROUP BY no ordena el conjunto de resultados. 
  
Valores NULL:
- Si una columna de agrupamiento contiene valores NULL, todos ellos se consideran equivalentes y se recopilan en un solo grupo.   
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Se aplica a: SQL Server (a partir de la versión 2008) y [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]

### <a name="maximum-capacity"></a>Capacidad máxima

Para una cláusula GROUP BY que usa ROLLUP, CUBE o GROUPING SETS, el número máximo de expresiones es 32. El número máximo de grupos es 4096 (2<sup>12</sup>). En los ejemplos siguientes se produce un error debido a que la cláusula GROUP BY tiene más de 4096 grupos.  
 
-   En el ejemplo siguiente se generan 4097 (2<sup>12</sup> + 1) conjuntos de agrupamiento y se producirá un error.  
  
    ```sql
    GROUP BY GROUPING SETS( CUBE(a1, ..., a12), b )  
    ```  
  
-   En el ejemplo siguiente se generan 4097 (2<sup>12</sup> + 1) grupos y se producirá un error. Los conjuntos de agrupación `CUBE ()` y `()` generan una fila de total general y los conjuntos de agrupación duplicados no se eliminan.  
  
    ```sql
    GROUP BY GROUPING SETS( CUBE(a1, ..., a12), ())  
    ```  

-   En este ejemplo se usa sintaxis compatible con versiones anteriores. Se generan se generan 8192 (2<sup>13</sup>) conjuntos de agrupamiento y se producirá un error.  
  
    ```sql
    GROUP BY CUBE (a1, ..., a13)   
    GROUP BY a1, ..., a13 WITH CUBE   
    ```    
    En las cláusulas GROUP BY compatibles con versiones anteriores que no contengan CUBE o ROLLUP, el número de grupos por elementos está limitado por los tamaños de columna de GROUP BY, las columnas de agregado y los valores de agregado que participan en la consulta. Este límite procede del límite de 8.060 bytes de la tabla de trabajo intermedia que se necesita para contener los resultados intermedios de la consulta. Se permite un máximo de 12 expresiones de agrupamiento cuando se especifica CUBE o ROLLUP.

### <a name="support-for-iso-and-ansi-sql-2006-group-by-features"></a>Compatibilidad con las características GROUP BY de ISO y ANSI SQL-2006

La cláusula GROUP BY admite todas las características GROUP BY incluidas en el estándar SQL-2006 con las excepciones de sintaxis siguientes:  
  
-   Los conjuntos de agrupamiento no se pueden usar en la cláusula GROUP BY a menos que formen parte de una lista GROUPING SETS explícita. Por ejemplo, `GROUP BY Column1, (Column2, ...ColumnN`) se admite en el estándar, pero no en Transact-SQL.  Transact-SQL admite `GROUP BY C1, GROUPING SETS ((Column2, ...ColumnN))` y `GROUP BY Column1, Column2, ... ColumnN`, que son equivalentes semánticamente. Éstos son equivalentes semánticamente al ejemplo de `GROUP BY` anterior. Con ello se evita la posibilidad de que `GROUP BY Column1, (Column2, ...ColumnN`) se pueda malinterpretar como `GROUP BY C1, GROUPING SETS ((Column2, ...ColumnN))`, que no son equivalentes semánticamente.  
  
-   No se pueden usar conjuntos de agrupamiento dentro de conjuntos de agrupamiento. Por ejemplo, `GROUP BY GROUPING SETS (A1, A2,...An, GROUPING SETS (C1, C2, ...Cn))` se admite en el estándar SQL-2006, pero no en Transact-SQL. Transact-SQL admite `GROUP BY GROUPING SETS( A1, A2,...An, C1, C2, ...Cn )` o `GROUP BY GROUPING SETS( (A1), (A2), ... (An), (C1), (C2), ... (Cn) )`, que son equivalentes semánticamente al primer ejemplo de GROUP BY y tienen una sintaxis más clara.  
  
-   GROUP BY [ALL/DISTINCT] solo se permite en una cláusula GROUP BY simple que contenga expresiones de columna. No se permite con las construcciones GROUPING SETS, ROLLUP, CUBE, WITH CUBE o WITH ROLLUP. ALL es el valor predeterminado y es implícito. Solo se permite en sintaxis compatible con versiones anteriores.
  
### <a name="comparison-of-supported-group-by-features"></a>Comparación de las características GROUP BY compatibles  
 En la tabla siguiente se describen las características de GROUP BY que son compatibles en función de las versiones de SQL y del nivel de compatibilidad de la base de datos.  
  
|Característica|SQL Server Integration Services|Nivel de compatibilidad 100 o superior con SQL Server|SQL Server 2008 o posterior con el nivel de compatibilidad 90.|  
|-------------|-------------------------------------|--------------------------------------------------|-----------------------------------------------------------|  
|Agregados DISTINCT|No se admite en WITH CUBE ni en WITH ROLLUP.|Se admite en WITH CUBE, WITH ROLLUP, GROUPING SETS, CUBE o ROLLUP.|Igual que el nivel de compatibilidad 100.|  
|Función definida por el usuario con un nombre CUBE o ROLLUP en la cláusula GROUP BY|Se admite la función definida por el usuario **dbo.cube(** _arg1_ **,** _...argN_ **)** o **dbo.rollup(** _arg1_ **,** ..._argN_ **)** en la cláusula GROUP BY.<br /><br /> Por ejemplo: `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`|No se admite la función definida por el usuario **dbo.cube (** _arg1_ **,** ...argN **)** o **dbo.rollup(** arg1 **,** _...argN_ **)** en la cláusula GROUP BY.<br /><br /> Por ejemplo: `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`<br /><br /> Se devuelve un mensaje de error que indica lo siguiente: "Sintaxis incorrecta cerca de la palabra clave "cube"&#124;"rollup"."<br /><br /> Para evitar este problema, reemplace `dbo.cube` por `[dbo].[cube]` o `dbo.rollup` por `[dbo].[rollup]`.<br /><br /> Se admite el siguiente ejemplo: `SELECT SUM (x) FROM T  GROUP BY [dbo].[cube](y);`.|Se admite la función definida por el usuario **dbo.cube (** _arg1_ **,** _...argN_) o **dbo.rollup(** _arg1_ **,** _...argN_ **)** en la cláusula GROUP BY.<br /><br /> Por ejemplo: `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`|  
|GROUPING SETS|No compatible|Compatible|Compatible|  
|CUBE|No compatible|Compatible|No compatible|  
|ROLLUP|No compatible|Compatible|No compatible|  
|Total general, como GROUP BY ()|No compatible|Compatible|Compatible|  
|GROUPING_ID, función|No compatible|Compatible|Compatible|  
|GROUPING, función|Compatible|Compatible|Compatible|  
|WITH CUBE|Compatible|Compatible|Compatible|  
|WITH ROLLUP|Compatible|Compatible|Compatible|  
|Eliminación de grupos duplicados de WITH CUBE o WITH ROLLUP|Compatible|Compatible|Compatible| 
 
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-use-a-simple-group-by-clause"></a>A. Usar una cláusula GROUP BY simple  
 En el ejemplo siguiente se recupera el total de cada `SalesOrderID` de la tabla `SalesOrderDetail`. En este ejemplo se usa AdventureWorks.  
  
```sql  
SELECT SalesOrderID, SUM(LineTotal) AS SubTotal  
FROM Sales.SalesOrderDetail AS sod  
GROUP BY SalesOrderID  
ORDER BY SalesOrderID;  
```  
  
### <a name="b-use-a-group-by-clause-with-multiple-tables"></a>B. Usar una cláusula GROUP BY con varias tablas  
 En el ejemplo siguiente se recupera el número de empleados de cada `City` de la tabla `Address` combinada con la tabla `EmployeeAddress`. En este ejemplo se usa AdventureWorks. 
  
```sql  
SELECT a.City, COUNT(bea.AddressID) EmployeeCount  
FROM Person.BusinessEntityAddress AS bea   
    INNER JOIN Person.Address AS a  
        ON bea.AddressID = a.AddressID  
GROUP BY a.City  
ORDER BY a.City;  
```  
  
### <a name="c-use-a-group-by-clause-with-an-expression"></a>C. Usar una cláusula GROUP BY con una expresión  
 En el ejemplo siguiente se recuperan las ventas totales de cada año con la función `DATEPART`. Debe incluirse la misma expresión en la lista `SELECT` y en la cláusula `GROUP BY`.  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,SUM(TotalDue) AS N'Total Order Amount'  
FROM Sales.SalesOrderHeader  
GROUP BY DATEPART(yyyy,OrderDate)  
ORDER BY DATEPART(yyyy,OrderDate);  
```  
  
### <a name="d-use-a-group-by-clause-with-a-having-clause"></a>D. Usar una cláusula GROUP BY con una cláusula HAVING  
 En el ejemplo siguiente se usa la cláusula `HAVING` para especificar cuáles de los grupos generados en la cláusula `GROUP BY` deben incluirse en el conjunto de resultados.  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,SUM(TotalDue) AS N'Total Order Amount'  
FROM Sales.SalesOrderHeader  
GROUP BY DATEPART(yyyy,OrderDate)  
HAVING DATEPART(yyyy,OrderDate) >= N'2003'  
ORDER BY DATEPART(yyyy,OrderDate);  
```  
  
## <a name="examples-azure-synapse-analytics-and-parallel-data-warehouse"></a>Ejemplos: Azure Synapse Analytics y Almacenamiento de datos en paralelo  
  
### <a name="e-basic-use-of-the-group-by-clause"></a>E. Uso básico de la cláusula GROUP BY  
 En el ejemplo siguiente se busca la cantidad total de todas las ventas de cada día. Se devuelve para cada día una fila que contiene la suma de todas las ventas.  
  
```sql  
-- Uses AdventureWorksDW  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales FROM FactInternetSales  
GROUP BY OrderDateKey ORDER BY OrderDateKey;  
```  
  
### <a name="f-basic-use-of-the-distributed_agg-hint"></a>F. Uso básico de la sugerencia DISTRIBUTED_AGG  
 En este ejemplo se usa la sugerencia de consulta DISTRIBUTED_AGG para forzar a que el dispositivo ordene la tabla en la columna `CustomerKey` antes de realizar la agregación.  
  
```sql  
-- Uses AdventureWorksDW  
  
SELECT CustomerKey, SUM(SalesAmount) AS sas  
FROM FactInternetSales  
GROUP BY CustomerKey WITH (DISTRIBUTED_AGG)  
ORDER BY CustomerKey DESC;  
```  
  
### <a name="g-syntax-variations-for-group-by"></a>G. Variaciones de sintaxis para GROUP BY  
 Cuando la lista SELECT no tiene agregaciones, todas las columnas de la lista SELECT deben incluirse en la lista GROUP BY. Las columnas calculadas de la lista SELECT pueden mostrarse en la lista GROUP BY, pero no son necesarias. Estos son algunos ejemplos de instrucciones SELECT correctas sintácticamente:  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, FirstName FROM DimCustomer GROUP BY LastName, FirstName;  
SELECT NumberCarsOwned FROM DimCustomer GROUP BY YearlyIncome, NumberCarsOwned;  
SELECT (SalesAmount + TaxAmt + Freight) AS TotalCost FROM FactInternetSales GROUP BY SalesAmount, TaxAmt, Freight;  
SELECT SalesAmount, SalesAmount*1.10 SalesTax FROM FactInternetSales GROUP BY SalesAmount;  
SELECT SalesAmount FROM FactInternetSales GROUP BY SalesAmount, SalesAmount*1.10;  
```  
  
### <a name="h-using-a-group-by-with-multiple-group-by-expressions"></a>H. Usar GROUP BY con varias expresiones GROUP BY  
 En el ejemplo siguiente se agrupan los resultados mediante varios criterios `GROUP BY`. Si dentro de cada grupo `OrderDateKey` hay subgrupos que se pueden diferenciar mediante `DueDateKey`, se definirá una nueva agrupación para el conjunto de resultados.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OrderDateKey, DueDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales
GROUP BY OrderDateKey, DueDateKey   
ORDER BY OrderDateKey;  
```  
  
### <a name="i-using-a-group-by-clause-with-a-having-clause"></a>I. Utilizar una cláusula GROUP BY con una cláusula HAVING  
 En el ejemplo siguiente se usa la cláusula `HAVING` para especificar los grupos generados en la cláusula `GROUP BY` que deben incluirse en el conjunto de resultados. Solo se incluirán en los resultados los grupos con fechas de pedido de 2004 o años posteriores.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales  
GROUP BY OrderDateKey   
HAVING OrderDateKey > 20040000   
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>Consulte también  
 [GROUPING_ID &#40;Transact-SQL&#41;](~/t-sql/functions/grouping-id-transact-sql.md)   
 [GROUPING &#40;Transact-SQL&#41;](~/t-sql/functions/grouping-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](~/t-sql/queries/select-transact-sql.md)   
 [Cláusula SELECT &#40;Transact-SQL&#41;](~/t-sql/queries/select-clause-transact-sql.md)  
  
  




