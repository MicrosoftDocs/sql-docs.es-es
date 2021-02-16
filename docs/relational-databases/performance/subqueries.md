---
title: Subconsultas (SQL Server) | Microsoft Docs
description: Busque un ejemplo de subconsulta, que es una consulta anidada en una instrucción SELECT, INSERT, UPDATE o DELETE, o bien en otra subconsulta de SQL Server.
ms.custom: ''
ms.date: 02/18/2018
ms.prod: sql
ms.technology: performance
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Subquery
- Subqueries
- subqueries [SQL Server], fundamentals
- subqueries [SQL Server], correlated
- subqueries [SQL Server], types
ms.assetid: bfc97432-c14c-4768-9dc5-a9c512f6b2bd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6ce8c74858311d125d8053272f0cff3e14917cf6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345531"
---
# <a name="subqueries-sql-server"></a>Subconsultas (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 
Una subconsulta es una consulta anidada en una instrucción `SELECT`, `INSERT`, `UPDATE` o `DELETE`, o bien en otra subconsulta. Las subconsultas se pueden utilizar en cualquier parte en la que se permita una expresión. En este ejemplo, se utiliza una subconsulta como una expresión de columna llamada MaxUnitPrice en una instrucción SELECT.

```sql
USE AdventureWorks2016;
GO
SELECT Ord.SalesOrderID, Ord.OrderDate,
    (SELECT MAX(OrdDet.UnitPrice)
     FROM Sales.SalesOrderDetail AS OrdDet
     WHERE Ord.SalesOrderID = OrdDet.SalesOrderID) AS MaxUnitPrice
FROM Sales.SalesOrderHeader AS Ord;
GO
```

## <a name="subquery-fundamentals"></a><a name="fundamentals"></a> Aspectos básicos de las subconsultas
Se llama también subconsulta a una consulta o selección interna, mientras que la instrucción que contiene una subconsulta también es conocida como consulta o selección externa.   

Muchas de las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que incluyen subconsultas se pueden formular también como combinaciones. Otras preguntas se pueden formular solo con subconsultas. En [!INCLUDE[tsql](../../includes/tsql-md.md)], normalmente no hay diferencias de rendimiento entre una instrucción que incluya una subconsulta y una versión semánticamente equivalente que no la incluya. Sin embargo, en algunos casos en los que se debe comprobar la existencia de un elemento, una combinación produce mejores resultados. De lo contrario, se debe procesar la consulta anidada para cada resultado de la consulta externa con el fin de garantizar la eliminación de los duplicados. En tales casos, la utilización de combinaciones producirá mejores resultados. A continuación aparece un ejemplo que muestra una subconsulta `SELECT` y una combinación `SELECT` que devuelven el mismo conjunto de resultados:

```sql
USE AdventureWorks2016;
GO

/* SELECT statement built using a subquery. */
SELECT Name
FROM Production.Product
WHERE ListPrice =
    (SELECT ListPrice
     FROM Production.Product
     WHERE Name = 'Chainring Bolts' );
GO

/* SELECT statement built using a join that returns
   the same result set. */
SELECT Prd1. Name
FROM Production.Product AS Prd1
     JOIN Production.Product AS Prd2
       ON (Prd1.ListPrice = Prd2.ListPrice)
WHERE Prd2. Name = 'Chainring Bolts';
GO
```

Una subconsulta anidada en la instrucción externa SELECT tiene los componentes siguientes:    
-   Una consulta `SELECT` normal, que incluye los componentes normales de la lista de selección.   
-   Una cláusula normal `FROM` que incluye uno o varios nombres de tablas o vistas.   
-   Una cláusula opcional `WHERE`.   
-   Una cláusula opcional `GROUP BY`.   
-   Una cláusula opcional `HAVING`.   

La consulta SELECT de una subconsulta se presenta siempre entre paréntesis. No puede incluir una cláusula `COMPUTE` o `FOR BROWSE` y solo puede incluir una cláusula `ORDER BY` cuando se especifica también una cláusula TOP.   

