---
title: Creación de un nuevo proyecto de base de datos
description: Aprenda a crear un proyecto de base de datos. Vea cómo importar el esquema desde una base de datos existente en el nuevo proyecto.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbprojectwizard.importschema
- sql.data.tools.SqlProjectImportDatabaseDialog.dialog
- sql.data.tools.importscriptwizard.welcome
- sql.data.tools.importscriptwizard.summary
- sql.data.tools.SqlProjectImportDatabaseSummaryDialog.dialog
- sql.data.tools.importscriptwizard.fileselection
ms.assetid: 0b7883fa-b6e1-4ccf-b1d8-f522fd03a59d
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6039138eda38f67aae2a1f4770ebc93380087607
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018375"
---
# <a name="how-to-create-a-new-database-project"></a>Procedimientos: Creación de un nuevo proyecto de base de datos

Puede crear un nuevo proyecto de base de datos e importar el esquema de la base de datos desde una base de datos existente, un archivo de script .sql o una aplicación de capa de datos (.dacpac). Después, puede invocar las mismas herramientas visuales de diseñador (Editor de Transact\-SQL, Diseñador de tablas) disponibles para el desarrollo de bases de datos conectadas con el fin de realizar cambios en el proyecto de base de datos sin conexión y volver a publicar los cambios en la base de datos de producción. Los cambios también se pueden guardar como un script para publicarlos posteriormente. Mediante el panel **Propiedades del proyecto**, puede cambiar la plataforma de destino a distintas versiones de SQL Server (incluido SQL Azure).  
  
Los dos procedimientos siguientes consiguen básicamente el mismo objetivo creando un nuevo proyecto de base de datos e importando el esquema desde una base de datos existente. Cada objeto de base de datos se representará como un archivo de script de SQL (.sql) en el **Explorador de soluciones**. Para obtener más información sobre cómo importar el esquema de la base de datos desde una instantánea, vea [Cómo: Crear una instantánea de un proyecto](../ssdt/how-to-create-a-snapshot-of-a-project.md).  
  
> [!WARNING]  
> En los procedimientos siguientes se usan entidades creadas en procedimientos anteriores de la sección [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md).  
  
### <a name="to-create-a-new-database-project-off-a-connected-database"></a>Para crear un nuevo proyecto de base de datos a partir de una base de datos conectada  
  
1.  Haga clic con el botón derecho en **TradeDev** del **Explorador de objetos de SQL Server** y seleccione **Crear nuevo proyecto**.  
  
2.  En el cuadro de diálogo **Importar base de datos**, observe que la base de datos seleccionada en el **Explorador de objetos de SQL Server** ha predefinido la configuración de **Conexión de base de datos de origen**. En **Proyecto de destino**, cambie el nombre del proyecto a **TradeDev**.  
  
3.  En la sección **Configuración de importación**, observe las opciones para importar determinados objetos y configuraciones, y para crear carpetas para cada tipo de esquema y/u objeto. Si hay una jerarquía organizada de todos los objetos de base de datos, acepte todas las configuraciones predeterminadas y haga clic en **Iniciar**.  
  
4.  El cuadro de diálogo **Importar base de datos** mostrará una barra de progreso y una lista de objetos que SSDT está importando. Una vez completada la operación de importación, haga clic en **Finalizar** para salir de la última pantalla.  
  
5.  Examine la jerarquía en el **Explorador de soluciones**. Expanda la carpeta **dbo** y verá distintas carpetas **Funciones**, **Tablas** y **Vistas**. Observe que las tablas y las funciones están agrupadas bajo sus carpetas de esquema.  
  
6.  Haga doble clic en **Products.sql** en **Tablas**. Se abrirá el **Diseñador de tablas**, mostrando la interpretación visual de la tabla en la cuadrícula de columnas y la definición de script de la tabla en el panel de scripts. Es idéntico a lo que se ve en la sección [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md).  
  
7.  Desactive la casilla **Permitir valores NULL** para la columna **CustomerId**. Presione CTRL + S para guardar el archivo.  
  
8.  Haga clic con el botón derecho en el proyecto **TradeDev** en el **Explorador de soluciones** y seleccione **Compilar** para compilar el proyecto de base de datos.  
  
    El resultado de la operación de compilación se puede ver en la ventana Resultados.  
  
### <a name="to-create-a-new-project-and-import-existing-database-schema"></a>Para crear un nuevo proyecto e importar un esquema de la base de datos existente  
  
1.  Haga clic sucesivamente en **Archivo**, en **Nuevo** y en **Proyecto**. En el cuadro de diálogo **Nuevo proyecto**, seleccione **SQL Server** en el panel izquierdo. Observe que solo hay un tipo de proyecto de base de datos: **Proyecto de base de datos de SQL Server**. No hay ningún proyecto específico de la plataforma como en las versiones anteriores de Visual Studio. Podrá establecer la plataforma de destino en el cuadro de diálogo **Configuración del proyecto** una vez creado el proyecto. Esa tarea se explicará en el tema [Cómo: Cambiar la plataforma de destino y publicación de un proyecto de base de datos](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md).  
  
2.  Cambie el nombre del proyecto a **TradeDev** y haga clic en **Aceptar** para crear el nuevo proyecto.  
  
3.  Haga clic con el botón derecho en el proyecto recién creado **TradeDev** en el **Explorador de soluciones**, seleccione **Importar** y, a continuación, seleccione **Base de datos**.  
  
    Aparecerá el cuadro de diálogo **Importar base de datos**. En la sección **Conexión de base de datos de origen**, haga clic en **Elija una base de datos** y seleccione **TradeDev**. Si **TradeDev** no aparece en la lista desplegable, use el botón **Nueva conexión** para editar las propiedades de la conexión.  
  
4.  En la sección **Configuración de importación**, observe las opciones para importar determinados objetos y configuraciones, y para crear carpetas para cada tipo de esquema y/u objeto. Si hay una jerarquía organizada de todos los objetos de base de datos, acepte todas las configuraciones predeterminadas y haga clic en **Iniciar**.  
  
5.  El cuadro de diálogo **Importar base de datos** mostrará una barra de progreso y una lista de objetos que SSDT está importando. Una vez completada la operación de importación, haga clic en **Finalizar** para salir de la última pantalla.  
  
6.  Examine la jerarquía en el **Explorador de soluciones**. Expanda la carpeta **dbo** y verá distintas carpetas **Funciones**, **Tablas** y **Vistas**. Observe que las tablas y las funciones están agrupadas bajo sus carpetas de esquema.  
  
7.  Haga doble clic en **Products.sql** en **Tablas**. Se abrirá el **Diseñador de tablas**, mostrando la interpretación visual de la tabla en la cuadrícula de columnas y la definición de script de la tabla en el panel de scripts. Es idéntico a lo que se ve en la sección [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md).  
  
8.  Desactive la casilla **Permitir valores NULL** para la columna **CustomerId**. Presione CTRL + S para guardar el archivo.  
  
9. Haga clic con el botón derecho en el proyecto **TradeDev** en el **Explorador de soluciones** y seleccione **Compilar** para compilar el proyecto de base de datos.  
  
## <a name="see-also"></a>Consulte también  
[Cómo: Cambiar la plataforma de destino y publicar un proyecto de base de datos](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)  
  
