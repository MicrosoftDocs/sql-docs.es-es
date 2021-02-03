---
description: COUNT (Transact-SQL)
title: COUNT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COUNT_TSQL
- COUNT
dev_langs:
- TSQL
helpviewer_keywords:
- totals [SQL Server], COUNT function
- totals [SQL Server]
- counting items in group
- groups [SQL Server], number of items in
- number of group items
- COUNT function [Transact-SQL]
ms.assetid: 28d39da6-bc2e-46c7-858c-b1721c938830
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ad8aa2f0f24dda5f2c67c813df08291fe6bca432
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184172"
---
# <a name="count-transact-sql"></a>COUNT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve el número de elementos encontrados en un grupo. `COUNT` funciona como la función [COUNT_BIG](../../t-sql/functions/count-big-transact-sql.md). Estas funciones difieren solo en los tipos de datos de sus valores devueltos. `COUNT` siempre devuelve un valor de tipo de datos **int**. `COUNT_BIG` siempre devuelve un valor de tipo de datos **bigint**.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql

-- Aggregation Function Syntax  
COUNT ( { [ [ ALL | DISTINCT ] expression ] | * } )  

-- Analytic Function Syntax  
COUNT ( [ ALL ]  { expression | * } ) OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
**ALL**  
Aplica la función de agregado a todos los valores. ALL sirve como valor predeterminado.
  
DISTINCT  
Especifica que `COUNT` devuelva el número de valores únicos no NULL.
  
*expression*  
Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de cualquier tipo excepto **image**, **ntext** o **text**. Tenga en cuenta que `COUNT` no admite funciones de agregado ni subconsultas en una expresión.
  
\*  
Especifica que `COUNT` debe contar todas las filas para determinar el total de filas de la tabla a devolver. `COUNT(*)` no toma ningún parámetro y no admite el uso de DISTINCT. `COUNT(*)` no requiere el parámetro *expression* porque, por definición, no usa información sobre ninguna columna concreta. `COUNT(*)` devuelve el número de filas de una tabla especificada y conserva las filas duplicadas. Cuenta cada fila por separado. Esto incluye las filas que contienen valores NULL.
  
OVER **(** [ *partition_by_clause* ] [ *order_by_clause* ] [ *ROW_or_RANGE_clause* ] **)**  
*partition_by_clause* divide el conjunto de resultados generado por la cláusula `FROM` en particiones a las que se aplica la función `COUNT`. Si no se especifica, la función trata todas las filas del conjunto de resultados de la consulta como un único grupo. *order_by_clause* determina el orden lógico de la operación. Para más información, consulte [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md). 

## <a name="return-types"></a>Tipos de valores devueltos
 **int**  
  
## <a name="remarks"></a>Observaciones  
COUNT(\*) devuelve el número de elementos de un grupo. Esto incluye los valores NULL y los duplicados
  
COUNT(ALL *expresión*) evalúa *expresión* en todas las filas del grupo y devuelve el número de valores no NULL.
  
COUNT(DISTINCT *expresión*) evalúa *expresión* en todas las filas del grupo y devuelve el número de valores únicos no NULL.
  
Para valores devueltos superiores a 2^31-1, `COUNT` devuelve un error. En estos casos, utilice `COUNT_BIG` en su lugar.
  
