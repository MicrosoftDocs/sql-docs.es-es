---
title: Introducción a la extensión SQL Database Projects
description: Introducción al uso de la extensión SQL Database Projects para Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 07/30/2020
ms.openlocfilehash: 397bdebe2bdea292009e0216c6b3004ac9475b18
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040319"
---
# <a name="getting-started-with-the-sql-database-projects-extension-preview"></a>Introducción a la extensión SQL Database Projects (versión preliminar)

En este artículo se describen tres formas de empezar a usar la extensión SQL Database Projects:

1. Para crear un proyecto de base de datos, vaya al viewlet **Projects** en Explorer o busque **New Database Project** (Nuevo proyecto de base de datos) en la paleta de comandos.
2. Los proyectos de base de datos existentes se pueden abrir a través de **Open Database Project** (Abrir proyecto de base de datos) en la paleta de comandos.
3. Comience a partir de una base de datos existente mediante **Import New Database Project** (Importar nuevo proyecto de base de datos) desde la paleta de comandos.

    ![Nuevo viewlet](media/sql-database-projects-extension/projects-viewlet.png)

## <a name="create-an-empty-database-project"></a>Creación de un proyecto de base de datos vacío

En el viewlet **Projects** (Proyectos) en **Explorer** (Explorador), seleccione el botón **New Project** (Nuevo proyecto) y escriba un nombre de proyecto en la entrada de texto que aparece.  En el cuadro de diálogo "Select a Folder" (Seleccionar una carpeta) que aparece, seleccione un directorio para guardar la carpeta del proyecto, el archivo .sqlproj y el resto del contenido.
El proyecto vacío se abre y se ve en el viewlet **Projects** para su edición.

## <a name="open-an-existing-project"></a>Abrir un proyecto existente

En el viewlet **Projects** (Proyectos), seleccione el botón **Open Project** (Abrir proyecto) y abra un archivo *.sqlproj* existente desde el selector de archivos que aparece. Los proyectos existentes se pueden originar en Azure Data Studio o [Visual Studio SQL Server Data Tools](../../ssdt/sql-server-data-tools.md).

El proyecto vacío se abre y sus contenidos se pueden ver en el viewlet **Projects** para su edición.

## <a name="create-a-database-project-from-an-existing-database"></a>Creación de un proyecto de base de datos a partir de una base de datos existente

En el viewlet **Project** (Proyecto), seleccione el botón **Import Project from Database** (Importar proyecto desde una base de datos) y conéctese a una instancia de SQL Server.  Una vez que se haya establecido la conexión, seleccione una base de datos en la lista de bases de datos disponibles y establezca el nombre del proyecto.

Por último, seleccione una estructura de destino para la extracción.  El nuevo proyecto se abre y contiene scripts de SQL para el contenido de la base de datos seleccionada.

## <a name="build-and-publish"></a>Compilación y publicación

En la extensión SQL Database Projects para Azure Data Studio, la implementación del proyecto de base de datos se realiza al compilarlo en un [archivo de aplicación de capa de datos](../../relational-databases/data-tier-applications/data-tier-applications.md) (DACPAC) y publicarlo en una plataforma compatible. Para más información sobre este proceso, consulte [Compilación y publicación de un proyecto](sql-database-project-extension-build.md).

## <a name="schema-compare"></a>Comparación de esquemas

La extensión SQL Database Projects interactúa con la extensión [Comparación de esquemas](schema-compare-extension.md), si está instalada, para comparar el contenido de un proyecto con un DACPAC o una base de datos existente.  La comparación de esquemas resultante se puede usar para ver y aplicar las diferencias entre el origen y el destino.

## <a name="next-steps"></a>Pasos siguientes

- [Compilación y publicación de un proyecto con la extensión SQL Database Projects para Azure Data Studio](sql-database-project-extension-build.md)