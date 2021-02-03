---
description: 'SELECT: cláusula INTO (Transact-SQL)'
title: Cláusula INTO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- INTO_TSQL
- INSERT_INTO_TSQL
- INSERT INTO
- INTO
- INTO clause
- INTO_clause_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- copying data [SQL Server], into a new table
- INTO clause
- moving data, to a new table
- table creation [SQL Server], INTO clause
- SELECT INTO statement
- inserting rows
- clauses [SQL Server], INTO
- row additions [SQL Server], INTO clause
ms.assetid: b48d69e8-5a00-48bf-b2f3-19278a72dd88
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3ce1740def1f7d2ebd70ad611f3b7b489ca04f99
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235861"
---
# <a name="select---into-clause-transact-sql"></a>SELECT: cláusula INTO (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SELECT…INTO crea una tabla en el grupo de archivos predeterminado e inserta las filas resultantes de la consulta en ella. Para conocer la sintaxis completa de SELECT, vea [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
[ INTO new_table ]
[ ON filegroup ]
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *tabla_nueva*   
 Especifica el nombre de una nueva tabla que se va a crear en función de las columnas de la lista de selección y de las filas elegidas desde el origen de datos.  
 
 El formato de *new_table* se determina mediante la evaluación de las expresiones de la lista de selección. Las columnas de *new_table* se crean en el orden que especifica la lista de selección. Cada columna de *new_table* tiene el mismo nombre, tipo de datos, nulabilidad y valor que la expresión correspondiente de la lista de selección. La propiedad IDENTITY de una columna se transfiere excepto bajo las condiciones definidas en "Trabajar con columnas de identidad" en la sección Comentarios.  
  
 Para crear la tabla en otra base de datos en la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], especifique *new_table* como un nombre completo con formato *database.schema.table_name*.  
  
 No se puede crear *new_table* en un servidor remoto, pero se puede rellenar *new_table* desde un origen de datos remoto. Para crear *new_table* a partir de una tabla de origen remota, especifique la tabla de origen mediante un nombre con cuatro partes con el formato *linked_server*.*catalog*.*schema*.*object* en la cláusula FROM de la instrucción SELECT. También puede usar la función [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) o la función [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) en la cláusula FROM para especificar el origen de datos remoto.  
 
 *grupo_de_archivos*    
 Especifica el nombre del grupo de archivos en el que se creará la tabla. El grupo de archivos especificado debe existir en la base de datos; de lo contrario, se mostrará un error en el motor de SQL Server.   
 
 **Se aplica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y versiones posteriores
  
## <a name="data-types"></a>Tipo de datos  
 El atributo FILESTREAM no transfiere a la nueva tabla. Los BLOB FILESTREAM se copian y se almacenan en la nueva tabla como BLOB **varbinary(max)** . Sin el atributo FILESTREAM, el tipo de datos **varbinary(max)** tiene una limitación de 2 GB. Si un FILESTREAM BLOB supera este valor, se produce el error 7119 y se detiene la instrucción.  
  
 Cuando se selecciona una columna de identidad existente en una nueva tabla, la nueva columna hereda la propiedad IDENTITY, a menos que se cumpla una de las siguientes condiciones:  
  
-   La instrucción SELECT contiene una combinación.  
  
-   Se han combinado varias instrucciones SELECT con UNION.  
  
-   La columna de identidad aparece más de una vez en la lista de selección.  
  
-   La columna de identidad forma parte de una expresión.  
  
-   La columna de identidad es de un origen de datos remoto.  
  
Si se cumple alguna de estas condiciones, la columna se crea como NOT NULL en lugar de heredar la propiedad IDENTITY. Si una columna de identidad se requiere en la nueva tabla pero este tipo de columna no está disponible o desea un valor de inicialización o de incremento diferente de la columna de identidad de origen, defina la columna en la lista de selección utilizando la función IDENTITY. Vea "Crear una columna de identidad utilizando la función IDENTITY" en la sección Ejemplos siguiente.  

