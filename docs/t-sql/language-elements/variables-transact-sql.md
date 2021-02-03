---
description: Variables (Transact-SQL)
title: Variables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: f372ae86-a003-40af-92de-fa52e3eea13f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 316cf7e5d62a651d3495d04ce34da450b781344b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184683"
---
# <a name="variables-transact-sql"></a>Variables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Una variable local de Transact-SQL es un objeto que contiene un valor individual de datos de un tipo específico. Normalmente, las variables se utilizan en lotes y scripts: 

* Como contadores, para contar el número de veces que se realiza un bucle o controlar cuántas veces debe ejecutarse.
* Para contener un valor de datos que desea probar mediante una instrucción de control de flujo.
* Para guardar el valor de un dato que se va a devolver en un código de retorno de un procedimiento almacenado o un valor devuelto de una función.

> [!NOTE]
> - Los nombres de algunas funciones del sistema Transact-SQL comienzan por dos *arrobas* (\@\@). A pesar de que en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se hacía referencia a las funciones que empiezan por \@\@ como variables globales, estas funciones \@\@ no son variables y no tienen el mismo comportamiento. Las funciones que empiezan por \@\@ son funciones del sistema, y el uso de su sintaxis sigue las reglas de las funciones.
> - No se pueden usar variables en una vista.
> - Los cambios en las variables no se ven afectados por la reversión de una transacción.

Este script crea una pequeña tabla de prueba y la rellena con 26 filas. El script usa una variable para hacer tres cosas: 

* Controlar cuántas filas se insertan al comprobar cuántas veces se ejecuta el bucle.
* Suministrar el valor insertado en la columna de enteros.
* Funcionar como parte de la expresión que genera cartas que se van a insertar en la columna de caracteres.  

```sql
-- Create the table.
CREATE TABLE TestTable (cola INT, colb CHAR(3));
GO
SET NOCOUNT ON;
GO
-- Declare the variable to be used.
DECLARE @MyCounter INT;

-- Initialize the variable.
SET @MyCounter = 0;

-- Test the variable to see if the loop is finished.
WHILE (@MyCounter < 26)
BEGIN;
   -- Insert a row into the table.
   INSERT INTO TestTable VALUES
       -- Use the variable to provide the integer value
       -- for cola. Also use it to generate a unique letter
       -- for each row. Use the ASCII function to get the
       -- integer value of 'a'. Add @MyCounter. Use CHAR to
       -- convert the sum back to the character @MyCounter
       -- characters after 'a'.
       (@MyCounter,
        CHAR( ( @MyCounter + ASCII('a') ) )
       );
   -- Increment the variable to count this iteration
   -- of the loop.
   SET @MyCounter = @MyCounter + 1;
END;
GO
SET NOCOUNT OFF;
GO
-- View the data.
SELECT cola, colb
FROM TestTable;
GO
DROP TABLE TestTable;
GO
```

## <a name="declaring-a-transact-sql-variable"></a>Declarar una variable de Transact-SQL
La instrucción DECLARE inicializa una variable de Transact-SQL al: 
* Asignar un nombre. El nombre debe tener un único \@ como primer carácter.
* Asignar un tipo de datos suministrado por el sistema o definido por el usuario y una longitud. Para las variables numéricas, se asignan también una precisión y una escala. Para las variables del tipo XML, puede asignarse una colección de esquemas opcional.
* Establecer el valor a NULL.

Por ejemplo, la siguiente instrucción **DECLARE** crea una variable local llamada **\@mycounter** con un tipo de datos int.  
```sql
DECLARE @MyCounter INT;
```
Para declarar más de una variable local, use una coma después de la primera variable local definida y, a continuación, especifique el nombre y tipo de datos de la siguiente variable local.

Por ejemplo, la siguiente instrucción **DECLARE** crea tres variables locales llamadas **\@LastName**, **\@FirstName** y **\@StateProvince**, e inicializa cada una de ellas a NULL:  
```sql
DECLARE @LastName NVARCHAR(30), @FirstName NVARCHAR(20), @StateProvince NCHAR(2);
```