Una subconsulta puede anidarse en la cláusula `WHERE` o `HAVING` de una instrucción externa `SELECT`, `INSERT`, `UPDATE` o `DELETE`, o bien en otra subconsulta. Se puede disponer de hasta 32 niveles de anidamiento, aunque el límite varía dependiendo de la memoria disponible y de la complejidad del resto de las expresiones de la consulta. Las consultas individuales no permiten anidamientos de más de 32 niveles. Una subconsulta puede aparecer en cualquier parte en la que se pueda usar una expresión, si devuelve un solo valor.   

Si una tabla solo aparece en una subconsulta y no en la consulta externa, las columnas de esa tabla no se podrán incluir en la salida (la lista de selección de la consulta externa).   

Las instrucciones que incluyen una subconsulta normalmente tienen uno de estos formatos:   
-   Expresión WHERE \[NOT] IN (subconsulta)
-   Expresión WHERE comparison_operator \[ANY | ALL] (subconsulta)
-   WHERE \[NOT] EXISTS (subconsulta)   

En algunas instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)], la subconsulta se puede evaluar como si fuera una consulta independiente. Conceptualmente, los resultados de la subconsulta se sustituyen en la consulta externa, aunque en realidad esta no es la forma en la que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procesa las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] con subconsultas.    

Hay tres tipos básicos de subconsultas, que son las siguientes: 
-   Las que operan en listas especificadas con `IN` o modificadas por un operador de comparación mediante `ANY` o `ALL`.
-   Las que se especifican con un operador de comparación sin modificar y deben devolver un solo valor.
-   Las que son pruebas de existencia especificadas con `EXISTS`.

## <a name="subquery-rules"></a><a name="rules"></a> Reglas de las subconsultas
Las subconsultas están sujetas a las restricciones siguientes: 
-   La lista de selección de una subconsulta que se especifica con un operador de comparación, solo puede incluir un nombre de expresión o columna (excepto `EXISTS` e `IN`, que operan en `SELECT *` o en una lista respectivamente).   
-   Si la cláusula `WHERE` de una consulta externa incluye un nombre de columna, debe ser compatible con una combinación con la columna indicada en la lista de selección de la subconsulta.   
-   Los tipos de datos **ntext**, **text** y **image** no están permitidos en las listas de selección de subconsultas.   
-   Puesto que deben devolver un solo valor, las subconsultas que se especifican con un operador de comparación sin modificar (no seguido de la palabra clave ANY o ALL) no pueden incluir las cláusulas `GROUP BY` y `HAVING`.   
-   La palabra clave `DISTINCT` no se puede usar con subconsultas que incluyan GROUP BY.
-   Las cláusulas `COMPUTE` y `INTO` no se pueden especificar.   
-   Solo se puede especificar `ORDER BY` si se especifica también `TOP`.   
-   Una vista creada con una subconsulta no se puede actualizar.   
-   La lista de selección de una subconsulta especificada con `EXISTS`, por convención, tiene un asterisco (\*) en lugar de un solo nombre de columna. Las reglas de una subconsulta especificada con `EXISTS` son idénticas a las de una lista de selección estándar, porque una subconsulta introducida por `EXISTS` crea una prueba de existencia y devuelve TRUE o FALSE en lugar de datos.   

## <a name="qualifying-column-names-in-subqueries"></a><a name="qualifying"></a> Calificar nombres de columna en subconsultas
En el siguiente ejemplo, la columna *BusinessEntityID* de la cláusula `WHERE` de la consulta externa está calificada implícitamente por el nombre de tabla de la cláusula `FROM` de la consulta externa (*Sales.Store*). La referencia a *CustomerID* en la lista de selección de la subconsulta está calificada por la cláusula `FROM` de la subconsulta, es decir, por la tabla *Sales.Customer*.

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Sales.Store
WHERE BusinessEntityID NOT IN
    (SELECT CustomerID
     FROM Sales.Customer
     WHERE TerritoryID = 5);
