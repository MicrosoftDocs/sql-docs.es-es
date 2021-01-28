---
description: TOP (Transact-SQL)
title: TOP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TOP_TSQL
- TOP
dev_langs:
- TSQL
helpviewer_keywords:
- TOP clause
- first set of query result rows [SQL Server]
- TOP clause, about TOP clause
- queries [SQL Server], results
ms.assetid: da983c0a-06c5-4cf8-a6a4-7f9d66f34f2c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eb6024d0ad3ef6f34d170201c0fbacc3447dab26
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783603"
---
# <a name="top-transact-sql"></a>TOP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Limita las filas devueltas en un conjunto de resultados de la consulta a un número o porcentaje de filas especificado en [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Al usar TOP con la cláusula ORDER BY, el conjunto de resultados se limita al primer número *N* de filas ordenadas. De lo contrario, TOP devuelve el primer número *N* de filas en un orden indefinido. Use esta cláusula para especificar el número de filas devueltas de una instrucción SELECT. O bien, use TOP para especificar las filas afectadas por una instrucción INSERT, UPDATE, MERGE o DELETE.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
 
 A continuación se muestra la sintaxis de SQL Server y Azure SQL Database:

```syntaxsql  
[   
    TOP (expression) [PERCENT]  
    [ WITH TIES ]  
]  
```  

