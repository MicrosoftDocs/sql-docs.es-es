---
title: Conexión a una instancia de SQL Server en una máquina virtual de Azure y realización de consultas con SQL Server Management Studio (SSMS)
description: Conéctese a una instancia de SQL Server en una máquina virtual de Azure mediante SSMS. Cree y consulte SQL Server en una máquina virtual de Azure mediante la ejecución de consultas de T-SQL básicas en SSMS.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: b6442715c1482ed581302667bcdaf7985948c554
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350601"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-on-an-azure-virtual-machine-using-sql-server-management-studio-ssms"></a>Inicio rápido: Conexión a una instancia de SQL Server en una máquina virtual de Azure y realización de consultas con SQL Server Management Studio (SSMS)

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

Empiece a usar SQL Server Management Studio (SSMS) para conectarse a la instancia de SQL Server en una máquina virtual de Azure y a ejecutar algunos comandos de Transact-SQL (T-SQL).

> [!div class="checklist"]
> - Conectarse a una instancia de SQL Server
> - Crear una base de datos
> - Crear una tabla en la nueva base de datos
> - Insertar filas en la nueva tabla
> - Consultar la nueva tabla y ver los resultados
> - Usar la tabla de la ventana de consulta para comprobar las propiedades de la conexión
## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesita SQL Server Management Studio y acceso a un origen de datos.