GO
```

La regla general es que los nombres de columna de una instrucción están calificados implícitamente por la tabla a la que se hace referencia en la cláusula `FROM` del mismo nivel. Si una columna no existe en la tabla a la que se hace referencia en la cláusula `FROM` de una subconsulta, la tabla a la que se hace referencia en la cláusula `FROM` de la consulta externa la califica implícitamente.   

Ésta es la consulta en la que se han especificado estos supuestos implícitos:

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Sales.Store
WHERE Sales.Store.BusinessEntityID NOT IN
    (SELECT Sales.Customer.CustomerID
     FROM Sales.Customer
     WHERE TerritoryID = 5);
GO
```

Nunca está de más indicar explícitamente el nombre de la tabla y siempre se pueden anular los supuestos implícitos acerca de los nombres de tabla con calificaciones explícitas.   

> [!IMPORTANT]
> Si se hace referencia a una columna en una subconsulta que no existe en la tabla a la que se hace referencia en la cláusula `FROM` de la subconsulta, pero que existe en una la tabla a la que se hace referencia en la cláusula `FROM` de la consulta externa, la consulta se ejecuta sin errores. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] califica implícitamente la columna en la subconsulta con el nombre de tabla en la consulta externa.   

## <a name="multiple-levels-of-nesting"></a><a name="nesting"></a> Múltiples niveles de anidamiento
Una subconsulta puede incluir una o varias subconsultas. En una instrucción se puede anidar cualquier número de subconsultas.   

La consulta siguiente busca los nombres de los empleados que también son vendedores.   

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person
WHERE BusinessEntityID IN
    (SELECT BusinessEntityID
     FROM HumanResources.Employee
     WHERE BusinessEntityID IN
        (SELECT BusinessEntityID
         FROM Sales.SalesPerson)
    );
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
LastName                                           FirstName
-------------------------------------------------- -----------------------
Jiang                                              Stephen
Abbas                                              Syed
Alberts                                            Amy
Ansman-Wolfe                                       Pamela
Campbell                                           David
Carson                                             Jillian
Ito                                                Shu
Mitchell                                           Linda
Reiter                                             Tsvi
Saraiva                                            Jos
Vargas                                             Garrett
Varkey Chudukatil                                  Ranjit
Valdez                                             Rachel
Tsoflias                                           Lynn
Pak                                                Jae
Blythe                                             Michael
Mensa-Annan                                        Tete

(17 row(s) affected)
```

La consulta más interna devuelve los Id. de los vendedores. La consulta del siguiente nivel superior se evalúa con estos Id. de vendedores y devuelve los números de Id. de contacto de los empleados. Finalmente, la consulta externa usa los Id. de contacto para buscar el nombre de los empleados.   

También puede expresar esta consulta como una combinación:

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person c
INNER JOIN HumanResources.Employee e
ON c.BusinessEntityID = e.BusinessEntityID
JOIN Sales.SalesPerson s 
ON e.BusinessEntityID = s.BusinessEntityID;
GO
```

## <a name="correlated-subqueries"></a><a name="correlated"></a> Subconsultas correlacionadas
Se pueden evaluar muchas consultas mediante la ejecución de la subconsulta una vez y la sustitución del valor o valores resultantes en la cláusula `WHERE` de la consulta externa. En las consultas que incluyen una subconsulta correlativa (conocida también como una consulta repetitiva), la subconsulta depende de la consulta externa para sus valores. Esto significa que la subconsulta se ejecuta varias veces, una vez por cada fila que pueda ser seleccionada por la consulta externa.
Esta consulta recupera una instancia del nombre y apellido de cada empleado cuya bonificación en la tabla *SalesPerson* es 5000 y cuyos números de identificación de empleado coinciden en las tablas *Employee* y *SalesPerson*.

```sql
USE AdventureWorks2016;
GO
SELECT DISTINCT c.LastName, c.FirstName, e.BusinessEntityID 
FROM Person.Person AS c JOIN HumanResources.Employee AS e
ON e.BusinessEntityID = c.BusinessEntityID 
WHERE 5000.00 IN
    (SELECT Bonus
    FROM Sales.SalesPerson sp
    WHERE e.BusinessEntityID = sp.BusinessEntityID) ;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
LastName FirstName BusinessEntityID
-------------------------- ---------- ------------
Ansman-Wolfe Pamela 280
Saraiva José 282

(2 row(s) affected)
```

