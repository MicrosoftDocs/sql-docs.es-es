---
description: DELETE (Transact-SQL)
title: DELETE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DELETE
- DELETE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row removal [SQL Server]
- DELETE statement [SQL Server], about DELETE statement
- views [SQL Server], deleting rows
- removing rows
- tables [SQL Server], deleting rows
- DELETE statement [SQL Server]
- deleting rows
- row removal [SQL Server], DELETE statement
- deleting data
ms.assetid: ed6b2105-0f35-408f-ba51-e36ade7ad5b2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 47dcac82e79c2b7d839a7072c995b252c0b45e8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177871"
---
# <a name="delete-transact-sql"></a>DELETE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Quita una o varias filas de una tabla o vista de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ WITH <common_table_expression> [ ,...n ] ]  
DELETE   
    [ TOP ( expression ) [ PERCENT ] ]   
    [ FROM ]   
    { { table_alias  
      | <object>   
      | rowset_function_limited   
      [ WITH ( table_hint_limited [ ...n ] ) ] }   
      | @table_variable  
    }  
    [ <OUTPUT Clause> ]  
    [ FROM table_source [ ,...n ] ]   
    [ WHERE { <search_condition>   
            | { [ CURRENT OF   
                   { { [ GLOBAL ] cursor_name }   
                       | cursor_variable_name   
                   }   
                ]  
              }  
            }   
    ]   
    [ OPTION ( <Query Hint> [ ,...n ] ) ]   
[; ]  
  
