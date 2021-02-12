---
title: Conexión a Azure Synapse Analytics y realización de consultas
description: Esta guía de inicio rápido muestra la conexión con un grupo de SQL dedicado de Azure Synapse Analytics mediante Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.reviewer: maghan, jrasnick
ms.custom: seodec18; seo-lt-2019
ms.date: 10/15/2020
ms.openlocfilehash: 91e91aeb69db1d3ff3f357289f0156cfcf739bab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040076"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-data-using-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Inicio rápido: Uso de Azure Data Studio para conectarse a datos y consultarlos mediante un grupo de SQL dedicado en Azure Synapse Analytics

Esta guía de inicio rápido muestra la conexión con un grupo de SQL dedicado de Azure Synapse Analytics mediante Azure Data Studio.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía de inicio rápido, necesita Azure Data Studio y un grupo de SQL dedicado en Azure Synapse Analytics.

- [Instale Azure Data Studio](./download-azure-data-studio.md).

Si aún no tiene un grupo de SQL dedicado, vea [Creación de un grupo de SQL dedicado](/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision).

Recuerde el nombre del servidor y las credenciales de inicio de sesión.


## <a name="connect-to-your-dedicated-sql-pool"></a>Conexión al grupo de SQL dedicado

Use Azure Data Studio para establecer una conexión con el servidor de Azure Synapse Analytics.

1. La primera vez que ejecute Azure Data Studio, se abrirá la página **Conexión**. Si no ve la página **Conexión**, seleccione **Agregar conexión**, o bien en el icono de **Nueva conexión** en la barra lateral **SERVIDORES**:
   
   ![Captura de pantalla de la página de conexiones con el icono de Nueva conexión resaltado.](media/quickstart-sql-dw/new-connection-icon.png)

2. En este artículo, se usa el *inicio de sesión de SQL*, pero se admite la *autenticación de Windows*. Rellene los campos como se indica a continuación con el nombre del servidor, el nombre de usuario y la contraseña de *su instancia* de Azure SQL Server:

   |   Configuración    | Valor sugerido | Descripción |
   |--------------|-----------------|-------------| 
   | **Nombre del servidor** | Nombre completo del servidor | Por ejemplo, el nombre debe tener este formato: **sqlpoolservername.database.windows.net**. |
   | **Autenticación** | Inicio de sesión SQL| En este tutorial, se usa la autenticación de SQL. |
   | **Nombre de usuario** | La cuenta de administrador del servidor | Es la cuenta que especificó cuando creó el servidor. |
   | **Contraseña (Inicio de sesión de SQL)** | La contraseña de la cuenta de administrador del servidor | Es la contraseña que especificó cuando creó el servidor. |
   | **¿Desea guardar la contraseña?** | Sí o no | Seleccione Sí si no quiere escribir la contraseña cada vez. |
   | **Nombre de la base de datos** | *dejar en blanco* | Nombre de la base de datos a la que se va a conectar. |
   | **Grupo de servidores** | Seleccione <Default>. | Si ha creado un grupo de servidores, puede establecerlo en un grupo de servidores específico. | 

3. Si el servidor no tiene una regla de firewall que permita a Azure Data Studio conectarse, se abrirá el formulario **Crear nueva regla de firewall**. Rellene el formulario para crear una nueva regla de firewall. Para obtener detalles, vea [Reglas de firewall](/azure/sql-database/sql-database-firewall-configure).

4. Después de conectarse correctamente, el servidor se abre en la barra lateral *SERVIDORES*.

## <a name="create-a-database-in-your-dedicated-sql-pool"></a>Creación de una base de datos en un grupo de SQL dedicado

1. En el explorador de objetos, Haga clic con el botón derecho en el servidor y seleccione **Nueva consulta**.

2. Pegue el siguiente fragmento de código en el editor de consultas y seleccione **Ejecutar**:

   ```sql
    IF NOT EXISTS (
       SELECT name
       FROM sys.databases
       WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB] (EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
    GO  
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
   ```

## <a name="create-a-table"></a>Creación de una tabla

El editor de consultas aún está conectado a la base de datos *maestra*, pero lo que queremos es crear una tabla en la base de datos *TutorialDB*. 

1. Cambie el contexto de conexión a **TutorialDB**:

2. Pegue el siguiente fragmento de código en el editor de consultas y seleccione **Ejecutar**:

   > [!NOTE]
   > Puede anexar esto, o bien puede sobrescribir la consulta anterior en el editor. Al seleccionar **Ejecutar**, solo se ejecuta la consulta seleccionada. Si no hay ninguna opción seleccionada, cuando seleccione **Ejecutar** se ejecutarán todas las consultas en el editor.

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT     NOT NULL,
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-table.png" alt-text="Creación de una tabla en la base de datos TutorialDB":::


## <a name="insert-rows"></a>Insertar filas

1. Pegue el siguiente fragmento de código en el editor de consultas y seleccione **Ejecutar**:

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
      SELECT 1, N'Orlando',N'Australia', N'' UNION ALL
      SELECT 2, N'Keith', N'India', N'keith0@adventure-works.com' UNION ALL
      SELECT 3, N'Donna', N'Germany', N'donna0@adventure-works.com' UNION ALL
      SELECT 4, N'Janet', N'United States', N'janet1@adventure-works.com'
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-rows.png" alt-text="Creación de filas en la tabla":::

## <a name="view-the-result"></a>Ver el resultado

1. Pegue el siguiente fragmento de código en el editor de consultas y seleccione **Ejecutar**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

2. Se muestran los resultados de la consulta:

    :::image type="content" source="media/quickstart-sql-dw/view-results.png" alt-text="Visualización de los resultados":::


## <a name="clean-up-resources"></a>Limpieza de recursos

Si no tiene pensado seguir trabajando con las bases de datos de ejemplo creadas en este artículo, [elimine el grupo de recursos](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información, visite [Conexión a Synapse SQL con Azure Data Studio](/azure/synapse-analytics/sql/get-started-azure-data-studio).

Ahora que ya se ha conectado correctamente a una instancia de Azure Synapse Analytics y ha ejecutado una consulta, pruebe el [Tutorial del editor de código](tutorial-sql-editor.md).