## <a name="remarks"></a>Observaciones  
El funcionamiento de la instrucción `SELECT...INTO` consta de dos pasos: se crea la tabla y, después, se insertan filas.  Esto significa que si se produce un error en las operaciones de inserción, se revertirán todas, pero la tabla nueva (vacía) se conservará.  Si necesita que la operación sea correcta o no en su totalidad, use una [transacción explícita](../language-elements/begin-transaction-transact-sql.md).
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 No puede especificar una variable de tabla o parámetro con valores de tabla como la nueva tabla.  
  
 No puede se usar `SELECT...INTO` para crear una tabla con particiones, incluso si la partición se realiza sobre la tabla de origen. En `SELECT...INTO` no se usa el esquema de partición de la tabla de origen; en su lugar, la tabla nueva se crea en el grupo de archivos predeterminado. Para insertar filas en una tabla con particiones, primero se debe crear la tabla con particiones y, después, usar la instrucción `INSERT INTO...SELECT...FROM`.  
  
 Los índices, restricciones y desencadenadores definidos en la tabla de origen no se transfieren a la tabla nueva, ni se pueden especificar en la instrucción `SELECT...INTO`. Si se requieren estos objetos, se pueden crear después de ejecutar la instrucción `SELECT...INTO`.  
  
 Especificar una cláusula `ORDER BY` no garantiza que las filas se inserten en el orden especificado.  
  
 Cuando se incluye una columna dispersa en la lista de selección, la propiedad de la columna dispersa no se transfiere a la columna de la nueva tabla. Si esta propiedad es necesaria en la nueva tabla, modifique la definición de columna después de ejecutar la instrucción SELECT...INTO para que incluya esta propiedad.  
  
 Cuando se incluye una columna calculada en la lista de selección, la columna correspondiente de la nueva tabla no es una columna calculada. Los valores de la columna nueva son los que se calcularon en el momento en que se ejecutó `SELECT...INTO`.  
  
## <a name="logging-behavior"></a>Comportamiento del registro  
 La cantidad de registro para `SELECT...INTO` depende del modelo de recuperación en vigor para la base de datos. En el modelo de recuperación simple o en el optimizado para cargas masivas de registros, las operaciones masivas se registran mínimamente. Con registro mínimo, el uso de la instrucción `SELECT...INTO` puede ser más eficaz que crear una tabla y rellenarla después con una instrucción INSERT. Para más información, consulte [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).
 
Las instrucciones `SELECT...INTO` que contiene funciones definidas por el usuario (UDF) son operaciones que se registran por completo. Si las funciones definidas por el usuario que se utilizan en la instrucción `SELECT...INTO` no realizan ninguna operación de acceso a datos, puede especificar la cláusula SCHEMABINDING para las funciones definidas por el usuario, lo que establecerá la propiedad UserDataAccess derivada para dichas funciones en 0. Después de este cambio, las instrucciones `SELECT...INTO` se registrarán de forma mínima. Si la instrucción `SELECT...INTO` sigue haciendo referencia a al menos una función definida por el usuario con esta propiedad establecida en 1, la operación se registrará por completo.
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CREATE TABLE en la base de datos de destino.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-table-by-specifying-columns-from-multiple-sources"></a>A. Crear una tabla especificando columnas de varios orígenes  
 En el ejemplo siguiente se crea la tabla `dbo.EmployeeAddresses` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] seleccionando siete columnas de varias tablas relacionadas con empleados y direcciones.  
  
```sql  
SELECT c.FirstName, c.LastName, e.JobTitle, a.AddressLine1, a.City,   
    sp.Name AS [State/Province], a.PostalCode  
INTO dbo.EmployeeAddresses  
FROM Person.Person AS c  
    JOIN HumanResources.Employee AS e   
    ON e.BusinessEntityID = c.BusinessEntityID  
    JOIN Person.BusinessEntityAddress AS bea  
    ON e.BusinessEntityID = bea.BusinessEntityID  
    JOIN Person.Address AS a  
    ON bea.AddressID = a.AddressID  
    JOIN Person.StateProvince as sp   
    ON sp.StateProvinceID = a.StateProvinceID;  
GO  
```  
  
### <a name="b-inserting-rows-using-minimal-logging"></a>B. Insertar las filas utilizando el registro mínimo  
 El ejemplo siguiente crea la tabla `dbo.NewProducts` e inserta filas de la tabla `Production.Product`. El ejemplo supone que el modelo de recuperación de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] está establecido en FULL. Para asegurarse de que se utiliza el registro mínimo, el modelo de recuperación de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] se establece en BULK_LOGGED antes de que las filas se inserten y se restablece en FULL después de la instrucción SELECT...INTO. De esta manera se asegura de que la instrucción SELECT...INTO use el espacio mínimo en el registro de transacciones y funcione eficazmente.  
  
```sql  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
  
SELECT * INTO dbo.NewProducts  
FROM Production.Product  
WHERE ListPrice > $25   
AND ListPrice < $100;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
```  
  
### <a name="c-creating-an-identity-column-using-the-identity-function"></a>C. Crear una columna de identidad utilizando la función IDENTITY  
 En el ejemplo siguiente se utiliza la función IDENTITY para crear una columna de identidad en la nueva tabla `Person.USAddress` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Se requiere esto porque la instrucción SELECT que define la tabla contiene una unión, que hace que la propiedad IDENTITY no transfiera a la nueva tabla. Tenga en cuenta que los valores de inicialización e incremento especificados en la función IDENTITY son diferentes de los de la columna `AddressID` de la tabla de origen `Person.Address`.  
  
