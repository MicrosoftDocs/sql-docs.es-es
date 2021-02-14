---
title: Cambio de nombre y refactorización para realizar cambios en los objetos de base de datos
description: Obtenga información sobre los menús Cambiar nombre y Refactorizar del Editor de Transact-SQL. Vea cómo cambiar el nombre de tipos, cambiar esquemas de objeto, expandir caracteres comodín y nombres completos.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbrefactoring.previewdialog
- sql.data.tools.editor.howto.refactoring
- sql.data.tools.dbrefactoring.renamedialog
- sql.data.tools.dbrefactoring.moveschemadialog
- sql.data.tools.dbrefactoring.renameserverdatabasedialog
ms.assetid: f35520e6-8e6e-47b1-87a3-22c0cf2cabdb
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8fd846301467dbbcbf08aa6bf4a02e52fb467c4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018025"
---
# <a name="how-to-use-rename-and-refactoring-to-make-changes-to-your-database-objects"></a>Procedimientos: Uso de Cambiar nombre y Refactorizar para realizar cambios en los objetos de base de datos

El menú contextual Refactorizar del Editor de Transact\-SQL le permite cambiar el nombre de un objeto o moverlo a un esquema diferente y obtener una vista previa de todas las áreas afectadas antes de confirmar el cambio. También puede usar el menú Refactorizar para calificar todas las referencias a objetos de base de datos o para expandir todos los caracteres comodín de las instrucciones `SELECT` en el proyecto de base de datos.  
  
> [!NOTE]  
> Los procedimientos siguientes usan entidades creadas en procedimientos anteriores de las secciones [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md) y [Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-rename-a-type"></a>Para cambiar el nombre de un tipo  
  
1.  Haga clic con el botón derecho en la tabla **Products** (Products.sql) en el **Explorador de soluciones** y seleccione **Ver código** para abrir el script en el Editor de Transact\-SQL.  
  
2.  Haga clic con el botón derecho en `[Products]` en el script, seleccione **Refactorizar** y, a continuación, haga clic en **Cambiar nombre**.  
  
3.  En el campo **Nuevo nombre**, cámbielo a **Product**. Deje activada la opción **Vista previa de los cambios** y haga clic en **Aceptar**.  
  
4.  En la próxima pantalla podrá ver una lista de los scripts que se verán afectados por esta operación de cambio de nombre. En concreto, aparecerán resaltados todos los lugares donde se hace referencia a `Products`. Esto es muy similar a la tarea Buscar todas las referencias del procedimiento anterior. Haga clic en algún sitio en el panel superior y vea el cambio real en los scripts (resaltados en verde) en el panel inferior.  
  
5.  Haga clic en **Aplicar**.  
  
6.  En el caso de los archivos de script que ya están abiertos en el Diseñador de tablas o en el Editor de Transact\-SQL, observe que el Editor de Transact\-SQL ha resaltado las ubicaciones donde se han realizado cambios con una barra verde a la izquierda.  
  
7.  Observe que se ha agregado **TradeDev.refactorlog** en el **Explorador de soluciones**. Haga doble clic para abrirlo. Contiene una representación XML de todos los cambios de esta sesión.  
  
8.  Presione F5 para compilar e implementar el proyecto en la base de datos local.  
  
9. Haga clic con el botón derecho en la base de datos **TradeDev** en **Local** en el **Explorador de objetos de SQL Server** y seleccione **Actualizar**.  
  
10. Expanda **Tablas** y observe que se ha cambiado el nombre de la tabla **Products**.  
  
11. Haga clic con el botón derecho en **Product** y seleccione **Ver datos**. Observe que los datos existentes están intactos independientemente de la operación de cambio de nombre.  
  
### <a name="to-expand-wildcards"></a>Para expandir caracteres comodín  
  
1.  Expanda el nodo **Funciones** en el **Explorador de soluciones** y haga doble clic en **GetProductsBySupplier.sql**.  
  
2.  Sitúe el cursor sobre el asterisco de esta línea y haga clic con el botón secundario. Seleccione **Refactorizar** y haga clic en **Expandir caracteres comodín**.  
  
    ```  
    SELECT * from Product p  
    ```  
  
3.  En el cuadro de diálogo **Vista previa de los cambios**, haga clic en `SELECT * from Product p` en el panel superior para resaltarlo.  
  
4.  En el panel **Vista previa de los cambios**, observe que `*` se ha expandido a lo siguiente en el script.  
  
    ```  
    [Id], [Name], [ShelfLife], [SupplierId], [CustomerId]  
    ```  
  
5.  Haga clic en el botón **Aplicar**.  Observe que la línea que contiene cambios realizados por la operación de expansión está resaltada de nuevo con una barra verde a la izquierda.  
  
### <a name="to-fully-qualify-database-object-names"></a>Para completar nombres de objetos de base de datos  
  
1.  Asegúrese de que **GetProductsBySupplier.sql** sigue abierto en el Editor de Transact\-SQL.  
  
2.  Sitúe el cursor sobre `Product` en esta línea y haga clic con el botón secundario. Seleccione **Refactorizar** y **Nombres completos**.  
  
    ```  
    SELECT [Id], [Name], [ShelfLife], [SupplierId], [CustomerId] from Product p  
    ```  
  
3.  Haga clic en el botón **Aplicar** del cuadro de diálogo **Vista previa de los cambios**.  Observe que todas las referencias de objetos se han actualizado para incluir el nombre del esquema del objeto y, si el objeto tiene un objeto primario, el nombre del objeto primario.  
  
    ```  
    SELECT [p].[Id], [p].[Name], [p].[ShelfLife], [p].[SupplierId], [p].[CustomerId] from [dbo].[Product] p  
    ```  
  
### <a name="to-move-schema"></a>Para mover el esquema  
  
1.  Haga clic con el botón secundario en el objeto que desea mover. Seleccione **Refactorizar** y **Mover esquema**.  
  
2.  En la lista **Nuevo esquema**, haga clic en el nombre del esquema al que desea mover el objeto. Haga clic en Aceptar.  
  
    Si activó la casilla de verificación **Vista previa de los cambios** aparece el cuadro de diálogo **Vista previa de los cambios**. De lo contrario, el nombre de objeto se actualiza y el objeto se mueve el nuevo esquema.  
  