<object> ::=  
{   
    [ server_name.database_name.schema_name.   
      | database_name. [ schema_name ] .   
      | schema_name.  
    ]  
    table_or_view_name   
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics

[ WITH <common_table_expression> [ ,...n ] ] 
DELETE [database_name . [ schema ] . | schema. ] table_name  
FROM [database_name . [ schema ] . | schema. ] table_name 
JOIN {<join_table_source>}[ ,...n ]  
ON <join_condition>
[ WHERE <search_condition> ]   
[ OPTION ( <query_options> [ ,...n ]  ) ]  
[; ]  

<join_table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [ <tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
}  
```

```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
DELETE 
    [ FROM [database_name . [ schema ] . | schema. ] table_name ]   
    [ WHERE <search_condition> ]   
    [ OPTION ( <query_options> [ ,...n ]  ) ]  
[; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 WITH \<common_table_expression>  
 Especifica el conjunto de resultados de nombre temporal, también conocido como expresión de tabla común, definido dentro del ámbito de la instrucción DELETE. El conjunto de resultados se deriva de una instrucción SELECT.  
  
 Las expresiones de tabla comunes también se pueden utilizar con las instrucciones SELECT, INSERT, UPDATE y CREATE VIEW. Para más información, vea [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 TOP **(** _expression_ **)** [ PERCENT ]  
 Especifica el número o el porcentaje de filas aleatorias que se van a eliminar. *expression* puede ser un número o un porcentaje de las filas. Las filas a las que se hace referencia en la expresión TOP utilizada con INSERT, UPDATE o DELETE no se ordenan. Para más información, vea [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
 FROM  
 Palabra clave opcional que se puede usar entre la palabra clave DELETE y el destino *table_or_view_name* o *rowset_function_limited*.  
  
 *table_alias*  
 Alias especificado en la cláusula FROM *table_source* que representa la tabla o vista de la que se van a eliminar las filas.  
  
 *server_name*  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Nombre del servidor (un nombre de servidor vinculado o la función [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) como nombre de servidor) en el que se encuentra la tabla o la vista. Si se especifica *server_name*, se requiere *database_name* y *schema_name*.  
  
 *database_name*  
 El nombre de la base de datos.  
  
 *schema_name*  
 Nombre del esquema al que pertenece la tabla o la vista.  
  
 *table_or_view_name*  
 Nombre de la tabla o vista cuyas filas se van a quitar.  
  
 En este ámbito, se puede utilizar una variable de tabla como origen de tabla de una instrucción DELETE.  
  
 La vista a la que hace referencia *table_or_view_name* debe poderse actualizar y debe hacer referencia exactamente a una tabla base de la cláusula FROM de la definición de vista. Para más información sobre las vistas actualizables, vea [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 *rowset_function_limited*  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Función [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) u [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md), dependiendo del proveedor.  
  
 WITH **(** \<table_hint_limited> [... *n*] **)**  
 Especifica una o varias sugerencias de tabla que están permitidas en una tabla de destino. La palabra clave WITH y los paréntesis son obligatorios. No se permiten NOLOCK ni READUNCOMMITTED. Para más información sobre las sugerencias de tabla, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 \<OUTPUT_Clause>  
 Devuelve filas eliminadas, o expresiones basadas en ellas, como parte de la operación DELETE. La cláusula OUTPUT no se admite en instrucciones DML dirigidas a tablas o vistas remotas. Para más información sobre los argumentos y el comportamiento de esta cláusula, vea [Cláusula OUTPUT (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md).  
  
 FROM *table_source*  
 Especifica una cláusula FROM adicional. Esta extensión de [!INCLUDE[tsql](../../includes/tsql-md.md)] para DELETE permite especificar datos de \<table_source> y eliminar las filas correspondientes de la tabla en la primera cláusula FROM.  
  
 Se puede utilizar esta extensión, que especifica una combinación, en lugar de una subconsulta en la cláusula WHERE para identificar las filas que se van a quitar.  
  
 Para obtener más información, vea [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
 WHERE  
 Especifica las condiciones utilizadas para limitar el número de filas que se van a eliminar. Si no se proporciona una cláusula WHERE, DELETE quita todas las filas de la tabla.  
  
 Hay dos formas de operaciones de eliminación, que se basan en las condiciones que se especifiquen en la cláusula WHERE:  
  
-   Las eliminaciones por búsqueda especifican una condición de búsqueda que califica las filas que se van a eliminar. Por ejemplo, WHERE *column_name* = *value*.  
  
-   Las eliminaciones por posición utilizan la cláusula CURRENT OF para especificar un cursor. La operación de eliminación se produce en la posición actual del cursor. Este método puede ser más preciso que una instrucción DELETE por búsqueda que utilice una cláusula WHERE *search_condition* para calificar las filas que se van a eliminar. Una instrucción DELETE por búsqueda elimina varias filas si la condición de búsqueda no identifica exclusivamente una única fila.  
  
\<search_condition>  
 Especifica las condiciones restrictivas de las filas que se van a eliminar. No hay límite en el número de predicados que se pueden incluir en una condición de búsqueda. Para más información, vea [Condición de búsqueda &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
 CURRENT OF  
 Especifica que la instrucción DELETE se ejecutará en la posición actual del cursor especificado.  
  
 GLOBAL  
 Especifica que *cursor_name* hace referencia a un cursor global.  
  
 *cursor_name*  
 Es el nombre del cursor abierto desde el que se realiza la captura. Si hay un cursor global y otro local con el nombre *cursor_name*, este argumento hace referencia al cursor global si se especifica GLOBAL; de lo contrario, hace referencia al cursor local. El cursor debe permitir actualizaciones.  
  
 *cursor_variable_name*  
 Nombre de una variable de cursor. La variable de cursor debe hacer referencia a un cursor que permita realizar actualizaciones.  
  
 OPTION **(** \<query_hint> [ **,** ... *n*] **)**  
 Palabras clave que indican que se utilizan sugerencias del optimizador para personalizar el procesamiento de la instrucción por parte del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Para obtener más información, vea [Sugerencias de consulta &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="best-practices"></a>Prácticas recomendadas  
 Para eliminar todas las filas de una tabla, use TRUNCATE TABLE. TRUNCATE TABLE es más rápido que DELETE y utiliza menos recursos de los registros de transacciones y de sistema. TRUNCATE TABLE tiene restricciones; por ejemplo, la tabla no puede participar en la replicación. Para obtener más información, vea [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md).  
  
 Use la función @@ROWCOUNT para devolver el número de filas eliminadas a la aplicación cliente. Para más información, vea [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md).  
  
## <a name="error-handling"></a>Tratamiento de errores  
 Puede implementar el control de errores de la instrucción DELETE especificando la instrucción en una construcción TRY...CATCH.  
  
 La instrucción DELETE puede tener un error si infringe un desencadenador o intenta quitar una fila a la que hacen referencia datos de otra tabla con una restricción FOREIGN KEY. Si la instrucción DELETE quita varias filas y cualquiera de las filas eliminadas infringe un desencadenador o restricción, se cancela la instrucción, se devuelve un error y no se elimina ninguna fila.  
  
 Cuando una instrucción DELETE encuentra un error aritmético (desbordamiento, división entre cero o error de dominio) al evaluar una expresión, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] trata ese error como si SET ARITHABORT fuese ON. Se cancela el resto del proceso por lotes y se devuelve un mensaje de error.  
  
## <a name="interoperability"></a>Interoperabilidad  
 Es posible utilizar DELETE en el cuerpo de una función definida por el usuario si el objeto que se va a modificar es una variable de tabla.  
  
 Al eliminar una fila que contiene una columna FILESTREAM, también elimina los archivos del sistema de archivos subyacentes. El recolector de elementos no utilizados de FILESTREAM quita los archivos subyacentes. Para obtener más información, consulte [Obtener acceso a datos FILESTREAM con Transact-SQL](../../relational-databases/blob/access-filestream-data-with-transact-sql.md).  
  
 No se puede especificar la cláusula FROM en una instrucción DELETE que haga referencia, directa o indirectamente, a una vista que tiene definido un desencadenador INSTEAD OF. Para más información sobre los desencadenadores INSTEAD OF, vea [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 Cuando se usa TOP con DELETE, las filas a las que hace referencia no están organizadas de ninguna manera y la cláusula ORDER BY no se puede especificar directamente en esta instrucción. Si necesita utilizar TOP para eliminar filas por un orden cronológico significativo, debe usar TOP junto con una cláusula ORDER BY en una instrucción de subselección. Vea la sección Ejemplos que aparece más adelante en este tema.  
  
 TOP no se puede usar en una instrucción DELETE con vistas divididas en particiones.  
  
## <a name="locking-behavior"></a>Comportamiento del bloqueo  
 De forma predeterminada, una instrucción DELETE siempre adquiere un bloqueo con intención exclusiva (IX) en el objeto de tabla que modifica y retiene ese bloqueo hasta que se completa la transacción. Al usar un bloqueo con intención exclusiva (IX), el resto de las transacciones no pueden modificar los datos; las operaciones de lectura solo se pueden realizar si se usa la sugerencia NOLOCK o el nivel de aislamiento de lectura no confirmada. Puede especificar sugerencias de tabla para invalidar este comportamiento predeterminado durante la ejecución de la instrucción DELETE especificando otro método de bloqueo, sin embargo se recomienda que solo los desarrolladores y administradores de bases de datos experimentados usen las sugerencias y únicamente como último recurso. Para obtener más información, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Cuando se eliminan filas de un montón, [!INCLUDE[ssDE](../../includes/ssde-md.md)] puede usar bloqueo de filas o páginas para la operación. Como consecuencia, las páginas que han quedado vacías por la operación de eliminación permanecen asignadas al montón. Si no se cancela la asignación de las páginas vacías, otros objetos de la base de datos no pueden volver a utilizar el espacio asociado.  
  
 Para eliminar las filas de un montón y cancelar la asignación de las páginas, use uno de los métodos siguientes.  
  
-   Especifique la sugerencia TABLOCK en la instrucción DELETE. Si se utiliza la sugerencia TABLOCK, la operación de eliminación aplica un bloqueo exclusivo a la tabla, en lugar de un bloqueo de fila o de página. Esto permite cancelar la asignación de las páginas. Para obtener más información sobre las sugerencias TABLOCK, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
-   Use `TRUNCATE TABLE` si se van a eliminar todas las filas de la tabla.  
  
-   Cree un índice clúster en el montón antes de eliminar las filas. Puede quitar el índice clúster tras eliminar las filas. Este método requiere más tiempo que los métodos anteriores y utiliza más recursos temporales.  
  
> [!NOTE]  
>  Las páginas vacías se pueden quitar de un montón en cualquier momento con la instrucción `ALTER TABLE <table_name> REBUILD`.  
  
## <a name="logging-behavior"></a>Comportamiento del registro  
La instrucción DELETE siempre está registrada totalmente.  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Se requieren permisos `DELETE` en la tabla de destino. También se requieren permisos `SELECT` si la instrucción contiene una cláusula WHERE.  
  
 Los permisos DELETE se adjudican de forma predeterminada a los miembros del rol fijo de servidor `sysadmin`, a los roles fijos de base de datos `db_owner` y `db_datawriter`, y al propietario de la tabla. Los miembros de los roles `sysadmin`, `db_owner` y `db_securityadmin` y el propietario de la tabla pueden transferir permisos a otros usuarios.  
  
## <a name="examples"></a>Ejemplos  
  
|Category|Elementos de sintaxis ofrecidos|  
|--------------|------------------------------|  
|[Sintaxis básica](#BasicSyntax)|Delete|  
|[Limitar las filas eliminadas](#LimitRows)|WHERE • FROM • cursor •|  
|[Eliminar filas de una tabla remota](#RemoteTables)|Servidor vinculado • función de conjunto de filas OPENQUERY • función de conjunto de filas OPENDATASOURCE|  
|[Capturar los resultados de la instrucción DELETE](#CaptureResults)|Cláusula OUTPUT|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Sintaxis básica  
 En los ejemplos de esta sección se muestra la funcionalidad básica de la instrucción DELETE usando la sintaxis mínima requerida.  
  
#### <a name="a-using-delete-with-no-where-clause"></a>A. Utilizar DELETE sin la cláusula WHERE  
 En el ejemplo siguiente se eliminan todas las filas de la tabla `SalesPersonQuotaHistory` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] porque no se utiliza una cláusula WHERE para limitar el número de filas eliminadas.  
  
```sql
DELETE FROM Sales.SalesPersonQuotaHistory;  
GO  
```  
  
###  <a name="limiting-the-rows-deleted"></a><a name="LimitRows"></a> Limitar las filas eliminadas  
 En los ejemplos de esta sección se muestra cómo se limita el número de filas que se van a eliminar.  
  
#### <a name="b-using-the-where-clause-to-delete-a-set-of-rows"></a>B. Usar la cláusula WHERE para eliminar un conjunto de filas  
 En el ejemplo siguiente se eliminan todas las filas de la tabla `ProductCostHistory` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] en las que el valor de la columna `StandardCost` es superior a `1000.00`.  
  
```sql
DELETE FROM Production.ProductCostHistory  
WHERE StandardCost > 1000.00;  
GO  
```  
  
 En el siguiente ejemplo se muestra una cláusula WHERE más compleja. La cláusula WHERE define dos condiciones que deben cumplirse para determinar las filas que se van a eliminar. El valor de la columna `StandardCost` debe estar comprendido entre `12.00` y `14.00` y el valor de la columna `SellEndDate` debe ser NULL. En el ejemplo también se imprime el valor de la función **\@\@ROWCOUNT** para devolver el número de filas eliminadas.  
  
```sql
DELETE Production.ProductCostHistory  
WHERE StandardCost BETWEEN 12.00 AND 14.00  
      AND EndDate IS NULL;  
PRINT 'Number of rows deleted is ' + CAST(@@ROWCOUNT as char(3));  
```  
  
#### <a name="c-using-a-cursor-to-determine-the-row-to-delete"></a>C. Usar un cursor para determinar la fila que se va a eliminar  
 En el ejemplo siguiente se elimina una fila única de la tabla `EmployeePayHistory` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] mediante un cursor denominado `complex_cursor`. La operación de eliminación solo afecta a la única fila que se captura actualmente del cursor.  
  
```sql
DECLARE complex_cursor CURSOR FOR  
    SELECT a.BusinessEntityID  
    FROM HumanResources.EmployeePayHistory AS a  
    WHERE RateChangeDate <>   
         (SELECT MAX(RateChangeDate)  
          FROM HumanResources.EmployeePayHistory AS b  
          WHERE a.BusinessEntityID = b.BusinessEntityID) ;  
OPEN complex_cursor;  
FETCH FROM complex_cursor;  
DELETE FROM HumanResources.EmployeePayHistory  
WHERE CURRENT OF complex_cursor;  
CLOSE complex_cursor;  
DEALLOCATE complex_cursor;  
GO  
```  
  
#### <a name="d-using-joins-and-subqueries-to-data-in-one-table-to-delete-rows-in-another-table"></a>D. Usar combinaciones y subconsultas en los datos de una tabla para eliminar filas de otra tabla  
 En los siguientes ejemplos se muestran dos maneras de eliminar filas de una tabla en función de los datos de otra tabla. En ambos ejemplos, se eliminan las filas de la tabla `SalesPersonQuotaHistory` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] basándose en las ventas del año hasta la fecha almacenadas en la tabla `SalesPerson`. La primera instrucción `DELETE` muestra la solución de subconsulta compatible con ISO y la segunda instrucción `DELETE` muestra la extensión de FROM de [!INCLUDE[tsql](../../includes/tsql-md.md)] para unir las dos tablas.  
  
```sql
-- SQL-2003 Standard subquery  
  
DELETE FROM Sales.SalesPersonQuotaHistory   
WHERE BusinessEntityID IN   
    (SELECT BusinessEntityID   
     FROM Sales.SalesPerson   
     WHERE SalesYTD > 2500000.00);  
GO  
```  
  
```sql
-- Transact-SQL extension  
  
DELETE FROM Sales.SalesPersonQuotaHistory   
FROM Sales.SalesPersonQuotaHistory AS spqh  
INNER JOIN Sales.SalesPerson AS sp  
ON spqh.BusinessEntityID = sp.BusinessEntityID  
WHERE sp.SalesYTD > 2500000.00;  
GO  
```  
  
```sql
-- No need to mention target table more than once.  
  
DELETE spqh  
  FROM  
        Sales.SalesPersonQuotaHistory AS spqh  
    INNER JOIN Sales.SalesPerson AS sp  
        ON spqh.BusinessEntityID = sp.BusinessEntityID  
  WHERE  sp.SalesYTD > 2500000.00;  
```  
  
#### <a name="e-using-top-to-limit-the-number-of-rows-deleted"></a>E. Utilizar TOP para limitar el número de filas eliminadas  
 Cuando se usa una cláusula TOP (*n*) con DELETE, la operación de eliminación se realiza en una selección aleatoria de un número de filas *n*. En el ejemplo siguiente se eliminan `20` filas aleatorias de la tabla `PurchaseOrderDetail` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] cuyas fechas de vencimiento sean anteriores al 1 de julio de 2006.  
  
```sql
DELETE TOP (20)   
FROM Purchasing.PurchaseOrderDetail  
WHERE DueDate < '20020701';  
GO  
```  
  
 Si necesita utilizar TOP para eliminar filas por un orden cronológico significativo, debe utilizarla junto con ORDER BY en una instrucción de subselección. La siguiente consulta elimina de la tabla `PurchaseOrderDetail` las 10 filas con las fechas de vencimiento más antiguas. Para garantizar que solo se eliminen 10 filas, la columna especificada en la instrucción de subselección (`PurchaseOrderID`) es la clave principal de la tabla. El uso de una columna sin clave en la instrucción de subselección podría causar la eliminación de más de 10 filas si la columna especificada contiene valores duplicados.  
  
```sql
DELETE FROM Purchasing.PurchaseOrderDetail  
WHERE PurchaseOrderDetailID IN  
   (SELECT TOP 10 PurchaseOrderDetailID   
    FROM Purchasing.PurchaseOrderDetail   
    ORDER BY DueDate ASC);  
GO  
```  
  
###  <a name="deleting-rows-from-a-remote-table"></a><a name="RemoteTables"></a> Eliminar filas de una tabla remota  
 En los ejemplos de esta sección se muestra cómo se eliminan filas de una tabla remota mediante un [servidor vinculado](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) o una [función de conjunto de filas](../functions/opendatasource-transact-sql.md) para hacer referencia a la tabla remota. Una tabla remota existe en un servidor o instancia diferente de SQL Server.  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
#### <a name="f-deleting-data-from-a-remote-table-by-using-a-linked-server"></a>F. Eliminar datos de una tabla remota usando un servidor vinculado  
 En el ejemplo siguiente se eliminan filas de una tabla remota. En el ejemplo primero se crea un vínculo al origen de datos remoto mediante [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). El nombre del servidor vinculado, `MyLinkServer`, se especifica después como parte del nombre de objeto de cuatro partes con el formato *server.catalog.schema.object*.  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' or 'server_name\instance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  
```  
  
```sql
-- Specify the remote data source using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
DELETE MyLinkServer.AdventureWorks2012.HumanResources.Department 
WHERE DepartmentID > 16;  
GO  
```  
  
#### <a name="g-deleting-data-from-a-remote-table-by-using-the-openquery-function"></a>G. Eliminar datos de una tabla remota con la función OPENQUERY  
 En el ejemplo siguiente se eliminan filas de una tabla remota especificando la función de conjunto de filas [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md). En este ejemplo se usa el nombre del servidor vinculado creado en el ejemplo anterior.  
  
```sql
DELETE OPENQUERY (MyLinkServer, 'SELECT Name, GroupName 
FROM AdventureWorks2012.HumanResources.Department  
WHERE DepartmentID = 18');  
GO  
```  
  
#### <a name="h-deleting-data-from-a-remote-table-by-using-the-opendatasource-function"></a>H. Eliminar datos de una tabla remota con una función OPENDATASOURCE  
 En el ejemplo siguiente se elimina una fila de una tabla remota especificando la función de conjunto de filas [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md). Especifique un nombre de servidor válido para el origen de datos con el formato *server_name* o *server_name\instance_name*.  
  
```sql
DELETE FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source= <server_name>; Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department   
WHERE DepartmentID = 17;
```  
  
###  <a name="capturing-the-results-of-the-delete-statement"></a><a name="CaptureResults"></a> Capturar los resultados de la instrucción DELETE  
  
#### <a name="i-using-delete-with-the-output-clause"></a>I. Usar DELETE con la cláusula OUTPUT  
 En el ejemplo siguiente se muestra cómo se guardan los resultados de una instrucción `DELETE` en una variable de tabla en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
DELETE Sales.ShoppingCartItem  
OUTPUT DELETED.*   
WHERE ShoppingCartID = 20621;  
  
--Verify the rows in the table matching the WHERE clause have been deleted.  
SELECT COUNT(*) AS [Rows in Table] 
FROM Sales.ShoppingCartItem 
WHERE ShoppingCartID = 20621;  
GO  
```  
  
#### <a name="j-using-output-with-from_table_name-in-a-delete-statement"></a>J. Usar OUTPUT con <from_table_name> en una instrucción DELETE  
 En el ejemplo siguiente se eliminan las filas de la tabla `ProductProductPhoto` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] según los criterios de búsqueda definidos en la cláusula `FROM` de la instrucción `DELETE`. La cláusula `OUTPUT` devuelve columnas de la tabla que se elimina ( `DELETED.ProductID`, `DELETED.ProductPhotoID`) y de la tabla `Product` . Esta información se utiliza en la cláusula `FROM` para especificar las filas que se deben eliminar.  
  
```sql
DECLARE @MyTableVar table (  
    ProductID int NOT NULL,   
    ProductName nvarchar(50)NOT NULL,  
    ProductModelID int NOT NULL,   
    PhotoID int NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
    WHERE p.ProductModelID BETWEEN 120 and 130;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, ProductModelID, PhotoID   
FROM @MyTableVar  
ORDER BY ProductModelID;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="k-delete-all-rows-from-a-table"></a>K. Eliminar todas las filas de una tabla  
 En el ejemplo siguiente se eliminan todas las filas de la tabla `Table1` porque no se utiliza una cláusula WHERE para limitar el número de filas eliminadas.  
  
```sql
DELETE FROM Table1;  
```  
  
### <a name="l-delete-a-set-of-rows-from-a-table"></a>L. Eliminar un conjunto de filas de una tabla

 En el ejemplo siguiente se eliminan todas las filas de la tabla `Table1` en las que el valor de la columna `StandardCost` es superior a 1000,00.  
  
```sql
DELETE FROM Table1  
WHERE StandardCost > 1000.00;  
```  
  
### <a name="m-using-label-with-a-delete-statement"></a>M. Usar LABEL con una instrucción DELETE

 En el ejemplo siguiente se usa una etiqueta con la instrucción DELETE.  
  
```sql
DELETE FROM Table1  
OPTION ( LABEL = N'label1' );  
  
```  
  
### <a name="n-using-a-label-and-a-query-hint-with-the-delete-statement"></a>Hora Usar una etiqueta y una sugerencia de consulta con la instrucción DELETE

 Esta consulta muestra la sintaxis básica para usar una sugerencia de combinación de consulta con la instrucción DELETE. Para más información sobre las sugerencias de combinación y cómo usar la cláusula OPTION, consulte [Cláusula OPTION (Transact-SQL)](../queries/option-clause-transact-sql.md).
  
```sql
-- Uses AdventureWorks  
  
DELETE FROM dbo.FactInternetSales  
WHERE ProductKey IN (   
    SELECT T1.ProductKey FROM dbo.DimProduct T1   
    JOIN dbo.DimProductSubcategory T2  
    ON T1.ProductSubcategoryKey = T2.ProductSubcategoryKey  
    WHERE T2.EnglishProductSubcategoryName = 'Road Bikes' )  
OPTION ( LABEL = N'CustomJoin', HASH JOIN ) ;  
```  

### <a name="o-delete-using-a-where-clause"></a>O. Eliminación con una cláusula WHERE

Esta consulta muestra cómo realizar una eliminación con una cláusula WHERE y no con una cláusula FROM.

```sql
DELETE tableA WHERE EXISTS (
SELECT TOP 1 1 FROM tableB tb WHERE tb.col1 = tableA.col1
)
```

### <a name="p-delete-based-on-the-result-of-joining-with-another-table"></a>P. Eliminación basada en el resultado de la unión con otra tabla

En este ejemplo se muestra cómo realizar una eliminación de una tabla en función del resultado de la combinación con otra tabla.

```sql
CREATE TABLE dbo.Table1   
    (ColA int NOT NULL, ColB decimal(10,3) NOT NULL);  
GO  

CREATE TABLE dbo.Table2   
    (ColA int PRIMARY KEY NOT NULL, ColB decimal(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0), (1, 20.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  

DELETE dbo.Table2   
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA)
    WHERE dboTable2.ColA = 1;  
```

## <a name="see-also"></a>Consulte también

 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md)   
 [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md)  
  