La subconsulta anterior de esta instrucción no se puede evaluar independientemente de la consulta externa. Necesita un valor para *Employee.BusinessEntityID*, pero este valor cambia a medida que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] examina diferentes filas en *Employee*.   
Así es, exactamente, como se evalúa esta consulta: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tiene en cuenta cada fila de la tabla Employee para incluirla en el resultado sustituyendo el valor de cada fila en la consulta interna.
Por ejemplo, si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] examina primero la fila de `Syed Abbas`, la variable *Employee.BusinessEntityID* toma el valor 285, que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sustituye en la consulta interna.

```sql
USE AdventureWorks2016;
GO
SELECT Bonus
FROM Sales.SalesPerson
WHERE BusinessEntityID = 285;
GO
```

El resultado es 0 (`Syed Abbas` no recibió una bonificación porque no es un vendedor), con lo que la consulta externa se evalúa como:

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person AS c JOIN HumanResources.Employee AS e
ON e.BusinessEntityID = c.BusinessEntityID 
WHERE 5000 IN (0.00);
GO
```

Puesto que esto es falso, la fila de `Syed Abbas` no se incluye en los resultados. Realice el mismo procedimiento con la fila de `Pamela Ansman-Wolfe`. Observe que esta fila se incluye en el resultado.     

Las subconsultas correlativas también pueden incluir funciones con valores de tabla en la cláusula `FROM` mediante la referencia a las columnas de una tabla de la consulta externa como un argumento de la función con valores de tabla. En este caso, para cada fila de la consulta externa, la función con valores de tabla se evalúa según la subconsulta.    
  
## <a name="subquery-types"></a><a name="types"></a> Tipos de subconsulta
Las subconsultas se pueden especificar en muchos sitios: 
-   Con alias Para obtener más información, consulte [Subconsultas con alias](#aliases).
-   Con `IN` o `NOT IN`. Para obtener más información, consulte [Subconsultas con IN](#in) y [Subconsultas con NOT IN](#notin).
-   En instrucciones `UPDATE`, `DELETE` y `INSERT`. Para obtener más información, consulte [Subconsultas en las instrucciones UPDATE, DELETE e INSERT](#upsert).
-   Con los operadores de comparación. Para obtener más información, consulte [Subconsultas con operadores de comparación](#comparison).
-   Con `ANY`, `SOME` o `ALL`. Para obtener más información, consulte [Operadores de comparación modificados por ANY, SOME o ALL](#comparison_modified).
-   Con `EXISTS` o `NOT EXISTS`. Para obtener más información, consulte [Subconsultas con EXISTS](#exists) y [Subconsultas con NOT EXISTS](#notexists).
-   En lugar de una expresión. Para obtener más información, consulte [Subconsultas usadas en lugar de una expresión](#expression).

### <a name="subqueries-with-aliases"></a><a name="aliases"></a> Subconsultas con alias
Muchas instrucciones en las que la subconsulta y la consulta externa hacen referencia a la misma tabla se pueden indicar como autocombinaciones (combinar una tabla con ella misma). Por ejemplo, puede buscar direcciones de empleados desde un estado determinado utilizando una subconsulta:   

```sql
USE AdventureWorks2016;
GO
SELECT StateProvinceID, AddressID
FROM Person.Address
WHERE AddressID IN
    (SELECT AddressID
     FROM Person.Address
     WHERE StateProvinceID = 39);
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
StateProvinceID AddressID
----------- -----------
39 942
39 955
39 972
39 22660

(4 row(s) affected)
```   

O bien, puede usar una autocombinación:   

```sql
USE AdventureWorks2016;
GO
SELECT e1.StateProvinceID, e1.AddressID
FROM Person.Address AS e1
INNER JOIN Person.Address AS e2
ON e1.AddressID = e2.AddressID
AND e2.StateProvinceID = 39;
GO
```

Los alias de tabla son necesarios porque la tabla que se está combinando consigo misma aparece en dos roles distintos. Los alias se pueden usar también en las consultas anidadas que hacen referencia a la misma tabla en una consulta interna y externa.    

```sql
USE AdventureWorks2016;
GO
SELECT e1.StateProvinceID, e1.AddressID
FROM Person.Address AS e1
WHERE e1.AddressID IN
    (SELECT e2.AddressID
     FROM Person.Address AS e2
     WHERE e2.StateProvinceID = 39);