La siguiente es la sintaxis de [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:

```syntaxsql  
[   
    TOP ( expression )   
    [ WITH TIES ]  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*expression*  
La expresión numérica que especifica el número de filas que se van a devolver. *expression* se convierte de forma implícita en un valor de tipo **Float** si especifica PERCENT. De lo contrario, *expression* se convierte en **bigint**.  
  
PERCENT  
Indica que la consulta devuelve solo el primer porcentaje de filas de *expression* del conjunto de resultados. Los valores fraccionarios se redondean al siguiente valor entero.  
  
WITH TIES  
Devuelve dos o más filas que ocupan el último lugar en el conjunto de resultados limitado. Debe usar este argumento con la cláusula **ORDER BY**. **WITH TIES** puede hacer que se devuelvan más filas que las del valor especificado en *expression*. Por ejemplo, si *expression* está establecido en 5, pero dos filas adicionales coinciden con los valores de las columnas **ORDER BY** en la fila 5, el conjunto de resultados contendrá siete filas.  
  
Puede especificar la cláusula TOP con el argumento WITH TIES solo en instrucciones SELECT y solo si también ha especificado la cláusula ORDER BY. El orden devuelto de los registros enlazados es arbitrario. ORDER BY no afecta a esta regla.  
  
## <a name="best-practices"></a>Prácticas recomendadas  
En una instrucción SELECT, utilice siempre una cláusula ORDER BY con la cláusula TOP. Porque es la única manera de indicar previsiblemente a qué filas afecta TOP.  
  
Utilice OFFSET y FETCH en la cláusula ORDER BY en lugar de la cláusula TOP para implementar una solución de paginación de consulta. Una solución de paginación (es decir, el envío de fragmentos o "páginas" de datos al cliente) es más fácil de implementar mediante OFFSET y FETCH. Para obtener más información, vea [ORDER BY &#40;cláusula de Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
Utilice TOP (o bien, OFFSET y FETCH) en lugar de SET ROWCOUNT para limitar el número de filas devueltas. Estos métodos son preferibles a utilizar SET ROWCOUNT por las siguientes razones:  
  
-   Como parte de una instrucción SELECT, el optimizador de consultas puede considerar el valor de *expression* en las cláusulas TOP o FETCH durante la optimización de consulta. Dado que usa SET ROWCOUNT fuera de una instrucción que ejecuta una consulta, su valor no se puede considerar en un plan de consulta.  
  
## <a name="compatibility-support"></a>Soporte de compatibilidad  
Por compatibilidad con versiones anteriores, los paréntesis son opcionales en las instrucciones SELECT si la expresión es una constante entera. Recomendamos que use siempre paréntesis para TOP en las instrucciones SELECT. Hacerlo proporciona coherencia con su uso necesario en las instrucciones INSERT, UPDATE, MERGE y DELETE. 
  
## <a name="interoperability"></a>Interoperabilidad  
La expresión TOP no afecta a las instrucciones que se pueden ejecutar debido a un desencadenador. Las tablas **insertadas** y **eliminadas** en los desencadenadores solo devuelven las filas verdaderamente afectadas por las instrucciones INSERT, UPDATE, MERGE o DELETE. Por ejemplo, si INSERT TRIGGER se desencadena como resultado de una instrucción INSERT que utilizó una cláusula TOP.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permite actualizar las filas a través de las vistas. Dado que puede incluir la cláusula TOP en la definición de vista, es posible que algunas filas desaparezcan de la vista si las filas ya no cumplen con los requisitos de la expresión TOP a cause de una actualización.  
  
Al especificarse en la instrucción MERGE, la cláusula TOP se aplica *después* de combinarse toda la tabla de origen y toda la tabla de destino. Además, se quitan las filas combinadas que no cumplen los requisitos para una acción de inserción, actualización o eliminación. La cláusula TOP reduce aún más el número de filas combinadas al valor especificado y se aplican las acciones de inserción, actualización o eliminación a las filas combinadas restantes de una manera desordenada. Es decir, no hay ningún orden en el que las filas se distribuyan entre las acciones definidas en las cláusulas WHEN. Por ejemplo, si especificar TOP (10) afecta a 10 filas, de estas, siete pueden actualizarse y tres insertarse. O bien, uno puede eliminarse, cinco actualizarse y cuatro insertarse, etc. Dado que la instrucción MERGE realiza exámenes de tabla completos de ambas tablas, de destino y de origen, el rendimiento de E/S puede verse afectado al utilizar la cláusula TOP para modificar una tabla grande mediante la creación de varios lotes. En este escenario, es importante asegurarse de que todos los lotes sucesivos tengan como destino nuevas filas.  
  
Tenga precaución al especificar la cláusula TOP en una consulta que contiene un operador UNION, UNION ALL, EXCEPT o INTERSECT. Es posible escribir una consulta que devuelva resultados inesperados porque el orden en el que se procesan lógicamente las cláusulas TOP y ORDER BY no siempre es intuitivo cuando estos operadores se utilizan en una operación Select. Por ejemplo, dados los siguientes datos y la siguiente tabla, suponga que desea devolver el coche rojo menos caro y el coche azul menos caro. Es decir, el sedán rojo y la camioneta azul.  
  
```sql  
CREATE TABLE dbo.Cars(Model VARCHAR(15), Price MONEY, Color VARCHAR(10));  
INSERT dbo.Cars VALUES  
    ('sedan', 10000, 'red'), ('convertible', 15000, 'blue'),   
    ('coupe', 20000, 'red'), ('van', 8000, 'blue');  
```  
  
Para lograr estos resultados, podría escribir la siguiente consulta.  
  
```sql  
SELECT TOP(1) Model, Color, Price  
FROM dbo.Cars  
WHERE Color = 'red'  
UNION ALL  
SELECT TOP(1) Model, Color, Price  
FROM dbo.Cars  
WHERE Color = 'blue'  
ORDER BY Price ASC;  
GO    
```  
  
A continuación se muestra el conjunto de resultados.  
  
 ```
 Model         Color      Price  
 ------------- ---------- -------  
 sedan         red        10000.00  
 convertible   blue       15000.00
 ```  
  
Se devuelven resultados inesperados porque la cláusula TOP se ejecuta lógicamente antes que la cláusula ORDER BY, que ordena los resultados del operador (UNION ALL en este caso). Así, la consulta anterior devuelve cualquier coche rojo y cualquier coche azul y, a continuación, ordena el resultado de esa unión por el precio. En el siguiente ejemplo se muestra el método correcto de escribir esta consulta para lograr el resultado deseado.  
  
```sql  
SELECT Model, Color, Price  
FROM (SELECT TOP(1) Model, Color, Price  
      FROM dbo.Cars  
      WHERE Color = 'red'  
      ORDER BY Price ASC) AS a  
UNION ALL  
SELECT Model, Color, Price  
FROM (SELECT TOP(1) Model, Color, Price  
      FROM dbo.Cars  
      WHERE Color = 'blue'  
      ORDER BY Price ASC) AS b;  
GO    
```  
  
Utilizando TOP y ORDER BY en una operación de subselección, se puede asegurar de que los resultados de la cláusula ORDER BY se apliquen a la cláusula TOP y no a ordenar el resultado de la operación UNION.  
  
 El conjunto de resultados es el siguiente:  
  
 ```
 Model         Color      Price  
 ------------- ---------- -------  
 sedan         red        10000.00  
 van           blue        8000.00
 ```  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
Al usar TOP con INSERT, UPDATE,MERGE o DELETE, las filas a las que se hace referencia no están organizadas en ningún orden. Además, no puede especificar directamente la cláusula ORDER BY en estas instrucciones. Si necesita usar TOP para insertar, eliminar o modificar las filas en un orden cronológico significativo, utilice TOP con una cláusula ORDER BY especificada en una instrucción de subselección. Consulte la sección Ejemplos siguiente que se incluye en este artículo.  
  
No puede usar TOP en las instrucciones UPDATE y DELETE en vistas con particiones.  
  
No puede combinar TOP con OFFSET y FETCH en la misma expresión de consulta (en el mismo ámbito de la consulta). Para obtener más información, vea [ORDER BY &#40;cláusula de Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
  
|Category|Elementos de sintaxis ofrecidos|  
|--------------|------------------------------|  
|[Sintaxis básica](#BasicSyntax)|TOP • PERCENT|  
|[Incluir valores equivalentes](#tie)|WITH TIES|  
|[Limitar las filas afectadas por DELETE, INSERT o UPDATE](#DML)|DELETE • INSERT • UPDATE|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Sintaxis básica  
En los ejemplos de esta sección se muestra la funcionalidad básica de la cláusula ORDER BY con la sintaxis mínima requerida.  
  
#### <a name="a-using-top-with-a-constant-value"></a>A. Utilizar TOP con un valor constante  
En los siguientes ejemplos se utiliza un valor constante para especificar el número de empleados que se devuelven en el conjunto de resultados de la consulta. En el primer ejemplo, se devuelven las 10 primeras filas sin definir porque no se usa una cláusula ORDER BY. En el segundo ejemplo, se utiliza una cláusula ORDER BY para devolver los primeros 10 empleados contratados recientemente.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Select the first 10 random employees.  
SELECT TOP(10)JobTitle, HireDate  
FROM HumanResources.Employee;  
GO  
-- Select the first 10 employees hired most recently.  
SELECT TOP(10)JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY HireDate DESC;  
GO  
```  
  
#### <a name="b-using-top-with-a-variable"></a>B. Usar TOP con una variable  
En el siguiente ejemplo se utiliza una variable para especificar el número de empleados que se devuelven en el conjunto de resultados de la consulta.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @p AS INT = 10;  
SELECT TOP(@p)JobTitle, HireDate, VacationHours  
FROM HumanResources.Employee  
ORDER BY VacationHours DESC;  
GO  
```  
  
#### <a name="c-specifying-a-percentage"></a>C. Especificar un porcentaje  
En el siguiente ejemplo se utiliza PERCENT para especificar el número de empleados que se devuelven en el conjunto de resultados de la consulta. Hay 290 empleados en la tabla `HumanResources.Employee`. Dado que el cinco por ciento de 290 es un valor fraccionario, el valor se redondea al número entero siguiente.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(5)PERCENT JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY HireDate DESC;  
GO    
```  
  
###  <a name="including-tie-values"></a><a name="tie"></a> Incluir valores equivalentes  
  
#### <a name="a-using-with-ties-to-include-rows-that-match-the-values-in-the-last-row"></a>A. Utilizar WITH TIES para incluir las filas que coinciden con los valores de la última fila  
En el ejemplo siguiente se obtiene el primer `10` por ciento de los empleados que tienen los salarios más altos y se devuelven en orden descendente de acuerdo con su salario. La especificación de `WITH TIES` garantiza que también se incluyan en el conjunto de resultados los empleados con salarios iguales al salario más bajo devuelto (la última fila), aun cuando esto exceda el `10` por ciento de los empleados.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(10) PERCENT WITH TIES  
pp.FirstName, pp.LastName, e.JobTitle, e.Gender, r.Rate  
FROM Person.Person AS pp   
    INNER JOIN HumanResources.Employee AS e  
        ON pp.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN HumanResources.EmployeePayHistory AS r  
        ON r.BusinessEntityID = e.BusinessEntityID  
ORDER BY Rate DESC;  
GO    
```  
  
###  <a name="limiting-the-rows-affected-by-delete-insert-or-update"></a><a name="DML"></a> Limitar las filas afectadas por DELETE, INSERT o UPDATE  
  
#### <a name="a-using-top-to-limit-the-number-of-rows-deleted"></a>A. Utilizar TOP para limitar el número de filas eliminadas  
Cuando usa una cláusula TOP (*n*) con DELETE, la operación de eliminación se realiza en una selección sin definir del número *n* de filas. Es decir, la instrucción DELETE elige cualquier número (*n*) de filas que cumplen los criterios definidos en la cláusula WHERE. En el ejemplo siguiente se eliminan `20` filas de la tabla `PurchaseOrderDetail` cuyas fechas de vencimiento son anteriores al 1 de julio de 2002.  
  
```sql  
USE AdventureWorks2012;  
GO  
DELETE TOP (20)   
FROM Purchasing.PurchaseOrderDetail  
WHERE DueDate < '20020701';  
GO  
```  
  
Si desea utilizar TOP para eliminar filas por un orden cronológico significativo, utilícela con ORDER BY en una instrucción de subselección. La siguiente consulta elimina de la tabla `PurchaseOrderDetail` las 10 filas con las fechas de vencimiento más antiguas. Para garantizar que solo se eliminen 10 filas, la columna especificada en la instrucción de subselección (`PurchaseOrderID`) es la clave principal de la tabla. El uso de una columna sin clave en la instrucción de subselección podría causar la eliminación de más de 10 filas si la columna especificada contiene valores duplicados.  
  
```sql  
USE AdventureWorks2012;  
GO  
DELETE FROM Purchasing.PurchaseOrderDetail  
WHERE PurchaseOrderDetailID IN  
   (SELECT TOP 10 PurchaseOrderDetailID   
    FROM Purchasing.PurchaseOrderDetail   
    ORDER BY DueDate ASC);  
GO  
```  
  
#### <a name="b-using-top-to-limit-the-number-of-rows-inserted"></a>B. Utilizar TOP para limitar el número de filas insertadas  
En el siguiente ejemplo se crea la tabla `EmployeeSales` y se insertan el nombre y los datos de ventas del año hasta la fecha para los primeros cinco empleados de la tabla `HumanResources.Employee`. La instrucción INSERT elige cinco filas cualesquiera devueltas por la instrucción `SELECT` que cumplen los criterios definidos en la cláusula WHERE. La cláusula OUTPUT muestra las filas que se insertan en la tabla `EmployeeSales`. Observe que la cláusula ORDER BY de la instrucción SELECT no se utiliza para determinar los primeros cinco empleados.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
IF OBJECT_ID ('dbo.EmployeeSales', 'U') IS NOT NULL  
    DROP TABLE dbo.EmployeeSales;  
GO  
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   NVARCHAR(11) NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  YearlySales  MONEY NOT NULL  
 );  
GO  
INSERT TOP(5)INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, inserted.LastName, inserted.YearlySales  
    SELECT sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
GO    
```  
  
Si desea utilizar TOP para insertar filas por un orden cronológico significativo, utilícela con ORDER BY en una instrucción de subselección. En el ejemplo siguiente se muestra cómo hacerlo. La cláusula OUTPUT muestra las filas que se insertan en la tabla `EmployeeSales`. Observe que los cinco primeros empleados se insertan ahora según los resultados de la cláusula ORDER BY en lugar de las filas sin definir.  
  
```sql  
INSERT INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, inserted.LastName, inserted.YearlySales  
    SELECT TOP (5) sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
GO    
```  
  
#### <a name="c-using-top-to-limit-the-number-of-rows-updated"></a>C. Utilizar TOP para limitar el número de filas actualizadas  
En el ejemplo siguiente se usa la cláusula TOP para actualizar filas de una tabla. Cuando se usa una cláusula TOP (*n*) con UPDATE, la operación de actualización se ejecuta en un número sin definir de filas. Es decir, la instrucción UPDATE elige cualquier número (*n*) de filas que cumplen los criterios definidos en la cláusula WHERE. En el ejemplo siguiente se asignan 10 clientes de un vendedor a otro.  
  
```sql  
USE AdventureWorks2012;  
UPDATE TOP (10) Sales.Store  
SET SalesPersonID = 276  
WHERE SalesPersonID = 275;  
GO  
```  
  
Si necesita usar TOP para aplicar actualizaciones según un orden cronológico significativo, debe usar TOP junto con ORDER BY en una instrucción de subselección. En el siguiente ejemplo se actualizan las horas de vacaciones de los 10 empleados cuyas fechas de alta son más antiguas.  
  
```sql  
UPDATE HumanResources.Employee  
SET VacationHours = VacationHours + 8  
FROM (SELECT TOP 10 BusinessEntityID FROM HumanResources.Employee  
     ORDER BY HireDate ASC) AS th  
WHERE HumanResources.Employee.BusinessEntityID = th.BusinessEntityID;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
En el ejemplo siguiente se devuelven las primeras 31 filas que coinciden con los criterios de búsqueda. La cláusula **ORDER BY** se asegura de que las 31 filas devueltas son las primeras 31 filas según una ordenación alfabética de la columna `LastName`.  
  
Use **TOP** sin especificar valores equivalentes.  
  
```sql  
SELECT TOP (31) FirstName, LastName   
FROM DimEmployee ORDER BY LastName;  
```  
  
Resultado: se devuelven 31 filas.  
  
Uso de TOP con la especificación WITH TIES.  
  
```sql  
SELECT TOP (31) WITH TIES FirstName, LastName   
FROM DimEmployee ORDER BY LastName;  
```  
  
Resultado: se devuelven 33 filas, ya que tres empleados llamados Brown ocupan la fila 31.  
  
## <a name="see-also"></a>Consulte también  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [Cláusula ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md)   
 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)  
  
 
