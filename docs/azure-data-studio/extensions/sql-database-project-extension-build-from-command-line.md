---
title: Compilación de un proyecto desde la línea de comandos
description: Creación de una extensión SQL Server Database Projects desde la línea de comandos
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 08/07/2020
ms.openlocfilehash: 8f8fd9745391d4fb85a12fa2136e4d3db0b797de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040330"
---
# <a name="build-a-database-project-from-command-line"></a>Creación de un proyecto de base de datos desde la línea de comandos

Aunque la extensión SQL Database Projects (en versión preliminar) para Azure Data Studio proporciona una interfaz de usuario gráfica para [crear un proyecto de base de datos](sql-database-project-extension-build.md), también hay disponible una experiencia de creación de línea de comandos para entornos Windows, macOS y Linux. En este artículo se describen los requisitos previos y la sintaxis necesarios para crear un proyecto SQL en dacpac desde la línea de comandos.

## <a name="prerequisites"></a>Requisitos previos

1. Instale y configure la [extensión Proyectos de base de datos de SQL para Azure Data Studio](sql-database-project-extension.md).

2. Los siguientes archivos dll de .NET Core y el archivo de destino `Microsoft.Data.Tools.Schema.SqlTasks.targets` son necesarios para crear un proyecto de base de datos SQL desde la línea de comandos desde todas las plataformas compatibles con la extensión de Azure Data Studio para SQL Database Projects. Estos archivos se crean mediante la extensión durante la primera compilación completada en la interfaz de Azure Data Studio y se colocan en la carpeta de la extensión en `BuildDirectory`.  Por ejemplo, en Linux, estos archivos se colocan en `~\.azuredatastudio\extensions\microsoft.sql-database-projects-x.x.x\BuildDirectory\`.  Copie estos 10 archivos en una carpeta nueva y accesible o tenga en cuenta su ubicación.  Se hará referencia a esta ubicación como `DotNet Core build folder` en este documento.

    - Microsoft.Data.Tools.Schema.Sql.dll
    - Microsoft.Data.Tools.Schema.Tasks.Sql.dll
    - Microsoft.Data.Tools.Utilities.dll
    - System.Io.Packaging.dll
    - Microsoft.SqlServer.Dac.dll
    - Microsoft.SqlServer.Dac.Extensions.dll
    - Microsoft.SqlServer.TransactSql.ScriptDom.dll
    - Microsoft.SqlServer.Types.dll
    - Microsoft.Data.Tools.Schema.SqlTasks.targets
    - System.ComponentModel.Composition.dll
    - Microsoft.Data.SqlClient.dll

3. Si el proyecto se creó en Azure Data Studio, vaya directamente a la sección sobre cómo [crear el proyecto desde la línea de comandos](#build-the-project-from-the-command-line). Si el proyecto se creó en SQL Server Data Tools (SSDT), abra el proyecto en la extensión SQL Database Projects de Azure Data Studio.  Al abrir el proyecto en Azure Data Studio, se actualiza automáticamente el archivo `sqlproj` con tres modificaciones, las cuales se indican a continuación para su información:

    1. Condiciones de importación

    ```console
    <Import Condition="'$(NetCoreBuild)' == 'true'" Project="$(NETCoreTargetsPath)\Microsoft.Data.Tools.Schema.SqlTasks.targets"/> 
    <Import Condition="'$(NetCoreBuild)' != 'true' AND '$(SQLDBExtensionsRefPath)' != ''" Project="$(SQLDBExtensionsRefPath)\Microsoft.Data.Tools.Schema.SqlTasks.targets"/>
    <Import Condition="'$(NetCoreBuild)' != 'true' AND '$(SQLDBExtensionsRefPath)' == ''" Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets"/>
    ```

    2. Referencia de paquete

    ```console
    <ItemGroup>
        <PackageReference Condition="'$(NetCoreBuild)' == 'true'" Include="Microsoft.NETFramework.ReferenceAssemblies" Version="1.0.0" PrivateAssets="All"/>
    </ItemGroup>
    ```

    3. Destino de limpieza, necesario para admitir la edición dual en SQL Server Data Tools (SSDT) y Azure Data Studio

        ```console
        <Target Name="AfterClean">
            <Delete Files="$(BaseIntermediateOutputPath)\project.assets.json"/>
        </Target>
        ```

## <a name="build-the-project-from-the-command-line"></a>Creación del proyecto desde la línea de comandos

En la carpeta .NET completa, use el comando siguiente:

```console
dotnet build "<sqlproj file path>" /p:NetCoreBuild=true /p:NETCoreTargetsPath="<DotNet Core build folder>"
```

Por ejemplo, desde `/usr/share/dotnet` en Linux:

```console
dotnet build "/home/myuser/Documents/DatabaseProject1/DatabaseProject1.sqlproj" /p:NetCoreBuild=true /p:NETCoreTargetsPath="/home/myuser/.azuredatastudio-insiders/extensions/microsoft.sql-database-projects-0.1.2/BuildDirectory"  
```

## <a name="next-steps"></a>Pasos siguientes

- [Extensión Proyectos de base de datos de SQL para Azure Data Studio](sql-database-project-extension.md)
- [Publicación de proyectos de base de datos SQL](sql-database-project-extension-build.md#publish-a-database-project)
