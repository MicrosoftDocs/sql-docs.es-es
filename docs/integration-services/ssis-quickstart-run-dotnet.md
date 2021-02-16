---
description: Ejecutar un paquete SSIS con código C# en una aplicación .NET
title: Ejecutar un proyecto de SSIS con código .NET (C#) | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ba99a313c4d446bdb07030355d7efce0f2d8e862
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336313"
---
# <a name="run-an-ssis-package-with-c-code-in-a-net-app"></a>Ejecutar un paquete SSIS con código C# en una aplicación .NET

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


En este inicio rápido se muestra cómo escribir código C# para conectarse a un servidor de bases de datos y ejecutar un paquete SSIS.

Para crear una aplicación de C#, puede usar Visual Studio, Visual Studio Code u otra herramienta de su elección.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de tener instalado Visual Studio o Visual Studio Code. Descargue la edición gratuita de Visual Studio Community Edition o Visual Studio Code desde [Descargas de Visual Studio](https://www.visualstudio.com/downloads/).

Un servidor de Azure SQL Database escucha en el puerto 1433. Si está intentando conectarse a un servidor de Azure SQL Database desde un firewall corporativo, este puerto debe estar abierto en el firewall corporativo para poder conectarse correctamente.

## <a name="for-azure-sql-database-get-the-connection-info"></a>Para Azure SQL Database, obtener la información de conexión

Para ejecutar el paquete en Azure SQL Database, debe obtener la información de conexión necesaria para conectarse a la base de datos del catálogo de SSIS (SSISDB). Necesita el nombre completo y la información de inicio de sesión del servidor en los procedimientos siguientes.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Bases de datos SQL** en el menú izquierdo y, después, seleccione la base de datos SSISDB en la página **Bases de datos SQL**. 
3. En la página **Introducción** de la base de datos, compruebe el nombre completo del servidor. Mantenga el puntero del ratón sobre el nombre del servidor para ver la opción **Haga clic para copiar**. 
4. Si olvida la información de inicio de sesión del servidor de Azure SQL Database, navegue a la página del servidor de SQL Database para ver el nombre del administrador del servidor. Si es necesario, puede restablecer la contraseña.
5. Haga clic en **Mostrar las cadenas de conexión de la base de datos**.
6. Revise la cadena de conexión **ADO.NET** completa. De manera opcional, el código puede usar una clase `SqlConnectionStringBuilder` para volver a crear esta cadena de conexión con los valores de parámetros individuales que proporcione.

## <a name="create-a-new-visual-studio-project"></a>Crear un nuevo proyecto de Visual Studio

1. En Visual Studio, elija **Archivo**, **Nuevo**, **Proyecto**. 
2. En el cuadro de diálogo **Nuevo proyecto**, expanda **Visual C#**.
3. Seleccione **Aplicación de consola** y escriba *run_ssis_project* como nombre del proyecto.
4. Haga clic en **Aceptar** para crear y abrir el nuevo proyecto en Visual Studio.

## <a name="add-references"></a>Agregar referencias
1. En el Explorador de soluciones, haga clic con el botón derecho en la carpeta **Referencias** y seleccione **Agregar referencia**. Se abre el cuadro de diálogo **Administrador de referencias**.
2. En el cuadro de diálogo **Administrador de referencias**, expanda **Ensamblados** y seleccione **Extensiones**.
3. Seleccione las dos referencias siguientes para agregarlas:
    -   Microsoft.SqlServer.Management.Sdk.Sfc
    -   Microsoft.SqlServer.Smo
4. Haga clic en el botón **Examinar** para agregar una referencia a **Microsoft.SqlServer.Management.IntegrationServices**. [Este ensamblado solo se instala en la caché global de ensamblados (GAC)]. Se abre el cuadro de diálogo **Seleccionar archivos para referencia**.
5. En el cuadro de diálogo **Seleccionar archivos para referencia**, vaya a la carpeta de GAC que contiene el ensamblado. Normalmente, esta carpeta es `C:\Windows\assembly\GAC_MSIL\Microsoft.SqlServer.Management.IntegrationServices\14.0.0.0__89845dcd8080cc91`.
6. Seleccione el ensamblado (es decir, el archivo .dll) en la carpeta y haga clic en **Agregar**.
7. Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Administrador de referencias** y agregue las tres referencias. Para asegurarse de que existen las referencias, consulte la lista **Referencias** en el Explorador de soluciones.

## <a name="add-the-c-code"></a>Agregar el código C# 
1. Abra **Program.cs**.

2. Reemplace el contenido de **Program.cs** por el código siguiente. Agregue los valores adecuados para el servidor, la base de datos, el usuario y la contraseña.

> [!NOTE]
> En el ejemplo siguiente se usa la autenticación de Windows. Para usar la autenticación de SQL Server, reemplace el argumento `Integrated Security=SSPI;` por `User ID=<user name>;Password=<password>;`. Si se va a conectar a un servidor de Azure SQL Database, no puede usar la autenticación de Windows.


```csharp
using Microsoft.SqlServer.Management.IntegrationServices;
using System.Data.SqlClient;

namespace run_ssis_package
{
    class Program
    {
        static void Main(string[] args)
        {
            // Variables
            string targetServerName = "localhost";
            string folderName = "Project1Folder";
            string projectName = "Integration Services Project1";
            string packageName = "Package.dtsx";

            // Create a connection to the server
            string sqlConnectionString = "Data Source=" + targetServerName +
                ";Initial Catalog=master;Integrated Security=SSPI;";
            SqlConnection sqlConnection = new SqlConnection(sqlConnectionString);

            // Create the Integration Services object
            IntegrationServices integrationServices = new IntegrationServices(sqlConnection);

            // Get the Integration Services catalog
            Catalog catalog = integrationServices.Catalogs["SSISDB"];

            // Get the folder
            CatalogFolder folder = catalog.Folders[folderName];

            // Get the project
            ProjectInfo project = folder.Projects[projectName];

            // Get the package
            PackageInfo package = project.Packages[packageName];

            // Run the package
            package.Execute(false, null);

        }
    }
}
```

## <a name="run-the-code"></a>Ejecución del código

1. Para ejecutar la aplicación, presione **F5**.
2. Compruebe que el paquete se ejecutó según lo previsto y, a continuación, cierre la ventana de la aplicación.

## <a name="next-steps"></a>Pasos siguientes
- Tenga en cuenta otras formas de ejecutar un paquete.
    - [Ejecutar un paquete SSIS con SSMS](./ssis-quickstart-run-ssms.md)
    - [Run an SSIS package with Transact-SQL (SSMS) (Ejecutar un paquete SSIS con Transact-SQL [SSMS])](./ssis-quickstart-run-tsql-ssms.md)
    - [Ejecutar un paquete SSIS con Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Ejecutar un paquete SSIS desde el símbolo del sistema](./ssis-quickstart-run-cmdline.md)
    - [Run an SSIS package with PowerShell](ssis-quickstart-run-powershell.md) (Ejecutar un paquete de SSIS con PowerShell)
