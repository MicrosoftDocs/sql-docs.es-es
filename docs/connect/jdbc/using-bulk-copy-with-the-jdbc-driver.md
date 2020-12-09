---
title: Uso de la copia masiva con el controlador JDBC
description: La clase SQLServerBulkCopy permite escribir soluciones de carga de datos en Java que ofrecen importantes ventajas de rendimiento con respecto a las API de JDBC estándares.
ms.custom: ''
ms.date: 08/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 21e19635-340d-49bb-b39d-4867102fb5df
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 52f465b4cfdcb2ff771a71c1ef956af78b522358
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442876"
---
# <a name="using-bulk-copy-with-the-jdbc-driver"></a>Uso de la copia masiva con el controlador JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Microsoft SQL Server incluye una conocida utilidad de línea de comandos denominada `bcp` para copiar rápidamente y de forma masiva archivos grandes en tablas o vistas en bases de datos de SQL Server. La clase `SQLServerBulkCopy` permite escribir soluciones de código en Java que proporcionan funciones similares. Hay otras maneras de cargar datos en una tabla de SQL Server (las instrucciones INSERT, por ejemplo) pero `SQLServerBulkCopy` ofrece una importante ventaja de rendimiento sobre ellas.  
  
La clase `SQLServerBulkCopy` puede usarse para escribir datos solo en tablas de SQL Server. Pero el origen de datos no se limita a SQL Server; se puede usar cualquiera, siempre y cuando los datos se puedan leer con una implementación de `ResultSet`, `RowSet` o `ISQLServerBulkRecord`.  
  
Con la clase `SQLServerBulkCopy`, puede ejecutar:  
  
- Una única operación de copia masiva.  
  
- Varias operaciones de copia masiva.  
  
- Una operación de copia masiva con una transacción  
  
> [!NOTE]  
> Cuando se usa el Microsoft JDBC Driver 4.1 para SQL Server o versiones anteriores (que no admite la clase SQLServerBulkCopy), puede ejecutar la instrucción BULK INSERT de SQL Server Transact-SQL en su lugar.  
  
## <a name="bulk-copy-example-setup"></a>Configuración de ejemplo de copia masiva  

La clase `SQLServerBulkCopy` puede usarse para escribir datos solo en tablas de SQL Server. En los ejemplos de código de este artículo se usa la base de datos de ejemplo de SQL Server [AdventureWorks](../../samples/adventureworks-install-configure.md). Para evitar modificar los ejemplos de código de las tablas existentes, escriba los datos en tablas que debe crear antes.  
  
Las tablas `BulkCopyDemoMatchingColumns` y `BulkCopyDemoDifferentColumns` se basan en la tabla `Production.Products` de AdventureWorks. En los ejemplos de código que usan estas tablas, los datos se agregan desde la tabla `Production.Products` a una de estas tablas de muestra. La tabla `BulkCopyDemoDifferentColumns` se usa cuando en el ejemplo se muestra cómo asignar columnas de los datos de origen a la tabla de destino; `BulkCopyDemoMatchingColumns` se usa para la mayoría de los ejemplos restantes.  
  
Algunos de los ejemplos de código muestran cómo usar una clase `SQLServerBulkCopy` para escribir en varias tablas. En estos ejemplos, `BulkCopyDemoOrderHeader` y `BulkCopyDemoOrderDetail` se usan como tablas de destino. Estas tablas se basan en las tablas `Sales.SalesOrderHeader` y `Sales.SalesOrderDetail` de AdventureWorks.  
  
> [!NOTE]  
> Los ejemplos de código `SQLServerBulkCopy` se proporcionan únicamente para mostrar la sintaxis para usar `SQLServerBulkCopy`. Si las tablas de origen y destino se encuentran en la misma instancia de SQL Server, es más fácil y rápido usar una instrucción Transact-SQL INSERT... Instrucción SELECT para copiar los datos.  

### <a name="table-setup"></a>Configuración de tabla  

Para crear las tablas necesarias para que los ejemplos de código se ejecuten correctamente, debe ejecutar las siguientes instrucciones de Transact-SQL en una base de datos de SQL Server.  

```sql
USE AdventureWorks  
  
IF EXISTS (SELECT * FROM dbo.sysobjects
 WHERE id = object_id(N'[dbo].[BulkCopyDemoMatchingColumns]')
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoMatchingColumns]  
  
CREATE TABLE [dbo].[BulkCopyDemoMatchingColumns]([ProductID] [int] IDENTITY(1,1) NOT NULL,  
    [Name] [nvarchar](50) NOT NULL,  
    [ProductNumber] [nvarchar](25) NOT NULL,  
 CONSTRAINT [PK_ProductID] PRIMARY KEY CLUSTERED
(  
    [ProductID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
IF EXISTS (SELECT * FROM dbo.sysobjects
 WHERE id = object_id(N'[dbo].[BulkCopyDemoDifferentColumns]')
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoDifferentColumns]  
  
CREATE TABLE [dbo].[BulkCopyDemoDifferentColumns]([ProdID] [int] IDENTITY(1,1) NOT NULL,  
    [ProdNum] [nvarchar](25) NOT NULL,  
    [ProdName] [nvarchar](50) NOT NULL,  
 CONSTRAINT [PK_ProdID] PRIMARY KEY CLUSTERED
(  
    [ProdID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
IF EXISTS (SELECT * FROM dbo.sysobjects
 WHERE id = object_id(N'[dbo].[BulkCopyDemoOrderHeader]')
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoOrderHeader]  
  
CREATE TABLE [dbo].[BulkCopyDemoOrderHeader]([SalesOrderID] [int] IDENTITY(1,1) NOT NULL,  
    [OrderDate] [datetime] NOT NULL,  
    [AccountNumber] [nvarchar](15) NULL,  
 CONSTRAINT [PK_SalesOrderID] PRIMARY KEY CLUSTERED
(  
    [SalesOrderID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
IF EXISTS (SELECT * FROM dbo.sysobjects
 WHERE id = object_id(N'[dbo].[BulkCopyDemoOrderDetail]')
 AND OBJECTPROPERTY(id, N'IsUserTable') = 1)  
    DROP TABLE [dbo].[BulkCopyDemoOrderDetail]  
  
CREATE TABLE [dbo].[BulkCopyDemoOrderDetail]([SalesOrderID] [int] NOT NULL,  
    [SalesOrderDetailID] [int] NOT NULL,  
    [OrderQty] [smallint] NOT NULL,  
    [ProductID] [int] NOT NULL,  
    [UnitPrice] [money] NOT NULL,  
 CONSTRAINT [PK_LineNumber] PRIMARY KEY CLUSTERED
(  
    [SalesOrderID] ASC,  
    [SalesOrderDetailID] ASC  
) ON [PRIMARY]) ON [PRIMARY]  
  
```

## <a name="single-bulk-copy-operations"></a>Operaciones de copia masiva únicas.

El método más sencillo para realizar una operación de copia masiva de SQL Server es realizar una operación única con una base de datos. De forma predeterminada, una operación de copia masiva se realiza como una operación aislada: la operación de copia tiene lugar sin transacciones y no es posible revertirla.  
  
