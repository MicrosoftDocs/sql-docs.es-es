---
description: Constructor con valores de tabla (Transact-SQL)
title: Constructor con valores de tabla (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- inserting multiple rows
- row value expression
- row constructor [SQL Server]
- table value constructor [SQL Server]
ms.assetid: e57cd31d-140e-422f-8178-2761c27b9deb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 32074a8c0229ace53d636f8ef9bb31b8caf729b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200437"
---
# <a name="table-value-constructor-transact-sql"></a>Constructor con valores de tabla (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Especifica un conjunto de expresiones de valores de fila que se va a construir en una tabla. El constructor de valor de tabla de [!INCLUDE[tsql](../../includes/tsql-md.md)] permite que se especifiquen varias filas de datos en una sola instrucción DML. El constructor del valor de la tabla se puede especificar como la cláusula VALUES de una instrucción INSERT ... VALUES o como una tabla derivada en la cláusula USING de la instrucción MERGE o en la cláusula FROM.
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
VALUES ( <row value expression list> ) [ ,...n ]   
  
<row value expression list> ::=  
    {<row value expression> } [ ,...n ]  
  
<row value expression> ::=  
    { DEFAULT | NULL | expression }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 VALUES  
 Introduce las listas de expresión de los valores de las filas. Cada lista debe aparecer entre paréntesis y separarse mediante una coma.  
  
 El número de valores especificados en cada lista debe ser el mismo y los valores deben estar en el mismo orden que las columnas de la tabla. Se debe especificar un valor para cada columna de la tabla o la lista de columnas debe especificar explícitamente las columnas para cada valor entrante.  
  
 DEFAULT  
 Hace que [!INCLUDE[ssDE](../../includes/ssde-md.md)] inserte el valor predeterminado definido para una columna. Si no existe ningún valor predeterminado para la columna y esta admite valores NULL, se inserta NULL. DEFAULT no es un valor válido para una columna de identidad. Cuando se especifica en un constructor con valores de tabla, DEFAULT solo se permite en una instrucción INSERT.  
  
 *expression*  
 Es una constante, variable o expresión. La expresión no puede contener una instrucción EXECUTE.  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 Cuando se usa como una tabla derivada, no hay ningún límite en el número de filas.  
 
 Cuando se usa como la cláusula VALUES de una instrucción INSERT ... VALUES, el límite es de 1000 filas. Si el número de filas supera el máximo, se devuelve el error 10738. Para insertar más de 1000 filas, use uno de los métodos siguientes:  
  
- Crear varias instrucciones INSERT  
  
- Usar una tabla derivada  
  
- Importe en bloque los datos mediante la [utilidad **bcp**](../../tools/bcp-utility.md), la clase .NET [SqlBulkCopy](/dotnet/api/system.data.sqlclient.sqlbulkcopy), [OPENROWSET (BULK ...)](../../t-sql/functions/openrowset-transact-sql.md) o la instrucción [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)  
  
 Como expresión de valores de fila solo se permiten valores escalares. Como expresión de valores de fila no se permiten las subconsultas que impliquen a varias columnas. Por ejemplo, el código siguiente produce un error de sintaxis porque la tercera lista de expresiones de valores de fila contiene una subconsulta con varias columnas.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.MyProducts (Name VARCHAR(50), ListPrice MONEY);  
GO  
-- This statement fails because the third values list contains multiple columns in the subquery.  
INSERT INTO dbo.MyProducts (Name, ListPrice)  
VALUES ('Helmet', 25.50),  
       ('Wheel', 30.00),  
       (SELECT Name, ListPrice FROM Production.Product WHERE ProductID = 720);  
GO  
```  
  
 Sin embargo, la instrucción se puede reescribir especificando cada columna en la subconsulta independientemente. El ejemplo siguiente inserta correctamente tres filas en la tabla `MyProducts`.  
  
```sql
INSERT INTO dbo.MyProducts (Name, ListPrice)  
VALUES ('Helmet', 25.50),  
       ('Wheel', 30.00),  
       ((SELECT Name FROM Production.Product WHERE ProductID = 720),  
        (SELECT ListPrice FROM Production.Product WHERE ProductID = 720));  
GO  
```  
  
## <a name="data-types"></a>Tipo de datos  
 Los valores especificados en una instrucción INSERT de varias filas siguen las propiedades de conversión de tipos de datos de la sintaxis de UNION ALL. Esto produce la conversión implícita de tipos no coincidentes al tipo de [precedencia](../../t-sql/data-types/data-type-precedence-transact-sql.md) superior. Si la conversión no es una conversión implícita admitida, se devuelve un error. Por ejemplo, la instrucción siguiente inserta un valor entero y un valor de carácter en una columna de tipo **char**.  
  
```sql
CREATE TABLE dbo.t (a INT, b CHAR);  
GO  
INSERT INTO dbo.t VALUES (1,'a'), (2, 1);  
GO  
```  
  
 Cuando se ejecuta la instrucción INSERT, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intenta convertir 'a' en un entero porque la precedencia del tipo de datos indica que un entero pertenece a un tipo más alto que un carácter. Se produce un error en la conversión y se devuelve un error. Para evitar este error, puede convertir explícitamente los valores según corresponda. Por ejemplo, la instrucción anterior puede escribirse del siguiente modo.  
  
```sql
INSERT INTO dbo.t VALUES (1,'a'), (2, CONVERT(CHAR,1));  
```  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-inserting-multiple-rows-of-data"></a>A. Insertar varias filas de datos  
 En el siguiente ejemplo se crea la tabla `dbo.Departments` y, a continuación, se utiliza el constructor de valor de tabla para insertar cinco filas en la tabla. Dado que los valores para todas las columnas se suministran e incluyen en el mismo orden que las columnas de la tabla, no es necesario especificar los nombres de columna en la lista de columnas.  
  
```sql
USE AdventureWorks2012;  
GO  
INSERT INTO Production.UnitMeasure  
VALUES (N'FT2', N'Square Feet ', '20080923'), (N'Y', N'Yards', '20080923'),
       (N'Y3', N'Cubic Yards', '20080923');  
GO  
```  
  
### <a name="b-inserting-multiple-rows-with-default-and-null-values"></a>B. Insertar varias filas con los valores DEFAULT y NULL  
 En el siguiente ejemplo se demuestra cómo especificar DEFAULT y NULL cuando se utiliza el constructor de valor de tabla para insertar filas en una tabla.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE Sales.MySalesReason(  
SalesReasonID int IDENTITY(1,1) NOT NULL,  
Name dbo.Name NULL ,  
ReasonType dbo.Name NOT NULL DEFAULT 'Not Applicable' );  
GO  
INSERT INTO Sales.MySalesReason   
VALUES ('Recommendation','Other'), ('Advertisement', DEFAULT), (NULL, 'Promotion');  
  
SELECT * FROM Sales.MySalesReason;  
```  
  
### <a name="c-specifying-multiple-values-as-a-derived-table-in-a-from-clause"></a>C. Especificar varios valores como una tabla derivada en una cláusula FROM  
 En los siguientes ejemplos se usa el constructor con valores de tabla para especificar varios valores en la cláusula FROM de una instrucción SELECT.  
  
```sql
SELECT a, b FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);  
GO  
-- Used in an inner join to specify values to return.  
SELECT ProductID, a.Name, Color  
FROM Production.Product AS a  
INNER JOIN (VALUES ('Blade'), ('Crown Race'), ('AWC Logo Cap')) AS b(Name)   
ON a.Name = b.Name;  
```  
  
### <a name="d-specifying-multiple-values-as-a-derived-source-table-in-a-merge-statement"></a>D. Especificar varios valores como una tabla de origen derivada en una instrucción MERGE  
 En el ejemplo siguiente se usa MERGE para modificar la tabla `SalesReason`, actualizando o insertando las filas. Cuando el valor de `NewName` de la tabla de origen coincide con un valor de la columna `Name` de la tabla de destino, (`SalesReason`), la columna `ReasonType` se actualiza en la tabla de destino. Cuando el valor de `NewName` no coincide, la fila del origen se inserta en la tabla de destino. La tabla de origen es una tabla derivada que usa la característica de constructor con valores de tabla de [!INCLUDE[tsql](../../includes/tsql-md.md)] para especificar varias filas en la tabla de origen.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Create a temporary table variable to hold the output actions.  
DECLARE @SummaryOfChanges TABLE(Change VARCHAR(20));  
  
MERGE INTO Sales.SalesReason AS Target  
USING (VALUES ('Recommendation','Other'), ('Review', 'Marketing'), ('Internet', 'Promotion'))  
       AS Source (NewName, NewReasonType)  
ON Target.Name = Source.NewName  
WHEN MATCHED THEN  
UPDATE SET ReasonType = Source.NewReasonType  
WHEN NOT MATCHED BY TARGET THEN  
INSERT (Name, ReasonType) VALUES (NewName, NewReasonType)  
OUTPUT $action INTO @SummaryOfChanges;  
  
-- Query the results of the table variable.  
SELECT Change, COUNT(*) AS CountPerChange  
FROM @SummaryOfChanges  
GROUP BY Change;  
```  
  
### <a name="e-inserting-more-than-1000-rows"></a>E. Insertar más de 1000 filas
  En el ejemplo siguiente se muestra cómo utilizar el constructor con valores de tabla como una tabla derivada. Esto permite insertar más de 1000 filas desde un constructor con valores de tabla única.
  
```sql
CREATE TABLE dbo.Test ([Value] INT);  
  
INSERT INTO dbo.Test ([Value])  
  SELECT drvd.[NewVal]
  FROM   (VALUES (0), (1), (2), (3), ..., (5000)) drvd([NewVal]);
```  


## <a name="see-also"></a>Consulte también  
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
  