El ámbito de una variable es el conjunto de instrucciones de Transact-SQL que pueden hacer referencia a la variable. El ámbito de una variable se extiende desde el punto en el que se declara hasta el final del lote o procedimiento almacenado en el que se ha declarado. Por ejemplo, el siguiente script genera un error de sintaxis porque la variable se declara en un lote y se hace referencia a la misma en otro:  
```sql
USE AdventureWorks2014;
GO
DECLARE @MyVariable INT;
SET @MyVariable = 1;
-- Terminate the batch by using the GO keyword.
GO 
-- @MyVariable has gone out of scope and no longer exists.

-- This SELECT statement generates a syntax error because it is
-- no longer legal to reference @MyVariable.
SELECT BusinessEntityID, NationalIDNumber, JobTitle
FROM HumanResources.Employee
WHERE BusinessEntityID = @MyVariable;
```

Las variables tienen un ámbito local y solamente están visibles dentro del lote o procedimiento en las que están definidas. En el siguiente ejemplo, el ámbito anidado creado para la ejecución de sp_executesql no tiene acceso a la variable declarada en un ámbito superior devuelve un error.  

```sql
DECLARE @MyVariable INT;
SET @MyVariable = 1;
EXECUTE sp_executesql N'SELECT @MyVariable'; -- this produces an error
```

## <a name="setting-a-value-in-a-transact-sql-variable"></a>Establecer un valor en una variable de Transact-SQL

Cuando una variable se declara por primera vez, su valor se establece en NULL. Para asignar un valor a una variable, use la instrucción SET. Éste es el método preferido para asignar un valor a una variable. También se puede asignar un valor a una variable si se hace referencia a ella en la lista de selección de una instrucción SELECT.

Para asignar un valor a una variable mediante la instrucción SET, incluya el nombre de la variable y el valor que desea asignar a la misma. Éste es el método preferido para asignar un valor a una variable. Por ejemplo, en el lote siguiente se declaran dos variables, se les asigna un valor y, a continuación, se utilizan en la cláusula `WHERE` de una instrucción `SELECT`:  

```sql
USE AdventureWorks2014;
GO
-- Declare two variables.
DECLARE @FirstNameVariable NVARCHAR(50),
   @PostalCodeVariable NVARCHAR(15);

-- Set their values.
SET @FirstNameVariable = N'Amy';
SET @PostalCodeVariable = N'BA5 3HX';

-- Use them in the WHERE clause of a SELECT statement.
SELECT LastName, FirstName, JobTitle, City, StateProvinceName, CountryRegionName
FROM HumanResources.vEmployee
WHERE FirstName = @FirstNameVariable
   OR PostalCode = @PostalCodeVariable;
GO
```

También se puede asignar un valor a una variable si se hace referencia a la misma en una lista de selección. Cuando se hace referencia a una variable en una lista de selección, se le debe asignar un valor escalar o la instrucción SELECT solo debe devolver una fila. Por ejemplo:  

```sql
USE AdventureWorks2014;
GO
DECLARE @EmpIDVariable INT;

SELECT @EmpIDVariable = MAX(EmployeeID)
FROM HumanResources.Employee;
GO
```

> [!WARNING]
> Si hay varias cláusulas de asignación en una sola instrucción SELECT, SQL Server no garantiza el orden de evaluación de las expresiones. Tenga en cuenta que los efectos solo están visibles si existen referencias entre las asignaciones.

Si una instrucción SELECT devuelve más de una fila y la variable hace referencia a una expresión no escalar, la variable se establece en el valor devuelto para la expresión en la última fila del conjunto de resultados. Por ejemplo, en el siguiente lote, **\@EmpIDVariable** se establece en el valor de **BusinessEntityID** de la última fila devuelta, que es 1:  

```sql
USE AdventureWorks2014;
GO
DECLARE @EmpIDVariable INT;

SELECT @EmpIDVariable = BusinessEntityID
FROM HumanResources.Employee
ORDER BY BusinessEntityID DESC;

SELECT @EmpIDVariable;
GO
```

## <a name="see-also"></a>Vea también  
 [Declare @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
 [SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
 [SELECT @local_variable](../../t-sql/language-elements/select-local-variable-transact-sql.md)  
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operadores compuestos &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
  
  