> [!NOTE]  
> Si necesita revertir toda o parte de la copia masiva cuando se produce un error, puede usar una transacción administrada por `SQLServerBulkCopy` o realizar la operación de copia masiva en una transacción existente.  
> Para obtener más información, consulte [Operaciones de transacción y de copia masiva](#transaction-and-bulk-copy-operations)  
  
 Los pasos generales para realizar una operación de copia masiva son:  
  
1. Conéctese al servidor de origen y obtenga los datos que se van a copiar. Los datos también pueden provenir de otros orígenes, si se pueden recuperar de un objeto `ResultSet` o una implementación de `ISQLServerBulkRecord`.  
  
2. Conéctese al servidor de destino (a menos que quiera que `SQLServerBulkCopy` establezca una conexión automáticamente).  
  
3. Cree un objeto `SQLServerBulkCopy` y establezca las propiedades necesarias a través de `setBulkCopyOptions`.  
  
4. Llame al método `setDestinationTableName` para indicar la tabla de destino para la operación de inserción masiva.  
  
5. Llame a uno de los métodos `writeToServer`.  
  
6. Opcionalmente, actualice las propiedades mediante `setBulkCopyOptions` y vuelva a llamar a `writeToServer` si fuese necesario.  
  
7. Llame a `close`, o bien encapsule las operaciones de copia masiva en una instrucción try con recursos.  
  
> [!CAUTION]  
> Se recomienda que los tipos de datos de la columna de origen y de destino coincidan. Si los tipos de datos no coinciden, `SQLServerBulkCopy` intenta convertir los valores de origen al tipo de datos de destino. Las conversiones pueden afectar al rendimiento y también pueden producir errores inesperados. Por ejemplo, un tipo de datos double puede convertirse a un tipo de datos decimal la mayoría de las veces, pero no siempre.  
  
### <a name="example"></a>Ejemplo

En la aplicación siguiente se muestra cómo cargar datos mediante la clase `SQLServerBulkCopy`. En este ejemplo se usa `ResultSet` para copiar datos de la tabla Production.Product de la base de datos AdventureWorks de SQL Server en una tabla similar de la misma base de datos.  
  
> [!IMPORTANT]  
> Este ejemplo no se ejecutará a menos que haya creado las tablas de trabajo como se describe en [Configuración de tabla](#table-setup). Este código se proporciona a fin de ilustrar únicamente la sintaxis para usar `SQLServerBulkCopy`. Si las tablas de origen y destino se encuentran en la misma instancia de SQL Server, es más fácil y rápido usar una instrucción Transact-SQL INSERT... Instrucción SELECT para copiar los datos.  

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;

public class BulkCopySingle {
    public static void main(String[] args) {
        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=AdventureWorks;user=<user>;password=<password>";
        String destinationTable = "dbo.BulkCopyDemoMatchingColumns";
        int countBefore, countAfter;
        ResultSet rsSourceData;

        try (Connection sourceConnection = DriverManager.getConnection(connectionUrl);
                Connection destinationConnection = DriverManager.getConnection(connectionUrl);
                Statement stmt = sourceConnection.createStatement();
                SQLServerBulkCopy bulkCopy = new SQLServerBulkCopy(destinationConnection)) {

            // Empty the destination table.
            stmt.executeUpdate("DELETE FROM " + destinationTable);

            // Perform an initial count on the destination table.
            countBefore = getRowCount(stmt, destinationTable);

            // Get data from the source table as a ResultSet.
            rsSourceData = stmt.executeQuery("SELECT ProductID, Name, ProductNumber FROM Production.Product");

            // In real world applications you would
            // not use SQLServerBulkCopy to move data from one table to the other
            // in the same database. This is for demonstration purposes only.

            // Set up the bulk copy object.
            // Note that the column positions in the source
            // table match the column positions in
            // the destination table so there is no need to
            // map columns.
            bulkCopy.setDestinationTableName(destinationTable);

            // Write from the source to the destination.
            bulkCopy.writeToServer(rsSourceData);

            // Perform a final count on the destination
            // table to see how many rows were added.
            countAfter = getRowCount(stmt, destinationTable);
            System.out.println((countAfter - countBefore) + " rows were added.");
        }
        // Handle any errors that may have occurred.
        catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static int getRowCount(Statement stmt,
            String tableName) throws SQLException {
        ResultSet rs = stmt.executeQuery("SELECT COUNT(*) FROM " + tableName);
        rs.next();
        int count = rs.getInt(1);
        rs.close();
        return count;
    }
}
```

### <a name="performing-a-bulk-copy-operation-using-transact-sql"></a>Realizar una operación de copia masiva mediante Transact-SQL

En el ejemplo siguiente se muestra cómo usar el método `executeUpdate` para ejecutar la instrucción BULK INSERT.  
  
> [!NOTE]  
> La ruta de acceso al origen de datos depende del servidor. El proceso del servidor debe tener acceso a esa ruta para que la operación de copia masiva se realice correctamente.  

```java
try (Connection con = DriverManager.getConnection(connectionUrl);
        Statement stmt = con.createStatement()) {
    // Perform the BULK INSERT
    stmt.executeUpdate(
            "BULK INSERT Northwind.dbo.[Order Details] " + "FROM 'f:\\mydata\\data.tbl' " + "WITH ( FORMATFILE='f:\\mydata\\data.fmt' )");
}
```  

## <a name="multiple-bulk-copy-operations"></a>Varias operaciones de copia masiva.  

Puede realizar varias operaciones de copia masiva con una única instancia de una clase `SQLServerBulkCopy`. Si los parámetros de operación cambian entre copias (por ejemplo, el nombre de la tabla de destino), debe actualizarlos antes de las siguientes llamadas a cualquiera de los métodos `writeToServer`, como se muestra en el ejemplo siguiente. A menos que se cambien explícitamente, todos los valores de propiedad permanecen igual que estaban en la operación de copia masiva anterior para una instancia determinada.  

> [!NOTE]  
> Generalmente, es más eficaz realizar varias operaciones de copia masiva con la misma instancia de `SQLServerBulkCopy` que usar una instancia independiente para cada operación.  
  
Si se realizan varias operaciones de copia masiva con el mismo objeto `SQLServerBulkCopy`, no hay restricciones en cuanto a si la información de origen o destino es igual o diferente en cada operación. Sin embargo, debe asegurarse de que la información de asociación de la columna está establecida correctamente cada vez que se escribe en el servidor.  
  
> [!IMPORTANT]  
> Este ejemplo no se ejecutará a menos que haya creado las tablas de trabajo como se describe en [Configuración de tabla](#table-setup). Este código se proporciona a fin de ilustrar únicamente la sintaxis para usar `SQLServerBulkCopy`. Si las tablas de origen y destino se encuentran en la misma instancia de SQL Server, es más fácil y rápido usar una instrucción Transact-SQL INSERT... Instrucción SELECT para copiar los datos.  

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopyOptions;

public class BulkCopyMultiple {
    public static void main(String[] args) {
        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=AdventureWorks;user=<user>;password=<password>";
        String destinationHeaderTable = "dbo.BulkCopyDemoOrderHeader";
        String destinationDetailTable = "dbo.BulkCopyDemoOrderDetail";
        int countHeaderBefore, countDetailBefore, countHeaderAfter, countDetailAfter;
        ResultSet rsHeader, rsDetail;

        try (Connection sourceConnection1 = DriverManager.getConnection(connectionUrl);
                Connection sourceConnection2 = DriverManager.getConnection(connectionUrl);
                Statement stmt = sourceConnection1.createStatement();
                PreparedStatement preparedStmt1 = sourceConnection1.prepareStatement(
                        "SELECT [SalesOrderID], [OrderDate], [AccountNumber] FROM [Sales].[SalesOrderHeader] WHERE [AccountNumber] = ?;");
                PreparedStatement preparedStmt2 = sourceConnection2.prepareStatement(
                        "SELECT [Sales].[SalesOrderDetail].[SalesOrderID], [SalesOrderDetailID], [OrderQty], [ProductID], [UnitPrice] FROM "
                                + "[Sales].[SalesOrderDetail] INNER JOIN [Sales].[SalesOrderHeader] ON "
                                + "[Sales].[SalesOrderDetail].[SalesOrderID] = [Sales].[SalesOrderHeader].[SalesOrderID] WHERE [AccountNumber] = ?;");
                SQLServerBulkCopy bulkCopy = new SQLServerBulkCopy(connectionUrl);) {

            // Empty the destination tables.
            stmt.executeUpdate("DELETE FROM " + destinationHeaderTable);
            stmt.executeUpdate("DELETE FROM " + destinationDetailTable);

            // Perform an initial count on the destination
            // table with matching columns.
            countHeaderBefore = getRowCount(stmt, destinationHeaderTable);

            // Perform an initial count on the destination
            // table with different column positions.
            countDetailBefore = getRowCount(stmt, destinationDetailTable);

            // Get data from the source table as a ResultSet.
            // The Sales.SalesOrderHeader and Sales.SalesOrderDetail
            // tables are quite large and could easily cause a timeout
            // if all data from the tables is added to the destination.
            // To keep the example simple and quick, a parameter is
            // used to select only orders for a particular account
            // as the source for the bulk insert.
            preparedStmt1.setString(1, "10-4020-000034");
            rsHeader = preparedStmt1.executeQuery();

            // Get the Detail data in a separate connection.
            preparedStmt2.setString(1, "10-4020-000034");
            rsDetail = preparedStmt2.executeQuery();

            // Create the SQLServerBulkCopySQLServerBulkCopy object.
            SQLServerBulkCopyOptions copyOptions = new SQLServerBulkCopyOptions();
            copyOptions.setBulkCopyTimeout(100);
            bulkCopy.setBulkCopyOptions(copyOptions);
            bulkCopy.setDestinationTableName(destinationHeaderTable);

            // Guarantee that columns are mapped correctly by
            // defining the column mappings for the order.
            bulkCopy.addColumnMapping("SalesOrderID", "SalesOrderID");
            bulkCopy.addColumnMapping("OrderDate", "OrderDate");
            bulkCopy.addColumnMapping("AccountNumber", "AccountNumber");

            // Write rsHeader to the destination.
            bulkCopy.writeToServer(rsHeader);

            // Set up the order details destination.
            bulkCopy.setDestinationTableName(destinationDetailTable);

            // Clear the existing column mappings
            bulkCopy.clearColumnMappings();

            // Add order detail column mappings.
            bulkCopy.addColumnMapping("SalesOrderID", "SalesOrderID");
            bulkCopy.addColumnMapping("SalesOrderDetailID", "SalesOrderDetailID");
            bulkCopy.addColumnMapping("OrderQty", "OrderQty");
            bulkCopy.addColumnMapping("ProductID", "ProductID");
            bulkCopy.addColumnMapping("UnitPrice", "UnitPrice");

            // Write rsDetail to the destination.
            bulkCopy.writeToServer(rsDetail);

            // Perform a final count on the destination
            // tables to see how many rows were added.
            countHeaderAfter = getRowCount(stmt, destinationHeaderTable);
            countDetailAfter = getRowCount(stmt, destinationDetailTable);

            System.out.println((countHeaderAfter - countHeaderBefore) + " rows were added to the Header table.");
            System.out.println((countDetailAfter - countDetailBefore) + " rows were added to the Detail table.");
        }
        // Handle any errors that may have occurred.
        catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static int getRowCount(Statement stmt,
            String tableName) throws SQLException {
        ResultSet rs = stmt.executeQuery("SELECT COUNT(*) FROM " + tableName);
        rs.next();
        int count = rs.getInt(1);
        rs.close();
        return count;
    }
}
```

## <a name="transaction-and-bulk-copy-operations"></a>Operaciones de transacción y de copia masiva

 Las operaciones de copia masiva se pueden realizar como operaciones aisladas o como parte de una transacción en varios pasos. Esta última opción permite realizar más de una operación de copia masiva en la misma transacción, así como realizar otras operaciones de base de datos (como inserciones, actualizaciones y eliminaciones) mientras todavía se puede confirmar o revertir la transacción entera.  
  
 De forma predeterminada, una operación de copia masiva se realiza como una operación aislada. La operación de copia masiva tiene lugar sin transacciones y sin la oportunidad de revertirla. Si tiene que revertir toda o parte de la copia masiva cuando se produce un error, puede usar una transacción administrada por `SQLServerBulkCopy`, o bien realizar la operación de copia masiva dentro de una transacción existente.  

## <a name="extended-bulk-copy-for-azure-data-warehouse"></a>Copia masiva extendida para Almacenamiento de datos de Azure

La versión 8.4.1 del controlador agrega una nueva propiedad de conexión, `sendTemporalDataTypesAsStringForBulkCopy`. Esta propiedad booleana es `true` de manera predeterminada.

Esta propiedad de conexión, al establecerse en `false`, enviará los tipos de datos **DATE**, **DATETIME**, **DATIMETIME2**, **DATETIMEOFFSET**, **SMALLDATETIME** y **TIME** como sus tipos correspondientes en lugar de enviarlos como String.

El envío de los tipos de datos temporales como sus tipos correspondientes permite al usuario enviar datos a esas columnas para Azure Synapse Analytics, lo que no era posible antes debido a la conversión por parte del controlador de los datos en String. El envío de datos String a columnas temporales funciona para SQL Server porque SQL Server realizaría la conversión implícita para nosotros, pero no es lo mismo con Azure Synapse Analytics.

Además, incluso sin establecer esta cadena de conexión en "false", de **v8.4.1** en adelante, los tipos de datos **MONEY** y **SMALLMONEY** se enviarán como tipos de datos **MONEY** / **SMALLMONEY** en lugar de **DECIMAL**, que también permite la copia masiva de esos tipos de datos en Azure Synapse Analytics.

### <a name="extended-bulk-copy-for-azure-data-warehouse-limitations"></a>Limitaciones de la copia masiva extendida para Almacenamiento de datos de Azure

Actualmente hay dos limitaciones:

1. Con esta propiedad de conexión establecida en `false`, el controlador solo aceptará el formato literal de cadena predeterminado de cada tipo de datos temporal, por ejemplo:

    `DATE: YYYY-MM-DD`

    `DATETIME: YYYY-MM-DD hh:mm:ss[.nnn]`

    `DATETIME2: YYYY-MM-DD hh:mm:ss[.nnnnnnn]`

    `DATETIMEOFFSET: YYYY-MM-DD hh:mm:ss[.nnnnnnn] [{+/-}hh:mm]`

    `SMALLDATETIME:YYYY-MM-DD hh:mm:ss`

    `TIME: hh:mm:ss[.nnnnnnn]`

2. Con esta propiedad de conexión establecida en `false`, el tipo de columna especificado para la copia masiva debe respetar el gráfico de asignación de tipos de datos desde [aquí](../../connect/jdbc/using-basic-data-types.md). Por ejemplo, anteriormente, los usuarios podían especificar `java.sql.Types.TIMESTAMP` para copiar datos de forma masiva en una columna `DATE`, pero con esta característica habilitada, deben especificar `java.sql.Types.DATE` para realizar la misma operación.
  
### <a name="performing-a-non-transacted-bulk-copy-operation"></a>Realizar una operación de copia masiva sin transacciones

La aplicación siguiente muestra lo que sucede cuando una operación de copia masiva sin transacciones encuentra un error en medio de la operación.  
  
En el ejemplo, la tabla de origen y la de destino incluyen, cada una, una columna de identidad denominada `ProductID`. En primer lugar, el código prepara la tabla de destino mediante la eliminación de todas las filas y luego inserta una sola fila cuyo `ProductID` se sabe que existe en la tabla de origen. De forma predeterminada, se genera un nuevo valor para la columna de identidad en la tabla de destino para cada fila agregada. En este ejemplo, cuando se abre la conexión se establece una opción que obliga al proceso de carga masiva a usar los valores de identidad de la tabla de origen en su lugar.  
  
La operación de copia masiva se ejecuta con la propiedad `BatchSize` establecida en 10. Cuando la operación encuentra la fila no válida, se produce una excepción. En este primer ejemplo, la operación de copia masiva es sin transacciones. Se confirman todos los lotes copiados hasta el punto del error; el lote que contiene la clave duplicada se revierte y la operación de copia masiva se detiene antes de procesar el resto de los lotes.  
  
> [!NOTE]  
> Este ejemplo no se ejecutará a menos que haya creado las tablas de trabajo como se describe en [Configuración de tabla](#table-setup). Este código se proporciona a fin de ilustrar únicamente la sintaxis para usar `SQLServerBulkCopy`. Si las tablas de origen y destino se encuentran en la misma instancia de SQL Server, es más fácil y rápido usar una instrucción Transact-SQL INSERT... Instrucción SELECT para copiar los datos.  

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopyOptions;

public class BulkCopyNonTransacted {
    public static void main(String[] args) {
        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=AdventureWorks;user=<user>;password=<password>";
        String destinationTable = "dbo.BulkCopyDemoMatchingColumns";
        int countBefore, countAfter;
        ResultSet rsSourceData;

        try (Connection sourceConnection = DriverManager.getConnection(connectionUrl);
                Statement stmt = sourceConnection.createStatement();
                SQLServerBulkCopy bulkCopy = new SQLServerBulkCopy(connectionUrl)) {

            // Empty the destination table.
            stmt.executeUpdate("DELETE FROM " + destinationTable);

            // Add a single row that will result in duplicate key
            // when all rows from source are bulk copied.
            // Note that this technique will only be successful in
            // illustrating the point if a row with ProductID = 446
            // exists in the AdventureWorks Production.Products table.
            // If you have made changes to the data in this table, change
            // the SQL statement in the code to add a ProductID that
            // does exist in your version of the Production.Products
            // table. Choose any ProductID in the middle of the table
            // (not first or last row) to best illustrate the result.
            stmt.executeUpdate("SET IDENTITY_INSERT " + destinationTable + " ON;" + "INSERT INTO " + destinationTable
                    + "([ProductID], [Name] ,[ProductNumber]) VALUES(446, 'Lock Nut 23','LN-3416'); SET IDENTITY_INSERT " + destinationTable
                    + " OFF");

            // Perform an initial count on the destination table.
            countBefore = getRowCount(stmt, destinationTable);

            // Get data from the source table as a ResultSet.
            rsSourceData = stmt.executeQuery("SELECT ProductID, Name, ProductNumber FROM Production.Product");

            // Set up the bulk copy object using the KeepIdentity option and BatchSize = 10.
            SQLServerBulkCopyOptions copyOptions = new SQLServerBulkCopyOptions();
            copyOptions.setKeepIdentity(true);
            copyOptions.setBatchSize(10);

            bulkCopy.setBulkCopyOptions(copyOptions);
            bulkCopy.setDestinationTableName(destinationTable);

            // Write from the source to the destination.
            // This should fail with a duplicate key error
            // after some of the batches have been copied.
            try {
                bulkCopy.writeToServer(rsSourceData);
            }
            catch (SQLException e) {
                e.printStackTrace();
            }

            // Perform a final count on the destination
            // table to see how many rows were added.
            countAfter = getRowCount(stmt, destinationTable);
            System.out.println((countAfter - countBefore) + " rows were added.");
        }
        // Handle any errors that may have occurred.
        catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static int getRowCount(Statement stmt,
            String tableName) throws SQLException {
        ResultSet rs = stmt.executeQuery("SELECT COUNT(*) FROM " + tableName);
        rs.next();
        int count = rs.getInt(1);
        rs.close();
        return count;
    }
}
```

### <a name="performing-a-dedicated-bulk-copy-operation-in-a-transaction"></a>Realización de una operación de copia masiva dedicada en una transacción

De forma predeterminada, una operación de copia masiva no crea transacciones. Si quiere realizar una operación de copia masiva dedicada, cree una instancia de `SQLServerBulkCopy` con una cadena de conexión. En este escenario, la base de datos confirma de forma implícita cada lote de la operación de copia masiva. Puede establecer la opción `UseInternalTransaction` en `true` en `SQLServerBulkCopyOptions` para hacer que la operación de copia masiva cree transacciones, y realizar una confirmación después de cada lote de la operación de copia masiva.
  
```java
SQLServerBulkCopyOptions copyOptions = new SQLServerBulkCopyOptions();
copyOptions.setKeepIdentity(true);
copyOptions.setBatchSize(10);
copyOptions.setUseInternalTransaction(true);
```

### <a name="using-existing-transactions"></a>Uso de transacciones existentes

Puede pasar un objeto `Connection` con las transacciones habilitadas como un parámetro en un constructor de `SQLServerBulkCopy`. En esta situación, la operación de copia masiva se realiza en una transacción existente y el estado de la transacción no sufre ningún cambio (es decir, ni se confirma ni se anula). Esto permite que una aplicación incluya la operación de copia masiva en una transacción con otras operaciones de base de datos. Si se produce un error y tiene que revertir toda la operación de copia masiva, o bien si la copia masiva debe ejecutarse como parte de un proceso mayor que se pueda revertir, puede realizar la reversión en el objeto `Connection` en cualquier momento después de la operación de copia masiva.  
  
La aplicación siguiente es similar a `BulkCopyNonTransacted`, con una excepción: en este ejemplo, la operación de copia masiva se incluye en una transacción externa más grande. Cuando se produce la infracción de la clave principal, toda la transacción se revierte y no se agrega ninguna fila a la tabla de destino.

> [!NOTE]  
> Este ejemplo no se ejecutará a menos que haya creado las tablas de trabajo como se describe en [Configuración de tabla](#table-setup). Este código se proporciona a fin de ilustrar únicamente la sintaxis para usar `SQLServerBulkCopy`. Si las tablas de origen y destino se encuentran en la misma instancia de SQL Server, es más fácil y rápido usar una instrucción Transact-SQL INSERT... Instrucción SELECT para copiar los datos.  

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopyOptions;

public class BulkCopyExistingTransactions {
    public static void main(String[] args) {
        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=AdventureWorks;user=<user>;password=<password>";
        String destinationTable = "dbo.BulkCopyDemoMatchingColumns";
        int countBefore, countAfter;
        ResultSet rsSourceData;
        SQLServerBulkCopyOptions copyOptions;

        try (Connection sourceConnection = DriverManager.getConnection(connectionUrl);
                Connection destinationConnection = DriverManager.getConnection(connectionUrl);
                Statement stmt = sourceConnection.createStatement();
                SQLServerBulkCopy bulkCopy = new SQLServerBulkCopy(destinationConnection);) {

            // Empty the destination table.
            stmt.executeUpdate("DELETE FROM " + destinationTable);

            // Add a single row that will result in duplicate key
            // when all rows from source are bulk copied.
            // Note that this technique will only be successful in
            // illustrating the point if a row with ProductID = 446
            // exists in the AdventureWorks Production.Products table.
            // If you have made changes to the data in this table, change
            // the SQL statement in the code to add a ProductID that
            // does exist in your version of the Production.Products
            // table. Choose any ProductID in the middle of the table
            // (not first or last row) to best illustrate the result.
            stmt.executeUpdate("SET IDENTITY_INSERT " + destinationTable + " ON;" + "INSERT INTO " + destinationTable
                    + "([ProductID], [Name] ,[ProductNumber]) VALUES(446, 'Lock Nut 23','LN-3416'); SET IDENTITY_INSERT " + destinationTable
                    + " OFF");

            // Perform an initial count on the destination table.
            countBefore = getRowCount(stmt, destinationTable);

            // Get data from the source table as a ResultSet.
            rsSourceData = stmt.executeQuery("SELECT ProductID, Name, ProductNumber FROM Production.Product");

            // Set up the bulk copy object inside the transaction.
            destinationConnection.setAutoCommit(false);

            copyOptions = new SQLServerBulkCopyOptions();
            copyOptions.setKeepIdentity(true);
            copyOptions.setBatchSize(10);

            bulkCopy.setBulkCopyOptions(copyOptions);
            bulkCopy.setDestinationTableName(destinationTable);

            // Write from the source to the destination.
            // This should fail with a duplicate key error.
            try {
                bulkCopy.writeToServer(rsSourceData);
                destinationConnection.commit();
            }
            catch (SQLException e) {
                e.printStackTrace();
                destinationConnection.rollback();
            }

            // Perform a final count on the destination
            // table to see how many rows were added.
            countAfter = getRowCount(stmt, destinationTable);
            System.out.println((countAfter - countBefore) + " rows were added.");
        }
        catch (Exception e) {
            // Handle any errors that may have occurred.
            e.printStackTrace();
        }
    }

    private static int getRowCount(Statement stmt,
            String tableName) throws SQLException {
        ResultSet rs = stmt.executeQuery("SELECT COUNT(*) FROM " + tableName);
        rs.next();
        int count = rs.getInt(1);
        rs.close();
        return count;
    }
}
```

### <a name="bulk-copy-from-a-csv-file"></a>Copia masiva desde un archivo CSV  

 En la aplicación siguiente se muestra cómo cargar datos mediante la clase `SQLServerBulkCopy`. En este ejemplo, se usa un archivo CSV para copiar datos exportados de la tabla Production.Product en la base de datos de AdventureWorks de SQL Server a una tabla similar en la base de datos.  
  
> [!IMPORTANT]  
> Este ejemplo no se ejecutará a menos que haya creado las tablas de trabajo como se describe en [Configuración de tabla](#table-setup) para obtenerlo.  
  
1. Abra **SQL Server Management Studio** y conéctese a SQL Server con la base de datos de AdventureWorks.  
  
2. Expanda las bases de datos, haga clic con el botón derecho en la base de datos de AdventureWorks, seleccione **Tareas** y **Exportar datos**...  
  
3. Para el origen de datos, seleccione el **origen de datos** que le permita conectarse a SQL Server (por ejemplo, SQL Server Native Client 11.0), compruebe la configuración y, después, seleccione **Siguiente**.  
  
4. Para el destino, seleccione el **Destino de archivo plano** y escriba un **Nombre de archivo** con un destino como `C:\Test\TestBulkCSVExample.csv`. Compruebe que el **formato** está delimitado, el **calificador de texto** es ninguno y habilite **Nombres de columna en la primera fila de datos**; después, haga clic en **Siguiente**.  
  
5. Seleccione **Escribir una consulta para especificar los datos que se van a transferir** y haga clic en **Siguiente**.  Escriba la **instrucción SQL** `SELECT ProductID, Name, ProductNumber FROM Production.Product` y seleccione **Siguiente**.  
  
6. Comprobar la configuración: puede dejar el delimitador de filas como `{CR}{LF}` y el delimitador de columna como Coma `{,}`.  Seleccione **Editar asignaciones** y compruebe que el **Tipo de datos** es correcto para cada columna (por ejemplo, entero para `ProductID` y cadena Unicode para las demás).  
  
7. Vaya directamente a **Finalizar** y ejecute la exportación.  

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerBulkCSVFileRecord;
import com.microsoft.sqlserver.jdbc.SQLServerBulkCopy;

public class BulkCopyCSV {
    public static void main(String[] args) {
        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=AdventureWorks;user=<user>;password=<password>";
        String destinationTable = "dbo.BulkCopyDemoMatchingColumns";
        int countBefore, countAfter;

        // Get data from the source file by loading it into a class that implements ISQLServerBulkRecord.
        // Here we are using the SQLServerBulkCSVFileRecord implementation to import the example CSV file.
        try (Connection destinationConnection = DriverManager.getConnection(connectionUrl);
                Statement stmt = destinationConnection.createStatement();
                SQLServerBulkCopy bulkCopy = new SQLServerBulkCopy(destinationConnection);
                SQLServerBulkCSVFileRecord fileRecord = new SQLServerBulkCSVFileRecord("C:\\Test\\TestBulkCSVExample.csv", true);) {

            // Set the metadata for each column to be copied.
            fileRecord.addColumnMetadata(1, null, java.sql.Types.INTEGER, 0, 0);
            fileRecord.addColumnMetadata(2, null, java.sql.Types.NVARCHAR, 50, 0);
            fileRecord.addColumnMetadata(3, null, java.sql.Types.NVARCHAR, 25, 0);

            // Empty the destination table.
            stmt.executeUpdate("DELETE FROM " + destinationTable);

            // Perform an initial count on the destination table.
            countBefore = getRowCount(stmt, destinationTable);

            // Set up the bulk copy object.
            // Note that the column positions in the source
            // data reader match the column positions in
            // the destination table so there is no need to
            // map columns.
            bulkCopy.setDestinationTableName(destinationTable);

            // Write from the source to the destination.
            bulkCopy.writeToServer(fileRecord);

            // Perform a final count on the destination
            // table to see how many rows were added.
            countAfter = getRowCount(stmt, destinationTable);
            System.out.println((countAfter - countBefore) + " rows were added.");
        }
        // Handle any errors that may have occurred.
        catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static int getRowCount(Statement stmt,
            String tableName) throws SQLException {
        ResultSet rs = stmt.executeQuery("SELECT COUNT(*) FROM " + tableName);
        rs.next();
        int count = rs.getInt(1);
        rs.close();
        return count;
    }
}
```  

### <a name="bulk-copy-with-delimiters-as-data-in-csv-file"></a>Copia masiva con delimitadores como datos en un archivo CSV

La versión 8.4.1 del controlador agrega una nueva API `SQLServerBulkCSVFileRecord.setEscapeColumnDelimitersCSV(boolean)`. Al establecerse en true, se aplicarán las siguientes reglas:

- Cada campo se puede incluir o no entre comillas dobles.
- Si los campos no se incluyen entre comillas dobles, es posible que no aparezcan comillas dobles dentro de los campos.
- Los campos que contienen comillas dobles y los delimitadores deben incluirse entre comillas dobles.
- Si se usan comillas dobles para incluir los campos, las comillas dobles que aparecen dentro de un campo deben tener caracteres de escape mediante la inclusión de otras comillas dobles delante.

### <a name="bulk-copy-with-always-encrypted-columns"></a>Copia masiva con columnas Always Encrypted  

A partir de Microsoft JDBC Driver 6.0 para SQL Server, la copia masiva es compatible con las columnas Always Encrypted.  
  
En función de las opciones de copia masiva y el tipo de cifrado de las tablas de origen y destino, el controlador JDBC puede descifrar los datos de forma transparente y después volver a cifrarlos, o bien puede enviar los datos cifrados tal como están. Por ejemplo, cuando se copian datos masivamente desde una columna cifrada a una columna sin cifrar, el controlador descifra de forma transparente los datos antes de enviarlos a SQL Server. De igual forma, cuando se copian datos masivamente desde una columna sin cifrar (o desde un archivo .csv) a una columna cifrada, el controlador descifra de forma transparente los datos antes de enviarlos a SQL Server. Si tanto el origen como el destino están cifrados, según la opción de copa masiva `allowEncryptedValueModifications`, el controlador enviaría los datos tal como están o los descifraría y los volvería a cifrar antes de enviarlos a SQL Server.  
  
Para obtener más información, vea la opción de copia masiva `allowEncryptedValueModifications` siguiente y [Uso de Always Encrypted con el controlador JDBC](using-always-encrypted-with-the-jdbc-driver.md).  
  
> [!IMPORTANT]  
> Limitación de Microsoft JDBC Driver 6.0 para SQL Server, al copiar masivamente datos desde un archivo .csv a columnas cifradas:  
>
> Solo el tipo de formato de literal de cadena predeterminado Transact-SQL es compatible con los tipos de fecha y hora  
>
> No se admiten los tipos de datos DATETIME y SMALLDATETIME  
  
## <a name="bulk-copy-api-for-jdbc-driver"></a>API de copia masiva para el controlador JDBC  
  
### <a name="sqlserverbulkcopy"></a>SQLServerBulkCopy

Le permite la carga masiva de forma eficaz de una tabla de SQL Server con datos procedentes de otro origen.  
  
Microsoft SQL Server incluye una conocida utilidad del símbolo del sistema denominada bcp para mover datos de una tabla a otra, ya sea en un único servidor o entre servidores. La clase `SQLServerBulkCopy` le permite escribir soluciones de código en Java que proporcionan una funcionalidad similar. Hay otras maneras de cargar datos en una tabla de SQL Server (las instrucciones INSERT, por ejemplo), pero `SQLServerBulkCopy` ofrece una importante ventaja de rendimiento sobre ellas.  
  
La clase `SQLServerBulkCopy` puede usarse para escribir datos solo en tablas de SQL Server. Pero el origen de datos no se limita a SQL Server; se puede usar cualquiera, siempre que los datos se puedan leer con una instancia de `ResultSet` o una implementación de `ISQLServerBulkRecord`.  
  
| Constructor | Descripción |
| ----------- | ----------- |
| `SQLServerBulkCopy(Connection connection)` | Inicializa una instancia nueva de la clase `SQLServerBulkCopy` mediante la instancia abierta especificada de `SQLServerConnection`. Si `Connection` tiene habilitadas las transacciones, las operaciones de copia se realizarán dentro de esa transacción. |
| `SQLServerBulkCopy(String connectionURL)` | Inicializa y abre una instancia nueva de `SQLServerConnection` en función del valor `connectionURL` proporcionado. El constructor usa `SQLServerConnection` para inicializar una instancia nueva de la clase `SQLServerBulkCopy`. |
  
| Propiedad | Descripción |
| -------- | ----------- |
| `String DestinationTableName` | Nombre de la tabla de destino en el servidor.<br /><br /> Si `DestinationTableName` no se ha establecido al llamar a `writeToServer`, se inicia una excepción `SQLServerException`.<br /><br /> `DestinationTableName` es un nombre de tres partes (`<database>.<owningschema>.<name>`). Si quiere, puede calificar el nombre de tabla con su base de datos y esquema propietario. Sin embargo, si el nombre de tabla usa un carácter de subrayado ("_") o cualquier otro carácter especial, el nombre ha de ir entre corchetes. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md). |
| `ColumnMappings` | Las asignaciones de columnas definen las relaciones entre las columnas del origen de datos y las columnas en el destino.<br /><br /> Si no se definen las asignaciones, las columnas se asignan de forma implícita según la posición ordinal. Para que esto funcione, los esquemas de origen y destino deben coincidir. Si no es así, se producirá una excepción.<br /><br /> Si las asignaciones no están vacías, no hay que especificar todas las columnas presentes en el origen de datos. Se ignoran aquellas que no estén asignadas.<br /><br /> Puede hacer referencia a las columnas de origen y de destino tanto por nombre como por ordinal. |
  
| Método | Descripción |
| ------ | ----------- |
| `void addColumnMapping(int sourceColumn, int destinationColumn)` | Agrega una nueva asignación de columnas, usando las posiciones ordinales para especificar tanto las columnas de origen como las de destino. |
| `void addColumnMapping (int sourceColumn, String destinationColumn)` | Agrega una nueva asignación de columnas, con un valor ordinal para la columna de origen y un nombre de columna para la columna de destino. |
| `void addColumnMapping (String sourceColumn, int destinationColumn)` | Agrega una nueva asignación de columnas, con un nombre de columna para describir la columna de origen y un valor ordinal para especificar la columna de destino. |
| `void addColumnMapping (String sourceColumn, String destinationColumn)` | Agrega una nueva asignación de columnas, usando los nombres de columna para especificar tanto las columnas de origen como las de destino. |
| `void clearColumnMappings()` | Elimina el contenido de las asignaciones de columnas. |
| `void close()` | Cierra la instancia de `SQLServerBulkCopy`. |
| `SQLServerBulkCopyOptions getBulkCopyOptions()` | Recupera el conjunto actual de `SQLServerBulkCopyOptions`. |
| `String getDestinationTableName()` | Recupera el nombre de tabla de destino actual. |
| `void setBulkCopyOptions(SQLServerBulkCopyOptions copyOptions)` | Actualiza el comportamiento de la instancia de `SQLServerBulkCopy` según las opciones proporcionadas. |
| `void setDestinationTableName(String tableName)` | Establece el nombre de la tabla de destino. |
| `void writeToServer(ResultSet sourceData)` | Copia todas las filas del objeto `ResultSet` proporcionado en una tabla de destino especificada por la propiedad `DestinationTableName` del objeto `SQLServerBulkCopy`. |
| `void writeToServer(RowSet sourceData)` | Copia todas las filas del objeto `RowSet` proporcionado en una tabla de destino especificada por la propiedad `DestinationTableName` del objeto `SQLServerBulkCopy`. |
| `void writeToServer(ISQLServerBulkRecord sourceData)` | Copia todas las filas de la implementación de `ISQLServerBulkRecord` proporcionada en una tabla de destino especificada por la propiedad `DestinationTableName` del objeto `SQLServerBulkCopy`. |
  
### <a name="sqlserverbulkcopyoptions"></a>SQLServerBulkCopyOptions  

 Una colección de valores que controlan cómo se comportan los métodos `writeToServer` en una instancia de `SQLServerBulkCopy`.  
  
| Constructor | Descripción |
| ----------- | ----------- |
| `SQLServerBulkCopyOptions()` | Inicializa una instancia nueva de la clase `SQLServerBulkCopyOptions` con los valores predeterminados para todas las configuraciones. |
  
 Los captadores y establecedores están disponibles para las siguientes opciones:  
  
| Opción | Descripción | Valor predeterminado |
| ------ | ----------- | ------- |
| `boolean CheckConstraints` | Comprueba las restricciones mientras se insertan los datos. | False: no se comprueban las restricciones |
| `boolean FireTriggers` | Provoca que el servidor active los desencadenadores de inserción para las filas que se insertan en la base de datos. | False: no se activa ningún desencadenador |
| `boolean KeepIdentity` | Mantiene los valores de identidad de origen. | False: el destino asigna los valores de identidad |
| `boolean KeepNulls` | Conserva los valores nulos en la tabla de destino independientemente de la configuración para los valores predeterminados. | False: los valores nulos se reemplazan por valores predeterminados si procede |
| `boolean TableLock` | Obtiene un bloqueo de actualización masiva durante toda la operación de copia masiva. | False: se usan bloqueos de fila. |
| `boolean UseInternalTransaction` | Cuando se establece en `true`, cada lote de la operación de copia masiva tendrá lugar en una transacción. Si `SQLServerBulkCopy` usa una conexión existente (como se especifica en el constructor), se iniciará una excepción `SQLServerException`.  Si `SQLServerBulkCopy` ha creado una conexión dedicada, se creará y confirmará una transacción para cada lote. | False: no hay ninguna transacción |
| `int BatchSize` | Número de filas en cada lote. Al final de cada lote, las filas del lote se envían al servidor.<br /><br /> Un lote estará completo cuando se hayan procesado las filas indicadas por `BatchSize` o no haya más filas para enviar al origen de datos de destino.  Si la instancia de `SQLServerBulkCopy` se ha declarado con la opción `UseInternalTransaction` establecida en `false`, se envían al servidor `BatchSize` filas a la vez, pero no se realiza ninguna acción relacionada con la transacción. Si `UseInternalTransaction` se establece en `true`, cada lote de filas se realiza dentro de una transacción explícita. | 0: indica que cada operación `writeToServer` es un lote único |
| `int BulkCopyTimeout` | Cantidad de segundos para que se complete la operación antes de que se agote el tiempo de espera. Un valor de 0 indica sin límite; la copia masiva esperará indefinidamente. | 60 segundos. |
| `boolean allowEncryptedValueModifications` | Esta opción está disponible con Microsoft JDBC Driver 6.0 (o superior) para SQL Server.<br /><br /> Cuando se establece en `true`, `allowEncryptedValueModifications` habilita la copia masiva de datos cifrados entre tablas o bases de datos, sin descifrarlos. Normalmente, una aplicación seleccionaría datos de las columnas cifradas de una tabla sin descifrar los datos (la aplicación se conectaría a la base de datos con la palabra clave de configuración de cifrado de columnas establecida en deshabilitada) y después usaría esta opción para insertar masivamente los datos, que siguen estando cifrados. Para obtener más información, vea [Uso de Always Encrypted con JDBC Driver](using-always-encrypted-with-the-jdbc-driver.md).<br /><br /> Tenga cuidado al establecer `allowEncryptedValueModifications` en `true`, ya que puede provocar daños en la base de datos porque el controlador no comprueba si los datos están realmente cifrados, o si se han cifrado correctamente mediante la misma clave, algoritmo y tipo de cifrado que la columna de destino. |
  
 Captadores y establecedores:  
  
| Métodos | Descripción |
| ------- | ----------- |
| `boolean isCheckConstraints()` | Indica si las restricciones deben comprobarse mientras se insertan o no los datos. |
| `void setCheckConstraints(boolean checkConstraints)` | Establece si las restricciones deben comprobarse mientras se insertan o no los datos. |
| `boolean isFireTriggers()` | Indica si el servidor debe activar los desencadenadores de inserción para las filas que se insertan en la base de datos. |
| `void setFireTriggers(boolean fireTriggers)` | Establece si el servidor se debe configurar para activar los desencadenadores de inserción para las filas que se insertan en la base de datos. |
| `boolean isKeepIdentity()` | Indica si se deben conservar o no los valores de identidad de origen. |
| `void setKeepIdentity(boolean keepIdentity)` | Establece si se deben conservar o no los valores de identidad. |
| `boolean isKeepNulls()` | Indica si se deben conservar valores NULL en la tabla de destino independientemente de la configuración de los valores predeterminados, o bien si deben reemplazarse por los valores predeterminados (si procede). |
| `void setKeepNulls(boolean keepNulls)` | Establece si se deben conservar valores NULL en la tabla de destino independientemente de la configuración de los valores predeterminados, o bien si deben reemplazarse por los valores predeterminados (si procede). |
| `boolean isTableLock()` | Indica si `SQLServerBulkCopy` debe obtener un bloqueo de actualización masiva durante toda la operación de copia masiva. |
| `void setTableLock(boolean tableLock)` | Establece si `SQLServerBulkCopy` debe obtener un bloqueo de actualización masiva durante toda la operación de copia masiva. |
| `boolean isUseInternalTransaction()` | Indica si cada lote de la operación de copia masiva tendrá lugar en una transacción. |
| `void setUseInternalTranscation(boolean useInternalTransaction)` | Establece si cada lote de las operaciones de copia masiva tendrá lugar en una transacción o no. |
| `int getBatchSize()` | Obtiene el número de filas en cada lote. Al final de cada lote, las filas del lote se envían al servidor. |
| `void setBatchSize(int batchSize)` | Establece el número de filas en cada lote. Al final de cada lote, las filas del lote se envían al servidor. |
| `int getBulkCopyTimeout()` | Obtiene la cantidad de segundos para que se complete la operación antes de que se agote el tiempo de espera. |
| `void  setBulkCopyTimeout(int timeout)` | Establece la cantidad de segundos para que se complete la operación antes de que se agote el tiempo de espera. |
| `boolean isAllowEncryptedValueModifications()` | Indica si el valor `allowEncryptedValueModifications` está habilitado o deshabilitado. |
| `void setAllowEncryptedValueModifications(boolean allowEncryptedValueModifications)` | Configura la opción `allowEncryptedValueModifications` que se usa para la copia masiva con columnas Always Encrypted. |
  
### <a name="isqlserverbulkrecord"></a>ISQLServerBulkRecord  

 La interfaz `ISQLServerBulkRecord` se puede usar para crear las clases que leen datos de cualquier origen (como un archivo) y permitir que una instancia de `SQLServerBulkCopy` cargue de forma masiva una tabla de SQL Server con esos datos.  
  
| Métodos de interfaz | Descripción |
| ----------------- | ----------- |
| `set<Integer> getColumnOrdinals()` | Obtiene los ordinales para cada una de las columnas representadas en este registro de datos. |
| `String getColumnName(int column)` | Obtiene el nombre de una columna determinada. |
| `int getColumnType(int column)` | Obtiene el tipo de datos JDBC de la columna especificada. |
| `int getPrecision(int column)`  | Obtiene la precisión de la columna especificada. |
| `object[] getRowData()` | Obtiene los datos de la fila actual como una matriz de objetos.<br /><br /> Cada objeto debe coincidir con el tipo de lenguaje Java que se usa para representar el tipo de datos JDBC indicado para la columna especificada.  Para obtener más información, vea [Descripción de los tipos de datos del controlador JDBC](understanding-the-jdbc-driver-data-types.md) para obtener las asignaciones adecuadas. |
| `int getScale(int column)` | Obtiene la escala de la columna especificada. |
| `boolean isAutoIncrement(int column)` | Indica si la columna es representa una columna de identidad. |
| `boolean next()` | Avanza a la siguiente fila de datos. |
  
### <a name="sqlserverbulkcsvfilerecord"></a>SQLServerBulkCSVFileRecord  

Se puede usar una implementación sencilla de la interfaz `ISQLServerBulkRecord` para leer en los tipos de datos básicos de Java desde un archivo delimitado, donde cada línea representa una fila de datos.  
  
Limitaciones y notas de implementación:  
  
1. La cantidad máxima de datos permitida en una fila determinada está limitada por la memoria disponible, ya que los datos se leen en una línea cada vez.  
  
2. No se admite el streaming de tipos de datos de gran tamaño como `varchar(max)`, `varbinary(max)`, `nvarchar(max)`, `sqlxml` y `ntext`.  
  
3. El delimitador especificado para el archivo CSV no debe aparecer en ninguna parte de los datos y debe convertirse correctamente si es un carácter restringido en expresiones regulares de Java.  
  
4. En la implementación del archivo CSV, las comillas dobles se tratan como parte de los datos. Por ejemplo, si el delimitador es una coma, la línea `hello,"world","hello,world"` se tratará como si tuviera cuatro columnas con los valores `hello`, `"world"`, `"hello` y `world"`.  
  
5. Los caracteres de nueva línea se usan como terminadores de fila y no están permitidos en ninguna parte de los datos.  
  
| Constructor | Descripción |
| ----------- | ----------- |
| `SQLServerBulkCSVFileRecord(String fileToParse, String encoding, String delimiter, boolean firstLineIsColumnNames)` | Inicializa una instancia nueva de la clase `SQLServerBulkCSVFileRecord` que analizará cada línea de `fileToParse` con el delimitador y la codificación proporcionados. Si `firstLineIsColumnNames` se establece en True, la primera línea del archivo se analizará como nombres de columna.  Si la codificación es NULL, se usará la codificación predeterminada. |
| `SQLServerBulkCSVFileRecord(String fileToParse, String encoding, boolean firstLineIsColumnNames)` | Inicializa una instancia nueva de la clase `SQLServerBulkCSVFileRecord` que analizará cada línea de `fileToParse` con una coma como delimitador y la codificación proporcionada. Si `firstLineIsColumnNames` se establece en True, la primera línea del archivo se analizará como nombres de columna.  Si la codificación es NULL, se usará la codificación predeterminada. |
| `SQLServerBulkCSVFileRecord(String fileToParse, boolean firstLineIsColumnNames` | Inicializa una instancia nueva de la clase `SQLServerBulkCSVFileRecord` que analizará cada línea de `fileToParse` con una coma como delimitador y la codificación predeterminada. Si `firstLineIsColumnNames` se establece en True, la primera línea del archivo se analizará como nombres de columna. |
  
| Método | Descripción |
| ------ | ----------- |
| `void addColumnMetadata(int positionInFile, String columnName, int jdbcType, int precision, int scale)` | Agrega metadatos de la columna especificada en el archivo. |
| `void close()` | Libera los recursos asociados con el lector de archivos. |
| `void setTimestampWithTimezoneFormat(DateTimeFormatter dateTimeFormatter)` | Establece el formato para analizar los datos Timestamp desde el archivo como `java.sql.Types.TIMESTAMP_WITH_TIMEZONE`. |
| `void setTimestampWithTimezoneFormat(String dateTimeFormat)` | Establece el formato para analizar los datos Time desde el archivo como `java.sql.Types.TIME_WITH_TIMEZONE`. |
| `void setTimeWithTimezoneFormat(DateTimeFormatter dateTimeFormatter)` | Establece el formato para analizar los datos Time desde el archivo como `java.sql.Types.TIME_WITH_TIMEZONE`. |
| `void setTimeWithTimezoneFormat(String timeFormat)` | Establece el formato para analizar los datos Time desde el archivo como `java.sql.Types.TIME_WITH_TIMEZONE`. |
  
## <a name="see-also"></a>Consulte también  

[Introducción al controlador JDBC](overview-of-the-jdbc-driver.md)  
