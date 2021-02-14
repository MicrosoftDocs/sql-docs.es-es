---
title: Eliminación de objetos y resolución de dependencias
description: Obtenga información sobre cómo cambiar el nombre de los objetos de base de datos o eliminarlos. Vea qué objetos dependientes actualiza SSDT de forma automática y qué dependencias tiene que actualizar.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- Microsoft.VisualStudio.Data.Tools.Project.HelpKeywords.SqlProjectDropDatabaseConfirmationDialog
- sql.data.tools.dropdatabaseconfirmation.dialog
- sql.data.tools.dropmultipledatabasesconfirmation.dialog
ms.assetid: fb31c2b1-ca4f-4e11-a0b6-5c26430f1c8c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 72e9c09651d473c25b2df84e0b6ccecee0964959
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018235"
---
# <a name="how-to-delete-objects-and-resolve-dependencies"></a>Procedimientos: Eliminación de objetos y resolución de dependencias

Al cambiar el nombre de un objeto o eliminarlo en el **Explorador de objetos de SQL Server**, SQL Server Data Tools detecta automáticamente todos sus objetos de dependencia y preparará un script ALTER para cambiar el nombre de la dependencia o quitarla según sea necesario.  
  
> [!WARNING]  
> Los procedimientos siguientes usan entidades creadas en procedimientos anteriores de la sección [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md).  
  
### <a name="to-delete-a-database"></a>Para eliminar una base de datos  
  
1.  Haga clic con el botón derecho en una base de datos del **Explorador de objetos de SQL Server** y seleccione **Eliminar**.  
  
2.  Acepte todas las configuraciones predeterminadas del cuadro de diálogo **Eliminar base de datos** y haga clic en **Aceptar**.  
  
### <a name="to-rename-a-table"></a>Para cambiar el nombre de una tabla  
  
1.  Asegúrese de que la tabla `Customer` no esté abierta en el Diseñador de tablas o en el Editor de Transact\-SQL.  
  
2.  Expanda el nodo **Tablas** en el **Explorador de objetos de SQL Server**. Haga clic con el botón derecho en la tabla **Customer** y seleccione **Cambiar nombre**.  
  
3.  Cambie el nombre de la tabla a **Customers** y presione ENTRAR.  
  
4.  Observe que se invocará automáticamente una operación **Actualización de base de datos** en su nombre. SSDT llamará al procedimiento almacenado sp_rename en su nombre para cambiar el nombre de la tabla. Si hay algún objeto dependiente como restricciones de clave externa, también se actualizarán.  
  
    > [!WARNING]  
    > SSDT no actualizará automáticamente las dependencias basadas en scripts, como referencias a una tabla desde una vista o procedimientos almacenados. Después del cambio de nombre, puede usar el panel **Lista de errores** para encontrar todas las demás dependencias y corregirlas manualmente.  
  
5.  Aplique el cambio siguiendo los pasos descritos en el procedimiento anterior [Cómo: Actualizar una base de datos conectada con Power Buffer](../ssdt/how-to-update-a-connected-database-with-power-buffer.md).  
  
6.  Vuelva a hacer clic con el botón derecho en la tabla **Customers** en el **Explorador de objetos de SQL Server** y seleccione **Ver datos**. Observe que los datos de la tabla están intactos después de la operación de cambio de nombre.  
  
7.  Haga clic con el botón derecho en la tabla **Products** y seleccione **Ver código**. Observe que la referencia de clave externa se ha actualizado automáticamente a `REFERENCES [dbo].[Customers] ([Id])` para reflejar el cambio de nombre.  
  
### <a name="to-delete-a-table"></a>Para eliminar una tabla  
  
1.  Haga clic con el botón derecho en la tabla **Customers** en el **Explorador de objetos de SQL Server** y seleccione **Eliminar**.  
  
2.  En el cuadro de diálogo **Vista previa de actualizaciones de base de datos**, en **Acción del usuario**, observe que SSDT ha identificado todos los objetos dependientes; en este caso, una referencia de clave externa que se quitará.  
  
3.  Haga clic en **Actualizar base de datos**.  
  
4.  Haga clic con el botón derecho en la tabla **Productos** en el **Explorador de objetos de SQL Server** y seleccione **Ver código**. Observe que la referencia de clave externa a la tabla `Customers` ha desaparecido.  
  
    > [!WARNING]  
    > Si ya tiene la tabla **Products** abierta en el Diseñador de tablas o en el Editor de Transact\-SQL cuando se realiza la operación de eliminación, no se actualizará automáticamente para mostrar la eliminación de la referencia de clave externa. Además, en la **Lista de errores** pueden aparecer errores sobre referencias no resueltas. Para resolver este problema, cierre el Diseñador de tablas o el Editor de Transact\-SQL y vuelva a abrir la tabla Products.  
  
