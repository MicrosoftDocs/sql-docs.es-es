---
title: Implementación de un proyecto de SSIS con SSMS
description: Implementar un proyecto de SSIS con SQL Server Management Studio (SSMS)
ms.topic: quickstart
ms.prod: sql
ms.technology: integration-services
f1_keywords:
- sql13.swb.deploymentwizard.deploymentsettings.f1
author: chugugrace
ms.author: chugu
ms.reviewer: ''
ms.custom: ''
ms.date: 05/21/2018
ms.openlocfilehash: 3749daf45eb51cda3e37d2bd3d31105fcb23c1c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354222"
---
# <a name="deploy-an-ssis-project-with-sql-server-management-studio-ssms"></a>Implementar un proyecto de SSIS con SQL Server Management Studio (SSMS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


En esta guía de inicio rápido se muestra cómo usar SQL Server Management Studio (SSMS) para conectarse a la base de datos del catálogo de SSIS y cómo ejecutar el Asistente para implementación de Integration Services para implementar un proyecto de SSIS en el catálogo de SSIS. 

SQL Server Management Studio es un entorno integrado para administrar cualquier infraestructura de SQL, desde SQL Server a SQL Database. Para obtener más información acerca de SSMS, consulte [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md).

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que tiene instalada la última versión de SQL Server Management Studio. Para descargar SSMS, consulte [Download SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) [Descargar SQL Server Management Studio (SSMS)].

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

## <a name="authentication-methods-for-deployment"></a>Métodos de autenticación para la implementación

Si va a efectuar una implementación en un servidor de SQL Server con el Asistente para implementación, deberá usar la autenticación de Windows, puesto que no se puede usar la autenticación de SQL Server.

Si va a efectuar una implementación en un servidor de Azure SQL Database, deberá usar la autenticación de SQL Server o de Azure Active Directory, puesto que no se puede usar la autenticación de Windows.

## <a name="connect-to-the-ssisdb-database"></a>Conectarse a la base de datos de SSISDB

Use SQL Server Management Studio para establecer una conexión con el catálogo de SSIS. 

1. Abra SQL Server Management Studio.

2. En el cuadro de diálogo **Conectar con el servidor**, especifique la siguiente información:

   | Configuración       | Valor sugerido | Más información | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Tipo de servidor** | Motor de base de datos | Este valor es necesario. |
   | **Nombre del servidor** | Nombre completo del servidor | Si se está conectando a un servidor de Azure SQL Database, el nombre tendrá este formato: `<server_name>.database.windows.net`. |
   | **Autenticación** | Autenticación de SQL Server | Con la autenticación de SQL Server puede conectarse a SQL Server o a Azure SQL Database. Vea [Métodos de autenticación en la implementación](#authentication-methods-for-deployment) en este artículo. |
   | **Inicio de sesión** | La cuenta de administrador del servidor | Esta es la cuenta que especificó cuando creó el servidor. |
   | **Contraseña** | La contraseña de la cuenta de administrador del servidor | Esta es la contraseña que especificó cuando creó el servidor. |

3. Haga clic en **Conectar**. La ventana Explorador de objetos se abre en SSMS. 

4. En el Explorador de objetos, expanda la opción **Catálogos de Integration Services** y, a continuación, expanda **SSISDB** para ver los objetos de la base de datos del catálogo de SSIS.

## <a name="start-the-integration-services-deployment-wizard"></a>Iniciar el Asistente para implementación de Integration Services
1. En el Explorador de objetos, con el nodo **Catálogos de Integration Services** y el nodo **SSISDB** expandidos, expanda una carpeta.

2.  Seleccione el nodo **Proyectos**.

3.  Haga doble clic en el nodo **Proyectos** y seleccione **Implementar proyecto**. Se abre el Asistente para implementación de Integration Services. Puede implementar un proyecto desde el catálogo actual o desde el sistema de archivos.

## <a name="deploy-a-project-with-the-wizard"></a>Implementar un proyecto con el asistente
1. En la página **Introducción** del asistente, revise la introducción. Haga clic en **Siguiente** para abrir la página **Seleccionar origen**.

2. En la página **Seleccionar origen**, seleccione el proyecto SSIS existente que hay que implementar.
    -   Para implementar un archivo de implementación de proyecto que haya creado compilando un proyecto en el entorno de desarrollo, seleccione **Archivo de implementación de proyecto** y especifique la ruta de acceso al archivo .ispac.
    -   Para implementar un proyecto que ya se ha implementado en una base de datos del catálogo de SSIS, seleccione **Catálogo de Integration Services** y especifique el nombre del servidor y la ruta de acceso al proyecto del catálogo.
    Haga clic en **Siguiente** para ver la página **Seleccionar destino** .
  
3.  En la página **Seleccionar destino**, seleccione el destino del proyecto.
    -   Escriba el nombre completo del servidor. Si el servidor de destino es un servidor de Azure SQL Database, el nombre tendrá el formato `<server_name>.database.windows.net`.
    -   Proporcione la información de autenticación y, después, seleccione **Conectar**. Vea [Métodos de autenticación en la implementación](#authentication-methods-for-deployment) en este artículo.
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
    - [Deploy an SSIS package with Transact-SQL (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md) [Implementar un paquete SSIS con Transact-SQL (SSMS)]
    - [Deploy an SSIS package with Transact-SQL (VS Code)](ssis-quickstart-deploy-tsql-vscode.md) [Implementar un paquete SSIS con Transact-SQL (VSCode)]
    - [Deploy an SSIS package from the command prompt](./ssis-quickstart-deploy-cmdline.md) (Implementar un paquete SSIS desde el símbolo del sistema)
    - [Deploy an SSIS package with PowerShell](ssis-quickstart-deploy-powershell.md) (Implementar un paquete SSIS con PowerShell)
    - [Deploy an SSIS package with C#](./ssis-quickstart-deploy-dotnet.md) (Implementar un paquete SSIS con C#) 
- Ejecute un paquete implementado. Para ejecutar un paquete, puede elegir entre varias herramientas y lenguajes. Para más información, consulte los siguientes artículos:
    - [Ejecutar un paquete SSIS con SSMS](./ssis-quickstart-run-ssms.md)
    - [Run an SSIS package with Transact-SQL (SSMS) (Ejecutar un paquete SSIS con Transact-SQL [SSMS])](./ssis-quickstart-run-tsql-ssms.md)
    - [Ejecutar un paquete SSIS con Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Ejecutar un paquete SSIS desde el símbolo del sistema](./ssis-quickstart-run-cmdline.md)
    - [Run an SSIS package with PowerShell](ssis-quickstart-run-powershell.md) (Ejecutar un paquete de SSIS con PowerShell)
    - [Ejecutar un paquete SSIS con C#](./ssis-quickstart-run-dotnet.md)