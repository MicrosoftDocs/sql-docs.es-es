---
description: 'Operadores de conjuntos: UNION (Transact-SQL)'
title: UNION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UNION
- UNION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UNION queries
- combining query results
- UNION operator [SQL Server]
ms.assetid: 607c296f-8a6a-49bc-975a-b8d0c0914df7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9214d0aff36406604b64ff8b642e1c1f56b136c8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158913"
---
# <a name="set-operators---union-transact-sql"></a>Operadores de conjuntos: UNION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Concatena los resultados de dos consultas en un único conjunto de resultados. Puede controlar si en el conjunto de resultados se incluyen filas duplicadas:

- **UNION ALL**: incluye duplicados.
- **UNION**: se excluyen los duplicados.

Una operación de **UNION** es distinta de una operación de **[JOIN](../queries/from-transact-sql.md)** :

- Una operación de **UNION** concatena conjuntos de resultados de dos consultas. Pero una operación de **UNION** no crea filas individuales de columnas obtenidas de dos tablas.
- Una operación de **JOIN** compara las columnas de dos tablas para crear filas de resultados compuestas de columnas de las dos tablas.
  
A continuación, se muestran las reglas básicas para combinar los conjuntos de resultados de dos consultas con **UNION**:  
  
-   El número y el orden de las columnas debe ser el mismo en todas las consultas.  
  
