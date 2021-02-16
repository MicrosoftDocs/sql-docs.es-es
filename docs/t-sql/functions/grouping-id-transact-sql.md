---
description: GROUPING_ID (Transact-SQL)
title: GROUPING_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUPING_ID_TSQL
- GROUPING_ID
dev_langs:
- TSQL
helpviewer_keywords:
- GROUP BY clause, GROUPING_ID
- GROUPING_ID function
ms.assetid: c1050658-b19f-42ee-9a05-ecd6a73b896c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 828a4c0e33722a22aae4779c7197a7630578a438
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338031"
---
# <a name="grouping_id-transact-sql"></a>GROUPING_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Es una función que calcula el nivel de agrupación. GROUPING_ID solo se puede usar en la lista de las cláusulas SELECT \<select>, HAVING o ORDER BY si se especifica GROUP BY.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
GROUPING_ID ( <column_expression>[ ,...n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 \<column_expression>  
 Es una *column_expression* en una cláusula [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md).  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **int**  
  
## <a name="remarks"></a>Observaciones  
 El elemento GROUPING_ID \<column_expression> debe coincidir exactamente con la expresión en la lista GROUP BY. Por ejemplo, si agrupa el contenido por DATEPART (yyyy, \<*column name*>), use GROUPING_ID (DATEPART (yyyy, \<*column name*>)); si lo hace por \<*column name*>, use GROUPING_ID (\<*column name*>).  
  
## <a name="comparing-grouping_id--to-grouping-"></a>Comparar GROUPING_ID() con GROUPING()  
 GROUPING_ID (\<column_expression> [ **,** ...*n* ]) introduce el equivalente del valor devuelto de GROUPING (\<column_expression>) para cada columna de su lista de columnas en cada fila de salida como una cadena de unos y ceros. GROUPING_ID interpreta dicha cadena como un número de base 2 y devuelve el número entero equivalente. Por ejemplo, considere la siguiente instrucción: `SELECT a, b, c, SUM(d),``GROUPING_ID(a,b,c)``FROM T GROUP BY <group by list>`. La tabla siguiente muestra los valores de entrada y salida de GROUPING_ID().  
  
|Columnas agregadas|Entrada de GROUPING_ID (a, b, c) = GROUPING(a) + GROUPING(b) + GROUPING(c)|Resultado de GROUPING_ID()|  
|------------------------|---------------------------------------------------------------------------------------|------------------------------|  
|`a`|`100`|`4`|  
|`b`|`010`|`2`|  
|`c`|`001`|`1`|  
|`ab`|`110`|`6`|  
|`ac`|`101`|`5`|  
|`bc`|`011`|`3`|  
|`abc`|`111`|`7`|  
  
## <a name="technical-definition-of-grouping_id-"></a>Definición técnica de GROUPING_ID ()  
 Cada argumento de GROUPING_ID debe ser un elemento de la lista GROUP BY. GROUPING_ID () devuelve un mapa de bits de **integer** cuyos bits N más bajos pueden ser literales. Un **bit** literal indica que el argumento correspondiente no es una columna de agrupación en la fila de salida determinada. El **bit** de menor orden corresponde al argumento N y el **bit** de menor orden de N-1 <sup>ésimo</sup>, al argumento 1.  
  
## <a name="grouping_id--equivalents"></a>Equivalentes de GROUPING_ID ()  
 En una consulta de agrupación única, GROUPING (\<column_expression>) equivale a GROUPING_ID (\<column_expression>) y ambos devuelven 0.  
  
 Por ejemplo, las siguientes instrucciones son equivalentes:  
  
 Instrucción A:  
  
```sql  
SELECT GROUPING_ID(A,B)  
FROM T   
GROUP BY CUBE(A,B)   
```  
  
 Instrucción B:  
  
```sql  
SELECT 3 FROM T GROUP BY ()  
UNION ALL  
SELECT 1 FROM T GROUP BY A  
UNION ALL  
SELECT 2 FROM T GROUP BY B  
UNION ALL  
SELECT 0 FROM T GROUP BY A,B  
```  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-grouping_id-to-identify-grouping-levels"></a>A. Usar GROUPING_ID para identificar niveles de agrupación  
 El ejemplo siguiente devuelve el recuento de empleados por `Name` y `Title`, `Name,` y total de la compañía de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. `GROUPING_ID()` se usa para crear un valor en cada fila de la columna `Title` que identifica su nivel de agregación.  
  
```sql  
SELECT D.Name  
    ,CASE   
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 0 THEN E.JobTitle  
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 1 THEN N'Total: ' + D.Name   
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 3 THEN N'Company Total:'  
        ELSE N'Unknown'  
    END AS N'Job Title'  
    ,COUNT(E.BusinessEntityID) AS N'Employee Count'  
FROM HumanResources.Employee E  
    INNER JOIN HumanResources.EmployeeDepartmentHistory DH  
        ON E.BusinessEntityID = DH.BusinessEntityID  
    INNER JOIN HumanResources.Department D  
        ON D.DepartmentID = DH.DepartmentID       
WHERE DH.EndDate IS NULL  
    AND D.DepartmentID IN (12,14)  
GROUP BY ROLLUP(D.Name, E.JobTitle);  
```  
  
### <a name="b-using-grouping_id-to-filter-a-result-set"></a>B. Usar GROUPING_ID para filtrar un conjunto de resultados  
  
#### <a name="simple-example"></a>Ejemplo sencillo  
 En el código siguiente, para devolver solo las filas que tienen un recuento de empleados por cargo, quite los caracteres de comentario de `HAVING GROUPING_ID(D.Name, E.JobTitle); = 0` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Para devolver solo las filas que tienen un recuento de empleados por departamento, quite los caracteres de comentario de `HAVING GROUPING_ID(D.Name, E.JobTitle) = 1;`.  
  
```sql  
SELECT D.Name  
    ,E.JobTitle  
    ,GROUPING_ID(D.Name, E.JobTitle) AS 'Grouping Level'  
    ,COUNT(E.BusinessEntityID) AS N'Employee Count'  
FROM HumanResources.Employee AS E  
    INNER JOIN HumanResources.EmployeeDepartmentHistory AS DH  
        ON E.BusinessEntityID = DH.BusinessEntityID  
    INNER JOIN HumanResources.Department AS D  
        ON D.DepartmentID = DH.DepartmentID       
WHERE DH.EndDate IS NULL  
    AND D.DepartmentID IN (12,14)  
GROUP BY ROLLUP(D.Name, E.JobTitle)  
--HAVING GROUPING_ID(D.Name, E.JobTitle) = 0; --All titles  
--HAVING GROUPING_ID(D.Name, E.JobTitle) = 1; --Group by Name;  
```  
  
 A continuación se muestra el conjunto de resultados sin filtrar.  
  
|Nombre|Título|Grouping Level|Employee Count|Nombre|  
|----------|-----------|--------------------|--------------------|----------|  
|Document Control|Control Specialist|0|2|Document Control|  
|Document Control|Document Control Assistant|0|2|Document Control|  
|Document Control|Document Control Manager|0|1|Document Control|  
|Document Control|NULL|1|5|Document Control|  
|Facilities and Maintenance|Facilities Administrative Assistant|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|Facilities Manager|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|Janitor|0|4|Facilities and Maintenance|  
|Facilities and Maintenance|Maintenance Supervisor|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|NULL|1|7|Facilities and Maintenance|  
|NULL|NULL|3|12|NULL|  
  
#### <a name="complex-example"></a>Ejemplo complejo  
 En el ejemplo siguiente se usa `GROUPING_ID()` para filtrar un conjunto de resultados que contiene varios niveles de agrupación por nivel de agrupación. Se puede usar un código similar para crear una vista con varios niveles de la agrupación y un procedimiento almacenado que llame a la vista con un parámetro que filtre la vista por nivel de agrupación. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DECLARE @Grouping NVARCHAR(50);  
DECLARE @GroupingLevel SMALLINT;  
SET @Grouping = N'CountryRegionCode Total';  
  
SELECT @GroupingLevel = (  
    CASE @Grouping  
        WHEN N'Grand Total'             THEN 15  
        WHEN N'SalesPerson Total'       THEN 14  
        WHEN N'Store Total'             THEN 13  
        WHEN N'Store SalesPerson Total' THEN 12  
        WHEN N'CountryRegionCode Total' THEN 11  
        WHEN N'Group Total'             THEN 7  
        ELSE N'Unknown'  
    END);  
  
SELECT   
    T.[Group]  
    ,T.CountryRegionCode  
    ,S.Name AS N'Store'  
    ,(SELECT P.FirstName + ' ' + P.LastName   
        FROM Person.Person AS P   
        WHERE P.BusinessEntityID = H.SalesPersonID)  
        AS N'Sales Person'  
    ,SUM(TotalDue)AS N'TotalSold'  
    ,CAST(GROUPING(T.[Group])AS char(1)) +   
        CAST(GROUPING(T.CountryRegionCode)AS char(1)) +   
        CAST(GROUPING(S.Name)AS char(1)) +   
        CAST(GROUPING(H.SalesPersonID)AS char(1))   
        AS N'GROUPING base-2'  
    ,GROUPING_ID((T.[Group])  
        ,(T.CountryRegionCode),(S.Name),(H.SalesPersonID)  
        ) AS N'GROUPING_ID'  
    ,CASE   
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 15 THEN N'Grand Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 14 THEN N'SalesPerson Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 13 THEN N'Store Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 12 THEN N'Store SalesPerson Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 11 THEN N'CountryRegionCode Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) =  7 THEN N'Group Total'  
        ELSE N'Error'  
        END AS N'Level'  
FROM Sales.Customer AS C  
    INNER JOIN Sales.Store AS S  
        ON C.StoreID  = S.BusinessEntityID   
    INNER JOIN Sales.SalesTerritory AS T  
        ON C.TerritoryID  = T.TerritoryID   
    INNER JOIN Sales.SalesOrderHeader AS H  
        ON C.CustomerID = H.CustomerID  
GROUP BY GROUPING SETS ((S.Name,H.SalesPersonID)  
    ,(H.SalesPersonID),(S.Name)  
    ,(T.[Group]),(T.CountryRegionCode),()  
    )  
HAVING GROUPING_ID(  
    (T.[Group]),(T.CountryRegionCode),(S.Name),(H.SalesPersonID)  
    ) = @GroupingLevel  
ORDER BY   
    GROUPING_ID(S.Name,H.SalesPersonID),GROUPING_ID((T.[Group])  
    ,(T.CountryRegionCode)  
    ,(S.Name)  
    ,(H.SalesPersonID))ASC;  
```  
  
### <a name="c-using-grouping_id--with-rollup-and-cube-to-identify-grouping-levels"></a>C. Usar GROUPING_ID () con ROLLUP y CUBE para identificar niveles de agrupación  
 El código de los ejemplos siguientes muestra el uso de `GROUPING()` para calcular la columna `Bit Vector(base-2)`. `GROUPING_ID()` se usa para calcular la columna `Integer Equivalent` correspondiente. El orden de columnas en la función `GROUPING_ID()` es el contrario del orden de las columnas que concatena la función `GROUPING()`.  
  
 En estos ejemplos, `GROUPING_ID()` se usa para crear un valor para cada fila de la columna `Grouping Level` que identifica el nivel de agrupación. Los niveles de agrupación no son siempre una lista consecutiva de enteros que comienzan por 1 (0, 1, 2,...*n*).  
  
> [!NOTE]  
>  GROUPING y GROUPING_ID se pueden usar en una cláusula HAVING para filtrar un conjunto de resultados.  
  
#### <a name="rollup-example"></a>Ejemplo de ROLLUP  
 En este ejemplo, no aparecen todos los niveles de agrupación como lo hacen en el ejemplo de CUBE siguiente. Si se cambia el orden de las columnas en la lista de `ROLLUP`, los valores de nivel de la columna `Grouping Level` también se tendrán que cambiar. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,DATEPART(mm,OrderDate) AS N'Month'  
    ,DATEPART(dd,OrderDate) AS N'Day'  
    ,SUM(TotalDue) AS N'Total Due'  
    ,CAST(GROUPING(DATEPART(dd,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(mm,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(yyyy,OrderDate)) AS CHAR(1))   
     AS N'Bit Vector(base-2)'  
    ,GROUPING_ID(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))   
        AS N'Integer Equivalent'  
    ,CASE  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 0 THEN N'Year Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 1 THEN N'Year Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 2 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 3 THEN N'Year'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 4 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 5 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 6 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 7 THEN N'Grand Total'  
    ELSE N'Error'  
    END AS N'Grouping Level'  
FROM Sales.SalesOrderHeader  
WHERE DATEPART(yyyy,OrderDate) IN(N'2007',N'2008')  
    AND DATEPART(mm,OrderDate) IN(1,2)  
    AND DATEPART(dd,OrderDate) IN(1,2)  
GROUP BY ROLLUP(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))  
ORDER BY GROUPING_ID(DATEPART(mm,OrderDate)  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(dd,OrderDate)  
    )  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate);  
```  
  
 A continuación se muestra un conjunto parcial de resultados.  
  
|Year|Month|Día|Total Due|Bit Vector (base-2)|Integer Equivalent|Grouping Level|  
|----------|-----------|---------|---------------|----------------------------|------------------------|--------------------|  
|2007|1|1|1497452,6066|000|0|Year Month Day|  
|2007|1|2|21772,3494|000|0|Year Month Day|  
|2007|2|1|2705653,5913|000|0|Year Month Day|  
|2007|2|2|21684,4068|000|0|Year Month Day|  
|2008|1|1|1908122,0967|000|0|Year Month Day|  
|2008|1|2|46458,0691|000|0|Year Month Day|  
|2008|2|1|3108771,9729|000|0|Year Month Day|  
|2008|2|2|54598,5488|000|0|Year Month Day|  
|2007|1|NULL|1519224,956|100|1|Year Month|  
|2007|2|NULL|2727337,9981|100|1|Year Month|  
|2008|1|NULL|1954580,1658|100|1|Year Month|  
|2008|2|NULL|3163370,5217|100|1|Year Month|  
|2007|NULL|NULL|4246562,9541|110|3|Year|  
|2008|NULL|NULL|5117950,6875|110|3|Year|  
|NULL|NULL|NULL|9364513,6416|111|7|Total general|  
  
#### <a name="cube-example"></a>Ejemplo de CUBE  
 En este ejemplo, la función `GROUPING_ID()` se usa para crear un valor para cada fila de la columna `Grouping Level` que identifica el nivel de agrupación.  
  
 A diferencia de `ROLLUP` en el ejemplo anterior, `CUBE` genera todos los niveles de agrupación. Si se cambia el orden de las columnas en la lista de `CUBE`, los valores de nivel de la columna `Grouping Level` también se tendrán que cambiar. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,DATEPART(mm,OrderDate) AS N'Month'  
    ,DATEPART(dd,OrderDate) AS N'Day'  
    ,SUM(TotalDue) AS N'Total Due'  
    ,CAST(GROUPING(DATEPART(dd,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(mm,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(yyyy,OrderDate)) AS CHAR(1))   
        AS N'Bit Vector(base-2)'  
    ,GROUPING_ID(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))   
        AS N'Integer Equivalent'  
    ,CASE  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 0 THEN N'Year Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 1 THEN N'Year Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 2 THEN N'Year Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 3 THEN N'Year'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 4 THEN N'Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 5 THEN N'Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 6 THEN N'Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 7 THEN N'Grand Total'  
    ELSE N'Error'  
    END AS N'Grouping Level'  
FROM Sales.SalesOrderHeader  
WHERE DATEPART(yyyy,OrderDate) IN(N'2007',N'2008')  
    AND DATEPART(mm,OrderDate) IN(1,2)  
    AND DATEPART(dd,OrderDate) IN(1,2)  
GROUP BY CUBE(DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate))  
ORDER BY GROUPING_ID(DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate)  
    )  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate);  