```sql  
-- Determine the IDENTITY status of the source column AddressID.  
SELECT OBJECT_NAME(object_id) AS TableName, name AS column_name, 
  is_identity, seed_value, increment_value  
FROM sys.identity_columns  
WHERE name = 'AddressID';  
  
-- Create a new table with columns from the existing table Person.Address. 
-- A new IDENTITY column is created by using the IDENTITY function.  
SELECT IDENTITY (int, 100, 5) AS AddressID,   
       a.AddressLine1, a.City, b.Name AS State, a.PostalCode  
INTO Person.USAddress   
FROM Person.Address AS a  
INNER JOIN Person.StateProvince AS b 
  ON a.StateProvinceID = b.StateProvinceID  
WHERE b.CountryRegionCode = N'US';   
  
-- Verify the IDENTITY status of the AddressID columns in both tables.  
SELECT OBJECT_NAME(object_id) AS TableName, name AS column_name, 
  is_identity, seed_value, increment_value  
FROM sys.identity_columns  
WHERE name = 'AddressID';  
```  
  
### <a name="d-creating-a-table-by-specifying-columns-from-a-remote-data-source"></a>D. Crear una tabla especificando las columnas de un origen de datos remoto  
 El ejemplo siguiente muestra tres métodos para crear una nueva tabla en el servidor local desde un origen de datos remoto. En el ejemplo se comienza creando un vínculo al origen de datos remoto. El nombre del servidor vinculado, `MyLinkServer,` se especifica en la cláusula FROM de la primera instrucción SELECT...INTO y en la función OPENQUERY de la segunda instrucción SELECT...INTO. La tercera instrucción SELECT...INTO utiliza la función OPENDATASOURCE, que especifica el origen de datos remoto directamente en lugar de utilizar el nombre del servidor vinculado.  
  
 **Se aplica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' 
-- or 'server_name\instance_name'.  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  

USE AdventureWorks2012;  
GO  
-- Specify the remote data source in the FROM clause using a four-part name   
-- in the form linked_server.catalog.schema.object.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.Departments  
FROM MyLinkServer.AdventureWorks2012.HumanResources.Department  
GO  
-- Use the OPENQUERY function to access the remote data source.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.DepartmentsUsingOpenQuery  
FROM OPENQUERY(MyLinkServer, 'SELECT *  
               FROM AdventureWorks2012.HumanResources.Department');   
GO  
-- Use the OPENDATASOURCE function to specify the remote data source.  
-- Specify a valid server name for Data Source using the format 
-- server_name or server_name\instance_name.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.DepartmentsUsingOpenDataSource  
FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source=server_name;Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department;  
GO  
```  
  
### <a name="e-import-from-an-external-table-created-with-polybase"></a>E. Importar desde una tabla externa creada con PolyBase  
 Importe datos desde Almacenamiento de Azure o Hadoop en SQL Server para obtener un almacenamiento persistente. Use `SELECT INTO` para importar datos a los que se hace referencia en una tabla externa para el almacenamiento persistente en SQL Server. Cree una tabla relacional sobre la marcha y luego cree un índice de almacén de columnas sobre la tabla en un segundo paso.  
  
 **Se aplica a:** [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
```sql
-- Import data for car drivers into SQL Server to do more in-depth analysis.  
SELECT DISTINCT   
        Insured_Customers.FirstName, Insured_Customers.LastName,   
        Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
        SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome;  
```  
### <a name="f-copying-the-data-from-one-table-to-another-and-create-the-new-table-on-a-specified-filegroup"></a>F. Copia de los datos de una tabla en otra y creación de la tabla en un grupo de archivos especificado
En el ejemplo siguiente se muestra cómo crear una tabla como una copia de otra tabla y cargarla en un grupo de archivos especificado diferente del grupo de archivos predeterminado del usuario.

 **Se aplica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y versiones posteriores

```sql
ALTER DATABASE [AdventureWorksDW2016] ADD FILEGROUP FG2;
ALTER DATABASE [AdventureWorksDW2016]
ADD FILE
(
NAME='FG2_Data',
FILENAME = '/var/opt/mssql/data/AdventureWorksDW2016_Data1.mdf'
)
TO FILEGROUP FG2;
GO
SELECT * INTO [dbo].[FactResellerSalesXL] ON FG2 FROM [dbo].[FactResellerSales];
```
  
## <a name="see-also"></a>Consulte también  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Ejemplos de SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [IDENTITY &#40;Function&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/identity-function-transact-sql.md)  
  
  
