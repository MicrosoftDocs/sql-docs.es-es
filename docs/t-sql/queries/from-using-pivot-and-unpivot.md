---
title: Uso de PIVOT y UNPIVOT | Microsoft Docs
description: Referencia de Transact-SQL para los operadores relacionales PIVOT y UNPIVOT. Use estos operadores en instrucciones SELECT para modificar una expresión con valores de tabla en otra tabla.
ms.date: 10/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PIVOT_TSQL
helpviewer_keywords:
- FROM clause, UNPIVOT operator
- unpivoting tables
- table pivoting [SQL Server]
- UNPIVOT operator
- crosstab query
- PIVOT operator
- rotating table-valued expressions
- pivoting tables
- FROM clause, PIVOT operator
- rotating columns
ms.assetid: 24ba54fc-98f7-4d35-8881-b5158aac1d66
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 21df23cf904c561d295553fb5fb79ebe2cd7702a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207733"
---
# <a name="from---using-pivot-and-unpivot"></a>FROM: uso de PIVOT y UNPIVOT

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Se pueden usar los operadores relacionales `PIVOT` y `UNPIVOT` para modificar una expresión con valores de tabla en otra tabla. `PIVOT` gira una expresión con valores de tabla al convertir los valores únicos de una columna en la expresión en varias columnas en la salida. Y `PIVOT` ejecuta agregaciones donde se requieren en los valores de columna restantes que se desean en la salida final. `UNPIVOT` realiza la operación contraria a PIVOT girando las columnas de una expresión con valores de tabla a valores de columna.  
  
La sintaxis de `PIVOT` es más sencilla y legible que la sintaxis que se puede especificar en una serie compleja de instrucciones `SELECT...CASE`. Para obtener una descripción completa de la sintaxis de `PIVOT`, vea [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md).  
  
## <a name="syntax"></a>Sintaxis  
La sintaxis siguiente resume cómo se usa el operador `PIVOT`.  
  
```syntaxsql  
SELECT <non-pivoted column>,  
    [first pivoted column] AS <column name>,  
    [second pivoted column] AS <column name>,  
    ...  
    [last pivoted column] AS <column name>  
FROM  
    (<SELECT query that produces the data>)   
    AS <alias for the source query>  
PIVOT  
(  
    <aggregation function>(<column being aggregated>)  
FOR   
[<column that contains the values that will become column headers>]   
    IN ( [first pivoted column], [second pivoted column],  
    ... [last pivoted column])  
) AS <alias for the pivot table>  
<optional ORDER BY clause>;  
```  

## <a name="remarks"></a>Observaciones  
Los identificadores de columna de la cláusula `UNPIVOT` siguen la intercalación del catálogo. Para [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], la intercalación es siempre `SQL_Latin1_General_CP1_CI_AS`. Para las bases de datos parcialmente independientes de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], la intercalación es siempre `Latin1_General_100_CI_AS_KS_WS_SC`. Si la columna se combina con otras columnas, se necesita una cláusula COLLATE (`COLLATE DATABASE_DEFAULT`) para evitar conflictos.  

  
## <a name="basic-pivot-example"></a>Ejemplo PIVOT básico  
En el ejemplo de código siguiente se genera una tabla de dos columnas con cuatro filas.  
  
```sql
USE AdventureWorks2014 ;  
GO  
SELECT DaysToManufacture, AVG(StandardCost) AS AverageCost   
FROM Production.Product  
GROUP BY DaysToManufacture;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
DaysToManufacture AverageCost
----------------- -----------
0                 5.0885
1                 223.88
2                 359.1082
4                 949.4105
```
  
No hay productos definidos con tres `DaysToManufacture`.  
  
En el código siguiente se muestra el mismo resultado, dinamizado para que los valores de `DaysToManufacture` se conviertan en encabezados de columna. Se proporciona una columna para tres `[3]` días, aunque los resultados son `NULL`.  
  
```sql
-- Pivot table with one row and five columns  
SELECT 'AverageCost' AS Cost_Sorted_By_Production_Days,   
[0], [1], [2], [3], [4]  
FROM  
(SELECT DaysToManufacture, StandardCost   
    FROM Production.Product) AS SourceTable  
PIVOT  
(  
AVG(StandardCost)  
FOR DaysToManufacture IN ([0], [1], [2], [3], [4])  
) AS PivotTable;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Cost_Sorted_By_Production_Days 0           1           2           3           4         
------------------------------ ----------- ----------- ----------- ----------- -----------
AverageCost                    5.0885      223.88      359.1082    NULL        949.4105
```
  
## <a name="complex-pivot-example"></a>Ejemplo PIVOT complejo  
Un escenario habitual en el que `PIVOT` puede ser útil es cuando se desea generar informes de tabulación cruzada para proporcionar un resumen de los datos. Por ejemplo, suponga que desea consultar la tabla `PurchaseOrderHeader` en la base de datos de ejemplo `AdventureWorks2014` para determinar el número de pedidos de compra colocados por ciertos empleados. En la siguiente consulta se proporciona este informe, ordenado por proveedor.  
  