```  
  
 A continuación se muestra un conjunto parcial de resultados.  
  
|Year|Month|Día|Total Due|Bit Vector (base-2)|Integer Equivalent|Grouping Level|  
|----------|-----------|---------|---------------|----------------------------|------------------------|--------------------|  
|2007|1|1|1497452,6066|000|0|Year Month Day|  
|2007|1|2|21772,3494|000|0|Year Month Day|  
|2007|2|1|2705653,5913|000|0|Year Month Day|  
|2007|2|2|21684,4068|000|0|Year Month Day|  
|2008|1|1|1908122,0967|000|0|Year Month Day|  
|2008|1|2|46458,0691|000|0|Year Month Day|  
|2008|2|1|3108771,9729|000|0|Year Month Day|  
|2008|2|2|54598,5488|000|0|Year Month Day|  
|2007|1|NULL|1519224,956|100|1|Year Month|  
|2007|2|NULL|2727337,9981|100|1|Year Month|  
|2008|1|NULL|1954580,1658|100|1|Year Month|  
|2008|2|NULL|3163370,5217|100|1|Year Month|  
|2007|NULL|1|4203106,1979|010|2|Year Day|  
|2007|NULL|2|43456.7562|010|2|Year Day|  
|2008|NULL|1|5016894.0696|010|2|Year Day|  
|2008|NULL|2|101056.6179|010|2|Year Day|  
|2007|NULL|NULL|4246562,9541|110|3|Year|  
|2008|NULL|NULL|5117950,6875|110|3|Year|  
|NULL|1|1|3405574.7033|001|4|Month Day|  
|NULL|1|2|68230.4185|001|4|Month Day|  
|NULL|2|1|5814425.5642|001|4|Month Day|  
|NULL|2|2|76282.9556|001|4|Month Day|  
|NULL|1|NULL|3473805.1218|101|5|Month|  
|NULL|2|NULL|5890708.5198|101|5|Month|  
|NULL|NULL|1|9220000.2675|011|6|Día|  
|NULL|NULL|2|144513.3741|011|6|Día|  
|NULL|NULL|NULL|9364513,6416|111|7|Total general|  
  
## <a name="see-also"></a>Consulte también  
 [GROUPING &#40;Transact-SQL&#41;](../../t-sql/functions/grouping-transact-sql.md)   
 [GROUP BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-group-by-transact-sql.md)  
  
  
