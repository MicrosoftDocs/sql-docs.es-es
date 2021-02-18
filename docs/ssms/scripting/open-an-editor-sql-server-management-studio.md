---
title: Abrir un editor (SQL Server Management Studio)
description: Aprenda a abrir los editores de consultas, MDX, DMX y XML/A del motor de base de datos en SQL Server Management Studio.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 5d654a60-d205-49d2-a831-b3d986d60024
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce81ec69f717f10c705b0d4e7fc1f5b1c6d6ea70
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354596"
---
# <a name="open-an-editor-sql-server-management-studio"></a>Abrir un editor (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este tema se describe cómo abrir el editor de consultas, MDX, DMX o XML/A de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Cuando se abre, cada ventana del editor aparece como una pestaña en el panel central de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
## <a name="before-you-begin"></a>Antes de empezar  
 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] admite cuatro editores: el editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] para editar scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] , los editores DMX y MDX para editar scripts con esos lenguajes y el editor XML/A para editar scripts XML/A o archivos XML. Cualquiera de los editores también se puede usar para editar archivos de texto.  
  
### <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 Si comparte archivos con usuarios de otros sitios que usan páginas de códigos diferentes, guarde el archivo con la página de códigos Unicode correspondiente para evitar errores al leerlo. Asimismo, cuando guarde archivos para UNIX o Macintosh, asegúrese de guardarlos con el formato de documento correcto. En el menú **Archivo**, haga clic en **Guardar como**, **Guardar con codificación** en la flecha hacia abajo situada junto al botón **Guardar** y, a continuación, elija **Unix** o **Macintosh** en **Fin de línea**.  
  
### <a name="permissions"></a>Permisos  
 Las operaciones que realiza en un editor de código están sujetas a los permisos concedidos a la cuenta de autenticación que usó para iniciar sesión. Por ejemplo, si abre una ventana del editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] mediante la autenticación de Windows, no puede ejecutar instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] que hacen referencia a los objetos para los que no tiene acceso la cuenta de inicio de sesión de Windows.  
  
## <a name="how-to-open-editors"></a>Procedimientos: Abrir editores  
 En esta sección se explica cómo abrir varios editores en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="using-the-filenew-menu"></a>Mediante el menú Archivo/Nuevo  
 En el menú **Archivo** , haga clic en **Nuevo** y, a continuación, seleccione una de las opciones del editor de consultas:  
  
-   **Consulta con conexión actual**: abre una nueva ventana del editor del tipo asociado a la conexión actual en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. La ventana del editor usa la misma información de autenticación que la conexión actual. Por ejemplo, si selecciona una instancia en el Explorador de objetos de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y, a continuación, usa **Consulta con conexión actual**, [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] abre un editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] conectado a la misma instancia con la misma información de autenticación.  
  
-   **Consulta de motor de base de datos**: abre un nuevo editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y un cuadro de diálogo para obtener la información necesaria para conectarse a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   **Consulta MDX de Analysis Services** : abre un nuevo editor de consultas MDX de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] y un cuadro de diálogo para obtener la información necesaria para conectarse a una instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Consulta DMX de Analysis Services** : abre un nuevo editor de consultas DMX de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] y un cuadro de diálogo para obtener la información necesaria para conectarse a una instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Consulta XML/A de Analysis Services** : abre un nuevo editor de consultas XML/A de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] y un cuadro de diálogo para obtener la información necesaria para conectarse a una instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
### <a name="using-the-fileopen-menu"></a>Usar el menú Archivo/Abrir  
 En el menú **Archivo** , haga clic en **Abrir** y, a continuación, navegue a un archivo y ábralo. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] abre el tipo de editor adecuado correspondiente a la extensión de archivo, copia el contenido del archivo en la ventana del editor y también abre un cuadro de diálogo de conexión si es necesario. Por ejemplo, si abre un archivo con la extensión .sql, [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] abre una ventana del editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , copia el contenido del archivo .sql, y abre un cuadro de diálogo de conexión. Si abre un archivo con una extensión no asociada a un editor determinado, [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] abre una ventana y copia el editor de texto de contenido del archivo.  
  
 Para obtener más información, vea [Associate File Extensions to a Code Editor (Asociar extensiones de archivo a un editor de código)](./associate-file-extensions-to-a-code-editor.md).  
  
### <a name="using-the-toolbar"></a>Usar la barra de herramientas  
 En la barra de herramientas de **Estándar** , haga clic en uno de los siguientes botones:  
  
-   **Nueva consulta**: abre una nueva ventana del editor del tipo asociado a la conexión actual en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. La ventana del editor usa la misma información de autenticación que la conexión actual. Por ejemplo, si selecciona una instancia en el Explorador de objetos de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y, a continuación, haga clic en el botón **Nueva consulta** , [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] abre un editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] conectado a la misma instancia con la misma información de autenticación.  
  
-   **Consulta de motor de base de datos**: abre un nuevo editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y un cuadro de diálogo para obtener la información necesaria para conectarse a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   **Consulta MDX de Analysis Services** : abre un nuevo editor de consultas MDX de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] y un cuadro de diálogo para obtener la información necesaria para conectarse a una instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Consulta DMX de Analysis Services** : abre un nuevo editor de consultas DMX de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] y un cuadro de diálogo para obtener la información necesaria para conectarse a una instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Consulta XML/A de Analysis Services** : abre un nuevo editor de consultas XML/A de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] y un cuadro de diálogo para obtener la información necesaria para conectarse a una instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
### <a name="using-object-explorer"></a>Usar el Explorador de objetos  
 Desde el **Explorador de objetos**:  
  
-   Haga clic con el botón derecho en el nodo de servidor conectado a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)]y, después, seleccione **Nueva consulta**. Se abrirá una ventana del editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] conectada a la misma instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y establecerá el contexto de la base de datos de la ventana a la base de datos predeterminada para el inicio de sesión.  
  
-   Haga clic con el botón derecho en un nodo de base de datos y, después, seleccione **Nueva consulta**. Se abrirá una ventana del editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] conectada a la misma instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y establecerá el contexto de la base de datos de la ventana en la misma base de datos.  
  
### <a name="using-solution-explorer"></a>Usar el Explorador de soluciones  
 En el **Explorador de soluciones**, expanda una carpeta, haga clic con el botón derecho en un elemento de esa carpeta y, después, haga clic en **Abrir** o haga doble clic en el elemento o el archivo.  
  
### <a name="using-template-browser-to-open-the-database-engine-query-editor"></a>Usar el Explorador de plantillas para abrir el editor de consultas del motor de base de datos  
  
-   En el menú **Ver** , haga clic en **Explorador de plantillas**.  
  
-   La ventana del **Explorador de plantillas** aparece en el panel derecho.  
  
-   Haga doble clic en una plantilla para abrir una ventana Consulta de motor de base de datos con el texto de la plantilla. Por ejemplo, para abrir una plantilla CREATE DATABASE, abra la carpeta **Plantillas de SQL Server** , abra la carpeta **Bases de datos** y haga doble clic en **Crear base de datos**.