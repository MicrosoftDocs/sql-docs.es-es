---
title: DECLARE @local_variable (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para usar la instrucción DECLARE para definir variables locales para su uso en un lote o un procedimiento.
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECLARE
- DECLARE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table-valued parameters
- variables [SQL Server], declaring
- DECLARE statement
- declaring variables
ms.assetid: d1635ebb-f751-4de1-8bbc-cae161f90821
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 46551c65a37fb4e2c5b98806835f9b0b0fb3701d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347565"
---
# <a name="declare-local_variable-transact-sql"></a>DECLARE @local_variable (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Las variables se declaran en el cuerpo de un proceso por lotes o un procedimiento con la instrucción DECLARE, y se les asignan valores con una instrucción SET o SELECT. Las variables de cursor pueden declararse con esta instrucción y utilizarse con otras instrucciones relacionadas con los cursores. Después de la declaración, todas las variables se inicializan como NULL, a menos que se proporcione un valor como parte de la declaración.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  
  
DECLARE   
{   
    { @local_variable [AS] data_type  [ = value ] }  
  | { @cursor_variable_name CURSOR }  
} [,...n]   
| { @table_variable_name [AS] <table_type_definition> }   
  
<table_type_definition> ::=   
     TABLE ( { <column_definition> | <table_constraint> } [ ,...n] )   
  
<column_definition> ::=   
     column_name { scalar_data_type | AS computed_column_expression }  
     [ COLLATE collation_name ]   
     [ [ DEFAULT constant_expression ] | IDENTITY [ (seed ,increment ) ] ]   
     [ ROWGUIDCOL ]   
     [ <column_constraint> ]   
  
<column_constraint> ::=   
     { [ NULL | NOT NULL ]   
     | [ PRIMARY KEY | UNIQUE ]   
     | CHECK ( logical_expression )   
     | WITH ( <index_option > )  
     }   
  
<table_constraint> ::=   
     { { PRIMARY KEY | UNIQUE } ( column_name [ ,...n] )   
     | CHECK ( search_condition )   
     }   
  
<index_option> ::=  
See CREATE TABLE for index option syntax.  
  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DECLARE   
{{ @local_variable [AS] data_type } [ =value [ COLLATE <collation_name> ] ] } [,...n]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
@*local_variable*  
 Es el nombre de una variable. Los nombres de variables deben comenzar con un signo de arroba (@). Los nombres de las variables locales deben respetar las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
*data_type*  
 Es cualquier tipo de tabla definido por el usuario CLR (Common Language Runtime) o tipo de datos de alias, suministrado por el sistema. Una variable no puede estar enlazada con los tipos de datos **text**, **ntext** o **image**.  
  
 Para más información sobre los tipos de datos del sistema, vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md). Para más información sobre los tipos de datos definidos por el usuario CLR o de alias, vea [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md).  
  
 =*value*  
 Asigna un valor a la variable en línea. El valor puede ser una constante o una expresión, pero debe coincidir con el tipo de declaración de la variable o poder convertirse implícitamente a ese tipo. Para obtener más información, vea [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
@*cursor_variable_name*  
 Es el nombre de una variable de cursor. Los nombres de variable de cursor deben comenzar con un signo de arroba (@) y seguir las reglas de los identificadores.  
  
CURSOR  
 Especifica que la variable es una variable de cursor local.  
  
@*table_variable_name*  
 Es el nombre de una variable de tipo **table**. Los nombres de variable deben comenzar con un signo de arroba (@) y seguir las reglas de los identificadores.  
  
<table_type_definition>  
Define el tipo de datos de **table**. La declaración de tabla incluye definiciones de columna, nombres, tipos de datos y restricciones. Solo se permiten los tipos de restricciones PRIMARY KEY, UNIQUE, NULL y CHECK. Un tipo de datos de alias no puede usarse como un tipo de datos de columna escalar si una regla o definición de valor predeterminado está enlazada al tipo.
  
\<table_type_definiton> Es un subconjunto de información que se usa para definir una tabla en CREATE TABLE. Aquí se incluyen los elementos y definiciones fundamentales. Para obtener más información, vea [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 *n*  
 Es un marcador de posición que indica que se pueden especificar y asignar valores a varias variables. Cuando se declara una variable de **table**, esta debe ser la única variable de **table** que se declara en la instrucción DECLARE.  
  
 *column_name*  
 Es el nombre de la columna de la tabla.  
  
 *scalar_data_type*  
 Especifica que la columna es de un tipo de datos escalar.  
  
 *computed_column_expression*  
 Es una expresión que define el valor de una columna calculada. Se calcula a partir de una expresión mediante otras columnas de la misma tabla. Por ejemplo, una columna calculada puede tener la definición **cost** AS **price \* qty**. La expresión puede ser un nombre de columna no calculada, una constante, una función integrada, una variable o cualquier combinación de estos elementos conectados mediante uno o más operadores. La expresión no puede ser una subconsulta o una función definida por el usuario. La expresión no puede hacer referencia a un tipo CLR definido por el usuario.  
  
 [ COLLATE *collation_name*]  
 Especifica la intercalación de la columna. *collation_name* puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL, y solo es aplicable a las columnas de los tipos de datos **char**, **varchar**, **text**, **nchar**, **nvarchar** y **ntext**. Si no se especifica, se asignará a la columna la intercalación del tipo de datos definido por el usuario, si la columna es de un tipo de datos definido por el usuario, o la intercalación de la base de datos actual.  
  
 Para obtener más información sobre los nombres de intercalación de Windows y SQL, vea [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md).  
  
 DEFAULT  
 Especifica el valor suministrado para la columna cuando no se ha especificado explícitamente un valor durante una inserción. Las definiciones DEFAULT se pueden aplicar a cualquier columna excepto a las definidas como **timestamp** o a aquellas que tengan la propiedad IDENTITY. Las definiciones DEFAULT desaparecen cuando se quita la tabla. Solo se puede utilizar un valor constante como valor predeterminado, por ejemplo, una cadena de caracteres, una función del sistema como SYSTEM_USER() o el valor NULL. Para mantener la compatibilidad con las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se puede asignar un nombre de restricción a DEFAULT.  
  
 *constant_expression*  
 Es una constante, el valor NULL o una función del sistema que se utiliza como el valor predeterminado de una columna.  
  
 IDENTITY  
 Indica que la nueva columna es una columna de identidad. Cuando se agrega una nueva fila a la tabla, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona un valor incremental único para la columna. Las columnas de identidad se utilizan normalmente junto con restricciones PRIMARY KEY para que actúen como identificador exclusivo de fila para la tabla. La propiedad IDENTITY se puede asignar a columnas **tinyint**, **smallint**, **int**, **decimal(p,0)** o **numeric(p,0)** . Solo se puede crear una columna de identidad para cada tabla. Las restricciones DEFAULT y los valores predeterminados enlazados no se pueden utilizar en las columnas de identidad. Se debe especificar los dos argumentos, seed e increment, o ninguno. Si no se especifica ninguno, el valor predeterminado es (1,1).  
  
 *seed*  
 Es el valor que se utiliza para la primera fila cargada en la tabla.  
  
 *increment*  
 Es el valor incremental que se agrega al valor de identidad de la fila cargada anteriormente.  
  
 ROWGUIDCOL  
 Indica que la nueva columna es una columna de identificador único global de fila. Solo se puede designar una columna **uniqueidentifier** por tabla como columna ROWGUIDCOL. La propiedad ROWGUIDCOL únicamente se puede asignar a una columna **uniqueidentifier**.  
  
 NULL | NOT NULL  
 Indica si NULL se permite en la variable. El valor predeterminado es NULL.  
  
 PRIMARY KEY  
 Es una restricción que exige la integridad de entidad para una o varias columnas dadas a través de un índice único. Solo se puede crear una restricción PRIMARY KEY para cada tabla.  
  
 UNIQUE  
 Es una restricción que proporciona la integridad de entidad para una o varias columnas dadas a través de un índice único. Las tablas pueden tener varias restricciones UNIQUE.  
  
 CHECK  
 Es una restricción que exige la integridad del dominio al limitar los valores posibles que se pueden escribir en una o varias columnas.  
  
 *logical_expression*  
 Es una expresión lógica que devuelve TRUE o FALSE.  
  
## <a name="remarks"></a>Observaciones  
 Las variables se suelen utilizar en un proceso por lotes o procedimiento como contadores para WHILE, LOOP o un bloque IF…ELSE.  
  
 Las variables solo se pueden usar en expresiones y no en lugar de nombres de objeto o palabras clave. Para formar instrucciones SQL dinámicas, utilice EXECUTE.  
  
 El alcance de una variable local es el lote en el que está declarada.  
 
 Una variable de tabla no tiene que estar residente en memoria necesariamente. En condiciones de presión de memoria, las páginas que pertenecen a una variable de tabla pueden enviarse a tempdb.
  
 Se puede hacer referencia como origen a una variable de cursor que actualmente tiene asignado un cursor en una instrucción:  
  
-   CLOSE.  
  
-   DEALLOCATE.  
  
-   FETCH.  
  
-   OPEN.  
  
-   DELETE o UPDATE por posición.  
  
-   SET CURSOR variable (en el lado derecho).  
  
 En todas estas instrucciones, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un error si la variable de cursor a la que se hace referencia existe pero actualmente no tiene asignado un cursor. Si una variable de cursor a la que se hace referencia no existe, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera el mismo error que genera para una variable no declarada de otro tipo.  
  
 Una variable de cursor:  
  
-   Puede ser el destino de un tipo de cursor u otra variable de cursor. Para más información, vea [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md).  
  
-   Se puede hacer referencia a la variable de cursor como el destino de un parámetro de cursor de salida en una instrucción EXECUTE si la variable de cursor no tiene actualmente un cursor asignado.  
  
-   Se debe considerar como un puntero al cursor.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-declare"></a>A. Usar DECLARE  
 En el ejemplo siguiente se utiliza una variable local denominada `@find` para recuperar información de contacto para todos los apellidos que comienzan por `Man`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @find VARCHAR(30);   
/* Also allowed:   
DECLARE @find VARCHAR(30) = 'Man%';   
*/  
SET @find = 'Man%';   
SELECT p.LastName, p.FirstName, ph.PhoneNumber  
FROM Person.Person AS p   
JOIN Person.PersonPhone AS ph ON p.BusinessEntityID = ph.BusinessEntityID  
WHERE LastName LIKE @find;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName            FirstName               Phone
------------------- ----------------------- -------------------------
Manchepalli         Ajay                    1 (11) 500 555-0174
Manek               Parul                   1 (11) 500 555-0146
Manzanares          Tomas                   1 (11) 500 555-0178
  
(3 row(s) affected)
```  
  
### <a name="b-using-declare-with-two-variables"></a>B. Usar DECLARE con dos variables  
 El ejemplo siguiente recupera los nombres de representantes de ventas de [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] que se encuentran en la zona de ventas de Norteamérica y tienen, como mínimo, $2.000.000 en ventas anuales.  
  
```sql  
USE AdventureWorks2012;  
GO  
SET NOCOUNT ON;  
GO  
DECLARE @Group nvarchar(50), @Sales MONEY;  
SET @Group = N'North America';  
SET @Sales = 2000000;  
SET NOCOUNT OFF;  
SELECT FirstName, LastName, SalesYTD  
FROM Sales.vSalesPerson  
WHERE TerritoryGroup = @Group and SalesYTD >= @Sales;  
```  
  
### <a name="c-declaring-a-variable-of-type-table"></a>C. Declarar una variable de tipo table  
 El ejemplo siguiente crea una variable `table` que almacena los valores especificados en la cláusula OUTPUT de la instrucción UPDATE. Las dos instrucciones `SELECT` que le siguen devuelven los valores en `@MyTableVar` y los resultados de la operación de actualización en la tabla `Employee`. Tenga en cuenta que los resultados de la columna `INSERTED.ModifiedDate` son diferentes de los valores de la columna `ModifiedDate` de la tabla `Employee`. Esto se debe a que el desencadenador `AFTER UPDATE`, que actualiza el valor de `ModifiedDate` a la fecha actual, se define en la tabla `Employee`. Sin embargo, las columnas que devuelve `OUTPUT` reflejan los datos anteriores a la activación de los desencadenadores. Para más información, vea [Cláusula OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE(  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25   
OUTPUT INSERTED.BusinessEntityID,  
       DELETED.VacationHours,  
       INSERTED.VacationHours,  
       INSERTED.ModifiedDate  
INTO @MyTableVar;  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
--Note that ModifiedDate reflects the value generated by an  
--AFTER UPDATE trigger.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```  
  
### <a name="d-declaring-a-variable-of-user-defined-table-type"></a>D. Declarar una variable de tipo de tabla definido por el usuario  
 El ejemplo siguiente crea un parámetro con valores de tabla o una variable de tabla denominada `@LocationTVP`. Esto requiere un tipo de tabla definido por el usuario correspondiente denominado `LocationTableType`. Para más información sobre cómo crear un tipo de tabla definido por el usuario, vea [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md). Para más información sobre los parámetros con valores de tabla, vea[Usar parámetros con valores de tabla &#40;motor de base de datos&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
```sql  
DECLARE @LocationTVP   
AS LocationTableType;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-declare"></a>E. Usar DECLARE  
 En el ejemplo siguiente se utiliza una variable local denominada `@find` para recuperar información de contacto para todos los apellidos que comienzan por `Walt`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @find VARCHAR(30);  
/* Also allowed:   
DECLARE @find VARCHAR(30) = 'Man%';  
*/  
SET @find = 'Walt%';  
  
SELECT LastName, FirstName, Phone  
FROM DimEmployee   
WHERE LastName LIKE @find;  
```  
  
### <a name="f-using-declare-with-two-variables"></a>F. Usar DECLARE con dos variables  
 En el siguiente ejemplo se recuperan variables de usuario para especificar el nombre y el apellido de los empleados de la tabla `DimEmployee`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @lastName VARCHAR(30), @firstName VARCHAR(30);  
  
SET @lastName = 'Walt%';  
SET @firstName = 'Bryan';  
  
SELECT LastName, FirstName, Phone  
FROM DimEmployee   
WHERE LastName LIKE @lastName AND FirstName LIKE @firstName;  
```  
  
## <a name="see-also"></a>Consulte también  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [table &#40;Transact-SQL&#41;](../../t-sql/data-types/table-transact-sql.md)   
 [Comparar XML con tipo y XML sin tipo](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)  
  
  




