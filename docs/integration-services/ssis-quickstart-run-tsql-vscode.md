---
description: Ejecutar un paquete SSIS desde Visual Studio Code con Transact-SQL
title: Ejecutar un paquete SSIS con Transact-SQL (VSCode) | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c911778dbf9321a09fb901806e50f9fae501507a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336302"
---
# <a name="run-an-ssis-package-from-visual-studio-code-with-transact-sql"></a>Ejecutar un paquete SSIS desde Visual Studio Code con Transact-SQL

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


En esta guía de inicio rápido se muestra cómo usar Visual Studio Code para conectarse a la base de datos del catálogo de SSIS y cómo usar instrucciones Transact-SQL para ejecutar un paquete SSIS almacenado en el catálogo de SSIS.

Visual Studio Code es un editor de código para Windows, macOS y Linux que admite extensiones, incluida la extensión `mssql` para conectarse a Microsoft SQL Server, Azure SQL Database o Azure Synapse Analytics. Para obtener más información sobre VSCode, consulte [Visual Studio Code](https://code.visualstudio.com/).

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de haber instalado la versión más reciente de Visual Studio Code y cargado la extensión `mssql`. Para descargar estas herramientas, consulte las páginas siguientes:
-   [Descargar Visual Studio Code](https://code.visualstudio.com/Download)
-   [Extensión mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)

## <a name="supported-platforms"></a>Plataformas compatibles

Puede usar la información que aparece en este inicio rápido para ejecutar un paquete de SSIS en las siguientes plataformas:

-   SQL Server en Windows.

-   Azure SQL Database. Para más información sobre cómo implementar y ejecutar paquetes en Azure, vea [Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

No puede usar la información que aparece en este inicio rápido para ejecutar un paquete de SSIS en Linux. Para más información sobre cómo ejecutar paquetes en Linux, vea [Extract, transform, and load data on Linux with SSIS](../linux/sql-server-linux-migrate-ssis.md) (Extraer, transformar y cargar datos en Linux con SSIS).

## <a name="set-language-mode-to-sql-in-vs-code"></a>Establecer el modo de lenguaje en SQL en VSCode

Para habilitar los comandos `mssql` y T-SQL IntelliSense, ajuste el modo de lenguaje en **SQL** en Visual Studio Code.

1. Abra Visual Studio Code y, a continuación, abra una nueva ventana. 

2. Haga clic en **Texto sin formato** en la esquina inferior derecha de la barra de estado.

3. En el menú desplegable **Seleccionar modo de lenguaje** que aparece, seleccione o escriba **SQL** y, a continuación, presione **ENTRAR** para establecer el modo de lenguaje en SQL. 

## <a name="for-azure-sql-database-get-the-connection-info"></a>Para Azure SQL Database, obtener la información de conexión

Para ejecutar el paquete en Azure SQL Database, debe obtener la información de conexión necesaria para conectarse a la base de datos del catálogo de SSIS (SSISDB). Necesita el nombre completo y la información de inicio de sesión del servidor en los procedimientos siguientes.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Bases de datos SQL** en el menú izquierdo y, después, seleccione la base de datos SSISDB en la página **Bases de datos SQL**. 
3. En la página **Introducción** de la base de datos, compruebe el nombre completo del servidor. Mantenga el puntero del ratón sobre el nombre del servidor para ver la opción **Haga clic para copiar**. 
4. Si olvida la información de inicio de sesión del servidor de Azure SQL Database, navegue a la página del servidor de SQL Database para ver el nombre del administrador del servidor. Si es necesario, puede restablecer la contraseña.

## <a name="connect-to-the-ssis-catalog-database"></a>Conectar con la base de datos del catálogo de SSIS

Use Visual Studio Code para establecer una conexión con el catálogo de SSIS.

> [!IMPORTANT]
> Antes de continuar, asegúrese de que tiene el servidor, la base de datos y la información de inicio de sesión a punto. Si cambia el foco de Visual Studio Code después de comenzar a escribir la información de perfil de conexión, tendrá que volver a iniciar la creación del perfil de conexión.

1. En VSCode, presione **CTRL+MAYÚS+P** (o **F1**) para abrir la paleta de comandos.

2. Escriba **sqlcon** y presione **ENTRAR**.

3. Presione **ENTRAR** para seleccionar **Create Connection Profile** (Crear perfil de conexión). Este paso crea un perfil de conexión para la instancia de SQL Server.

4. Siga las indicaciones para especificar las propiedades de conexión del nuevo perfil de conexión. Después de especificar cada valor, presione **ENTRAR** para continuar. 

   | Configuración       | Valor sugerido | Más información |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nombre del servidor** | Nombre completo del servidor | Si se está conectando a un servidor de Azure SQL Database, el nombre tendrá este formato: `<server_name>.database.windows.net`. |
   | **Nombre de la base de datos** | **SSISDB** | Nombre de la base de datos a la que se va a conectar. |
   | **Autenticación** | Inicio de sesión SQL | Con la autenticación de SQL Server puede conectarse a SQL Server o a Azure SQL Database. Si se va a conectar a un servidor de Azure SQL Database, no puede usar la autenticación de Windows. |
   | **Nombre de usuario** | La cuenta de administrador del servidor | Esta es la cuenta que especificó cuando creó el servidor. |
   | **Contraseña (Inicio de sesión de SQL)** | La contraseña de la cuenta de administrador del servidor | Esta es la contraseña que especificó cuando creó el servidor. |
   | **¿Desea guardar la contraseña?** | Sí o no | Si no quiere escribir la contraseña cada vez, seleccione Sí. |
   | **Enter a name for this profile** (Escriba un nombre para el perfil) | Nombre de perfil, como **mySSISServer** | Un nombre de perfil guardado acelera la conexión en inicios de sesión posteriores. | 

5. Presione la tecla **ESC** para cerrar el mensaje de información que indica que el perfil se ha creado y conectado.

6. Compruebe la conexión en la barra de estado.

## <a name="run-the-t-sql-code"></a>Ejecutar el código T-SQL
Ejecute el siguiente código Transact-SQL para ejecutar un paquete SSIS.

1. En la ventana **Editor**, escriba la siguiente consulta en la ventana de consulta vacía. (Este código es el que genera la opción **Script** en el cuadro de diálogo **Ejecutar paquete** en SSMS).

2. Actualice los valores de parámetro del procedimiento almacenado `catalog.create_execution` del sistema.

3. Presione **CTRL+MAYÚS+E** para ejecutar el código y ejecute el paquete.

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx',
    @execution_id=@execution_id OUTPUT,
    @folder_name=N'Deployed Projects',
      @project_name=N'Integration Services Project1',
    @use32bitruntime=False,
      @reference_id=Null
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,
    @object_type=50,
      @parameter_name=N'LOGGING_LEVEL',
      @parameter_value=@var0
EXEC [SSISDB].[catalog].[start_execution] @execution_id
GO
```

## <a name="next-steps"></a>Pasos siguientes
- Tenga en cuenta otras formas de ejecutar un paquete.
    - [Ejecutar un paquete SSIS con SSMS](./ssis-quickstart-run-ssms.md)
    - [Run an SSIS package with Transact-SQL (SSMS) (Ejecutar un paquete SSIS con Transact-SQL [SSMS])](./ssis-quickstart-run-tsql-ssms.md)
    - [Ejecutar un paquete SSIS desde el símbolo del sistema](./ssis-quickstart-run-cmdline.md)
    - [Run an SSIS package with PowerShell](ssis-quickstart-run-powershell.md) (Ejecutar un paquete de SSIS con PowerShell)
    - [Ejecutar un paquete SSIS con C#](./ssis-quickstart-run-dotnet.md) 
