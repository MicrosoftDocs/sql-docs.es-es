---
description: table (Transact-SQL)
title: table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- table data type [SQL Server]
- table variables [SQL Server]
ms.assetid: 1ef0b60e-a64c-4e97-847b-67930e3973ef
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4ed393c00500c0f195d9552daf9bf6414db1da88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179707"
---
# <a name="table-transact-sql"></a>table (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Es un tipo de datos especial usado para almacenar un conjunto de resultados y procesarlo en otro momento. **table** se usa principalmente para almacenar temporalmente un conjunto de filas que se devuelven como el conjunto de resultados de la función con valores de tabla. Las funciones y las variables se pueden declarar como de tipo **table**. Las variables **table** se pueden usar en funciones, procedimientos almacenados y lotes. Para declarar variables de tipo **table**, use [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md).
  

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
table_type_definition ::=   
    TABLE ( { <column_definition> | <table_constraint> } [ ,...n ] )   
  
<column_definition> ::=   
    column_name scalar_data_type   
    [ COLLATE <collation_definition> ]   
    [ [ DEFAULT constant_expression ] | IDENTITY [ ( seed , increment ) ] ]   
    [ ROWGUIDCOL ]   
    [ column_constraint ] [ ...n ]   
  
 <column_constraint> ::=   
    { [ NULL | NOT NULL ]   
    | [ PRIMARY KEY | UNIQUE ]   
    | CHECK ( logical_expression )   
    }   
  
<table_constraint> ::=   
     { { PRIMARY KEY | UNIQUE } ( column_name [ ,...n ] )  
     | CHECK ( logical_expression )   
     }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*table_type_definition*  
Es el mismo subconjunto de información que se utiliza para definir una tabla en CREATE TABLE. La declaración de tabla incluye definiciones de columna, nombres, tipos de datos y restricciones. Solo se permiten los tipos de restricciones PRIMARY KEY, UNIQUE KEY y NULL.  
Para saber más sobre la sintaxis, vea [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md), [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md) y [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).
  
*collation_definition*  
Es la intercalación de la columna que consta de una configuración regional de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows y un estilo de comparación, una configuración regional de Windows y la notación binaria o una intercalación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si no se especifica *collation_definition*, la columna hereda la intercalación de la base de datos actual. Si la columna se ha definido como un tipo definido por el usuario CLR (Common Language Runtime), la columna hereda la intercalación del tipo definido por el usuario.
  
## <a name="remarks"></a>Observaciones  
Haga referencia a las variables **table** por el nombre en una cláusula FROM del lote, como se muestra en el ejemplo siguiente:
  
```sql
SELECT Employee_ID, Department_ID FROM @MyTableVar;  
```  
  
Fuera de una cláusula FROM, se debe hacer referencia a las variables **table** mediante un alias, como se muestra en este ejemplo:
  
```sql
SELECT EmployeeID, DepartmentID   
FROM @MyTableVar m  
JOIN Employee on (m.EmployeeID =Employee.EmployeeID AND  
   m.DepartmentID = Employee.DepartmentID);  
```  
  
Las variables **table** proporcionan las siguientes ventajas para consultas a pequeña escala que tienen planes de consulta invariables y cuando la recompilación es un tema importante:
-   Una variable **table** se comporta como una variable local. Tiene un ámbito bien definido. Esta variable se puede usar en la función, el procedimiento almacenado o el lote en el que se declare.  
     Dentro de su ámbito, la variable **table** se puede usar como una tabla normal. Puede aplicarse en cualquier lugar de las instrucciones SELECT, INSERT, UPDATE y DELETE donde se utilice una tabla o expresión de tabla. Sin embargo, **table** no puede usarse en la siguiente instrucción:  
  
```sql
SELECT select_list INTO table_variable;
```
  
Las variables **table** se limpian automáticamente al final de la función, el procedimiento almacenado o el lote en que se definen.
  
-   Cuando no hay elecciones basadas en el costo que afecten al rendimiento, las variables **table** usadas en procedimientos almacenados provocan menos recompilaciones de procedimientos almacenados que cuando se usan tablas temporales.  
-   Las transacciones con variables **table** existen solo mientras dura una actualización en la variable **table**. Por tanto, las variables **table** requieren menos recursos de registro y bloqueo.  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones
Las variables **table** no tienen estadísticas de distribución. No desencadenan nuevas compilaciones. En muchos casos, el optimizador compilará un plan de consulta sobre el supuesto de que la variable de tabla no tiene filas. Por este motivo, las variables table deben usarse con precaución si se espera una gran cantidad de filas (más de 100). En estos casos, las tablas Temp pueden representar una mejor solución. Para las consultas que se unen a la variable de tabla con otras tablas, use la sugerencia RECOMPILE, que hará que el optimizador use la cardinalidad correcta para esta variable.
  
