---
description: LAG (Transact-SQL)
title: LAG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LAG_TSQL
- LAG
dev_langs:
- TSQL
helpviewer_keywords:
- LAG function
- analytic functions, LAG
ms.assetid: a9a90bdb-3f80-4c97-baca-b7407bcdc7f0
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: adc27879b571a32c0e2f7f0536f1314180ad7f10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202186"
---
# <a name="lag-transact-sql"></a>LAG (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Tiene acceso a datos de una fila anterior en el mismo conjunto de resultados sin usar una autocombinación que empiece por [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. LAG proporciona acceso a una fila en un desplazamiento físico especificado que hay antes de la fila actual. Use esta función analítica en una instrucción SELECT para comparar valores de la fila actual con valores de una fila anterior.  
  
 ![Icono de vínculo a temas](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
LAG (scalar_expression [,offset] [,default])  
    OVER ( [ partition_by_clause ] order_by_clause )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *scalar_expression*  
 El valor que se va a devolver en función del desplazamiento especificado. Es una expresión de cualquier tipo que devuelve un único valor (escalar). *scalar_expression* no puede ser una función analítica.  
  
 *offset*  
 El número de filas hacia atrás de la fila actual de la que se va a obtener un valor. Si no se especifica, el valor predeterminado es 1. *offset* puede ser una columna, una subconsulta u otra expresión que se evalúa como un entero positivo o que se puede convertir implícitamente en **bigint**. *offset* no puede ser un valor negativo o una función analítica.  
  
 *default*  
 Valor que se devuelve cuando *offset* está fuera del ámbito de la partición. Si no se especifica ningún valor predeterminado, se devuelve NULL. *default* puede ser una columna, una subconsulta u otra expresión, pero no puede ser una función analítica. *default* debe tener un tipo compatible con *scalar_expression*.  
  
 OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_ **)**  
 *partition_by_clause* divide el conjunto de resultados generado por la cláusula FROM en particiones a las que se aplica la función. Si no se especifica, la función trata todas las filas del conjunto de resultados de la consulta como un único grupo. *order_by_clause* determina el orden de los datos antes de que se aplique la función. Si se especifica *partition_by_clause*, determina el orden de los datos en la partición. *order_by_clause* es obligatorio. Para más información, vea [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 El tipo de datos de la *scalar_expression* especificada. Se devuelve NULL si *scalar_expression* acepta valores NULL o si *default* se establece en NULL.  
  
## <a name="general-remarks"></a>Notas generales  
 LAG es no determinista. Para obtener más información, consulte [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-compare-values-between-years"></a>A. Comparar valores entre años  
 En el ejemplo siguiente se usa la función LAG para devolver la diferencia en cuotas de venta para un empleado concreto en años anteriores. Observe que como no hay ningún valor de intervalo disponible para la primera fila, se devuelve el valor predeterminado de cero (0).  
  
```sql   
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, YEAR(QuotaDate) AS SalesYear, SalesQuota AS CurrentQuota,   
       LAG(SalesQuota, 1,0) OVER (ORDER BY YEAR(QuotaDate)) AS PreviousQuota  
FROM Sales.SalesPersonQuotaHistory  
WHERE BusinessEntityID = 275 AND YEAR(QuotaDate) IN ('2005','2006');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```    
BusinessEntityID SalesYear   CurrentQuota          PreviousQuota  
---------------- ----------- --------------------- ---------------------  
275              2005        367000.00             0.00  
275              2005        556000.00             367000.00  
275              2006        502000.00             556000.00  
275              2006        550000.00             502000.00  
275              2006        1429000.00            550000.00  
275              2006        1324000.00            1429000.00  
  
```  
  
### <a name="b-compare-values-within-partitions"></a>B. Comparar valores dentro de particiones  
 En el ejemplo siguiente se usa la función LAG para comparar las ventas anuales hasta la fecha entre los empleados. La cláusula PARTITION BY se especifica para dividir las filas del conjunto de resultados por territorio de ventas. La función LAG se aplica a cada partición por separado y el cálculo se reinicia para cada partición. La cláusula ORDER BY de la cláusula OVER ordena las filas de cada partición. La cláusula ORDER BY de la instrucción SELECT ordena las filas del conjunto de resultados completo. Observe que como no hay ningún valor de intervalo disponible para la primera fila de cada partición, se devuelve el valor predeterminado de cero (0).  
  
```sql   
USE AdventureWorks2012;  
GO  
SELECT TerritoryName, BusinessEntityID, SalesYTD,   
       LAG (SalesYTD, 1, 0) OVER (PARTITION BY TerritoryName ORDER BY SalesYTD DESC) AS PrevRepSales  
FROM Sales.vSalesPerson  
WHERE TerritoryName IN (N'Northwest', N'Canada')   
ORDER BY TerritoryName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```    
TerritoryName            BusinessEntityID SalesYTD              PrevRepSales  
-----------------------  ---------------- --------------------- ---------------------  
Canada                   282              2604540.7172          0.00  
Canada                   278              1453719.4653          2604540.7172  
Northwest                284              1576562.1966          0.00  
Northwest                283              1573012.9383          1576562.1966  
Northwest                280              1352577.1325          1573012.9383  
  
```  
  
### <a name="c-specifying-arbitrary-expressions"></a>C. Especificar expresiones arbitrarias  
 En el ejemplo siguiente se muestra cómo especificar una serie de expresiones arbitrarias en la sintaxis de la función LAG.  
  
```sql   
CREATE TABLE T (a INT, b INT, c INT);   
GO  
INSERT INTO T VALUES (1, 1, -3), (2, 2, 4), (3, 1, NULL), (4, 3, 1), (5, 2, NULL), (6, 1, 5);   
  
SELECT b, c,   
    LAG(2*c, b*(SELECT MIN(b) FROM T), -c/2.0) OVER (ORDER BY a) AS i  
FROM T;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
b           c           i  
----------- ----------- -----------  
1           -3          1  
2           4           -2  
1           NULL        8  
3           1           -6  
2           NULL        NULL  
1           5           NULL  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-compare-values-between-quarters"></a>D. Comparar valores entre trimestres  
 En este ejemplo se muestra el uso de la función LAG. La consulta usa la función LAG para devolver la diferencia en cuotas de ventas para un empleado concreto en trimestres anteriores. Observe que como no hay ningún valor de intervalo disponible para la primera fila, se devuelve el valor predeterminado de cero (0).  
  
```sql   
-- Uses AdventureWorks  
  
SELECT CalendarYear, CalendarQuarter, SalesAmountQuota AS SalesQuota,  
       LAG(SalesAmountQuota,1,0) OVER (ORDER BY CalendarYear, CalendarQuarter) AS PrevQuota,  
       SalesAmountQuota - LAG(SalesAmountQuota,1,0) OVER (ORDER BY CalendarYear, CalendarQuarter) AS Diff  
FROM dbo.FactSalesQuota  
WHERE EmployeeKey = 272 AND CalendarYear IN (2001, 2002)  
ORDER BY CalendarYear, CalendarQuarter;   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Year Quarter  SalesQuota  PrevQuota  Diff  
---- -------  ----------  ---------  -------------  
2001 3        28000.0000      0.0000   28000.0000  
2001 4         7000.0000  28000.0000  -21000.0000  
2001 1        91000.0000   7000.0000   84000.0000  
2002 2       140000.0000  91000.0000   49000.0000  
2002 3         7000.0000 140000.0000  -70000.0000  
2002 4       154000.0000   7000.0000   84000.0000
```  
  
## <a name="see-also"></a>Consulte también  
 [LEAD &#40;Transact-SQL&#41;](../../t-sql/functions/lead-transact-sql.md)  
  
  


