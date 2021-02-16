---
description: Implementar un proyecto de SSIS desde el símbolo del sistema con ISDeploymentWizard.exe
title: Implementar un proyecto de SSIS desde el símbolo del sistema | Microsoft Docs
ms.date: 05/21/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bb78c1ab660ff3ac0cccfdc36779b659ba654474
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354264"
---
# <a name="deploy-an-ssis-project-from-the-command-prompt-with-isdeploymentwizardexe"></a>Implementar un proyecto de SSIS desde el símbolo del sistema con ISDeploymentWizard.exe

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


En este inicio rápido se muestra cómo implementar un proyecto de SSIS desde el símbolo del sistema mediante la ejecución del asistente para la implementación de Integration Services, `ISDeploymentWizard.exe`.

Para obtener más información sobre el asistente para la implementación de Integration Services, consulte [Integration Services Deployment Wizard (Asistente para la implementación de Integration Services)](packages/deploy-integration-services-ssis-projects-and-packages.md#integration-services-deployment-wizard).

## <a name="prerequisites"></a>Prerrequisitos

La validación que de describe en este artículo para la implementación en Azure SQL Database requiere SQL Server Data Tools (SSDT), versión 17.4 o posterior. Para obtener la versión más reciente de SSDT, consulte [Descargar SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md).

Un servidor de Azure SQL Database escucha en el puerto 1433. Si está intentando conectarse a un servidor de Azure SQL Database desde un firewall corporativo, este puerto debe estar abierto en el firewall corporativo para poder conectarse correctamente.

## <a name="supported-platforms"></a>Plataformas compatibles

Puede usar la información que aparece en este inicio rápido para implementar un proyecto de SSIS en las siguientes plataformas:

-   SQL Server en Windows.

-   Azure SQL Database. Para más información sobre cómo implementar y ejecutar paquetes en Azure, vea [Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

No puede usar la información que aparece en este inicio rápido para implementar un paquete de SSIS en SQL Server en Linux. Para más información sobre cómo ejecutar paquetes en Linux, vea [Extract, transform, and load data on Linux with SSIS](../linux/sql-server-linux-migrate-ssis.md) (Extraer, transformar y cargar datos en Linux con SSIS).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Para Azure SQL Database, obtener la información de conexión

Para implementar el proyecto en Azure SQL Database, debe obtener la información de conexión necesaria para conectarse a la base de datos del catálogo de SSIS (SSISDB). Necesita el nombre completo y la información de inicio de sesión del servidor en los procedimientos siguientes.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Bases de datos SQL** en el menú izquierdo y, después, seleccione la base de datos SSISDB en la página **Bases de datos SQL**. 
3. En la página **Introducción** de la base de datos, compruebe el nombre completo del servidor. Mantenga el puntero del ratón sobre el nombre del servidor para ver la opción **Haga clic para copiar**. 
4. Si olvida la información de inicio de sesión del servidor de Azure SQL Database, navegue a la página del servidor de SQL Database para ver el nombre del administrador del servidor. Si es necesario, puede restablecer la contraseña.

## <a name="supported-authentication-method"></a>Método de autenticación compatible

Consulte [Métodos de autenticación para la implementación](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment).

## <a name="start-the-integration-services-deployment-wizard"></a>Iniciar el Asistente para implementación de Integration Services
1. Abra una ventana de símbolo del sistema.

2. Ejecute `ISDeploymentWizard.exe`. Se abre el Asistente para implementación de Integration Services.

    Si la carpeta que contiene `ISDeploymentWizard.exe` no está en la variable de entorno `path`, es posible que deba usar el comando `cd` para cambiar al directorio de esta. En SQL Server 2017, esta carpeta está normalmente en `C:\Program Files (x86)\Microsoft SQL Server\140\DTS\Binn`.

## <a name="deploy-a-project-with-the-wizard"></a>Implementar un proyecto con el asistente
1. En la página **Introducción** del asistente, revise la introducción. Haga clic en **Siguiente** para abrir la página **Seleccionar origen**.

2. En la página **Seleccionar origen**, seleccione el proyecto SSIS existente que hay que implementar.
    -   Para implementar un archivo de implementación de proyecto que haya creado compilando un proyecto en el entorno de desarrollo, seleccione **Archivo de implementación de proyecto** y especifique la ruta de acceso al archivo .ispac.
    -   Para implementar un proyecto que ya se ha implementado en una base de datos del catálogo de SSIS, seleccione **Catálogo de Integration Services** y especifique el nombre del servidor y la ruta de acceso al proyecto del catálogo.
    Haga clic en **Siguiente** para ver la página **Seleccionar destino** .
  
3.  En la página **Seleccionar destino**, seleccione el destino del proyecto.
    -   Escriba el nombre completo del servidor. Si el servidor de destino es un servidor de Azure SQL Database, el nombre tendrá el formato `<server_name>.database.windows.net`.
    -   Proporcione la información de autenticación y, después, seleccione **Conectar**. Consulte [Métodos de autenticación en la implementación](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment) en este artículo.
    -   A continuación, haga clic en **Examinar** para seleccionar la carpeta de destino de SSISDB.
    -   Después, seleccione **Siguiente** para abrir la página **Revisión** (el botón **Siguiente** solo se habilitará después de que haya seleccionado **Conectar**).

4.  En la página **Revisión**, revise la configuración seleccionada.
    -   Puede cambiar las selecciones si hace clic en **Anterior** o si hace clic en cualquiera de los pasos del panel izquierdo.
    -   Haga clic en **Implementar** para iniciar el proceso de implementación.

5.  Si va a efectuar una implementación en un servidor de Azure SQL Database, se abrirá la página **Validar** y se comprobará si hay errores conocidos en los paquetes del proyecto que puedan evitar que se ejecuten según lo esperado en Azure SSIS Integration Runtime. Para obtener más información, vea [Validación de paquetes de SSIS implementados en Azure](lift-shift/ssis-azure-validate-packages.md).

6.  Una vez completado el proceso de implementación, se abrirá la página **Resultados**. Esta página muestra si cada acción si se completó correctamente o no.
    -   Si se produce un error en la acción, haga clic en **Error** en la columna **Resultado** para que aparezca una explicación del error.
    -   De forma opcional, puede hacer clic en **Guardar informe** para guardar los resultados en un archivo XML.
    -   Haga clic en **Cerrar** para salir del asistente.

## <a name="next-steps"></a>Pasos siguientes
- Tenga en cuenta otras formas de implementar un paquete.
    - [Implementar un paquete SSIS con SSMS](./ssis-quickstart-deploy-ssms.md)
    - [Deploy an SSIS package with Transact-SQL (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md) [Implementar un paquete SSIS con Transact-SQL (SSMS)]
    - [Deploy an SSIS package with Transact-SQL (VS Code)](ssis-quickstart-deploy-tsql-vscode.md) [Implementar un paquete SSIS con Transact-SQL (VSCode)]
    - [Deploy an SSIS package with PowerShell](ssis-quickstart-deploy-powershell.md) (Implementar un paquete SSIS con PowerShell)
    - [Deploy an SSIS package with C#](./ssis-quickstart-deploy-dotnet.md) (Implementar un paquete SSIS con C#) 
- Ejecute un paquete implementado. Para ejecutar un paquete, puede elegir entre varias herramientas y lenguajes. Para más información, consulte los siguientes artículos:
    - [Ejecutar un paquete SSIS con SSMS](./ssis-quickstart-run-ssms.md)
    - [Run an SSIS package with Transact-SQL (SSMS) (Ejecutar un paquete SSIS con Transact-SQL [SSMS])](./ssis-quickstart-run-tsql-ssms.md)
    - [Ejecutar un paquete SSIS con Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Ejecutar un paquete SSIS desde el símbolo del sistema](./ssis-quickstart-run-cmdline.md)
    - [Run an SSIS package with PowerShell](ssis-quickstart-run-powershell.md) (Ejecutar un paquete de SSIS con PowerShell)
    - [Ejecutar un paquete SSIS con C#](./ssis-quickstart-run-dotnet.md) 
