---
title: Actualización de una base de datos conectada con Power Buffer
description: Aprenda a usar Power Buffer para actualizar una base de datos. Vea cómo comprobar los cambios antes de aplicarlos y cómo guardar los cambios en un script para su posterior implementación.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.commitpreview.dialog
ms.assetid: 4048b7f8-71a9-47ad-b812-3fc1e8066240
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2ecad870723402e33026668055368dac5dfd02c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018095"
---
# <a name="how-to-update-a-connected-database-with-power-buffer"></a>Procedimientos: Actualización de una base de datos conectada con Power Buffer

La tecnología Power Buffer de SQL Server Data Tools simplifica la aplicación de cambios a una base de datos conectada almacenando todas las ediciones de la sesión actual. Todos los errores ocasionados por la edición en la ventana Power Buffer (en el Editor de Transact\-SQL o en el Diseñador de tablas) aparecen inmediatamente en el panel **Lista de errores**, lo que le permite seguir los errores identificados para solucionarlos mejor. Puede comprobar los cambios pendientes hasta que esté preparado para aplicarlos a la base de datos. Durante el proceso de actualización, SSDT crea automáticamente un script ALTER basado en las ediciones y le avisa de cualquier problema posible. A continuación, puede aplicar a la misma base de datos todos los cambios que se han acumulado en todas las ventanas Power Buffer abiertas o puede guardar el script ALTER para implementarlo más adelante.  
  
SSDT también reconoce los cambios realizados al esquema de la base de datos fuera de Visual Studio. Por ejemplo, si agrega una tabla nueva a una base de datos existente en SQL Server Management Studio, ese cambio aparecerá inmediatamente en el Explorador de objetos de SQL Server en Visual Studio sin necesidad de actualizarlo manualmente. La característica de detección de desfases garantiza que siempre está viendo la definición de esquema más reciente de una base de datos en el Explorador de objetos de SQL Server. Tenga en cuenta que los objetos de base de datos abiertos en el Diseñador de tablas o en el Editor de Transact\-SQL para su edición no se actualizarán para mostrar los cambios fuera de Visual Studio.  
  
En los procedimientos siguientes se usan entidades creadas en procedimientos anteriores de la sección [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md).  
  
### <a name="to-apply-the-changes-made-in-the-previous-procedures"></a>Para aplicar los cambios realizados en los procedimientos anteriores  
  
1.  Haga clic en el botón **Actualizar** de la barra de herramientas (se mostrará la información sobre herramientas “Actualizar base de datos” si mantiene el mouse sobre el botón). La barra de herramientas está situada encima de la cuadrícula de columnas del Diseñador de tablas.  
  
2.  Aparecerá el cuadro de diálogo **Vista previa de actualizaciones de base de datos**. Se generará en segundo plano un script de implementación basado en los cambios. A continuación, el cuadro de diálogo mostrará un resumen de las acciones que SSDT va a realizar (por ejemplo, crear o quitar entidades de base de datos), junto con los posibles problemas que ha identificado (esto no es aplicable a nuestro procedimiento, pero será útil cuando la definición de base de datos contenga errores que impidan una acción de actualización hasta que no se resuelvan).  
  
3.  Si no desea actualizar la base de datos en este momento, haga clic en el botón **Cancelar** para salir del cuadro de diálogo **Vista previa de actualizaciones de base de datos**.  
  
4.  Si desea realizar los cambios, haga clic en el botón **Actualizar base de datos** del cuadro de diálogo **Vista previa de actualizaciones de base de datos**. El script de implementación se ejecutará en su nombre y los cambios acumulados se aplicarán ahora a la base de datos.  
  
5.  Si desea ver el script de implementación para comprobarlo o si desea hacer algún cambio antes de la actualización, haga clic en el botón **Generar script** del cuadro de diálogo **Vista previa de actualizaciones de base de datos**. El script generado se abrirá en una nueva ventana del Editor de Transact\-SQL. Puede hacer clic en el botón **Ejecutar consulta** de la barra de herramientas del Editor de Transact\-SQL para ejecutar esta consulta. Esto es similar a lo que hacía el botón **Actualizar base de datos** en el paso 4.  
  
    > [!WARNING]  
    > Si realiza cambios en el script de implementación y lo ejecuta, esos cambios no aparecerán en las entidades de base de datos abiertas. Por ejemplo, si cambia el nombre de una columna de la tabla `Customers` en el script de implementación y lo ejecuta para actualizar la base de datos, y si la tabla `Customers` está abierta en el Diseñador de tablas, el nombre de columna seguirá siendo el anterior cuando hizo clic en el botón **Actualizar base de datos**. Tiene que cerrar manualmente el Diseñador de tablas sin guardarlo localmente como un script. Cuando vuelva a abrir la tabla desde el **Explorador de objetos de SQL Server**, verá que la base de datos se ha actualizado realmente con los cambios efectuados en el script de implementación.  
  
6.  En el panel **Salida** del Editor de Transact\-SQL (o en el panel **Mensaje** si está ejecutando manualmente el script de implementación), observe lo siguiente que indica que la actualización se realizó correctamente.  
  
**Creating [dbo].[Customers]...Creating [dbo].[Products]...Creating [dbo].[Suppliers]...Creating FK_Products_SupplierId...Creating FK_Products_CustomerId...Creating CK_Products_ShelfLife The transacted portion of the database update succeeded.Checking existing data against newly created constraintsUpdate complete.**  
  
7.  En el **Explorador de objetos de SQL Server**, observe que las nuevas tablas han aparecido bajo el nodo **Tablas** de la base de datos **Trade**.  
  
### <a name="to-view-changes-made-to-a-database-outside-visual-studio"></a>Para ver los cambios realizados en una base de datos fuera de Visual Studio  
  
1.  Abra SQL Server Management Studio. En el cuadro de diálogo **Conectar con el servidor**, especifique el mismo servidor de bases de datos con el que ha conectado en Visual Studio y haga clic en **Conectar**.  
  
2.  En el **Explorador de objetos de SQL Server**, expanda **Bases de datos** y vaya la base de datos **Trade**.  
  
3.  Haga clic con el botón derecho en **Tablas** bajo **Trade** y seleccione **Nueva tabla**. En el Diseñador de tablas, escriba **id** como nombre de columna e **int** como tipo de datos.  
  
4.  Haga clic en el icono **Guardar** de la barra de herramientas para guardar la tabla. Acepte el nombre predeterminado y haga clic en **Aceptar**.  
  
    Vuelva a Visual Studio. Examine el nodo **Tablas** bajo la base de datos **Trade** en el **Explorador de objetos de SQL Server**. Observe que aparece la tabla **Table_1** recién creada.  
  
5.  Haga clic con el botón derecho en **Table_1** y seleccione **Eliminar**. Haga clic en **Actualizar base de datos** en el cuadro de diálogo **Vista previa de actualizaciones de base de datos**.  
  
## <a name="see-also"></a>Consulte también  
[Cómo: Corregir errores](../ssdt/how-to-fix-errors.md)  
  