`COUNT` es una función determinista cuando se utiliza ***sin** _ las cláusulas OVER y ORDER BY. Es no determinista si se especifica _*_con_*_ las cláusulas OVER y ORDER BY. Para más información, consulte [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-count-and-distinct"></a>A. Usar COUNT y DISTINCT  
Este ejemplo se muestra el número de diferentes cargos que puede tener un empleado de [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].
  
```sql
SELECT COUNT(DISTINCT Title)  
FROM HumanResources.Employee;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
67  
  
(1 row(s) affected)
```
  
### <a name="b-using-count_"></a>B. Usar COUNT(\_)  
Este ejemplo devuelve el número total de empleados de [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].
  
```sql
SELECT COUNT(*)  
FROM HumanResources.Employee;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
290  
  
(1 row(s) affected)
```
  
### <a name="c-using-count-with-other-aggregates"></a>C. Usar COUNT(\*) con otros agregados  
En este ejemplo se muestra que `COUNT(*)` funciona con otras funciones agregadas en la lista `SELECT`. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
SELECT COUNT(*), AVG(Bonus)  
FROM Sales.SalesPerson  
WHERE SalesQuota > 25000;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------- ---------------------
14            3472.1428
  
(1 row(s) affected)
```
  
### <a name="d-using-the-over-clause"></a>D. Usar la cláusula OVER  
En este ejemplo se utilizan las funciones `MIN`, `MAX`, `AVG` y `COUNT` con la cláusula `OVER` para devolver valores agregados para cada departamento en la tabla [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] de la base de datos `HumanResources.Department`.
  
```sql
SELECT DISTINCT Name  
       , MIN(Rate) OVER (PARTITION BY edh.DepartmentID) AS MinSalary  
       , MAX(Rate) OVER (PARTITION BY edh.DepartmentID) AS MaxSalary  
       , AVG(Rate) OVER (PARTITION BY edh.DepartmentID) AS AvgSalary  
       ,COUNT(edh.BusinessEntityID) OVER (PARTITION BY edh.DepartmentID) AS EmployeesPerDept  
FROM HumanResources.EmployeePayHistory AS eph  
JOIN HumanResources.EmployeeDepartmentHistory AS edh  
     ON eph.BusinessEntityID = edh.BusinessEntityID  
JOIN HumanResources.Department AS d  
ON d.DepartmentID = edh.DepartmentID
WHERE edh.EndDate IS NULL  
ORDER BY Name;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Name                          MinSalary             MaxSalary             AvgSalary             EmployeesPerDept  
----------------------------- --------------------- --------------------- --------------------- ----------------  
Document Control              10.25                 17.7885               14.3884               5  
Engineering                   32.6923               63.4615               40.1442               6  
Executive                     39.06                 125.50                68.3034               4  
Facilities and Maintenance    9.25                  24.0385               13.0316               7  
Finance                       13.4615               43.2692               23.935                10  
Human Resources               13.9423               27.1394               18.0248               6  
Information Services          27.4038               50.4808               34.1586               10  
Marketing                     13.4615               37.50                 18.4318               11  
Production                    6.50                  84.1346               13.5537               195  
Production Control            8.62                  24.5192               16.7746               8  
Purchasing                    9.86                  30.00                 18.0202               14  
Quality Assurance             10.5769               28.8462               15.4647               6  
Research and Development      40.8654               50.4808               43.6731               4  
Sales                         23.0769               72.1154               29.9719               18  
Shipping and Receiving        9.00                  19.2308               10.8718               6  
Tool Design                   8.62                  29.8462               23.5054               6  
  
(16 row(s) affected)
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-count-and-distinct"></a>E. Usar COUNT y DISTINCT  
Este ejemplo se muestra el número de diferentes cargos que puede tener un empleado de una empresa concreta.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(DISTINCT Title)  
FROM dbo.DimEmployee;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
67
```  
  
### <a name="f-using-count"></a>F. Usar COUNT(\*)  
Este ejemplo devuelve el número total de filas de la tabla `dbo.DimEmployee`.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(*)  
FROM dbo.DimEmployee;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-------------
296
```  
  
### <a name="g-using-count-with-other-aggregates"></a>G. Usar COUNT(\*) con otros agregados  
Este ejemplo combina `COUNT(*)` con otras funciones de agregado en la lista `SELECT`. Devuelve el número de los representantes de ventas con una cuota de ventas anual superior a 500 000 dólares y el promedio de cuota de ventas de dichos representantes.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(EmployeeKey) AS TotalCount, AVG(SalesAmountQuota) AS [Average Sales Quota]  
FROM dbo.FactSalesQuota  
WHERE SalesAmountQuota > 500000 AND CalendarYear = 2001;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
TotalCount  Average Sales Quota
----------  -------------------
10          683800.0000
```
  
### <a name="h-using-count-with-having"></a>H. Usar COUNT con HAVING  
En este ejemplo se utiliza `COUNT` con la cláusula `HAVING` para devolver los departamentos de una compañía, cada uno de los cuales tiene más de 15 empleados.
  
```sql
USE ssawPDW;  
  
SELECT DepartmentName,   
       COUNT(EmployeeKey)AS EmployeesInDept  
FROM dbo.DimEmployee  
GROUP BY DepartmentName  
HAVING COUNT(EmployeeKey) > 15;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
DepartmentName  EmployeesInDept
--------------  ---------------
Sales           18
Production      179
```
  
### <a name="i-using-count-with-over"></a>I. Usar COUNT con OVER  
Este ejemplo usa `COUNT` con la cláusula `OVER` para devolver el número de productos que figura en cada uno de los pedidos de ventas especificados.
  
```sql
USE ssawPDW;  
  
SELECT DISTINCT COUNT(ProductKey) OVER(PARTITION BY SalesOrderNumber) AS ProductCount  
    ,SalesOrderNumber  
FROM dbo.FactInternetSales  
WHERE SalesOrderNumber IN (N'SO53115',N'SO55981');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
ProductCount   SalesOrderID`
------------   -----------------
3              SO53115
1              SO55981
```
  
## <a name="see-also"></a>Vea también
[Funciones de agregado &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/count-big-transact-sql.md)  
[OVER &#40;cláusula de Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  


