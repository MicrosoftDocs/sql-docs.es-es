---
title: ROW_NUMBER (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para la función ROW_NUMBER. Esta función enumera los resultados de un conjunto de resultados.
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROW_NUMBER
- ROW_NUMBER_TSQL
- ROW_NUMBER()_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ROW_NUMBER function
- row numbers [SQL Server]
- sequential row numbers [SQL Server]
ms.assetid: 82fa9016-77db-4b42-b4c8-df6095b81906
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 716ea6b395569ea129f1b10e21f4b880a3fb761d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159728"
---
# <a name="row_number-transact-sql"></a>ROW_NUMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Enumera los resultados de un conjunto de resultados. Concretamente, devuelve el número secuencial de una fila dentro de una partición de un conjunto de resultados, empezando por 1 para la primera fila de cada partición. 
  
`ROW_NUMBER` y `RANK` son similares. `ROW_NUMBER` enumera todas las filas secuencialmente (por ejemplo 1, 2, 3, 4, 5). `RANK` proporciona el mismo valor numérico para valores equivalentes (por ejemplo 1, 2, 2, 4, 5).   
  
> [!NOTE]
> `ROW_NUMBER` es un valor temporal que se calcula cuando se ejecuta la consulta. Para conservar los números de una tabla, vea [Propiedad IDENTITY](../../t-sql/statements/create-table-transact-sql-identity-property.md) y [SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md). 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ROW_NUMBER ( )   
    OVER ( [ PARTITION BY value_expression , ... [ n ] ] order_by_clause )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 PARTITION BY *value_expression*  
 Divide el conjunto de resultados generado por la cláusula [FROM](../../t-sql/queries/from-transact-sql.md) en particiones a las que se aplica la función ROW_NUMBER. *value_expression* especifica la columna a partir de la cual se particiona el conjunto de resultados. Si no se especifica `PARTITION BY`, la función trata todas las filas del conjunto de resultados de la consulta como un único grupo. Para más información, vea [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 *order_by_clause*  
 La cláusula `ORDER BY` determina la secuencia en la que se asigna a las filas el `ROW_NUMBER` único correspondiente en una partición especificada. Es obligatorio. Para más información, vea [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **bigint**  
  
## <a name="general-remarks"></a>Notas generales  
 No hay ninguna garantía de que las filas devueltas por una consulta con al usar `ROW_NUMBER()` se ordenen exactamente igual con cada ejecución a menos que se cumplan estas condiciones:  
  
1.  Los valores de la columna de la partición sean únicos.  
  
2.  Los valores de las columnas `ORDER BY` sean únicos.  
  
3.  Las combinaciones de los valores de la columna de la partición y las columnas `ORDER BY` sean únicas.  
  
 `ROW_NUMBER()` sea no determinista. Para obtener más información, consulte [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-simple-examples"></a>A. Ejemplos sencillos 

La siguiente consulta devuelve las cuatro tablas del sistema en orden alfabético.

```sql
SELECT 
  name, recovery_model_desc
FROM sys.databases 
WHERE database_id < 5
ORDER BY name ASC;
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|name    |recovery_model_desc |  
|-----------  |------------ |  
|maestro |SIMPLE |
|model |FULL |
|msdb |SIMPLE |
|tempdb |SIMPLE |

Para agregar una columna de número de fila delante de cada fila, agregue una columna con la función `ROW_NUMBER`, en este caso denominada `Row#`. Debe mover la cláusula `ORDER BY` hasta la cláusula `OVER`.

```sql
SELECT 
  ROW_NUMBER() OVER(ORDER BY name ASC) AS Row#,
  name, recovery_model_desc
FROM sys.databases 
WHERE database_id < 5;
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|Row# |name    |recovery_model_desc |  
|------- |-----------  |------------ |  
|1 |maestro |SIMPLE |
|2 |model |FULL |
|3 |msdb |SIMPLE |
|4 |tempdb |SIMPLE |

Al agregar una cláusula `PARTITION BY` en la columna `recovery_model_desc`, se reiniciará la numeración cuando cambie el valor `recovery_model_desc`. 
 
```sql
SELECT 
  ROW_NUMBER() OVER(PARTITION BY recovery_model_desc ORDER BY name ASC) 
    AS Row#,
  name, recovery_model_desc
FROM sys.databases WHERE database_id < 5;
```  

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|Row# |name    |recovery_model_desc |  
|------- |-----------  |------------ |  
|1 |model |FULL |
|1 |maestro |SIMPLE |
|2 |msdb |SIMPLE |
|3 |tempdb |SIMPLE |


### <a name="b-returning-the-row-number-for-salespeople"></a>B. Devolver el número de fila de vendedor  
 En el ejemplo siguiente se calcula un número de fila para los vendedores de [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] según la categoría de ventas anuales hasta la fecha.  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT ROW_NUMBER() OVER(ORDER BY SalesYTD DESC) AS Row,   
    FirstName, LastName, ROUND(SalesYTD,2,1) AS "Sales YTD"   
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL AND SalesYTD <> 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Row FirstName    LastName               SalesYTD  
--- -----------  ---------------------- -----------------  
1   Linda        Mitchell               4251368.54  
2   Jae          Pak                    4116871.22  
3   Michael      Blythe                 3763178.17  
4   Jillian      Carson                 3189418.36  
5   Ranjit       Varkey Chudukatil      3121616.32  
6   José         Saraiva                2604540.71  
7   Shu          Ito                    2458535.61  
8   Tsvi         Reiter                 2315185.61  
9   Rachel       Valdez                 1827066.71  
10  Tete         Mensa-Annan            1576562.19  
11  David        Campbell               1573012.93  
12  Garrett      Vargas                 1453719.46  
13  Lynn         Tsoflias               1421810.92  
14  Pamela       Ansman-Wolfe           1352577.13  
```  
  
### <a name="c-returning-a-subset-of-rows"></a>C. Devolver un subconjunto de filas  
 En el ejemplo siguiente se calculan los números de fila para todas las filas de la tabla `SalesOrderHeader` en el orden de `OrderDate` y solo se devuelven las filas `50` a `60` inclusive.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH OrderedOrders AS  
(  
    SELECT SalesOrderID, OrderDate,  
    ROW_NUMBER() OVER (ORDER BY OrderDate) AS RowNumber  
    FROM Sales.SalesOrderHeader   
)   
SELECT SalesOrderID, OrderDate, RowNumber    
FROM OrderedOrders   
WHERE RowNumber BETWEEN 50 AND 60;  
```  
  
### <a name="d-using-row_number-with-partition"></a>D. Usar ROW_NUMBER() con PARTITION  
 En el ejemplo siguiente se usa el argumento `PARTITION BY` para crear particiones del conjunto de resultados de la consulta por la columna `TerritoryName`. La cláusula `ORDER BY` especificada en la cláusula `OVER` ordena las filas de cada partición por la columna `SalesYTD`. La cláusula `ORDER BY` de la instrucción `SELECT` ordena todo el conjunto de resultados de la consulta por `TerritoryName`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT FirstName, LastName, TerritoryName, ROUND(SalesYTD,2,1) AS SalesYTD,  
ROW_NUMBER() OVER(PARTITION BY TerritoryName ORDER BY SalesYTD DESC) 
  AS Row  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL AND SalesYTD <> 0  
ORDER BY TerritoryName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
FirstName  LastName             TerritoryName        SalesYTD      Row  
---------  -------------------- ------------------   ------------  ---  
Lynn       Tsoflias             Australia            1421810.92    1  
José       Saraiva              Canada               2604540.71    1  
Garrett    Vargas               Canada               1453719.46    2  
Jillian    Carson               Central              3189418.36    1  
Ranjit     Varkey Chudukatil    France               3121616.32    1  
Rachel     Valdez               Germany              1827066.71    1  
Michael    Blythe               Northeast            3763178.17    1  
Tete       Mensa-Annan          Northwest            1576562.19    1  
David      Campbell             Northwest            1573012.93    2  
Pamela     Ansman-Wolfe         Northwest            1352577.13    3  
Tsvi       Reiter               Southeast            2315185.61    1  
Linda      Mitchell             Southwest            4251368.54    1  
Shu        Ito                  Southwest            2458535.61    2  
Jae        Pak                  United Kingdom       4116871.22    1  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-returning-the-row-number-for-salespeople"></a>E. Devolver el número de fila de vendedor  
 En este ejemplo se devuelve `ROW_NUMBER` para los representantes de ventas en función de su cuota de ventas asignada.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(ORDER BY SUM(SalesAmountQuota) DESC) 
    AS RowNumber,  
    FirstName, LastName,   
    CONVERT(varchar(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName;  
```  
  
 A continuación se muestra un conjunto parcial de resultados.  

```  

RowNumber  FirstName  LastName            SalesQuota  
---------  ---------  ------------------  -------------  
1          Jillian    Carson              12,198,000.00  
2          Linda      Mitchell            11,786,000.00  
3          Michael    Blythe              11,162,000.00  
4          Jae        Pak                 10,514,000.00  
```

### <a name="f-using-row_number-with-partition"></a>F. Usar ROW_NUMBER() con PARTITION  
 El ejemplo siguiente muestra cómo utilizar la función `ROW_NUMBER` con el argumento `PARTITION BY`. Esto provoca que la función `ROW_NUMBER` enumere las filas de cada partición.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(PARTITION BY SalesTerritoryKey 
        ORDER BY SUM(SalesAmountQuota) DESC) AS RowNumber,  
    LastName, SalesTerritoryKey AS Territory,  
    CONVERT(varchar(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName, SalesTerritoryKey;  
```  
  
 A continuación se muestra un conjunto parcial de resultados.  
 
```  
 
RowNumber  LastName            Territory  SalesQuota  
---------  ------------------  ---------  -------------  
1          Campbell            1           4,025,000.00  
2          Ansman-Wolfe        1           3,551,000.00  
3          Mensa-Annan         1           2,275,000.00  
1          Blythe              2          11,162,000.00  
1          Carson              3          12,198,000.00  
1          Mitchell            4          11,786,000.00  
2          Ito                 4           7,804,000.00  
```
  
## <a name="see-also"></a>Consulte también  
 [RANK &#40;Transact-SQL&#41;](../../t-sql/functions/rank-transact-sql.md)   
 [DENSE_RANK &#40;Transact-SQL&#41;](../../t-sql/functions/dense-rank-transact-sql.md)   
 [NTILE &#40;Transact-SQL&#41;](../../t-sql/functions/ntile-transact-sql.md)  
  
  