GO
```

Los alias explícitos indican claramente que las referencias a *Person.Address* en la subconsulta no significan lo mismo que las referencias de la consulta externa.   

### <a name="subqueries-with-in"></a><a name="in"></a> Subconsultas con IN
El resultado de una subconsulta especificada con `IN` (o con `NOT IN`) es una lista de cero o más valores. Una vez que la consulta devuelve los resultados, la consulta externa hace uso de ellos.    
La siguiente consulta busca los nombres de todos los productos de ruedas que Adventure Works Cycles fabrica.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]     

```
Name
----------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```

Esta instrucción se evalúa en dos pasos. En primer lugar, la consulta interior devuelve el número de identificación de subcategoría que coincide con el nombre 'Wheel' (17). En segundo lugar, este valor se sustituye en la consulta externa, que busca los nombres de productos asociados a los números de identificación de subcategoría en Product.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN ('17');
GO
```

Una diferencia de la utilización de una combinación en lugar de una subconsulta, para este problema y otros similares, es que la combinación permite mostrar, en el resultado, columnas de más de una tabla. Por ejemplo, si desea incluir en el resultado la subcategoría de nombre del producto, debe usar una versión con combinaciones.    

```sql
USE AdventureWorks2016;
GO
SELECT p.Name, s.Name
FROM Production.Product p
INNER JOIN Production.ProductSubcategory s
ON p.ProductSubcategoryID = s.ProductSubcategoryID
AND s.Name = 'Wheels';
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]    

```
Name
LL Mountain Front Wheel Wheels
ML Mountain Front Wheel Wheels
HL Mountain Front Wheel Wheels
LL Road Front Wheel Wheels
ML Road Front Wheel Wheels
HL Road Front Wheel Wheels
Touring Front Wheel Wheels
LL Mountain Rear Wheel Wheels
ML Mountain Rear Wheel Wheels
HL Mountain Rear Wheel Wheels
LL Road Rear Wheel Wheels
ML Road Rear Wheel Wheels
HL Road Rear Wheel Wheels
Touring Rear Wheel Wheels

(14 row(s) affected)
```    

En la siguiente consulta se busca el nombre de todos los proveedores cuya solvencia de crédito sea buena, de los que Adventure Works Cycles solicita al menos 20 artículos y cuyo tiempo hasta la entrega es de menos de 16 días.    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Purchasing.Vendor
WHERE CreditRating = 1
AND BusinessEntityID IN
    (SELECT BusinessEntityID
     FROM Purchasing.ProductVendor
     WHERE MinOrderQty >= 20
     AND AverageLeadTime < 16);
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
Name
--------------------------------------------------
Compete Enterprises, Inc
International Trek Center
First National Sport Co.
Comfort Road Bicycles
Circuit Cycles
First Rate Bicycles
Jeff's Sporting Goods
Competition Bike Training Systems
Electronic Bike Repair & Supplies
Crowley Sport
Expert Bike Co
Team Athletic Co.
Compete, Inc.   

(13 row(s) affected)
```   

Se evalúa la consulta interna, que da como resultado los números de Id. de los proveedores que cumplen las calificaciones de la subconsulta. Después se evalúa la consulta externa. Observe que puede incluir más de una condición en la cláusula WHERE tanto de la consulta interna como de la externa.   

Mediante una combinación, la misma consulta se expresa de esta forma:

```sql
USE AdventureWorks2016;
GO
SELECT DISTINCT Name
FROM Purchasing.Vendor v
INNER JOIN Purchasing.ProductVendor p
ON v.BusinessEntityID = p.BusinessEntityID
WHERE CreditRating = 1
  AND MinOrderQty >= 20
  AND AverageLeadTime < 16;
GO
```

Una combinación se puede expresar siempre como una subconsulta. Una subconsulta se puede expresar a menudo, aunque no siempre, como una combinación. Esto es así porque las combinaciones son simétricas: si combina la tabla A con a la B en cualquier orden obtendrá la misma respuesta. Esto no es cierto si se implica a una subconsulta.    