```sql
USE AdventureWorks2014;  
GO  
SELECT VendorID, [250] AS Emp1, [251] AS Emp2, [256] AS Emp3, [257] AS Emp4, [260] AS Emp5  
FROM   
(SELECT PurchaseOrderID, EmployeeID, VendorID  
FROM Purchasing.PurchaseOrderHeader) p  
PIVOT  
(  
COUNT (PurchaseOrderID)  
FOR EmployeeID IN  
( [250], [251], [256], [257], [260] )  
) AS pvt  
ORDER BY pvt.VendorID;  
```  
  
A continuación se muestra un conjunto parcial de resultados.  
  
```
VendorID    Emp1        Emp2        Emp3        Emp4        Emp5  
----------- ----------- ----------- ----------- ----------- -----------
1492        2           5           4           4           4
1494        2           5           4           5           4
1496        2           4           4           5           5
1498        2           5           4           4           4
1500        3           4           4           5           4
```
  
Los resultados devueltos por esta instrucción de subselección se dinamizan en la columna `EmployeeID`.  
  
```sql
SELECT PurchaseOrderID, EmployeeID, VendorID  
FROM PurchaseOrderHeader;  
```  
  
Los valores únicos devueltos por la columna `EmployeeID` se convierten en campos en el conjunto de resultados finales. Como tal, hay una columna para cada número de `EmployeeID` especificado en la cláusula dinámica: en este caso los empleados `250`, `251`, `256`, `257` y `260`. La columna `PurchaseOrderID` se utiliza como columna de valores, respecto a la que se ordenan las columnas del resultado final, denominadas columnas de agrupamiento. En este caso, las columnas de agrupamiento se agregan mediante la función `COUNT`. Tenga presente que aparece un mensaje de advertencia que indica que los valores NULL que aparecen en la columna `PurchaseOrderID` no se tuvieron en cuenta cuando se contabilizó `COUNT` para cada empleado.  
  
> [!IMPORTANT]  
>  Cuando se usan funciones de agregado con `PIVOT`, la presencia de valores NULL en la columna de valores no se tiene en cuenta cuando se calcula una agregación.  

## <a name="unpivot-example"></a>Ejemplo de UNPIVOT
  
`UNPIVOT` realiza casi la operación inversa de `PIVOT`, girando columnas en filas. Suponga que la tabla producida en el ejemplo anterior se almacena en la base de datos como `pvt` y que desea girar los identificadores de columna `Emp1`, `Emp2`, `Emp3`, `Emp4` y `Emp5` a valores de fila que correspondan a un determinado proveedor. Como tal, debe identificar dos columnas adicionales. La columna que contendrá los valores de columna que se están girando (`Emp1`, `Emp2`,...) se denominará `Employee` y la columna que contendrá los valores que existen actualmente en las columnas que se giran se denominará `Orders`. Estas columnas corresponden a *columna_dinámica* y *columna_de_valor*, respectivamente, en la definición de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Esta es la consulta.  
  
```sql
-- Create the table and insert values as portrayed in the previous example.  
CREATE TABLE pvt (VendorID INT, Emp1 INT, Emp2 INT,  
    Emp3 INT, Emp4 INT, Emp5 INT);  
GO  
INSERT INTO pvt VALUES (1,4,3,5,4,4);  
INSERT INTO pvt VALUES (2,4,1,5,5,5);  
INSERT INTO pvt VALUES (3,4,3,5,4,4);  
INSERT INTO pvt VALUES (4,4,2,5,5,4);  
INSERT INTO pvt VALUES (5,5,1,5,5,5);  
GO  
-- Unpivot the table.  
SELECT VendorID, Employee, Orders  
FROM   
   (SELECT VendorID, Emp1, Emp2, Emp3, Emp4, Emp5  
   FROM pvt) p  
UNPIVOT  
   (Orders FOR Employee IN   
      (Emp1, Emp2, Emp3, Emp4, Emp5)  
)AS unpvt;  
GO  
```  
  
A continuación se muestra un conjunto parcial de resultados.  
  
```
VendorID    Employee    Orders
----------- ----------- ------
1            Emp1       4
1            Emp2       3 
1            Emp3       5
1            Emp4       4
1            Emp5       4
2            Emp1       4
2            Emp2       1
2            Emp3       5
2            Emp4       5
2            Emp5       5
...
```
  
Tenga en cuenta que `UNPIVOT` no es exactamente lo contrario a `PIVOT`. `PIVOT` realiza una agregación y combina posibles múltiples filas en una sola fila en la salida. `UNPIVOT` no reproduce el resultado de la expresión con valores de tabla original porque las filas se han combinado. Además, los valores null de la entrada de `UNPIVOT` desaparecen en la salida. Cuando los valores desaparecen, se muestra que puede haber habido valores null originales en la entrada antes de la operación `PIVOT`.  
  
En la vista `Sales.vSalesPersonSalesByFiscalYears` de la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] se usa `PIVOT` para devolver el total de ventas de cada vendedor, para cada año fiscal. Para generar el script de la vista en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en el **Explorador de objetos**, localice la vista en la carpeta **Views** de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Haga clic con el botón derecho en el nombre de la vista y después seleccione **Incluir vista como**.  
  
## <a name="see-also"></a>Consulte también  
[FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)   
[CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)  
  