- Instale [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
- [SQL Server en una máquina virtual de Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE)

## <a name="connect-to-sql-virtual-machines"></a>Conexión a máquinas virtuales de SQL

En los pasos siguientes se muestra cómo crear una etiqueta opcional de DNS para la máquina virtual de Azure y, luego, conectarla con SQL Server Management Studio (SSMS).

### <a name="configure-a-dns-label-for-the-public-ip-address"></a>Configuración de una etiqueta DNS para la dirección IP pública

Para conectarse al motor de base de datos de SQL Server desde Internet, considere configurar una etiqueta DNS para la dirección IP pública. Puede conectarse mediante una dirección IP, pero la etiqueta DNS crea un registro A que es más fácil de identificar y abstrae la dirección IP pública subyacente.

> [!NOTE]
> Si solo piensa conectarse a la instancia de SQL Server desde de la misma red virtual o de forma local, no necesita etiquetas DNS.

1. Para crear una etiqueta DNS, seleccione **Máquinas virtuales** en el portal. Seleccione su máquina virtual de SQL Server para que aparezcan sus propiedades.

2. En la introducción de la máquina virtual, seleccione **Dirección IP pública**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-ip-address-.png" alt-text="Dirección IP pública":::

3. En las propiedades de la dirección IP pública, expanda **Configuración**.

4. Escriba un nombre para la etiqueta DNS. Este nombre es un registro A que se puede usar para conectarse directamente a la máquina virtual de SQL Server por el nombre en lugar de la dirección IP.

5. Seleccione el botón **Guardar**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-dns-label.png" alt-text="Etiqueta DNS":::

### <a name="connect"></a>Conectar

1. Inicie SQL Server Management Studio. La primera vez que ejecute SSMS se abrirá la ventana **Conectarse al servidor**. Si no se abre, puede abrirla manualmente seleccionando **Explorador de objetos** > **Conectar** > **Motor de base de datos**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-object-explorer.png" alt-text="Vínculo Conectar en el Explorador de objetos":::

2. Aparecerá el cuadro de diálogo **Conectar con el servidor** . Escriba la siguiente información:

    |   Configuración   |   Valor sugerido   |   Descripción   |
    |--------------|-----------------------|-----------------|
    | **Tipo de servidor** | Motor de base de datos | En **Tipo de servidor**, seleccione **Motor de base de datos** (suele ser la opción predeterminada). |
    | **Nombre del servidor** | Nombre completo del servidor | En **Nombre del servidor**, escriba el nombre de la máquina virtual de Azure SQL. También puede usar la dirección IP de la máquina virtual de Azure SQL para conectarse. | 
    | **Autenticación** | Autenticación de SQL Server | Use **Autenticación de SQL Server** para que se conecte la máquina virtual de Azure SQL. Además, si tiene una configuración de entorno de Azure Active Directory, puede usar cualquiera de las opciones de Azure Active Directory. </br> </br> No se admite el método **Autenticación de Windows** para la máquina virtual de Azure SQL. Para obtener más información, vea [Autenticación de Azure SQL](/azure/sql-database/sql-database-security-overview#access-management).|
    | **Inicio de sesión** | Identificador de usuario de la cuenta del servidor | Identificador de usuario de la cuenta del servidor que se ha usado para crear el servidor. Se necesita un inicio de sesión cuando se usa **Autenticación de SQL Server**. |
    | **Contraseña** | Contraseña de la cuenta del servidor | Contraseña de la cuenta del servidor que se ha usado para crear el servidor. Se necesita una contraseña cuando se usa **Autenticación de SQL Server**. |

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-to-azure-sql-vm-object-explorer.png" alt-text="Campo Nombre del servidor para máquinas virtuales de SQL":::

3. Una vez cumplimentados todos los campos, seleccione **Conectar**.

    También puede modificar otras opciones de conexión seleccionando **Opciones**. Como ejemplos de las opciones de conexión tiene la base de datos a la que se está conectando, el valor de tiempo de espera de conexión y el protocolo de red. En este artículo se usan los valores predeterminados para todas las opciones.

4. Para comprobar que la máquina virtual de Azure SQL se ha conectado correctamente, expanda y explore los objetos en el **Explorador de objetos**, donde se muestran el nombre del servidor, la versión de SQL Server y el nombre de usuario. Estos objetos son diferentes según el tipo de servidor.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-azure-sql-vm.png" alt-text="Conexión de la máquina virtual de Azure SQL":::

## <a name="troubleshoot-connectivity-issues"></a>Solución de problemas de conectividad

Aunque el portal proporciona opciones para configurar automáticamente la conectividad, es útil saber cómo configurarla manualmente. Comprender los requisitos también puede ayudar a solucionar problemas.

En la tabla siguiente se enumeran los requisitos para conectarse a SQL Server en la máquina virtual de Azure.

| Requisito | Descripción |
|---|---|
| [Habilitación del modo de autenticación de SQL Server](../../database-engine/configure-windows/change-server-authentication-mode.md#use-ssms) | Para la conexión remota a la máquina virtual se necesita autenticación de SQL Server, a menos que se haya configurado Active Directory en una red virtual. |
| [Creación de un inicio de sesión de SQL](../../relational-databases/security/authentication-access/create-a-login.md) | Si usa la autenticación SQL, necesita un inicio de sesión de SQL con un nombre de usuario y un contraseña que también tenga permisos para la base de datos de destino. |
| Habilitación del protocolo TCP/IP | SQL Server debe permitir conexiones a través de TCP. |
| [Habilitación de la regla de firewall para el puerto de SQL Server](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md) | El firewall de la máquina virtual debe permitir el tráfico entrante en el puerto de SQL Server (predeterminado: 1433). |
| [Creación de una regla del grupo de seguridad de red para el puerto 1433 de TCP](/azure/virtual-network/manage-network-security-group#create-a-security-rule) | Permita que la máquina virtual reciba tráfico en el puerto de SQL Server (el 1433 de forma predeterminada) si quiere conectarse a través de Internet. No es necesario para conexiones locales y exclusivas de red virtual. Este paso solo es necesario en Azure Portal. |

> [!TIP]
> Los pasos descritos en la tabla anterior se realizan automáticamente al configurar la conectividad en el portal. Solo debe seguir estos pasos para confirmar la configuración o configurar manualmente la conectividad de SQL Server.

## <a name="create-a-database"></a>Crear una base de datos

Haga lo siguiente para crear una base de datos denominada TutorialDB:

1. Haga clic con el botón derecho en la instancia del servidor en el Explorador de objetos y seleccione **Nueva consulta**:

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-query.png" alt-text="Vínculo Nueva consulta":::

2. Pegue el siguiente fragmento de código de T-SQL en la ventana de consulta:

    ```sql
    IF NOT EXISTS (
    SELECT name
    FROM sys.databases
    WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB]
    GO
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
    ```

3. Para ejecutar la consulta, seleccione **Ejecutar** o presione F5 en el teclado.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/execute.png" alt-text="Comando Ejecutar":::
  
    Una vez hecha la consulta, en la lista de bases de datos del Explorador de objetos aparecerá la nueva base de datos TutorialDB. Si no aparece, haga clic con el botón derecho en el nodo **Bases de datos** y seleccione **Actualizar**.

## <a name="create-a-table-in-the-new-database"></a>Crear una tabla en la nueva base de datos

En esta sección creará una tabla en la base de datos TutorialDB recién creada. Como el editor de consultas sigue en el contexto de la base de datos *master*, debe cambiar el contexto de la conexión a la base de datos *TutorialDB* siguiendo estos pasos:

1. En la lista desplegable de bases de datos, seleccione la base de datos que quiera, como se muestra aquí:

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/change-db.png" alt-text="Cambiar la base de datos":::

2. Pegue el siguiente fragmento de código de T-SQL en la ventana de consulta:

    ```sql
    USE [TutorialDB]
    -- Create a new table called 'Customers' in schema 'dbo'
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
    DROP TABLE dbo.Customers
    GO
    -- Create the table in the specified schema
    CREATE TABLE dbo.Customers
    (
       CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
       Name      [NVARCHAR](50)  NOT NULL,
       Location  [NVARCHAR](50)  NOT NULL,
       Email     [NVARCHAR](50)  NOT NULL
    );
    GO
    ```

3. Para ejecutar la consulta, seleccione **Ejecutar** o presione F5 en el teclado.

Una vez hecha la consulta, se mostrará la nueva tabla Customers (Clientes) en la lista de tablas del Explorador de objetos. Si la tabla no aparece, haga clic con el botón derecho en el nodo **TutorialDB** > **Tablas** en el Explorador de objetos y, después, seleccione **Actualizar**.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-table.png" alt-text="Tabla nueva":::

## <a name="insert-rows-into-the-new-table"></a>Insertar filas en la nueva tabla

Ahora se insertarán algunas filas en la tabla Customers que ha creado. Pegue el siguiente fragmento de código de T-SQL en la ventana de consulta y, después, seleccione **Ejecutar**:

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```

## <a name="query-the-table-and-view-the-results"></a>Consultar la tabla y ver los resultados

Los resultados de una consulta aparecen debajo de la ventana de texto de la consulta. Siga estos pasos para consultar la tabla Customers y ver las filas que se han insertado:

1. Pegue el siguiente fragmento de código de T-SQL en la ventana de consulta y, después, seleccione **Ejecutar**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    Los resultados de la consulta se muestran debajo del área en la que se ha escrito el texto.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/query-results.png" alt-text="Lista de resultados":::

    También puede modificar la presentación de los resultados si selecciona una de las opciones siguientes:

   ![Tres opciones para mostrar los resultados de la consulta](media/ssms-connect-query-sql-server-azure-vm/results.png)

   - El primer botón muestra los resultados en una **vista de texto**, tal y como se muestra en la imagen de la siguiente sección.
   - El botón central muestra los resultados en una **vista de cuadrícula**, que es la opción predeterminada.
       - Este valor se establece como el predeterminado.
   - El tercer botón le permite guardar los resultados en un archivo cuya extensión es .rpt de forma predeterminada.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Comprobar las propiedades de la conexión usando la tabla de la ventana de consulta

Puede buscar información sobre las propiedades de la conexión en los resultados de la consulta. Después de ejecutar la consulta mencionada en el paso anterior, revise las propiedades de la conexión en la parte inferior de la ventana de consulta.

- Puede determinar el servidor y la base de datos a los que se ha conectado, así como el nombre de usuario que ha utilizado.
- También puede ver la duración de la consulta y el número de filas devueltas por la consulta ejecutada.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connection-properties.png" alt-text="Propiedades de la conexión":::

## <a name="additional-tools"></a>Herramientas adicionales

También puede usar [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) para conectarse y consultar [SQL Server](../../azure-data-studio/quickstart-sql-server.md), [Azure SQL Database](../../azure-data-studio/quickstart-sql-database.md) y [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md).

## <a name="next-steps"></a>Pasos siguientes

La mejor forma de familiarizarse con SSMS es practicar. Estos artículos lo ayudan con varias características disponibles dentro de SSMS.

- [Editor de consultas de SQL Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Scripting](../tutorials/scripting-ssms.md)
- [Uso de plantillas en SSMS](../template/templates-ssms.md)
- [Configuración de SSMS](../tutorials/ssms-configuration.md)
- [Otras recomendaciones y trucos al usar SSMS](../tutorials/ssms-tricks.md)