### <a name="subqueries-with-not-in"></a><a name="notin"></a> Subconsultas con NOT IN
Las subconsultas que empiezan por la palabra clave NOT IN, devuelven también una lista de cero o más valores.   
En la siguiente consulta se buscan los nombres de los productos que no son bicicletas acabadas.   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID NOT IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Mountain Bikes' 
        OR Name = 'Road Bikes'
        OR Name = 'Touring Bikes');
GO
```

Esta instrucción no se puede convertir en una combinación. La combinación análoga "no igual" tiene un significado distinto: busca los nombres de todos los productos presentes en una subcategoría que no es la de bicicletas acabadas.      

### <a name="subqueries-in-update-delete-and-insert-statements"></a><a name="upsert"></a> Subconsultas en las instrucciones UPDATE, DELETE e INSERT
Las subconsultas se pueden anidar en las instrucciones de manipulación de datos (DML) `UPDATE`, `DELETE`, `INSERT` y `SELECT`.    

En el ejemplo siguiente se duplica el valor de la columna *ListPrice* en la tabla *Production.Product*. La subconsulta de la cláusula `WHERE` hace referencia a la tabla *Purchasing.ProductVendor* para limitar las filas que se actualizan en la tabla *Product* únicamente a las que proporciona *BusinessEntity* 1540.

```sql
USE AdventureWorks2016;
GO 
UPDATE Production.Product
SET ListPrice = ListPrice * 2
WHERE ProductID IN
    (SELECT ProductID 
     FROM Purchasing.ProductVendor
     WHERE BusinessEntityID = 1540);
GO
```

A continuación se muestra una instrucción `UPDATE` equivalente que usa una combinación:

```sql
USE AdventureWorks2016;
GO 
UPDATE Production.Product
SET ListPrice = ListPrice * 2
FROM Production.Product AS p
INNER JOIN Purchasing.ProductVendor AS pv
    ON p.ProductID = pv.ProductID AND BusinessEntityID = 1540;
GO   
```

### <a name="subqueries-with-comparison-operators"></a><a name="comparison"></a> Subconsultas con operadores de comparación
Las subconsultas se pueden presentar con uno de los operadores de comparación (=, < >, >, > =, <, ! >, ! < o < =).   

Una subconsulta precedida de un operador de comparación sin modificar (un operador de comparación no seguido de `ANY` o `ALL`) debe devolver un valor individual en lugar de una lista de valores, al igual que las subconsultas precedidas de `IN`. Si una subconsulta de este tipo devuelve más de un valor, SQL Server muestra un mensaje de error.    

Para usar una subconsulta presentada con un operador de comparación sin modificar, debe estar suficientemente familiarizado con los datos y con la naturaleza del problema para saber que la subconsulta devolverá exactamente un valor.     

Por ejemplo, si supone que cada vendedor solo cubre un territorio de ventas y desea localizar los clientes del territorio que cubre `Linda Mitchell`, puede escribir una instrucción con una subconsulta presentada con el operador de comparación =.    

```sql
USE AdventureWorks2016;
GO
SELECT CustomerID
FROM Sales.Customer
WHERE TerritoryID =
    (SELECT TerritoryID
     FROM Sales.SalesPerson
     WHERE BusinessEntityID = 276);
GO
```

Sin embargo, si `Linda Mitchell` cubre más de un territorio de ventas, se genera un mensaje de error. En lugar del operador de comparación =, se podría usar una formulación `IN` (= ANY también funciona).    

Las subconsultas presentadas con operadores de comparación sin modificar incluyen, a menudo, funciones de agregado, puesto que éstas devuelven un valor individual. Por ejemplo, la instrucción siguiente localiza los nombres de todos los productos cuya lista de precios es superior al precio medio de la lista.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >
    (SELECT AVG (ListPrice)
     FROM Production.Product);
GO
```     

