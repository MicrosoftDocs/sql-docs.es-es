---
title: 'Inicio rápido: Conectarse y consultar SQL Server'
description: Realice este inicio rápido para usar Azure Data Studio para conectarse a SQL Server y, luego, emplear instrucciones Transact-SQL (T-SQL) para crear una base de datos.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18, sqlfreshmay19
ms.date: 08/02/2019
ms.openlocfilehash: 5e9973932338ee2937f5e0942242e95af6d6164d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040045"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-sql-server"></a>Inicio rápido: Conectarse y consultar SQL Server con Azure Data Studio

En este inicio rápido se muestra cómo usar Azure Data Studio para conectarse a SQL Server y luego usar instrucciones Transact-SQL (T-SQL) para crear el elemento *TutorialDB* empleado en los tutoriales de Azure Data Studio.

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido, se necesita Azure Data Studio y acceso a SQL Server.

- [Instale Azure Data Studio](./download-azure-data-studio.md).

Si no tiene acceso a SQL Server, seleccione su plataforma en uno de los vínculos siguientes (deberá acordarse de sus credenciales de inicio de sesión de SQL y de su contraseña):

- [Windows - Descargar SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)
- [macOS - Descargar SQL Server 2017 en Docker](../linux/quickstart-install-connect-docker.md)
- [Linux - Descargar SQL Server 2017 Developer Edition](../linux/sql-server-linux-overview.md#install): solo tiene que seguir los pasos de *creación y consulta de datos*.

## <a name="connect-to-a-sql-server"></a>Conectarse a un servidor SQL Server

1. Inicie **Azure Data Studio**.

2. La primera vez que ejecute Azure Data Studio, se debe abrir la **página principal**. Si no ve la **página principal**, seleccione **Ayuda** > **Página principal**. Seleccione **Nueva conexión** para abrir el panel **Conexión**:

   ![Icono de Nueva conexión](media/quickstart-sql-server/new-connection-icon.png)

3. En este artículo se usa el *inicio de sesión de SQL*, pero se admite la *autenticación de Windows*. Rellene los campos como se indica a continuación:

   - **Nombre del servidor:** escriba aquí el nombre del servidor. Por ejemplo, “localhost”.
   - **Tipo de autenticación:** Inicio de sesión SQL
   - **Nombre de usuario:** El nombre de usuario para SQL Server
   - **Contraseña:** Contraseña para SQL Server
   - **Nombre de base de datos:** \<Default\>
   - **Server Grupo de servidores:** \<Default\>

   ![Pantalla Nueva conexión](media/quickstart-sql-server/new-connection-screen.png)

## <a name="create-a-database"></a>Crear una base de datos

En los pasos siguientes se crea una base de datos denominada **TutorialDB**:

1. Haga clic con el botón derecho en el servidor, **localhost**, y seleccione **Nueva consulta**.

2. Pegue el siguiente fragmento de código en la ventana de consulta y, después, seleccione **Ejecutar**.

    ```sql
    USE master
    GO
    IF NOT EXISTS (
     SELECT name
     FROM sys.databases
     WHERE name = N'TutorialDB'
    )
     CREATE DATABASE [TutorialDB];
    GO
    IF SERVERPROPERTY('ProductVersion') > '12'
     ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON;
    GO
    ```

   Una vez hecha la consulta, aparecerá la nueva **TutorialDB** en la lista de bases de datos. Si no la ve, haga clic con el botón derecho en el nodo **Bases de datos** y seleccione **Actualizar**.

   ![Crear base de datos](media/quickstart-sql-server/create-database.png)

## <a name="create-a-table"></a>Creación de una tabla

El editor de consultas aún está conectado a la base de datos *maestra*, pero lo que queremos es crear una tabla en la base de datos *TutorialDB*.

1. Cambie el contexto de conexión a **TutorialDB**:

   ![Cambiar el contexto](media/quickstart-sql-server/change-context.png)

2. Pegue el siguiente fragmento de código en la ventana de consulta y haga clic en **Ejecutar**:

   > [!NOTE]
   > Puede anexar esto también, o bien puede sobrescribir la consulta anterior en el editor. Al hacer clic en **Ejecutar**, solo se ejecuta la consulta seleccionada. Si hay ninguna opción seleccionada, al hacer clic en **Ejecutar**, se ejecutarán todas las consultas en el editor.

    ```sql
    -- Create a new table called 'Customers' in schema 'dbo'
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
     DROP TABLE dbo.Customers;
    GO
    -- Create the table in the specified schema
    CREATE TABLE dbo.Customers
    (
     CustomerId int NOT NULL PRIMARY KEY, -- primary key column
     Name nvarchar(50) NOT NULL,
     Location nvarchar(50) NOT NULL,
     Email nvarchar(50) NOT NULL
    );
    GO
    ```

Una vez hecha la consulta, aparecerá la tabla nueva **Customers** (Clientes) en la lista de tablas. Es posible que tenga que hacer clic con el botón derecho en el nodo **TutorialDB > Tablas** y seleccionar **Actualizar**.

## <a name="insert-rows"></a>Insertar filas

- Pegue el siguiente fragmento de código en la ventana de consulta y haga clic en **Ejecutar**:

    ```sql
    -- Insert rows into table 'Customers'
    INSERT INTO dbo.Customers
     ([CustomerId], [Name], [Location], [Email])
    VALUES
     ( 1, N'Orlando', N'Australia', N''),
     ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
     ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
     ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
    GO
    ```

## <a name="view-the-data-returned-by-a-query"></a>Ver los datos devueltos por una consulta

 - Pegue el siguiente fragmento de código en la ventana de consulta y haga clic en **Ejecutar**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

   ![Seleccionar los resultados](media/quickstart-sql-server/select-results.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que se ha conectado correctamente a SQL Server y ha ejecutado una consulta, pruebe el [tutorial del editor de código](tutorial-sql-editor.md).