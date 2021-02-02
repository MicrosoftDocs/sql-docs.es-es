---
description: DENSE_RANK (Transact-SQL)
title: DENSE_RANK (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DENSE_RANK_TSQL
- DENSE_RANK
dev_langs:
- TSQL
helpviewer_keywords:
- row ranking [SQL Server]
- DENSE_RANK function
- tied rows [SQL Server]
- ranking rows
ms.assetid: 03871fc6-9592-4016-b0b2-ff543f132b20
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1f86e3c466357474727c02e41df074de395281c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158395"
---
# <a name="dense_rank-transact-sql"></a>DENSE_RANK (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve el rango de cada fila dentro de una partición del conjunto de resultados, sin espacios en los valores de clasificación. El rango de una fila específica es uno más el número de valores de rango distintos anteriores a esa fila específica.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
DENSE_RANK ( ) OVER ( [ <partition_by_clause> ] < order_by_clause > )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 \<partition_by_clause>  
Primero divide el conjunto de resultados generado por la cláusula [FROM](../../t-sql/queries/from-transact-sql.md) en particiones y después se aplica la función `DENSE_RANK` a cada partición. Vea [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md) para obtener la sintaxis de `PARTITION BY`.  
  
 \<order_by_clause>  
Determina el orden en el que se aplica la función `DENSE_RANK` a las filas de una partición.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **bigint**  
  
## <a name="remarks"></a>Observaciones  
Si dos o más filas tienen el mismo valor de rango en la misma partición, cada una de esas filas recibirá el mismo rango. Por ejemplo, si los dos mejores vendedores tienen el mismo valor de SalesYTD, los dos tendrán un valor de rango de uno. El siguiente vendedor con mayor valor en SalesYTD tendrá un valor de rango de dos. Esto supera en uno el número de rangos distintos anteriores a la fila en cuestión. Por tanto, los números devueltos por la función `DENSE_RANK` no tienen espacios y siempre tienen valores de rango consecutivos.  
  
El criterio de ordenación que usa la consulta global determina el orden de las filas en el conjunto de resultados. Esto implica que una fila que tiene el rango uno no tiene que ser la primera fila de la partición.  
  
`DENSE_RANK` sea no determinista. Para más información, consulte [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-ranking-rows-within-a-partition"></a>A. Clasificar filas dentro de una partición  
En este ejemplo se clasifican los productos de inventario, por las ubicaciones de inventario especificadas, según sus cantidades. `DENSE_RANK` divide el conjunto de resultados por `LocationID` y ordena lógicamente el conjunto de resultados por `Quantity`. Observe que los productos 494 y 495 tienen la misma cantidad. Como los dos tienen el mismo valor de cantidad, los dos tienen un valor de rango de uno.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT i.ProductID, p.Name, i.LocationID, i.Quantity  
    ,DENSE_RANK() OVER   
    (PARTITION BY i.LocationID ORDER BY i.Quantity DESC) AS Rank  
FROM Production.ProductInventory AS i   
INNER JOIN Production.Product AS p   
    ON i.ProductID = p.ProductID  
WHERE i.LocationID BETWEEN 3 AND 4  
ORDER BY i.LocationID;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ProductID   Name                               LocationID Quantity Rank  
----------- ---------------------------------- ---------- -------- -----  
494         Paint - Silver                     3          49       1  
495         Paint - Blue                       3          49       1  
493         Paint - Red                        3          41       2  
496         Paint - Yellow                     3          30       3  
492         Paint - Black                      3          17       4  
495         Paint - Blue                       4          35       1  
496         Paint - Yellow                     4          25       2  
493         Paint - Red                        4          24       3  
492         Paint - Black                      4          14       4  
494         Paint - Silver                     4          12       5  
  
(10 row(s) affected)  
  
```  
  
### <a name="b-ranking-all-rows-in-a-result-set"></a>B. Clasificar todas las filas de un conjunto de resultados  
En este ejemplo se devuelven los diez primeros empleados clasificados por su salario. Como en la instrucción `SELECT` no se ha especificado una cláusula `PARTITION BY`, la función `DENSE_RANK` se ha aplicado a todas las filas del conjunto de resultados.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(10) BusinessEntityID, Rate,   
       DENSE_RANK() OVER (ORDER BY Rate DESC) AS RankBySalary  
FROM HumanResources.EmployeePayHistory;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID Rate                  RankBySalary  
---------------- --------------------- --------------------  
1                125.50                1  
25               84.1346               2  
273              72.1154               3  
2                63.4615               4  
234              60.0962               5  
263              50.4808               6  
7                50.4808               6  
234              48.5577               7  
285              48.101                8  
274              48.101                8  
```  
  
## <a name="c-four-ranking-functions-used-in-the-same-query"></a>C. Cuatro funciones de categoría usadas en la misma consulta  
En este ejemplo se muestran las cuatro funciones de categoría

+ [DENSE_RANK()](./dense-rank-transact-sql.md)
+ [NTILE()](./ntile-transact-sql.md)
+ [RANK()](./rank-transact-sql.md)
+ [ROW_NUMBER()](./row-number-transact-sql.md)

que se usaron en la misma consulta. Vea cada función de categoría para obtener ejemplos específicos de la función.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS Rank  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS Quartile  
    ,s.SalesYTD  
    ,a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|Nombre|Apellidos|Row Number|Rank|Dense Rank|Quartile|SalesYTD|PostalCode|  
|---------------|--------------|----------------|----------|----------------|--------------|--------------|----------------|  
|Michael|Blythe|1|1|1|1|4 557 045,0459|98027|  
|Linda|Mitchell|2|1|1|1|5 200 475,2313|98027|  
|Jillian|Carson|3|1|1|1|3 857 163,6332|98027|  
|Garrett|Vargas|4|1|1|1|1 764 938,9859|98027|  
|Tsvi|Reiter|5|1|1|2|2 811 012,7151|98027|  
|Shu|Ito|6|6|2|2|3 018 725,4858|98055|  
|José|Saraiva|7|6|2|2|3 189 356,2465|98055|  
|David|Campbell|8|6|2|3|3 587 378,4257|98055|  
|Tete|Mensa Annan|9|6|2|3|1 931 620,1835|98055|  
|Lynn|Tsoflias|10|6|2|3|1 758 385,926|98055|  
|Rachel|Valdez|11|6|2|4|2 241 204,0424|98055|  
|Jae|Pak|12|6|2|4|5 015 682,3752|98055|  
|Ranjit|Varkey Chudukatil|13|6|2|4|3 827 950,238|98055| 


## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-ranking-rows-within-a-partition"></a>D. Clasificar filas dentro de una partición  
En este ejemplo se clasifican los representantes de ventas de cada territorio de ventas en función de sus ventas totales. `DENSE_RANK` divide el conjunto de filas por `SalesTerritoryGroup` y ordena el conjunto de resultados por `SalesAmountQuota`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, SUM(SalesAmountQuota) AS TotalSales, SalesTerritoryGroup,  
    DENSE_RANK() OVER (PARTITION BY SalesTerritoryGroup ORDER BY SUM(SalesAmountQuota) DESC ) AS RankResult  
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryGroup != N'NA'  
GROUP BY LastName, SalesTerritoryGroup;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
 LastName          TotalSales     SalesTerritoryGroup  RankResult  
----------------  -------------  -------------------  --------  
Pak               10514000.0000  Europe               1  
Varkey Chudukatil  5557000.0000  Europe               2  
Valdez             2287000.0000  Europe               3  
Carson            12198000.0000  North America        1  
Mitchell          11786000.0000  North America        2  
Blythe            11162000.0000  North America        3  
Reiter             8541000.0000  North America        4  
Ito                7804000.0000  North America        5  
Saraiva            7098000.0000  North America        6  
Vargas             4365000.0000  North America        7  
Campbell           4025000.0000  North America        8  
Ansman-Wolfe       3551000.0000  North America        9  
Mensa-Annan        2753000.0000  North America        10  
Tsoflias           1687000.0000  Pacific              1 
```  

## <a name="see-also"></a>Consulte también  
 [RANK &#40;Transact-SQL&#41;](../../t-sql/functions/rank-transact-sql.md)   
 [ROW_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/row-number-transact-sql.md)   
 [NTILE &#40;Transact-SQL&#41;](../../t-sql/functions/ntile-transact-sql.md)   
 [Funciones de categoría &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [Funciones](../../t-sql/functions/functions.md)  
  
  