Puesto que las subconsultas presentadas con operadores de comparación sin modificar deben devolver un valor individual, no pueden incluir cláusulas `GROUP BY` o `HAVING`, a menos que sepa que las propias cláusulas devuelven un valor individual. Por ejemplo, la siguiente consulta busca los productos con un precio superior al producto con el precio mínimo incluido en la subcategoría 14.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >
    (SELECT MIN (ListPrice)
     FROM Production.Product
     GROUP BY ProductSubcategoryID
     HAVING ProductSubcategoryID = 14);
GO
```

### <a name="comparison-operators-modified-by-any-some-or-all"></a><a name="comparison_modified"></a> Operadores de comparación modificados por ANY, SOME o ALL
Los operadores de comparación que presentan una subconsulta se pueden modificar mediante las palabras clave ALL o ANY. SOME es un equivalente del estándar ISO de `ANY`.     

Las subconsultas presentadas con un operador de comparación modificado devuelven una lista de cero o más valores y pueden incluir una cláusula `GROUP BY` o `HAVING`. Estas subconsultas se pueden formular con `EXISTS`.     

Si se usa como ejemplo el operador de comparación >, `>ALL` significa mayor que cualquier valor. Es decir, significa mayor que el valor máximo. Por ejemplo, `>ALL (1, 2, 3)` significa mayor que 3. `>ANY` significa mayor que por lo menos un valor; es decir, mayor que el mínimo. Entonces, `>ANY (1, 2, 3)` significa mayor que 1.
Para que una fila de una subconsulta con `>ALL` satisfaga la condición especificada en la consulta externa, el valor de la columna que presenta la subconsulta debe ser mayor que cada valor de la lista de los valores devueltos por la subconsulta.    

De forma parecida, `>ANY` significa que, para que una fila satisfaga la condición especificada en la consulta externa, el valor de la columna que presenta la subconsulta debe ser mayor que, como mínimo, uno de los valores de la lista devuelta por la subconsulta.    

La siguiente consulta proporciona un ejemplo de una subconsulta presentada con un operador de comparación modificado por ANY. Busca los productos cuyos precios de venta sean mayores o iguales que el precio de venta máximo de cualquier subcategoría de producto.    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >= ANY
    (SELECT MAX (ListPrice)
     FROM Production.Product
     GROUP BY ProductSubcategoryID);
GO
```    

Para cada subcategoría de producto, la consulta interna busca el precio de venta máximo. La consulta externa consulta todos estos valores y determina los precios de venta de cada producto que sean mayores o iguales que cualquier precio de venta máximo de la subcategoría de producto. Si `ANY` se cambia a `ALL`, la consulta devolverá solo los productos cuyos precios de venta sean mayores o iguales que todos los precios de venta devueltos en la consulta interna.    

Si la subconsulta no devuelve ningún valor, la consulta completa no puede devolver ningún valor.    

El operador `=ANY` equivale a `IN`. Por ejemplo, para buscar los nombres de todos los productos de ruedas que fabrica Adventure Works Cycles, puede usar `IN` o `=ANY`.

```sql
--Using =ANY
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID =ANY
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO

--Using IN
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```

Éste es el conjunto de resultados de las consultas:

```
Name
--------------------------------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```    

El operador `<>ANY`, sin embargo, difiere de `NOT IN`: `<>ANY` significa no = a, no = b o no = c. `NOT IN` significa no = a, no = b y no = c. `<>ALL` es lo mismo que `NOT IN`.     

Por ejemplo, la siguiente consulta busca los clientes ubicados en un territorio no cubierto por ningún vendedor.     

```sql
USE AdventureWorks2016;
GO
SELECT CustomerID
FROM Sales.Customer
WHERE TerritoryID <> ANY
    (SELECT TerritoryID
     FROM Sales.SalesPerson);
GO
```    

Los resultados incluyen todos los clientes, salvo aquellos cuyos territorios de ventas sean NULL, porque cada territorio asignado a un cliente está cubierto por un vendedor. La consulta interna busca todos los territorios de ventas cubiertos por los vendedores y, a continuación, para cada territorio, la consulta externa busca los clientes que no están en ninguna.    

Por el mismo motivo, cuando se utiliza `NOT IN` en esta consulta, los resultados no incluyen ningún cliente.      