Las variables **table** no se admiten en el modelo de razonamiento basado en costos del optimizador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Por lo tanto, no se deben usar cuando se requieren elecciones basadas en el costo para lograr un plan de consultas eficaz. Se prefieren las tablas temporales cuando se requieren opciones basadas en costos. Este plan incluye normalmente consultas con uniones, decisiones de paralelismo y opciones de selección de índice.
  
Las consultas que modifican variables **table** no generan planes de ejecución de consultas en paralelo. El rendimiento puede verse afectado cuando se modifican variables **table** muy grandes o variables **table** en consultas complejas. Considere la posibilidad de usar tablas temporales en situaciones donde se modifican las variables **table**. Para obtener más información, vea [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md). Las consultas que leen variables **table** sin modificarlas pueden generarse en paralelo.

> [!IMPORTANT]
> El nivel de compatibilidad de la base de datos 150 mejora el rendimiento de las variables de tabla con la introducción de la **compilación diferida de variables de tabla**.  Para obtener más información, consulte [Compilación diferida de variables de tabla](../../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation).
>
  
En las variables **table** no se pueden crear índices de forma explícita; en estas variables **table** tampoco se conserva ninguna estadística. A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], se introdujo una sintaxis nueva que le permite crear determinado tipos de índice alineados con la definición de tabla.  Con esta nueva sintaxis, puede crear índices en las variables de **tabla** como parte de la definición de tabla. En determinados casos, el rendimiento puede mejorar si en su lugar se utilizan tablas temporales, las que proporcionan estadísticas y compatibilidad total del índice. Para más información sobre la creación de índices alineados y las tablas temporales, consulte[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).

Las restricciones CHECK, los valores DEFAULT y las columnas calculadas de la declaración de tipos **table** no pueden llamar a funciones definidas por el usuario.
  
No se permite la operación de asignación entre variables **table**.
  
Como las variables **table** tienen ámbito limitado y no son parte de la base de datos persistente, las operaciones de reversión de transacciones no les afectan.
  
Las variables table no se pueden modificar una vez creadas.
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-declaring-a-variable-of-type-table"></a>A. Declarar una variable de tipo table  
El ejemplo siguiente crea una variable `table` que almacena los valores especificados en la cláusula OUTPUT de la instrucción UPDATE. Las dos instrucciones `SELECT` que le siguen devuelven los valores en `@MyTableVar` y los resultados de la operación de actualización en la tabla `Employee`. Los resultados de la columna `INSERTED.ModifiedDate` son diferentes de los valores de la columna `ModifiedDate` de la tabla `Employee`. Esta diferencia se debe a que el desencadenador `AFTER UPDATE`, que actualiza el valor de `ModifiedDate` con la fecha actual, se define en la tabla `Employee`. Sin embargo, las columnas que devuelve `OUTPUT` reflejan los datos anteriores a la activación de los desencadenadores. Para más información, vea [Cláusula OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar table(  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate datetime);  
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
  
### <a name="b-creating-an-inline-table-valued-function"></a>B. Crear una función alineada con valores de tabla  
En el siguiente ejemplo se devuelve una función alineada con valores de tabla. Devuelve tres columnas `ProductID`, `Name` y el agregado de ventas totales anuales hasta la fecha por tienda como `YTD Total` para cada producto vendido a la tienda.
  
```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Sales.ufn_SalesByStore', N'IF') IS NOT NULL  
    DROP FUNCTION Sales.ufn_SalesByStore;  
GO  
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
    FROM Production.Product AS P   
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
    WHERE C.StoreID = @storeid  
    GROUP BY P.ProductID, P.Name  
);  
GO  
```  
  
Para invocar la función, ejecute esta consulta.
  
```sql
SELECT * FROM Sales.ufn_SalesByStore (602);  
```  
  
## <a name="see-also"></a>Consulte también
[COLLATE &#40;Transact-SQL&#41;](../statements/collations.md)  
[CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)  
[Funciones definidas por el usuario](../../relational-databases/user-defined-functions/user-defined-functions.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[Usar parámetros con valores de tabla &#40;motor de base de datos&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)  
[Sugerencias de consulta &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)