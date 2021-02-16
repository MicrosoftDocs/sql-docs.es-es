---
description: Ejecutar un paquete SSIS con Transact-SQL desde SSMS
title: Ejecutar un paquete SSIS con Transact-SQL (SSMS) | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5b45f89b10e9e5405e7f85c662120f2cf04d7d02
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336323"
---
# <a name="run-an-ssis-package-from-ssms-with-transact-sql"></a>Ejecutar un paquete SSIS con Transact-SQL desde SSMS

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


En esta guía de inicio rápido se muestra cómo usar SQL Server Management Studio (SSMS) para conectarse a la base de datos del catálogo de SSIS y cómo usar las instrucciones Transact-SQL para ejecutar un paquete SSIS almacenado en el catálogo de SSIS.

SQL Server Management Studio es un entorno integrado para administrar cualquier infraestructura de SQL, desde SQL Server a SQL Database. Para obtener más información acerca de SSMS, consulte [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md).

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que tiene instalada la última versión de SQL Server Management Studio (SSMS). Para descargar SSMS, consulte [Download SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) [Descargar SQL Server Management Studio (SSMS)].

Un servidor de Azure SQL Database escucha en el puerto 1433. Si está intentando conectarse a un servidor de Azure SQL Database desde un firewall corporativo, este puerto debe estar abierto en el firewall corporativo para poder conectarse correctamente.

## <a name="supported-platforms"></a>Plataformas compatibles

Puede usar la información que aparece en este inicio rápido para ejecutar un paquete de SSIS en las siguientes plataformas:

-   SQL Server en Windows.

-   Azure SQL Database. Para más información sobre cómo implementar y ejecutar paquetes en Azure, vea [Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

No puede usar la información que aparece en este inicio rápido para ejecutar un paquete de SSIS en Linux. Para más información sobre cómo ejecutar paquetes en Linux, vea [Extract, transform, and load data on Linux with SSIS](../linux/sql-server-linux-migrate-ssis.md) (Extraer, transformar y cargar datos en Linux con SSIS).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Para Azure SQL Database, obtener la información de conexión

Para ejecutar el paquete en Azure SQL Database, debe obtener la información de conexión necesaria para conectarse a la base de datos del catálogo de SSIS (SSISDB). Necesita el nombre completo y la información de inicio de sesión del servidor en los procedimientos siguientes.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Bases de datos SQL** en el menú izquierdo y, después, seleccione la base de datos SSISDB en la página **Bases de datos SQL**. 
3. En la página **Introducción** de la base de datos, compruebe el nombre completo del servidor. Mantenga el puntero del ratón sobre el nombre del servidor para ver la opción **Haga clic para copiar**. 
4. Si olvida la información de inicio de sesión del servidor de Azure SQL Database, navegue a la página del servidor de SQL Database para ver el nombre del administrador del servidor. Si es necesario, puede restablecer la contraseña.

## <a name="connect-to-the-ssisdb-database"></a>Conectarse a la base de datos de SSISDB

Use SQL Server Management Studio para establecer una conexión con el catálogo de SSIS en el servidor de Azure SQL Database. 

1. Abra SQL Server Management Studio.

2. En el cuadro de diálogo **Conectar con el servidor**, especifique la siguiente información:

   | Configuración       | Valor sugerido | Más información | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Tipo de servidor** | Motor de base de datos | Este valor es necesario. |
   | **Nombre del servidor** | Nombre completo del servidor | Si se está conectando a un servidor de Azure SQL Database, el nombre tendrá este formato: `<server_name>.database.windows.net`. |
   | **Autenticación** | Autenticación de SQL Server | Con la autenticación de SQL Server puede conectarse a SQL Server o a Azure SQL Database. Si se va a conectar a un servidor de Azure SQL Database, no puede usar la autenticación de Windows. |
   | **Inicio de sesión** | La cuenta de administrador del servidor | Esta es la cuenta que especificó cuando creó el servidor. |
   | **Contraseña** | La contraseña de la cuenta de administrador del servidor | Esta es la contraseña que especificó cuando creó el servidor. |

3.  Haga clic en **Conectar**. La ventana Explorador de objetos se abre en SSMS.

4. En el Explorador de objetos, expanda la opción **Catálogos de Integration Services** y, a continuación, expanda **SSISDB** para ver los objetos de la base de datos del catálogo de SSIS.

## <a name="run-a-package"></a>Ejecutar un paquete
Ejecute el siguiente código Transact-SQL para ejecutar un paquete SSIS.

1.  En SSMS, abra una ventana de consulta nueva y pegue el siguiente código. (Este código es el que genera la opción **Script** en el cuadro de diálogo **Ejecutar paquete** en SSMS).

2.  Actualice los valores de parámetro del procedimiento almacenado `catalog.create_execution` del sistema.

3.  Asegúrese de que SSISDB es la base de datos actual.

4.  Ejecute el script.

5. En el Explorador de objetos, actualice el contenido de **SSISDB** si es necesario y compruebe el proyecto que ha implementado.

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
    - [Ejecutar un paquete SSIS con Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Ejecutar un paquete SSIS desde el símbolo del sistema](./ssis-quickstart-run-cmdline.md)
    - [Run an SSIS package with PowerShell](ssis-quickstart-run-powershell.md) (Ejecutar un paquete de SSIS con PowerShell)
    - [Ejecutar un paquete SSIS con C#](./ssis-quickstart-run-dotnet.md)