Puede obtener los mismos resultados con el operador `<>ALL`, que es equivalente a `NOT IN`.   

### <a name="subqueries-with-exists"></a><a name="exists"></a> Subconsultas con EXISTS
Cuando una subconsulta se especifica con la palabra clave `EXISTS`, funciona como una prueba de existencia. La cláusula `WHERE` de la consulta externa comprueba si existen las filas devueltas por la subconsulta. En realidad, la subconsulta no produce ningún dato, devuelve el valor TRUE o FALSE.   

Una subconsulta que se especifica con EXISTS tiene la sintaxis siguiente:   

`WHERE [NOT] EXISTS (subquery)`    

La consulta siguiente busca los nombres de todos los productos presentes en la subcategoría Wheels:    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE EXISTS
    (SELECT * 
     FROM Production.ProductSubcategory
     WHERE ProductSubcategoryID = 
            Production.Product.ProductSubcategoryID
        AND Name = 'Wheels');
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]    

```
Name
--------------------------------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```    

Para comprender el resultado de esta consulta, considere el nombre de cada producto de uno en uno. ¿Este valor hace que la subconsulta devuelva como mínimo una fila? En otras palabras, ¿la consulta hace que la prueba de existencia se evalúe como TRUE?   

Observe que las subconsultas que se especifican con EXISTS son ligeramente distintas de las demás subconsultas en los aspectos siguientes: 
-   La palabra clave `EXISTS` no viene precedida de un nombre de columna, constante u otra expresión.     
-   La lista de selección de una subconsulta que se especifica con `EXISTS` casi siempre consta de un asterisco (*). No hay razón para enumerar los nombres de las columnas porque simplemente se está comprobando la existencia de filas que cumplan las condiciones especificadas en la subconsulta.    

La palabra clave `EXISTS` es importante, porque a menudo no hay una formulación alternativa sin subconsultas. Aunque algunas consultas creadas con EXISTS no se pueden expresar de otra forma, muchas consultas pueden usar IN o un operador de comparación modificado por `ANY` o `ALL` para lograr resultados similares.     

Por ejemplo, la consulta anterior se puede expresar con IN:   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```   

### <a name="subqueries-with-not-exists"></a><a name="notexists"></a> Subconsultas con NOT EXISTS
`NOT EXISTS`S funciona igual que `EXISTS`, con la diferencia de que la cláusula `WHERE` en la que se utiliza se cumple si la subconsulta no devuelve ninguna fila.    

Por ejemplo, para buscar los nombres de productos que no pertenecen a la subcategoría de ruedas:   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE NOT EXISTS
    (SELECT * 
     FROM Production.ProductSubcategory
     WHERE ProductSubcategoryID = 
            Production.Product.ProductSubcategoryID
        AND Name = 'Wheels');
GO
```   

### <a name="subqueries-used-in-place-of-an-expression"></a><a name="expression"></a> Subconsultas usadas en lugar de una expresión
En [!INCLUDE[tsql](../../includes/tsql-md.md)], una subconsulta se puede usar en el lugar en que se puede usar una expresión en las instrucciones `SELECT`, `UPDATE`, `INSERT` y `DELETE`, excepto en una lista `ORDER BY`.    

En el ejemplo siguiente se muestra cómo se puede usar esta mejora. Esta consulta busca los precios de todas las bicicletas de montaña, el precio medio y la diferencia entre el precio de cada bicicleta y el precio medio.    

```sql
USE AdventureWorks2016;
GO
SELECT Name, ListPrice, 
(SELECT AVG(ListPrice) FROM Production.Product) AS Average, 
    ListPrice - (SELECT AVG(ListPrice) FROM Production.Product)
    AS Difference
FROM Production.Product
WHERE ProductSubcategoryID = 1;
GO
```   

## <a name="see-also"></a>Consulte también  
[IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md)       
[EXISTS &#40;Transact-SQL&#41;](../../t-sql/language-elements/exists-transact-sql.md)     
[ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md)     
[SOME | ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md)     
[Combinaciones](../../relational-databases/performance/joins.md)    
[Operadores de comparación &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md)       
  