-   Los tipos de datos deben ser compatibles.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
{ <query_specification> | ( <query_expression> ) }   
{ UNION [ ALL ]   
  { <query_specification> | ( <query_expression> ) } 
  [ ...n ] }
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
\<query_specification> | ( \<query_expression> ) Es una especificación de consulta o una expresión de consulta que devuelve datos que se combinarán con los datos de otra especificación de consulta o expresión de consulta. No es preciso que las definiciones de las columnas que forman parte de una operación UNION sean iguales, pero deben ser compatibles en una conversión implícita. Cuando los tipos de datos difieren, el tipo de datos resultante se determina según las reglas de [prioridad de tipo de datos](../../t-sql/data-types/data-type-precedence-transact-sql.md). Cuando los tipos son los mismos pero varían en cuanto a precisión, escala o longitud, el resultado se basa en las mismas reglas para combinar expresiones. Para obtener más información, vea [Precisión, escala y longitud (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
Las columnas con el tipo de datos **xml** deben ser iguales. Todas las columnas deben tener un tipo de esquema XML o no tener tipo. Si tienen tipo, debe ser el de la misma colección de esquemas XML.  
  
UNION  
Especifica que se deben combinar varios conjuntos de resultados para ser devueltos como un solo conjunto de resultados.  
  
ALL  
Incorpora todas las filas a los resultados, incluidos los duplicados. Si no se especifica, las filas duplicadas se quitan.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-a-simple-union"></a>A. Usar una instrucción UNION simple  
En el ejemplo siguiente, el conjunto de resultados incluye el contenido de las columnas `ProductModelID` y `Name` de las tablas `ProductModel` y `Gloves`.  
 
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Here is the simple union.  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="b-using-select-into-with-union"></a>B. Usar SELECT INTO con UNION  
En el ejemplo siguiente, la cláusula `INTO` de la segunda instrucción `SELECT` especifica que la tabla denominada `ProductResults` contiene el conjunto final de resultados de la unión de las columnas seleccionadas de las tablas `ProductModel` y `Gloves`. La tabla `Gloves` se crea en la primera instrucción `SELECT`.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.ProductResults', 'U') IS NOT NULL  
DROP TABLE dbo.ProductResults;  
GO  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
INTO dbo.ProductResults  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
SELECT ProductModelID, Name   
FROM dbo.ProductResults;  
```  
  
### <a name="c-using-union-of-two-select-statements-with-order-by"></a>C. Usar UNION con dos instrucciones SELECT y ORDER BY  
El orden de algunos parámetros empleados con la cláusula UNION es importante. En el ejemplo siguiente se muestra el uso correcto e incorrecto de `UNION` en dos instrucciones `SELECT` en las que se va a cambiar el nombre de una columna en el resultado.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
/* INCORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
ORDER BY Name  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
/* CORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="d-using-union-of-three-select-statements-to-show-the-effects-of-all-and-parentheses"></a>D. Usar UNION de tres instrucciones SELECT para mostrar los efectos de ALL y los paréntesis  
En los siguientes ejemplos se utiliza `UNION` para combinar los resultados de tres tablas que tienen las mismas 5 filas de datos. En el primer ejemplo se utiliza `UNION ALL` para mostrar los registros duplicados y se devuelven las 15 filas. En el segundo ejemplo se utiliza `UNION` sin `ALL` para eliminar las filas duplicadas de los resultados combinados de las tres instrucciones `SELECT` y se devuelven 5 filas.  
  
En el tercer ejemplo, se utiliza `ALL` con la primera operación `UNION` y los paréntesis incluyen la segunda cláusula `UNION` que no utiliza `ALL`. La segunda operación `UNION` se procesa en primer lugar porque se encuentra entre paréntesis. Devuelve 5 filas porque no se utiliza la opción `ALL` y se quitan los duplicados. Estas 5 filas se combinan con los resultados del primer `SELECT` mediante las palabras clave `UNION ALL`. En este ejemplo no se quitan los duplicados entre los dos conjuntos de cinco filas. El resultado final es de 10 filas.  
  
```sql
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.EmployeeOne', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeOne;  
GO  
IF OBJECT_ID ('dbo.EmployeeTwo', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeTwo;  
GO  
IF OBJECT_ID ('dbo.EmployeeThree', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeThree;  
GO  
  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeOne  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeTwo  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeThree  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
-- Union ALL  
SELECT LastName, FirstName, JobTitle  
FROM dbo.EmployeeOne  
UNION ALL  
SELECT LastName, FirstName ,JobTitle  
FROM dbo.EmployeeTwo  
UNION ALL  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle  
FROM dbo.EmployeeOne  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeOne  
UNION ALL  
(  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree  
);  
GO  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-a-simple-union"></a>E. Usar una instrucción UNION simple  
En el ejemplo siguiente, el conjunto de resultados incluye el contenido de las columnas `CustomerKey` de las tablas `FactInternetSales` y `DimCustomer`. Puesto que no se usa la palabra clave ALL, los duplicados se excluyen de los resultados.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="f-using-union-of-two-select-statements-with-order-by"></a>F. Usar UNION con dos instrucciones SELECT y ORDER BY  
 Cuando una instrucción SELECT en una instrucción UNION incluye una cláusula ORDER BY, esa cláusula se debe colocar detrás de todas las instrucciones SELECT. En el ejemplo siguiente se muestra el uso correcto e incorrecto de `UNION` en dos instrucciones `SELECT` en las que se ordena una columna con ORDER BY.  
  
```sql
-- Uses AdventureWorks  
  
-- INCORRECT  
SELECT CustomerKey   
FROM FactInternetSales    
ORDER BY CustomerKey  
UNION   
SELECT CustomerKey   
FROM DimCustomer  
ORDER BY CustomerKey;  
  
-- CORRECT   
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="g-using-union-of-two-select-statements-with-where-and-order-by"></a>G. Uso de UNION de dos instrucciones SELECT con WHERE y ORDER BY  
En el ejemplo siguiente se muestra el uso correcto e incorrecto de `UNION` en dos instrucciones `SELECT` en las que se necesita WHERE y ORDER BY.  
  
```sql
-- Uses AdventureWorks  
  
-- INCORRECT   
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
ORDER BY CustomerKey   
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
  
-- CORRECT  
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="h-using-union-of-three-select-statements-to-show-effects-of-all-and-parentheses"></a>H. Uso de UNION de tres instrucciones SELECT para mostrar los efectos de ALL y los paréntesis  
En los ejemplos siguientes, se usa `UNION` para combinar los resultados de **la misma tabla** para mostrar los efectos de ALL y los paréntesis al usar `UNION`.  
  
En el primer ejemplo se usa `UNION ALL` para mostrar los registros duplicados y se devuelve tres veces cada fila de la tabla de origen. En el segundo ejemplo se usa `UNION` sin `ALL` para eliminar las filas duplicadas de los resultados combinados de las tres instrucciones `SELECT` y solo se devuelven las filas sin duplicar de la tabla de origen.  
  
En el tercer ejemplo, se usa `ALL` con la primera operación `UNION` y los paréntesis para incluir la segunda operación `UNION` que no usa `ALL`. La segunda `UNION` se procesa en primer lugar porque está entre paréntesis. Solo devuelve las filas sin duplicar de la tabla porque no se usa la opción `ALL` y los duplicados se quitan. Estas filas se combinan con los resultados de la primera instrucción `SELECT` mediante las palabras clave `UNION ALL`. En este ejemplo, no se quitan los duplicados entre los dos conjuntos.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL  
(  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
);  
```  
  
## <a name="see-also"></a>Consulte también  
[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
[Ejemplos de SELECT (Transact-SQL)](../../t-sql/queries/select-examples-transact-sql.md)  
