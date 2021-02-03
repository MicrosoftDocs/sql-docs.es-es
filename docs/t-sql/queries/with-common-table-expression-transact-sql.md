---
title: WITH common_table_expression (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL sobre cómo usar expresiones de tabla comunes (CTE) en las consultas.
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WITH common_table_expression
- WITH_TSQL
- WITH
- common table expression
dev_langs:
- TSQL
helpviewer_keywords:
- WITH common_table_expression clause
- CTEs
- hierarchical queries [SQL Server], WITH common_table_expression
- recursive CTEs [SQL Server]
- recursive queries [SQL Server]
- common table expressions
- MAXRECURSION hint
- clauses [SQL Server], WITH common_table_expression
ms.assetid: 27cfb819-3e8d-4274-8bbe-cbbe4d9c2e23
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1f2455ad3601ac5139766e732046d9436513185c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192808"
---
# <a name="with-common_table_expression-transact-sql"></a>WITH common_table_expression (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Especifica un conjunto de resultados temporal con nombre, conocido como expresión de tabla común (CTE). Se deriva de una consulta simple y se define en el ámbito de ejecución de una sola instrucción SELECT, INSERT, UPDATE, DELETE o MERGE. Esta cláusula también se puede utilizar en una instrucción CREATE VIEW como parte de la instrucción SELECT que la define. Una expresión de tabla común puede incluir referencias a ella misma. Esto se conoce como expresión de tabla común recursiva.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
[ WITH <common_table_expression> [ ,...n ] ]  
  
<common_table_expression>::=  
    expression_name [ ( column_name [ ,...n ] ) ]  
    AS  
    ( CTE_query_definition )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression_name*  
Es un identificador válido de la expresión de tabla común. *expression_name* debe ser diferente del nombre de cualquier otra expresión de tabla común definida en la misma cláusula WITH \<common_table_expression>, pero *expression_name* puede coincidir con el nombre de una vista o tabla base. Cualquier referencia a *expression_name* en la consulta usa la expresión de tabla común y no el objeto base.
  
 *column_name*  
 Especifica un nombre de columna en la expresión de tabla común. No se permiten nombres duplicados en una misma definición de CTE. El número de nombres de columna especificado debe coincidir con el número de columnas del conjunto de resultados de *CTE_query_definition*. La lista de nombres de columna es opcional solamente si en la definición de la consulta se suministran nombres diferentes para todas las columnas resultantes.  
  
 *CTE_query_definition*  
 Especifica una instrucción SELECT cuyo conjunto de resultados llena la expresión de tabla común. La instrucción SELECT de *CTE_query_definition* debe cumplir los mismos requisitos que en la creación de una vista, excepto que una expresión CTE no puede definir otra expresión CTE. Para más información, vea la sección Comentarios y [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 Si se define más de una *definición_de_consulta_de_CTE*, es preciso unir las definiciones de consulta mediante uno de estos operadores de conjuntos: UNION ALL, UNION, EXCEPT o INTERSECT.  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="guidelines-for-creating-and-using-common-table-expressions"></a>Instrucciones para crear y utilizar expresiones de tabla comunes  
Las instrucciones siguientes se aplican a expresiones de tabla comunes no recursivas. Para obtener instrucciones que se aplican a expresiones de tabla comunes recursivas, consulte [Instrucciones para definir y usar expresiones de tabla comunes recursivas](#guidelines-for-defining-and-using-recursive-common-table-expressions) más adelante.  
  
-   Una expresión CTE debe ir seguida de una única instrucción `SELECT`, `INSERT`, `UPDATE` o `DELETE` que haga referencia a una parte o a la totalidad de sus columnas. Una expresión CTE también se puede especificar en una instrucción `CREATE VIEW` como parte de la instrucción `SELECT` de definición de la vista.  
  
-   Se pueden especificar varias definiciones de consulta de CTE en una CTE no recursiva. Las definiciones deben combinarse mediante uno de estos operadores de conjuntos: `UNION ALL`, `UNION`, `INTERSECT` o `EXCEPT`.  
  
-   Una expresión CTE puede hacer referencia a ella misma y a otras expresiones CTE previamente definidas en la misma cláusula `WITH`. No se permite la referencia adelantada.  
  
-   No se permite especificar más de una cláusula WITH en una expresión CTE. Por ejemplo, si un argumento *CTE_query_definition* contiene una subconsulta, esta no puede contener ninguna cláusula WITH anidada que defina otra expresión CTE.  
  
-   En la definición de *CTE_query_definition* no se pueden usar las siguientes cláusulas:  
  
    -   `ORDER BY` (excepto cuando se especifica una cláusula `TOP`)  
  
    -   `INTO`  
  
    -   Cláusula `OPTION` con sugerencias de consulta  
  
    -   `FOR BROWSE`  
  
-   Cuando se utiliza una expresión CTE en una instrucción que forma parte de un lote, la instrucción que la precede debe ir seguida de punto y coma.  
  
-   Una consulta que haga referencia a una CTE se puede utilizar para definir un cursor.  
  
-   En la expresión CTE se puede hacer referencia a tablas de servidores remotos.  
  
-   Cuando se ejecuta una CTE, todas las sugerencias que hagan referencia a ella pueden entrar en conflicto con otras sugerencias detectadas cuando la CTE tiene acceso a sus tablas subyacentes, de la misma manera que las sugerencias que hacen referencia a vistas en las consultas. En ese caso, la consulta devuelve un error.  
  
## <a name="guidelines-for-defining-and-using-recursive-common-table-expressions"></a>Instrucciones para definir y usar expresiones de tabla comunes recursivas  
 Las instrucciones siguientes se aplican a la definición de una expresión de tabla común recursiva:  
  
-   La definición de la CTE recursiva debe contener al menos dos definiciones de consulta de CTE, un miembro no recursivo y un miembro recursivo. Se pueden definir varios miembros no recursivos y recursivos, aunque todas las definiciones de consultas de miembros no recursivos deben colocarse delante de la primera definición de miembro recursivo. Todas las definiciones de consulta de CTE son miembros no recursivos a menos que hagan referencia a la propia CTE.  
  
-   Los miembros no recursivos deben combinarse mediante uno de estos operadores de conjuntos: UNION ALL, UNION, INTERSECT o EXCEPT. UNION ALL es el único operador de conjuntos permitido entre el último miembro no recursivo y el primer miembro recursivo, y si se combinan varios miembros recursivos.  
  
-   El número de columnas de los miembros no recursivo y recursivo debe coincidir.  
  
-   El tipo de datos de una columna del miembro recursivo debe ser igual al tipo de datos de la columna correspondiente en el miembro no recursivo.  
  
-   La cláusula FROM de un miembro recursivo solo debe hacer referencia una vez a *expression_name* de CTE.  
  
-   No se permiten los siguientes elementos en el parámetro *CTE_query_definition* de un miembro recursivo:  
  
    -   `SELECT DISTINCT`  
  
    -   `GROUP BY`  
  
    -   `PIVOT` (cuando el nivel de compatibilidad de la base de datos sea 110 o superior. Vea [Cambios substanciales en las características del Motor de base de datos de SQL Server 2016](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md)).  
  
    -   `HAVING`  
  
    -   Agregación escalar  
  
    -   `TOP`  
  
    -   `LEFT`, `RIGHT`, `OUTER JOIN` (`INNER JOIN` se permite)  
  
    -   Subconsultas  
  
    -   Una sugerencia aplicada a una referencia recursiva a una CTE dentro de *CTE_query_definition*.  
  
 Las instrucciones siguientes se aplican al uso de una expresión de tabla común recursiva:  
  
-   Todas las columnas devueltas por la expresión CTE recursiva aceptan valores NULL independientemente de la nulabilidad de las columnas devueltas por las instrucciones `SELECT` participantes.  
  
-   Una expresión CTE formada incorrectamente puede generar un bucle infinito. Por ejemplo, si la definición de la consulta del miembro recursivo devuelve los mismos valores para las columnas primarias y secundarias, se crea un bucle infinito. Para evitar que se genere un bucle infinito, se puede limitar el número de niveles de recursividad permitidos para una instrucción determinada mediante el uso de la sugerencia `MAXRECURSION` y un valor de 0 a 32 767 en la cláusula OPTION de la instrucción `INSERT`, `UPDATE`, `DELETE` o `SELECT`. De esta manera, se puede controlar la ejecución de la instrucción hasta que se resuelva el problema de código que genera el bucle. El valor predeterminado de todo el servidor es 100. Cuando se especifica 0, no se aplica ningún límite. Solo se puede especificar un valor `MAXRECURSION` por instrucción. Para obtener más información, vea [Sugerencias de consulta &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
-   No se puede utilizar una vista que contenga una expresión de tabla común recursiva para actualizar datos.  
  
-   Se pueden definir cursores en las consultas que utilicen expresiones CTE. La expresión CTE es el argumento de *select_statement* que define el conjunto de resultados del cursor. En el caso de las CTE recursivas únicamente se permiten los cursores de solo avance rápido y estáticos (de instantánea). Si se especifica otro tipo de cursor en una CTE recursiva, el tipo de cursor se convierte a estático.  
  
-   En la expresión CTE se puede hacer referencia a tablas de servidores remotos. Si se hace referencia al servidor remoto en el miembro recursivo de la CTE, se crea una cola para cada tabla remota de manera que se pueda tener acceso local a las tablas repetidas veces. Si es una consulta de CTE, aparecerá Index Spool/Lazy Spools en el plan de consulta y tendrá el predicado adicional `WITH STACK`. Esta es una forma de confirmar la recursividad apropiada.  
  
-   Las funciones analíticas y de agregado de la parte recursiva del CTE se aplican al conjunto para el nivel de recursividad actual y no al conjunto para el CTE. Las funciones como `ROW_NUMBER` solo funcionan sobre el subconjunto de datos que les pasa el nivel de recursividad actual y no sobre todo el conjunto de datos pasados a la parte recursiva de la CTE. Para más información, vea el ejemplo K "Utilizar funciones analíticas en una CTE recursiva" más abajo.  
  
## <a name="features-and-limitations-of-common-table-expressions-in-sssdw-and-sspdw"></a>Características y limitaciones de las expresiones de tabla comunes en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 La implementación actual de CTE en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] presenta las siguientes características y limitaciones:  
  
-   Una CTE se puede especificar en una instrucción `SELECT`.  
  
-   Una CTE se puede especificar en una instrucción `CREATE VIEW`.  
  
-   Una CTE se puede especificar en una instrucción `CREATE TABLE AS SELECT` (CTAS).  
  
-   Una CTE se puede especificar en una instrucción `CREATE REMOTE TABLE AS SELECT` (CRTAS).  
  
-   Una CTE se puede especificar en una instrucción `CREATE EXTERNAL TABLE AS SELECT` (CETAS).  
  
-   Se puede hacer referencia a una tabla remota desde una CTE.  
  
-   Se puede hacer referencia a una tabla externa desde una CTE.  
  
-   Se pueden especificar varias definiciones de consulta de CTE en una CTE.  
  
-   Una CTE debe ir seguida de una sola instrucción `SELECT`. No se admiten las instrucciones `INSERT`, `UPDATE`, `DELETE` y `MERGE`.  
  
-   No se admiten expresiones de tabla comunes que incluyan referencias a sí mismas (es decir, expresiones de tabla comunes recursivas).  
  
-   No se puede especificar más de una cláusula `WITH` en una CTE. Por ejemplo, si una definición de consulta de CTE contiene una subconsulta, esta no puede contener ninguna cláusula `WITH` anidada que defina otra CTE.  
  
-   No se puede usar una cláusula `ORDER BY` en CTE_query_definition, excepto cuando se especifique una cláusula `TOP`.  
  
-   Cuando se utiliza una expresión CTE en una instrucción que forma parte de un lote, la instrucción que la precede debe ir seguida de punto y coma.  
  
-   Al usarlas en instrucciones preparadas por `sp_prepare`, las CTE se comportarán del mismo modo que otras instrucciones `SELECT` en PDW. Sin embargo, si las CTE se usan como parte de una instrucción CREATE EXTERNAL TABLE AS SELECT preparada por `sp_prepare`, el comportamiento puede diferir de las instrucciones [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de otras instrucciones de PDW debido a la forma en que el enlace se implementa con `sp_prepare`. Si la instrucción `SELECT` que hace referencia a la CTE usa una columna incorrecta que no existe en la CTE, `sp_prepare` se pasará sin detectar el error, pero el error sí que se generará durante `sp_execute`.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-simple-common-table-expression"></a>A. Crear una expresión de tabla común simple  
 En el siguiente ejemplo se muestra el número total de pedidos de venta por año para cada representante de ventas en [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].  
  
```sql   
-- Define the CTE expression name and column list.  
WITH Sales_CTE (SalesPersonID, SalesOrderID, SalesYear)  
AS  
-- Define the CTE query.  
(  
    SELECT SalesPersonID, SalesOrderID, YEAR(OrderDate) AS SalesYear  
    FROM Sales.SalesOrderHeader  
    WHERE SalesPersonID IS NOT NULL  
)  
-- Define the outer query referencing the CTE name.  
SELECT SalesPersonID, COUNT(SalesOrderID) AS TotalSales, SalesYear  
FROM Sales_CTE  
GROUP BY SalesYear, SalesPersonID  
ORDER BY SalesPersonID, SalesYear;  
```  
  
### <a name="b-using-a-common-table-expression-to-limit-counts-and-report-averages"></a>B. Usar una expresión de tabla común para limitar recuentos y promedios de informes  
 En el siguiente ejemplo se muestra el número medio de pedidos de venta correspondiente a todos los años para los representantes de ventas.  
  
```sql  
WITH Sales_CTE (SalesPersonID, NumberOfOrders)  
AS  
(  
    SELECT SalesPersonID, COUNT(*)  
    FROM Sales.SalesOrderHeader  
    WHERE SalesPersonID IS NOT NULL  
    GROUP BY SalesPersonID  
)  
SELECT AVG(NumberOfOrders) AS "Average Sales Per Person"  
FROM Sales_CTE;  
```  
  
### <a name="c-using-multiple-cte-definitions-in-a-single-query"></a>C. Usar varias definiciones de CTE en una sola consulta  
 En el ejemplo siguiente se muestra cómo definir más de una CTE en una sola consulta. Observe que se usa una coma para separar las definiciones de consulta CTE. La función FORMAT, utilizada para mostrar las cantidades de moneda en un formato de moneda, está disponible en SQL Server 2012 y versiones posteriores.  
  
```sql  
WITH Sales_CTE (SalesPersonID, TotalSales, SalesYear)  
AS  
-- Define the first CTE query.  
(  
    SELECT SalesPersonID, SUM(TotalDue) AS TotalSales, YEAR(OrderDate) AS SalesYear  
    FROM Sales.SalesOrderHeader  
    WHERE SalesPersonID IS NOT NULL  
       GROUP BY SalesPersonID, YEAR(OrderDate)  
  
)  
,   -- Use a comma to separate multiple CTE definitions.  
  
-- Define the second CTE query, which returns sales quota data by year for each sales person.  
Sales_Quota_CTE (BusinessEntityID, SalesQuota, SalesQuotaYear)  
AS  
(  
       SELECT BusinessEntityID, SUM(SalesQuota)AS SalesQuota, YEAR(QuotaDate) AS SalesQuotaYear  
       FROM Sales.SalesPersonQuotaHistory  
       GROUP BY BusinessEntityID, YEAR(QuotaDate)  
)  
  
-- Define the outer query by referencing columns from both CTEs.  
SELECT SalesPersonID  
  , SalesYear  
  , FORMAT(TotalSales,'C','en-us') AS TotalSales  
  , SalesQuotaYear  
  , FORMAT (SalesQuota,'C','en-us') AS SalesQuota  
  , FORMAT (TotalSales -SalesQuota, 'C','en-us') AS Amt_Above_or_Below_Quota  
FROM Sales_CTE  
JOIN Sales_Quota_CTE ON Sales_Quota_CTE.BusinessEntityID = Sales_CTE.SalesPersonID  
                    AND Sales_CTE.SalesYear = Sales_Quota_CTE.SalesQuotaYear  
ORDER BY SalesPersonID, SalesYear;    
```  
  
A continuación se muestra un conjunto parcial de resultados.  
  
```  
SalesPersonID SalesYear   TotalSales    SalesQuotaYear SalesQuota  Amt_Above_or_Below_Quota  
------------- ---------   -----------   -------------- ---------- ----------------------------------   
274           2005        $32,567.92    2005           $35,000.00  ($2,432.08)  
274           2006        $406,620.07   2006           $455,000.00 ($48,379.93)  
274           2007        $515,622.91   2007           $544,000.00 ($28,377.09)  
274           2008        $281,123.55   2008           $271,000.00  $10,123.55  
```  
  
### <a name="d-using-a-recursive-common-table-expression-to-display-multiple-levels-of-recursion"></a>D. Usar una expresión de tabla común recursiva para mostrar varios niveles de recursividad  
 En el ejemplo siguiente se muestra la lista jerárquica de los directivos y de los empleados que tienen a su cargo. En el ejemplo se empieza creando y rellenando la tabla `dbo.MyEmployees`.  
  
```sql  
-- Create an Employee table.  
CREATE TABLE dbo.MyEmployees  
(  
EmployeeID SMALLINT NOT NULL,  
FirstName NVARCHAR(30)  NOT NULL,  
LastName  NVARCHAR(40) NOT NULL,  
Title NVARCHAR(50) NOT NULL,  
DeptID SMALLINT NOT NULL,  
ManagerID INT NULL,  
 CONSTRAINT PK_EmployeeID PRIMARY KEY CLUSTERED (EmployeeID ASC)   
);  
-- Populate the table with values.  
INSERT INTO dbo.MyEmployees VALUES   
 (1, N'Ken', N'Sánchez', N'Chief Executive Officer',16,NULL)  
,(273, N'Brian', N'Welcker', N'Vice President of Sales',3,1)  
,(274, N'Stephen', N'Jiang', N'North American Sales Manager',3,273)  
,(275, N'Michael', N'Blythe', N'Sales Representative',3,274)  
,(276, N'Linda', N'Mitchell', N'Sales Representative',3,274)  
,(285, N'Syed', N'Abbas', N'Pacific Sales Manager',3,273)  
,(286, N'Lynn', N'Tsoflias', N'Sales Representative',3,285)  
,(16,  N'David',N'Bradley', N'Marketing Manager', 4, 273)  
,(23,  N'Mary', N'Gibson', N'Marketing Specialist', 4, 16);  
```  
  
```sql
WITH DirectReports(ManagerID, EmployeeID, Title, EmployeeLevel) AS   
(  
    SELECT ManagerID, EmployeeID, Title, 0 AS EmployeeLevel  
    FROM dbo.MyEmployees   
    WHERE ManagerID IS NULL  
    UNION ALL  
    SELECT e.ManagerID, e.EmployeeID, e.Title, EmployeeLevel + 1  
    FROM dbo.MyEmployees AS e  
        INNER JOIN DirectReports AS d  
        ON e.ManagerID = d.EmployeeID   
)  
SELECT ManagerID, EmployeeID, Title, EmployeeLevel   
FROM DirectReports  
ORDER BY ManagerID;   
```  
  
#### <a name="using-a-recursive-common-table-expression-to-display-two-levels-of-recursion"></a>Usar una expresión de tabla común recursiva para mostrar dos niveles de recursividad  
 En el ejemplo siguiente se muestran los directivos y los empleados que tienen a su cargo. El número de niveles devueltos está limitado a dos.  
  
```sql
WITH DirectReports(ManagerID, EmployeeID, Title, EmployeeLevel) AS   
(  
    SELECT ManagerID, EmployeeID, Title, 0 AS EmployeeLevel  
    FROM dbo.MyEmployees   
    WHERE ManagerID IS NULL  
    UNION ALL  
    SELECT e.ManagerID, e.EmployeeID, e.Title, EmployeeLevel + 1  
    FROM dbo.MyEmployees AS e  
        INNER JOIN DirectReports AS d  
        ON e.ManagerID = d.EmployeeID   
)  
SELECT ManagerID, EmployeeID, Title, EmployeeLevel   
FROM DirectReports  
WHERE EmployeeLevel <= 2 ;  
```  
  
#### <a name="using-a-recursive-common-table-expression-to-display-a-hierarchical-list"></a>Usar una expresión de tabla común recursiva para mostrar una lista jerárquica  
 En el siguiente ejemplo se agregan los nombres del directivo y de los empleados, así como sus cargos respectivos. La jerarquía de directivos y empleados se resalta más mediante la aplicación de sangrías a cada nivel.  
  
```sql
WITH DirectReports(Name, Title, EmployeeID, EmployeeLevel, Sort)  
AS (SELECT CONVERT(VARCHAR(255), e.FirstName + ' ' + e.LastName),  
        e.Title,  
        e.EmployeeID,  
        1,  
        CONVERT(VARCHAR(255), e.FirstName + ' ' + e.LastName)  
    FROM dbo.MyEmployees AS e  
    WHERE e.ManagerID IS NULL  
    UNION ALL  
    SELECT CONVERT(VARCHAR(255), REPLICATE ('|    ' , EmployeeLevel) +  
        e.FirstName + ' ' + e.LastName),  
        e.Title,  
        e.EmployeeID,  
        EmployeeLevel + 1,  
        CONVERT (VARCHAR(255), RTRIM(Sort) + '|    ' + FirstName + ' ' +   
                 LastName)  
    FROM dbo.MyEmployees AS e  
    JOIN DirectReports AS d ON e.ManagerID = d.EmployeeID  
    )  
SELECT EmployeeID, Name, Title, EmployeeLevel  
FROM DirectReports   
ORDER BY Sort;  
```  
  
#### <a name="using-maxrecursion-to-cancel-a-statement"></a>Usar MAXRECURSION para cancelar una instrucción  
 `MAXRECURSION` se puede utilizar para impedir que una CTE recursiva con formato incorrecto entre en un bucle infinito. En el ejemplo siguiente se crea un bucle infinito intencionadamente y se utiliza la sugerencia `MAXRECURSION` para limitar el número de niveles de recursividad a dos.  
  
```sql
--Creates an infinite loop  
WITH cte (EmployeeID, ManagerID, Title) AS  
(  
    SELECT EmployeeID, ManagerID, Title  
    FROM dbo.MyEmployees  
    WHERE ManagerID IS NOT NULL  
  UNION ALL  
    SELECT cte.EmployeeID, cte.ManagerID, cte.Title  
    FROM cte   
    JOIN  dbo.MyEmployees AS e   
        ON cte.ManagerID = e.EmployeeID  
)  
--Uses MAXRECURSION to limit the recursive levels to 2  
SELECT EmployeeID, ManagerID, Title  
FROM cte  
OPTION (MAXRECURSION 2);  
```  
  
 Después de corregir el error de código, ya no se requiere MAXRECURSION. En el siguiente ejemplo se muestra el código corregido.  
  
```sql
WITH cte (EmployeeID, ManagerID, Title)  
AS  
(  
    SELECT EmployeeID, ManagerID, Title  
    FROM dbo.MyEmployees  
    WHERE ManagerID IS NOT NULL  
  UNION ALL  
    SELECT  e.EmployeeID, e.ManagerID, e.Title  
    FROM dbo.MyEmployees AS e  
    JOIN cte ON e.ManagerID = cte.EmployeeID  
)  
SELECT EmployeeID, ManagerID, Title  
FROM cte;  
```  
  
### <a name="e-using-a-common-table-expression-to-selectively-step-through-a-recursive-relationship-in-a-select-statement"></a>E. Usar una expresión de tabla común para recorrer selectivamente y paso a paso una relación recursiva en una instrucción SELECT  
 En el ejemplo siguiente se muestra la jerarquía de ensamblados y componentes de producto necesarios para fabricar la bicicleta para `ProductAssemblyID = 800`.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH Parts(AssemblyID, ComponentID, PerAssemblyQty, EndDate, ComponentLevel) AS  
(  
    SELECT b.ProductAssemblyID, b.ComponentID, b.PerAssemblyQty,  
        b.EndDate, 0 AS ComponentLevel  
    FROM Production.BillOfMaterials AS b  
    WHERE b.ProductAssemblyID = 800  
          AND b.EndDate IS NULL  
    UNION ALL  
    SELECT bom.ProductAssemblyID, bom.ComponentID, p.PerAssemblyQty,  
        bom.EndDate, ComponentLevel + 1  
    FROM Production.BillOfMaterials AS bom   
        INNER JOIN Parts AS p  
        ON bom.ProductAssemblyID = p.ComponentID  
        AND bom.EndDate IS NULL  
)  
SELECT AssemblyID, ComponentID, Name, PerAssemblyQty, EndDate,  
        ComponentLevel   
FROM Parts AS p  
    INNER JOIN Production.Product AS pr  
    ON p.ComponentID = pr.ProductID  
ORDER BY ComponentLevel, AssemblyID, ComponentID;  
```  
  
### <a name="f-using-a-recursive-cte-in-an-update-statement"></a>F. Usar una CTE recursiva en una instrucción UPDATE  
 En el siguiente ejemplo se actualiza el valor de `PerAssemblyQty` para todos los componentes que se usan para fabricar el producto 'Road-550-W Yellow, 44' `(ProductAssemblyID``800`). La expresión de tabla común devuelve una lista jerárquica de los elementos que se utilizan para fabricar `ProductAssemblyID 800` y los componentes que se utilizan para crear esos elementos, etc. Solo se modifican las filas devueltas por la expresión de tabla común.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH Parts(AssemblyID, ComponentID, PerAssemblyQty, EndDate, ComponentLevel) AS  
(  
    SELECT b.ProductAssemblyID, b.ComponentID, b.PerAssemblyQty,  
        b.EndDate, 0 AS ComponentLevel  
    FROM Production.BillOfMaterials AS b  
    WHERE b.ProductAssemblyID = 800  
          AND b.EndDate IS NULL  
    UNION ALL  
    SELECT bom.ProductAssemblyID, bom.ComponentID, p.PerAssemblyQty,  
        bom.EndDate, ComponentLevel + 1  
    FROM Production.BillOfMaterials AS bom   
        INNER JOIN Parts AS p  
        ON bom.ProductAssemblyID = p.ComponentID  
        AND bom.EndDate IS NULL  
)  
UPDATE Production.BillOfMaterials  
SET PerAssemblyQty = c.PerAssemblyQty * 2  
FROM Production.BillOfMaterials AS c  
JOIN Parts AS d ON c.ProductAssemblyID = d.AssemblyID  
WHERE d.ComponentLevel = 0;  
```  
  
### <a name="h-using-multiple-anchor-and-recursive-members"></a>H. Usar varios miembros no recursivos y recursivos  
 En el ejemplo siguiente se utilizan varios miembros no recursivos y recursivos para devolver todos los antecesores de una persona especificada. Se crea una tabla y se insertan valores en ella para establecer la genealogía familiar devuelta por la CTE recursiva.  
  
```sql  
-- Genealogy table  
IF OBJECT_ID('dbo.Person','U') IS NOT NULL DROP TABLE dbo.Person;  
GO  
CREATE TABLE dbo.Person(ID int, Name VARCHAR(30), Mother INT, Father INT);  
GO  
INSERT dbo.Person   
VALUES(1, 'Sue', NULL, NULL)  
      ,(2, 'Ed', NULL, NULL)  
      ,(3, 'Emma', 1, 2)  
      ,(4, 'Jack', 1, 2)  
      ,(5, 'Jane', NULL, NULL)  
      ,(6, 'Bonnie', 5, 4)  
      ,(7, 'Bill', 5, 4);  
GO  
-- Create the recursive CTE to find all of Bonnie's ancestors.  
WITH Generation (ID) AS  
(  
-- First anchor member returns Bonnie's mother.  
    SELECT Mother   
    FROM dbo.Person  
    WHERE Name = 'Bonnie'  
UNION  
-- Second anchor member returns Bonnie's father.  
    SELECT Father   
    FROM dbo.Person  
    WHERE Name = 'Bonnie'  
UNION ALL  
-- First recursive member returns male ancestors of the previous generation.  
    SELECT Person.Father  
    FROM Generation, Person  
    WHERE Generation.ID=Person.ID  
UNION ALL  
-- Second recursive member returns female ancestors of the previous generation.  
    SELECT Person.Mother  
    FROM Generation, dbo.Person  
    WHERE Generation.ID=Person.ID  
)  
SELECT Person.ID, Person.Name, Person.Mother, Person.Father  
FROM Generation, dbo.Person  
WHERE Generation.ID = Person.ID;  
GO  
```  
  
###  <a name="i-using-analytical-functions-in-a-recursive-cte"></a><a name="bkmkUsingAnalyticalFunctionsInARecursiveCTE"></a> I. Utilizar funciones analíticas en una CTE recursiva  
 En el siguiente ejemplo se muestra un error que puede producirse al utilizar una función analítica o de agregado en la parte recursiva de una CTE.  
  
```sql  
DECLARE @t1 TABLE (itmID INT, itmIDComp INT);  
INSERT @t1 VALUES (1,10), (2,10);   
  
DECLARE @t2 TABLE (itmID INT, itmIDComp INT);   
INSERT @t2 VALUES (3,10), (4,10);   
  
WITH vw AS  
 (  
    SELECT itmIDComp, itmID  
    FROM @t1  
  
    UNION ALL  
  
    SELECT itmIDComp, itmID  
    FROM @t2  
)   
,r AS  
 (  
    SELECT t.itmID AS itmIDComp  
           , NULL AS itmID  
           ,CAST(0 AS BITING) AS N  
           ,1 AS Lvl  
    FROM (SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4) AS t (itmID)   
  
UNION ALL  
  
SELECT t.itmIDComp  
    , t.itmID  
    , ROW_NUMBER() OVER(PARTITION BY t.itmIDComp ORDER BY t.itmIDComp, t.itmID) AS N  
    , Lvl + 1  
FROM r   
    JOIN vw AS t ON t.itmID = r.itmIDComp  
)   
  
SELECT Lvl, N FROM r;  
```  
  
Los siguientes resultados son los esperados para la consulta.  
  
```  
Lvl  N  
1    0  
1    0  
1    0  
1    0  
2    4  
2    3  
2    2  
2    1  
```  
  
Los siguientes resultados son los resultados reales de la consulta.  
  
```  
Lvl  N  
1    0  
1    0  
1    0  
1    0  
2    1  
2    1  
2    1  
2    1  
```  
  
 `N` devuelve 1 para cada paso de la parte recursiva del CTE, porque solo el subconjunto de datos para ese nivel de recursividad se pasa a `ROWNUMBER`. Por cada iteración de la parte recursiva de la consulta solo se pasa una fila a `ROWNUMBER`.  
  
## <a name="examples-sssdw-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="j-using-a-common-table-expression-within-a-ctas-statement"></a>J. Usar una expresión de tabla común en una instrucción CREATE TABLE AS SELECT  
 En el siguiente ejemplo se crea una tabla que contiene el número total de pedidos de venta por año de cada representante de ventas en [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO   
CREATE TABLE SalesOrdersPerYear  
WITH  
(  
    DISTRIBUTION = HASH(SalesPersonID)  
)  
AS  
    -- Define the CTE expression name and column list.  
    WITH Sales_CTE (SalesPersonID, SalesOrderID, SalesYear)  
    AS  
    -- Define the CTE query.  
    (  
        SELECT SalesPersonID, SalesOrderID, YEAR(OrderDate) AS SalesYear  
        FROM Sales.SalesOrderHeader  
        WHERE SalesPersonID IS NOT NULL  
    )  
    -- Define the outer query referencing the CTE name.  
    SELECT SalesPersonID, COUNT(SalesOrderID) AS TotalSales, SalesYear  
    FROM Sales_CTE  
    GROUP BY SalesYear, SalesPersonID  
    ORDER BY SalesPersonID, SalesYear;  
GO  
```  
  
### <a name="k-using-a-common-table-expression-within-a-cetas-statement"></a>K. Usar una expresión de tabla común en una instrucción CREATE EXTERNAL TABLE AS SELECT  
 En el siguiente ejemplo se crea una tabla externa que contiene el número total de pedidos de venta por año de cada representante de ventas en [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO    
CREATE EXTERNAL TABLE SalesOrdersPerYear  
WITH  
(  
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:5000/files/Customer',  
    FORMAT_OPTIONS ( FIELD_TERMINATOR = '|' )   
)  
AS  
    -- Define the CTE expression name and column list.  
    WITH Sales_CTE (SalesPersonID, SalesOrderID, SalesYear)  
    AS  
    -- Define the CTE query.  
    (  
        SELECT SalesPersonID, SalesOrderID, YEAR(OrderDate) AS SalesYear  
        FROM Sales.SalesOrderHeader  
        WHERE SalesPersonID IS NOT NULL  
    )  
    -- Define the outer query referencing the CTE name.  
    SELECT SalesPersonID, COUNT(SalesOrderID) AS TotalSales, SalesYear  
    FROM Sales_CTE  
    GROUP BY SalesYear, SalesPersonID  
    ORDER BY SalesPersonID, SalesYear;  
GO  
```  
  
### <a name="l-using-multiple-comma-separated-ctes-in-a-statement"></a>L. Uso de varias CTE separadas por comas en una instrucción  
 En el siguiente ejemplo se muestra cómo incluir dos CTE en una misma instrucción. Las CTE no se pueden anidar (no debe haber recursión).  
  
```sql  
WITH   
 CountDate (TotalCount, TableName) AS  
    (  
     SELECT COUNT(datekey), 'DimDate' FROM DimDate  
    ) ,  
 CountCustomer (TotalAvg, TableName) AS  
    (  
     SELECT COUNT(CustomerKey), 'DimCustomer' FROM DimCustomer  
    )  
SELECT TableName, TotalCount FROM CountDate  
UNION ALL  
SELECT TableName, TotalAvg FROM CountCustomer;  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [EXCEPT e INTERSECT &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  
  
 
