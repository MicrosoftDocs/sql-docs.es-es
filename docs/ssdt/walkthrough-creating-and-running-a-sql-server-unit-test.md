---
title: Crear y ejecutar una prueba unitaria de SQL Server
description: Aprenda a crear una prueba unitaria de SQL Server. Siga los pasos de configuración de una prueba que detecta un error en un procedimiento almacenado.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 992c1d8e-3729-438b-9ef4-cd103e28f145
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6b0eaf98f4d10c08548e898c261a489ad71f0ff4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016735"
---
# <a name="walkthrough-creating-and-running-a-sql-server-unit-test"></a>Tutorial: Crear y ejecutar una prueba unitaria de SQL Server

En este tutorial, se crea una prueba unitaria de SQL Server que comprueba el comportamiento de varios procedimientos almacenados. Las pruebas unitarias de SQL Server se crean para ayudar a identificar los defectos del código que podrían producir un comportamiento incorrecto de la aplicación. Las pruebas unitarias de SQL Server y las pruebas de aplicación se pueden ejecutar como parte de un conjunto de pruebas automatizado.  
  
En este tutorial, realizará las tareas siguientes:  
  
-   [Crear un script que contiene un esquema de la base de datos](#CreateScript)  
  
-   [Crear un nuevo proyecto de base de datos e importar el esquema](#CreateProjectAndImport)  
  
-   [Implementar el proyecto de base de datos en un entorno de desarrollo aislado](#DeployDBProj)  
  
-   [Crear pruebas unitarias de SQL Server](#CreateDBUnitTests)  
  
-   [Definir la lógica de prueba](#DefineTestLogic)  
  
-   [Ejecutar pruebas unitarias de SQL Server](#RunTests)  
  
-   [Agregar una prueba unitaria negativa](#NegativeTest)  
  
Cuando una de las pruebas unitarias detecta un error en un procedimiento almacenado, se corrige el error y se vuelve a ejecutar la prueba.  
  
## <a name="prerequisites"></a>Requisitos previos  
Para completar este tutorial, debe poder conectarse a un servidor de bases de datos (o a una base de datos LocalDB) en el que tenga permisos para crear e implementar una base de datos. Para más información, consulte [Permisos necesarios para las características de base de datos de Visual Studio](/previous-versions/visualstudio/visual-studio-2010/aa833413(v=vs.100)).  
  
## <a name="create-a-script-that-contains-a-database-schema"></a><a name="CreateScript"></a>Crear un script que contiene un esquema de la base de datos  
  
#### <a name="to-create-a-script-from-which-you-can-import-a-schema"></a>Para crear un script desde el que se puede importar un esquema  
  
1.  En el menú **Archivo** , elija **Nuevo** y haga clic en **Archivo**.  
  
    Aparece el cuadro de diálogo **Nuevo archivo** .  
  
2.  En la lista **Categorías** , haga clic en **General** si no está ya resaltado.  
  
3.  En la lista **Plantillas** , haga clic en **Archivo SQL** y en **Abrir**.  
  
    Se abre el editor Transact\-SQL.  
  
4.  Copie el código Transact\-SQL y péguelo en el editor de Transact\-SQL.  
  
    ```  
    PRINT N'Creating Sales...';  
    GO  
    CREATE SCHEMA [Sales]  
        AUTHORIZATION [dbo];  
    GO  
    PRINT N'Creating Sales.Customer...';  
    GO  
    CREATE TABLE [Sales].[Customer] (  
        [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
        [CustomerName] NVARCHAR (40) NOT NULL,  
        [YTDOrders]    INT           NOT NULL,  
        [YTDSales]     INT           NOT NULL  
    );  
    GO  
    PRINT N'Creating Sales.Orders...';  
    GO  
    CREATE TABLE [Sales].[Orders] (  
        [CustomerID] INT      NOT NULL,  
        [OrderID]    INT      IDENTITY (1, 1) NOT NULL,  
        [OrderDate]  DATETIME NOT NULL,  
        [FilledDate] DATETIME NULL,  
        [Status]     CHAR (1) NOT NULL,  
        [Amount]     INT      NOT NULL  
    );  
    GO  
    PRINT N'Creating Sales.Def_Customer_YTDOrders...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [Def_Customer_YTDOrders] DEFAULT 0 FOR [YTDOrders];  
    GO  
    PRINT N'Creating Sales.Def_Customer_YTDSales...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [Def_Customer_YTDSales] DEFAULT 0 FOR [YTDSales];  
    GO  
    PRINT N'Creating Sales.Def_Orders_OrderDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [Def_Orders_OrderDate] DEFAULT GetDate() FOR [OrderDate];  
    GO  
    PRINT N'Creating Sales.Def_Orders_Status...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [Def_Orders_Status] DEFAULT 'O' FOR [Status];  
    GO  
    PRINT N'Creating Sales.PK_Customer_CustID...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [PK_Customer_CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) WITH (ALLOW_PAGE_LOCKS = ON, ALLOW_ROW_LOCKS = ON, PAD_INDEX = OFF, IGNORE_DUP_KEY = OFF, STATISTICS_NORECOMPUTE = OFF);  
    GO  
    PRINT N'Creating Sales.PK_Orders_OrderID...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [PK_Orders_OrderID] PRIMARY KEY CLUSTERED ([OrderID] ASC) WITH (ALLOW_PAGE_LOCKS = ON, ALLOW_ROW_LOCKS = ON, PAD_INDEX = OFF, IGNORE_DUP_KEY = OFF, STATISTICS_NORECOMPUTE = OFF);  
    GO  
    PRINT N'Creating Sales.FK_Orders_Customer_CustID...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [FK_Orders_Customer_CustID] FOREIGN KEY ([CustomerID]) REFERENCES [Sales].[Customer] ([CustomerID]) ON DELETE NO ACTION ON UPDATE NO ACTION;  
    GO  
    PRINT N'Creating Sales.CK_Orders_FilledDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [CK_Orders_FilledDate] CHECK ((FilledDate >= OrderDate) AND (FilledDate < '01/01/2030'));  
    GO  
    PRINT N'Creating Sales.CK_Orders_OrderDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [CK_Orders_OrderDate] CHECK ((OrderDate > '01/01/2005') and (OrderDate < '01/01/2030'));  
    GO  
    PRINT N'Creating Sales.uspCancelOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspCancelOrder]  
    @OrderID INT  
    AS  
    BEGIN  
    DECLARE @Delta INT, @CustomerID INT  
    BEGIN TRANSACTION  
        SELECT @Delta = [Amount], @CustomerID = [CustomerID]  
         FROM [Sales].[Orders] WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Orders]  
       SET [Status] = 'X'  
    WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Customer]  
       SET  
       YTDOrders = YTDOrders - @Delta  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    END  
    GO  
    PRINT N'Creating Sales.uspFillOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspFillOrder]  
    @OrderID INT, @FilledDate DATETIME  
    AS  
    BEGIN  
    DECLARE @Delta INT, @CustomerID INT  
    BEGIN TRANSACTION  
        SELECT @Delta = [Amount], @CustomerID = [CustomerID]  
         FROM [Sales].[Orders] WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Orders]  
       SET [Status] = 'F',  
           [FilledDate] = @FilledDate  
    WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales - @Delta  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    END  
    GO  
    PRINT N'Creating Sales.uspNewCustomer...';  
    GO  
    CREATE PROCEDURE [Sales].[uspNewCustomer]  
    @CustomerName NVARCHAR (40)  
    AS  
    BEGIN  
    INSERT INTO [Sales].[Customer] (CustomerName) VALUES (@CustomerName);  
    RETURN SCOPE_IDENTITY()  
    END  
    GO  
    PRINT N'Creating Sales.uspPlaceNewOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspPlaceNewOrder]  
    @CustomerID INT, @Amount INT, @OrderDate DATETIME, @Status CHAR (1)='O'  
    AS  
    BEGIN  
    DECLARE @RC INT  
    BEGIN TRANSACTION  
    INSERT INTO [Sales].[Orders] (CustomerID, OrderDate, FilledDate, Status, Amount)   
         VALUES (@CustomerID, @OrderDate, NULL, @Status, @Amount)  
    SELECT @RC = SCOPE_IDENTITY();  
    UPDATE [Sales].[Customer]  
       SET  
       YTDOrders = YTDOrders + @Amount  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    RETURN @RC  
    END  
    GO  
    CREATE PROCEDURE [Sales].[uspShowOrderDetails]  
    @CustomerID INT=0  
    AS  
    BEGIN  
    SELECT [C].[CustomerName], CONVERT(date, [O].[OrderDate]), CONVERT(date, [O].[FilledDate]), [O].[Status], [O].[Amount]  
      FROM [Sales].[Customer] AS C  
      INNER JOIN [Sales].[Orders] AS O  
         ON [O].[CustomerID] = [C].[CustomerID]  
      WHERE [C].[CustomerID] = @CustomerID  
    END  
    GO  
    ```  
  
5.  Guarde el archivo. Anote la ubicación porque debe usar este script en el procedimiento siguiente.  
  
6.  En el menú **Archivo** , haga clic en **Cerrar solución**.  
  
    A continuación creará un proyecto de base de datos e importará el esquema desde el script que ha creado.  
  
## <a name="create-a-database-project-and-import-a-schema"></a><a name="CreateProjectAndImport"></a>Crear un proyecto de base de datos e importar un esquema  
  
#### <a name="to-create-a-database-project"></a>Para crear un proyecto de base de datos  
  
1.  En el menú **Archivo** , elija **Nuevo** y, a continuación, haga clic en **Proyecto**.  
  
    Aparecerá el cuadro de diálogo **Nuevo proyecto** .  
  
2.  En **Plantillas instaladas**, seleccione el nodo **SQL Server** y, a continuación, **Proyecto de base de datos de SQL Server**.  
  
3.  En **Nombre**, escriba **SimpleUnitTestDB**.  
  
4.  Active la casilla **Crear directorio para la solución** si aún no está seleccionada.  
  
5.  Desactive la casilla **Agregar al control de código fuente** si aún no lo está y haga clic en **Aceptar**.  
  
    Se crea el proyecto de base de datos, que aparece en el **Explorador de soluciones**. A continuación, importará el esquema de la base de datos de un script.  
  
#### <a name="to-import-a-database-schema-from-a-script"></a>Para importar un esquema de la base de datos de un script  
  
1.  En el menú **Proyecto**, haga clic en **Importar** y, a continuación, en  **Script (\*.sql)** .  
  
2.  Haga clic en **Siguiente** después de leer la página de bienvenida.  
  
3.  Haga clic en **Examinar** para ir al directorio donde guardó el archivo .sql.  
  
4.  Haga doble clic en el archivo .sql y haga clic en **Finalizar**.  
  
    Se importa el script y los objetos definidos en el script se agregan al proyecto de base de datos.  
  
5.  Revise el resumen y haga clic en **Finalizar** para completar la operación.  
  
    > [!NOTE]  
    > El procedimiento Sales.uspFillOrder contiene un error de código intencional que se detectará y se corregirá más adelante en este procedimiento.  
  
#### <a name="to-examine-the-resulting-project"></a>Para examinar el proyecto resultante  
  
1.  En el **Explorador de soluciones**, examine los archivos de script que se importaron al proyecto.  
  
2.  En el **Explorador de objetos de** , examine la base de datos del nodo Proyectos.  
  
## <a name="deploying-to-localdb"></a><a name="DeployDBProj"></a>Implementar en LocalDB  
De forma predeterminada, al presionar F5 se implementa (o publica) la base de datos en una base de datos LocalDB. Puede cambiar la ubicación de la base de datos si va a la pestaña Depurar de la página de propiedades del proyecto y cambia la cadena de conexión.  
  
## <a name="create-sql-server-unit-tests"></a><a name="CreateDBUnitTests"></a>Crear pruebas unitarias de SQL Server  
  
#### <a name="to-create-a-sql-server-unit-test-for-the-stored-procedures"></a>Para crear una prueba unitaria de SQL Server para los procedimientos almacenados  
  
1.  En el **Explorador de objetos de SQL Server**, expanda el nodo de proyectos de **SimpleUnitTestDB** y, después, expanda los nodos **Programación** y **Procedimientos almacenados**.  
  
2.  Haga clic con el botón derecho en alguno de los procedimientos almacenados y haga clic en **Crear pruebas unitarias** para mostrar el cuadro de diálogo **Crear pruebas unitarias**.  
  
3.  Active las casillas de los cinco procedimientos almacenados: **Sales.uspCancelOrder**, **Sales.uspFillOrder**, **Sales.uspNewCustomer**, **Sales.uspPlaceNewOrder** y **Sales.uspShowOrderDetails**.  
  
4.  En la lista desplegable **Proyecto**, seleccione **Crear un nuevo proyecto de prueba de Visual C#** .  
  
5.  Acepte los nombres predeterminados para el nombre de proyecto y el nombre de clase, y haga clic en **Aceptar**.  
  
6.  En el cuadro de diálogo de configuración de pruebas, en **Ejecutar pruebas unitarias usando la siguiente conexión de datos**, especifique una conexión a la base de datos que implementó anteriormente en este tutorial. Por ejemplo, si usó la ubicación de implementación predeterminada, que es LocalDB, haría clic en **Nueva conexión** y especificaría **(LocalDB)\Projects**. Después, elija el nombre de la base de datos. A continuación, haga clic en Aceptar para cerrar el cuadro de diálogo **Propiedades de la conexión** .  
  
    > [!NOTE]  
    > Si debe probar las vistas o los procedimientos almacenados que tienen permisos restringidos, especificaría normalmente esa conexión en este paso. Después debe especificar la conexión secundaria, con permisos más amplios, para validar la prueba. Si tiene una conexión secundaria, debe agregar el usuario al proyecto de base de datos y crear un inicio de sesión para el usuario en el script anterior a la implementación.  
  
7.  En el cuadro de diálogo de configuración de pruebas, en la sección **Implementación** , active la casilla **Implementar automáticamente el proyecto de base de datos antes de ejecutar pruebas unitarias** .  
  
8.  En **Proyecto de base de datos**, haga clic en **SimpleUnitTestDB.sqlproj**.  
  
9. En **Configuración de implementación**, haga clic en **Depurar**.  
  
    Podría generar también datos de prueba como parte de las pruebas unitarias de SQL Server. En este tutorial, puede omitir este paso porque las pruebas van a crear sus propios datos.  
  
10. Haga clic en **OK**.  
  
    Se compilará el proyecto de prueba y aparecerá el Diseñador de pruebas unitarias de SQL Server. A continuación, actualizará la lógica de prueba en el script Transact\-SQL de las pruebas unitarias.  
  
## <a name="define-test-logic"></a><a name="DefineTestLogic"></a>Definir la lógica de prueba  
Esta base de datos muy simple tiene dos tablas, Customer y Order. Actualiza la base de datos mediante los procedimientos almacenados siguientes:  
  
-   uspNewCustomer: Este procedimiento almacenado agrega un registro a la tabla Customer, que establece las columnas YTDOrders y YTDSales del cliente en cero.  
  
-   uspPlaceNewOrder: Este procedimiento almacenado agrega un registro a la tabla Orders para el cliente especificado y actualiza el valor de YTDOrders en el registro correspondiente en la tabla Customer.  
  
-   uspFillOrder: Este procedimiento almacenado actualiza un registro en la tabla Orders al cambiar el estado de 'O' a 'F' y aumenta la cantidad de YTDSales en el registro correspondiente en la tabla Customer.  
  
-   uspCancelOrder: Este procedimiento almacenado actualiza un registro en la tabla Orders al cambiar el estado de 'O' a 'X' y disminuye la cantidad de YTDOrders en el registro correspondiente en la tabla Customer.  
  
-   uspShowOrderDetails: Este procedimiento almacenado combina la tabla Orders con la tabla Customer y muestra los registros de un cliente específico.  
  
> [!NOTE]  
> En este ejemplo se muestra cómo crear una prueba unitaria simple de SQL Server. En una base de datos del mundo real, podría sumar los importes totales de todos los pedidos con un estado 'O' o 'F' para un determinado cliente. Los procedimientos de este tutorial no contienen tampoco ningún control de errores. Por ejemplo, no le impiden llamar uspFillOrder para un orden que se ha rellenado previamente.  
  
Las pruebas suponen que la base de datos se inicia en un estado limpio. Creará las pruebas que comprueban las condiciones siguientes:  
  
-   uspNewCustomer: Compruebe que la tabla Customer contiene una fila después de ejecutar el procedimiento almacenado.  
  
-   uspPlaceNewOrder: Para el cliente que tiene un CustomerID de 1, haga un pedido de 100 dólares. Compruebe que la cantidad de YTDOrders del cliente es 100 y que la cantidad de YTDSales es cero.  
  
-   uspFillOrder: Para el cliente que tiene un CustomerID de 1, haga un pedido de 50 dólares. Rellene este pedido. Compruebe que las cantidades de YTDOrders e YTDSales son ambos 50.  
  
-   uspShowOrderDetails: Para el cliente que tiene un CustomerID de 1, haga pedidos de 100, 50 y 5 dólares. Compruebe que uspShowOrderDetails devuelve el número de columnas correcto y que el conjunto de resultados tiene la suma de comprobación esperada.  
  
> [!NOTE]  
> Para un conjunto completo de pruebas unitarias de SQL Server, se comprobaría normalmente que las demás columnas se establecieron correctamente. Para mantener este tutorial en un tamaño administrable, no describe cómo comprobar el comportamiento de uspCancelOrder.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspnewcustomer"></a>Para escribir la prueba unitaria de SQL Server para uspNewCustomer  
  
1.  En la barra de navegación del Diseñador de pruebas unitarias de SQL Server, haga clic en **Sales_uspNewCustomerTest** y asegúrese de que **Prueba** está resaltado en la lista adyacente.  
  
    Después de realizar el paso anterior, puede crear el script de prueba para la acción de prueba en la prueba unitaria.  
  
2.  Actualice las instrucciones Transact\-SQL en el editor de Transact\-SQL para que coincidan con las instrucciones siguientes:  
  
    ```  
    -- ssNoVersion unit test for Sales.uspNewCustomer  
    DECLARE @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @CustomerName = 'Fictitious Customer';  
  
    EXECUTE @RC = [Sales].[uspNewCustomer] @CustomerName;  
  
    SELECT * FROM [Sales].[Customer];  
    ```  
  
3.  En el panel **Condiciones de prueba**, haga clic en la Condición de prueba no concluyente y, a continuación, haga clic en el icono **Eliminar condición de prueba** (la X de color rojo).  
  
4.  En el panel **Condiciones de prueba**, haga clic en **Recuento de filas** en la lista y, a continuación, haga clic en el icono **Agregar condición de prueba** (el signo + de color verde).  
  
5.  Abra la ventana **Propiedades** (seleccione la condición de prueba y presione F4) y establezca la propiedad **Recuento de filas** en 1.  
  
6.  En el menú **Archivo** , haga clic en **Guardar todo**.  
  
    A continuación definirá la lógica de prueba unitaria para uspPlaceNewOrder.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspplaceneworder"></a>Para escribir la prueba unitaria de SQL Server para uspPlaceNewOrder  
  
1.  En la barra de navegación del Diseñador de pruebas unitarias de SQL Server, haga clic en **Sales_uspPlaceNewOrderTest** y asegúrese de que **Prueba** está resaltado en la lista adyacente.  
  
    Después de realizar este paso, puede crear el script de prueba para la acción de prueba en la prueba unitaria.  
  
2.  Actualice las instrucciones Transact\-SQL en el editor de Transact\-SQL para que coincidan con las instrucciones siguientes:  
  
    ```  
    -- ssNoVersion unit test for Sales.uspPlaceNewOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- place an order for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    -- verify that the YTDOrders value is correct.  
    SELECT @RC = [YTDOrders] FROM [Sales].[Customer] WHERE [CustomerID] = @CustomerID  
  
    SELECT @RC AS RC  
    ```  
  
3.  En el panel **Condiciones de prueba** , haga clic en la Condición de prueba no concluyente y, a continuación, haga clic en **Eliminar condición de prueba**.  
  
4.  En el panel **Condiciones de prueba** , haga clic en **Valor escalar** en la lista y, a continuación, haga clic en **Agregar condición de prueba**.  
  
5.  En la ventana **Propiedades** , establezca la propiedad **Valor esperado** en 100.  
  
6.  En la barra de navegación del Diseñador de pruebas unitarias de SQL Server, haga clic en **Sales_uspPlaceNewOrderTest** y asegúrese de que **Anterior a la prueba** está resaltado en la lista adyacente.  
  
    Después de realizar este paso, puede especificar las instrucciones que ponen los datos en el estado necesario para ejecutar la prueba. En este ejemplo, debe crear el registro Customer para poder hacer un pedido.  
  
7.  Haga clic en **Haga clic aquí para crear** para crear un script anterior a la prueba.  
  
8.  Actualice las instrucciones Transact\-SQL en el editor de Transact\-SQL para que coincidan con las instrucciones siguientes:  
  
    ```  
    /*  
    Add Transact-SQL statements here that you want to run before  
    the test script is run.  
    */  
    -- Add a customer for this test with the name 'Fictitious Customer'  
    DECLARE @NewCustomerID AS INT, @CustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
       @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
    ```  
  
9. En el menú **Archivo** , haga clic en **Guardar todo**.  
  
    A continuación creará la prueba unitaria para uspFillOrder.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspfillorder"></a>Para escribir la prueba unitaria de SQL Server para uspFillOrder  
  
1.  En la barra de navegación del Diseñador de pruebas unitarias de SQL Server, haga clic en **Sales_uspFillOrderTest** y asegúrese de que **Prueba** está resaltado en la lista adyacente.  
  
    Después de realizar este paso, puede crear el script de prueba para la acción de prueba en la prueba unitaria.  
  
2.  Actualice las instrucciones Transact\-SQL en el editor de Transact\-SQL para que coincidan con las instrucciones siguientes:  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
    -- Get the most recently added order.  
    SELECT @OrderID = MAX([OrderID]) FROM [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspFillOrder] @OrderID, @FilledDate;  
  
    -- verify that the YTDOrders value is correct.  
    SELECT @RC = [YTDSales] FROM [Sales].[Customer] WHERE [CustomerID] = @CustomerID  
  
    SELECT @RC AS RC;  
    ```  
  
3.  En el panel **Condiciones de prueba** , haga clic en la Condición de prueba no concluyente y, a continuación, haga clic en **Eliminar condición de prueba**.  
  
4.  En el panel **Condiciones de prueba** , haga clic en **Valor escalar** en la lista y, a continuación, haga clic en **Agregar condición de prueba**.  
  
5.  En la ventana **Propiedades** , establezca la propiedad **Valor esperado** en 100.  
  
6.  En la barra de navegación del Diseñador de pruebas unitarias de SQL Server, haga clic en **Sales_uspFillOrderTest** y asegúrese de que **Anterior a la prueba** está resaltado en la lista adyacente. Después de realizar este paso, puede especificar las instrucciones que ponen los datos en el estado necesario para ejecutar la prueba. En este ejemplo, debe crear el registro Customer para poder hacer un pedido.  
  
7.  Haga clic en **Haga clic aquí para crear** para crear un script anterior a la prueba.  
  
8.  Actualice las instrucciones Transact\-SQL en el editor de Transact\-SQL para que coincidan con las instrucciones siguientes:  
  
    ```  
    /*  
    Add Transact-SQL statements here that you want to run before  
    the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place an order for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
    ```  
  
9. En el menú **Archivo** , haga clic en **Guardar todo**.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspshoworderdetails"></a>Para escribir la prueba unitaria de SQL Server para uspShowOrderDetails  
  
1.  En la barra de navegación del Diseñador de pruebas unitarias de SQL Server, haga clic en **Sales_uspShowOrderDetailsTest** y asegúrese de que **Prueba** está resaltado en la lista adyacente.  
  
    Después de realizar este paso, puede crear el script de prueba para la acción de prueba en la prueba unitaria.  
  
2.  Actualice las instrucciones Transact\-SQL en el editor de Transact\-SQL para que coincidan con las instrucciones siguientes:  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspShowOrderDetails] @CustomerID;  
  
    SELECT @RC AS RC;  
    ```  
  
3.  En el panel **Condiciones de prueba** , haga clic en la Condición de prueba no concluyente y, a continuación, haga clic en **Eliminar condición de prueba**.  
  
4.  En el panel **Condiciones de prueba** , haga clic en **Esquema esperado** en la lista y, a continuación, haga clic en **Agregar condición de prueba**.  
  
5.  En la ventana **Propiedades**, en la propiedad **Configuración**, haga clic en el botón Examinar (" **…** ").  
  
6.  En el cuadro de diálogo **Configuración de expectedSchemaCondition1** , especifique una conexión a la base de datos. Por ejemplo, si usó la ubicación de implementación predeterminada, que es LocalDB, haría clic en **Nueva conexión** y especificaría **(LocalDB)\Projects**. Después, elija el nombre de la base de datos.  
  
7.  Haga clic en **Recuperar**. (Si es necesario, haga clic en **Recuperar** hasta que vea los datos).  
  
    Se ejecuta el cuerpo de Transact\-SQL de la prueba unitaria y el esquema resultante aparece en el cuadro de diálogo. Dado que el código anterior a la prueba no se ejecutó, no se devuelve ningún dato. Ya que solo se está comprobando el esquema y no los datos, esto es correcto.  
  
8.  Haga clic en **OK**.  
  
    El esquema esperado se almacena con la condición de prueba.  
  
9. En la barra de navegación del Diseñador de pruebas unitarias de SQL Server, haga clic en **Sales_uspShowOrderDetailsTest** y asegúrese de que **Anterior a la prueba** está resaltado en la lista adyacente. Después de realizar este paso, puede especificar las instrucciones que ponen los datos en el estado necesario para ejecutar la prueba. En este ejemplo, debe crear el registro Customer para poder hacer un pedido.  
  
10. Haga clic en **Haga clic aquí para crear** para crear un script anterior a la prueba.  
  
11. Actualice las instrucciones Transact\-SQL en el editor de Transact\-SQL para que coincidan con las instrucciones siguientes:  
  
    ```  
    /*  
    Add Transact-SQL statements here to run before the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'FictitiousCustomer'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place 3 orders for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 100, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 50, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 5, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
    ```  
  
12. En la barra de navegación del Diseñador de pruebas unitarias de SQL Server, haga clic en **Sales_uspShowOrderDetailsTest** y en **Prueba** en la lista adyacente.  
  
    Debe hacerlo porque desea aplicar la condición de suma de comprobación a la prueba, no a la acción anterior a la prueba.  
  
13. En el panel **Condiciones de prueba** , haga clic en **Suma de comprobación de datos** en la lista y, a continuación, haga clic en **Agregar condición de prueba**.  
  
14. En la ventana **Propiedades**, en la propiedad **Configuración**, haga clic en el botón Examinar (" **…** ").  
  
15. En el cuadro de diálogo **Configuración de checksumCondition1** , especifique una conexión a la base de datos.  
  
16. Reemplace el código Transact\-SQL del cuadro de diálogo (bajo el botón **Editar conexión**) con el código siguiente:  
  
    ```  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place 3 orders for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 100, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 50, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 5, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @FilledDate AS DATETIME;  
    DECLARE @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspShowOrderDetails] @CustomerID;  
  
    SELECT @RC AS RC;  
    ```  
  
    Este código combina el código Transact\-SQL anterior a la prueba con el Transact\-SQL de la propia prueba. Necesita ambos para devolver los mismos resultados que la prueba devolverá cuando se ejecute.  
  
17. Haga clic en **Recuperar**. (Si es necesario, haga clic en **Recuperar** hasta que vea los datos).  
  
    Se ejecuta el Transact\-SQL especificado y se calcula una suma de comprobación para los datos devueltos.  
  
18. Haga clic en **OK**.  
  
    La suma de comprobación calculada se almacena con la condición de prueba. La suma de comprobación esperada aparece en la columna Valor de la condición de prueba Suma de comprobación de datos.  
  
19. En el menú **Archivo** , haga clic en **Guardar todo**.  
  
    En este punto, estará listo para ejecutar las pruebas.  
  
## <a name="run-sql-server-unit-tests"></a><a name="RunTests"></a>Ejecutar pruebas unitarias de SQL Server  
  
#### <a name="to-run-the-sql-server-unit-tests"></a>Para ejecutar las pruebas unitarias de SQL Server  
  
1.  En el menú **Prueba**, elija **Windows** y haga clic en **Vista de pruebas** de Visual Studio 2010 o en **Explorador de pruebas** de Visual Studio 2012.  
  
2.  En la ventana **Vista de pruebas** (Visual Studio 2010), haga clic en **Actualizar** en la barra de herramientas para actualizar la lista de pruebas. Para ver la lista de pruebas en el **Explorador de pruebas** (Visual Studio 2012), compile la solución.  
  
    En la ventana **Vista de pruebas** o **Explorador de pruebas** se muestran las pruebas que creó anteriormente en este tutorial y a las que agregó las instrucciones y las condiciones de prueba de Transact\-SQL. La prueba que se denomina TestMethod1 está vacía y no se usa en este tutorial.  
  
3.  Haga clic con el botón derecho en **Sales_uspNewCustomerTest** y haga clic en **Ejecutar selección**.  
  
    Visual Studio utiliza el contexto privilegiado que especificó para conectarse a la base de datos y aplicar el plan de generación de datos. Visual Studio se activa en el contexto de ejecución antes de ejecutar el script Transact\-SQL en la prueba. Finalmente, Visual Studio evalúa los resultados del script Transact\-SQL con los especificados en la condición de prueba, y aparece un resultado de correcto o error en la ventana **Resultados de pruebas**.  
  
4.  Vea el resultado en la ventana **Resultados de pruebas** .  
  
    La prueba se supera, lo que significa que la instrucción **SELECT** devuelve una fila cuando se ejecuta.  
  
5.  Repita el paso 3 para las pruebas Sales_uspPlaceNewOrderTest, Sales_uspFillOrderTest y Sales_uspShowOrderDetailsTest. Los resultados deberían ser los siguientes:  
  
    |Prueba|Resultado previsto|  
    |--------|-------------------|  
    |Sales_uspPlaceNewOrderTest|Pass (pasado)|  
    |Sales_uspShowOrderDetailsTest|Pass (pasado)|  
    |Sales_uspFillOrderTest|Se produce el siguiente error: “Error en la condición ScalarValueCondition (scalarValueCondition2): ResultSet 1 Fila 1 Columna 1: los valores no coinciden, real ‘-100’, esperado ‘100’”. Este error aparece porque la definición del procedimiento almacenado contiene un error menor.|  
  
    A continuación, se corregirá el error y volverá a ejecutar la prueba.  
  
#### <a name="to-correct-the-error-in-salesuspfillorder"></a>Para corregir el error en Sales.uspFillOrder  
  
1.  En el nodo Proyectos del **Explorador de objetos de SQL Server** correspondiente a la base de datos, haga doble clic en el procedimiento almacenado **uspFillOrder** para abrir su definición en el editor de Transact\-SQL.  
  
2.  En la definición, busque la siguiente instrucción Transact\-SQL:  
  
    ```  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales - @Delta  
        WHERE [CustomerID] = @CustomerID  
    ```  
  
3.  Cambie la cláusula SET en la instrucción para que coincida con la siguiente:  
  
    ```  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales + @Delta  
        WHERE [CustomerID] = @CustomerID  
    ```  
  
4.  En el menú **Archivo** , haga clic en **Guardar uspFillOrder.sql**.  
  
5.  En **Vista de pruebas**, haga clic con el botón derecho en **Sales_uspFillOrderTest** y haga clic en **Ejecutar selección**.  
  
    Se supera la prueba.  
  
## <a name="add-a-negative-unit-test"></a><a name="NegativeTest"></a>Agregar una prueba unitaria negativa  
Puede crear una prueba negativa para comprobar que una prueba genera un error cuando debe dar error. Por ejemplo, si intenta cancelar un pedido que ya estaba rellenado, la prueba debe generar un error. En esta parte del tutorial, se crea una prueba unitaria negativa para el procedimiento almacenado Sales.uspCancelOrder.  
  
Para crear y comprobar una prueba negativa, debe realizar las siguientes tareas:  
  
-   Actualizar el procedimiento almacenado para probar las condiciones de error  
  
-   Definir una nueva prueba unitaria  
  
-   Modificar el código de la prueba unitaria para indicar que se espera un error  
  
-   Ejecutar la prueba unitaria  
  
#### <a name="to-update-the-stored-procedure"></a>Para actualizar el procedimiento almacenado  
  
1.  En el nodo Proyectos del **Explorador de objetos de SQL Server** de la base de datos SimpleUnitTestDB, expanda los nodos Programación y Procedimientos almacenados y, a continuación, haga doble clic en uspCancelOrder.  
  
2.  En el editor de Transact\-SQL, actualice la definición del procedimiento para que coincida con el código siguiente:  
  
    ```  
    CREATE PROCEDURE [Sales].[uspCancelOrder]  
    @OrderID INT  
    AS  
    BEGIN  
        DECLARE @Delta INT, @CustomerID INT, @PriorStatus CHAR(1)  
        BEGIN TRANSACTION  
            BEGIN TRY  
                IF (NOT EXISTS(SELECT [CustomerID] from [Sales].[Orders] WHERE [OrderID] = @OrderID))  
                BEGIN  
                    -- Specify WITH LOG option so that the error is  
                    -- written to the application log.  
                    RAISERROR( 'That order does not exist.', -- Message text  
                               16, -- severity  
                                1 -- state  
                            ) WITH LOG;  
                END  
  
                SELECT @Delta = [Amount], @CustomerID = [CustomerID], @PriorStatus = [Status]  
                 FROM [Sales].[Orders] WHERE [OrderID] = @OrderID  
  
                IF @PriorStatus <> 'O'   
                BEGIN  
                    -- Specify WITH LOG option so that the error is  
                    -- written to the application log.  
                    RAISERROR ( 'You can only cancel open orders.', -- Message text  
                                16, -- Severity  
                                1 -- State  
                                ) WITH LOG;  
                END  
                ELSE  
                BEGIN  
                    -- If we make it to here, then we can cancel the order. Update the status to 'X' first...  
                    UPDATE [Sales].[Orders]  
                       SET [Status] = 'X'  
                    WHERE [OrderID] = @OrderID  
                    -- and then remove the amount from the YTDOrders for the customer  
                    UPDATE [Sales].[Customer]  
                           SET  
                               YTDOrders = YTDOrders - @Delta  
                    WHERE [CustomerID] = @CustomerID  
                    COMMIT TRANSACTION  
                    RETURN 1; -- indicate success  
                END  
            END TRY  
            BEGIN CATCH  
                DECLARE @ErrorMessage NVARCHAR(4000);  
                DECLARE @ErrorSeverity INT;  
                DECLARE @ErrorState INT;  
  
                SELECT @ErrorMessage = ERROR_MESSAGE(),  
                       @ErrorSeverity = ERROR_SEVERITY(),  
                       @ErrorState = ERROR_STATE();  
  
                ROLLBACK TRANSACTION  
                -- Use RAISERROR inside the CATCH block to return  
                -- error information about the original error that  
                -- caused execution to jump to the CATCH block.  
                RAISERROR (@ErrorMessage, -- Mesasge text  
                           @ErrorSeverity, -- Severity  
                           @ErrorState -- State  
                          );  
                RETURN 0; -- indicate failure  
            END CATCH;  
    END  
    ```  
  
3.  En el menú **Archivo** , haga clic en **Guardar uspCancelOrder.sql**.  
  
4.  Presione F5 para implementar **SimpleUnitTestDB**.  
  
    Implemente las actualizaciones en el procedimiento almacenado uspCancelOrder. No cambió ningún otro objeto, tan solo ha actualizado el procedimiento almacenado.  
  
    A continuación definirá la prueba unitaria asociada para este procedimiento.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspcancelorder"></a>Para escribir la prueba unitaria de SQL Server para uspCancelOrder  
  
1.  En la barra de navegación del Diseñador de pruebas unitarias de SQL Server, haga clic en **Sales_uspCancelOrderTest** y asegúrese de que **Prueba** está resaltado en la lista adyacente.  
  
    Después de realizar este paso, puede crear el script de prueba para la acción de prueba en la prueba unitaria.  
  
2.  Actualice las instrucciones Transact\-SQL en el editor de Transact\-SQL para que coincidan con las instrucciones siguientes:  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
    -- Get the most recently added order.  
    SELECT @OrderID = MAX([OrderID]) FROM [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
  
    -- try to cancel an order for that customer that has already been filled  
    EXECUTE @RC = [Sales].[uspCancelOrder] @OrderID;  
  
    SELECT @RC AS RC;  
    ```  
  
3.  En el panel **Condiciones de prueba** , haga clic en la Condición de prueba no concluyente y, a continuación, haga clic en el icono **Eliminar condición de prueba** .  
  
4.  En el panel **Condiciones de prueba** , haga clic en **Valor escalar** en la lista y, a continuación, haga clic en el icono **Agregar condición de prueba** .  
  
5.  En la ventana **Propiedades** , establezca la propiedad **Valor esperado** en 0.  
  
6.  En la barra de navegación del Diseñador de pruebas unitarias de SQL Server, haga clic en **Sales_uspCancelOrderTest** y asegúrese de que **Anterior a la prueba** está resaltado en la lista adyacente. Después de realizar este paso, puede especificar las instrucciones que ponen los datos en el estado necesario para ejecutar la prueba. En este ejemplo, debe crear el registro Customer para poder hacer un pedido.  
  
7.  Haga clic en **Haga clic aquí para crear** para crear un script anterior a la prueba.  
  
8.  Actualice las instrucciones Transact\-SQL en el editor de Transact\-SQL para que coincidan con las instrucciones siguientes:  
  
    ```  
    /*  
    Add Transact-SQL statements here to run before the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @FilledDate AS DATETIME, @Status AS CHAR (1), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
       @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
       @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place an order for that customer  
    EXECUTE @OrderID = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    -- fill the order for that customer  
    EXECUTE @RC = [Sales].[uspFillOrder] @OrderID, @FilledDate;  
  
    COMMIT TRANSACTION  
    ```  
  
9. En el menú **Archivo** , haga clic en **Guardar todo**.  
  
    En este punto, estará listo para ejecutar las pruebas.  
  
#### <a name="to-run-the-sql-server-unit-tests"></a>Para ejecutar las pruebas unitarias de SQL Server  
  
1.  En **Vista de pruebas**, haga clic con el botón derecho en **Sales_uspCancelOrderTest** y haga clic en **Ejecutar selección**.  
  
2.  Vea el resultado en la ventana **Resultados de pruebas** .  
  
    La prueba genera un error y se muestra el siguiente mensaje de error:  
  
    **El método de prueba TestProject1.SqlServerUnitTests1.Sales_uspCancelOrderTest generó la excepción: System.Data.SqlClient.SqlException: solo puede cancelar pedidos abiertos.**  
  
    A continuación, modifique el código para indicar que se espera la excepción.  
  
#### <a name="to-modify-the-code-for-the-unit-test"></a>Para modificar el código de la prueba unitaria  
  
1.  En el **Explorador de soluciones**, expanda **TestProject1**, haga clic con el botón derecho en **SqlServerUnitTests1.cs** y, a continuación, haga clic en **Ver código**.  
  
2.  En el editor de código, navegue al método Sales_uspCancelOrderTest. Modifique los atributos del método para que coincidan con el código siguiente:  
  
    ```  
    [TestMethod(), ExpectedSqlException(Severity=16, MatchFirstError=false, State=1)]  
    public void Sales_uspCancelOrderTest()  
    ```  
  
    Especifique que se espera ver una excepción concreta. Opcionalmente, puede especificar un número de error concreto. Si no agrega este atributo, la prueba unitaria generará un error y aparecerá un mensaje en la ventana Resultados de pruebas  
  
    > [!IMPORTANT]  
    > Actualmente, Visual Studio 2012 no admite el atributo ExpectedSqlException. Para obtener información sobre cómo solucionar este problema, vea [No se puede ejecutar la prueba unitaria de base de datos "Error esperado"](https://social.msdn.microsoft.com/Forums/en-US/e74e06ad-e3c9-4cb0-97ad-a6f235a52345/unable-to-run-quotexpected-failurequot-database-unit-test).  
  
3.  En el menú Archivo, haga clic en Guardar SqlServerUnitTests1.cs.  
  
    A continuación, vuelva a ejecutar la prueba unitaria para comprobar que genera un error como se esperaba.  
  
#### <a name="to-re-run-the-sql-server-unit-tests"></a>Para volver a ejecutar las pruebas unitarias de SQL Server  
  
1.  En **Vista de pruebas**, haga clic con el botón derecho en **Sales_uspCancelOrderTest** y haga clic en **Ejecutar selección**.  
  
2.  Vea el resultado en la ventana **Resultados de pruebas** .  
  
    La prueba se supera, lo que significa que se produjo un error en el procedimiento cuando se suponía que se debía producir.  
  
## <a name="next-steps"></a>Pasos siguientes  
En un proyecto típico, se definirían pruebas unitarias adicionales para comprobar que todos los objetos de base de datos críticos funcionan correctamente. Una vez completado el conjunto de pruebas, se protegerían las pruebas en el control de versiones para compartirlas con el equipo.  
  
Después de establecer una línea base, puede crear y modificar los objetos de base de datos y crear pruebas asociadas para comprobar si un cambio interrumpirá el comportamiento esperado.  
  
## <a name="see-also"></a>Consulte también  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Comprobar el código de base de datos con pruebas unitarias de SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[Cómo: Crear una prueba unitaria de SQL Server vacía](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)  
[Cómo: Configurar una ejecución de prueba unitaria de SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md)  
