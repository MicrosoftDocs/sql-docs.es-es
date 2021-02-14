---
title: Depuración de procedimientos almacenados
description: Aprenda a usar el depurador de Transact-SQL para depurar un procedimiento almacenado de forma interactiva. Vea cómo mostrar la pila de llamadas de SQL, las variables locales y los parámetros.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.EXECUTESTOREDPROCEDURE.DIALOG
ms.assetid: e3c8707f-0f6b-4265-8a5a-81f079330b52
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6c34db253855127451f375045e51418850bc824a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018245"
---
# <a name="how-to-debug-stored-procedures"></a>Procedimientos: Depuración de procedimientos almacenados

El depurador de Transact\-SQL le permite depurar de forma interactiva procedimientos almacenados mostrando la pila de llamadas de SQL, variables locales y parámetros para el procedimiento almacenado de SQL. Como ocurre con la depuración en otros lenguajes de programación, puede ver y modificar variables locales y parámetros, ver variables globales, así como controlar y administrar puntos de interrupción mientras depura un script de Transact\-SQL.  
  
En este ejemplo se muestra cómo crear y depurar un procedimiento almacenado de Transact\-SQL paso a paso por instrucciones.  
  
> [!WARNING]  
> El procedimiento siguiente usa entidades creadas en procedimientos de las secciones [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md) y [Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-debug-stored-procedures"></a>Para depurar procedimientos almacenados  
  
1.  En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **TradeDev**, seleccione **Agregar** y, a continuación, haga clic en **Procedimiento almacenado**. Asigne a este nuevo procedimiento almacenado el nombre **AddProduct** y haga clic en **Agregar**.  
  
2.  Pegue el código siguiente en el procedimiento almacenado.  
  
    ```  
    CREATE PROCEDURE [dbo].[AddProduct]  
    @id int,  
    @name nvarchar(128)  
    AS  
    INSERT INTO [dbo].[Product] (Id, Name) VALUES (@id, @name)  
    ```  
  
3.  Presione F5 para compilar e implementar el proyecto.  
  
4.  En el Explorador de objetos de SQL Server, en el nodo **Local**, haga clic con el botón derecho en la base de datos **TradeDev** y seleccione **Nueva consulta**.  
  
5.  Pegue el código siguiente en la ventana de consulta.  
  
    ```  
    EXEC [dbo].[AddProduct] 50, N'Contoso';  
    GO  
    ```  
  
6.  Haga clic en el margen izquierdo de la ventana para agregar un punto de interrupción a la instrucción `EXEC`.  
  
7.  Haga clic en la flecha desplegable del botón verde de flecha en la barra de herramientas del editor de Transact\-SQL y seleccione **Ejecutar con el depurador** para ejecutar la consulta con la depuración activada.  
  
8.  Como alternativa, puede iniciar la depuración desde el Explorador de objetos de SQL Server. Haga clic con el botón derecho en el procedimiento almacenado **AddProduct** (que se encuentra bajo la base de datos **Local** -> **TradeDev** - > **Programación** -> **Procedimientos almacenados**). Seleccione **Procedimiento de depuración…** . Si el objeto necesita parámetros, aparecerá el cuadro de diálogo **Procedimiento de depuración** con una tabla que contiene una fila para cada parámetro. Cada fila de la tabla contiene una columna para el nombre del parámetro y otra para el valor de dicho parámetro. Especifique valores para los parámetros y haga clic en Aceptar.  
  
9. Asegúrese de que se abre la ventana **Variables locales**. De lo contrario, haga clic en el menú **Depuración**, seleccione **Ventanas** y **Local**.  
  
10. Presione F11 para depurar paso a paso por instrucciones la consulta. Observe que los parámetros del procedimiento almacenado y sus valores respectivos aparecen en la ventana **Variables locales**. O bien, mantenga el puntero sobre el parámetro `@name` de la cláusula `INSERT` y verá que tiene asignado el valor **Contoso**.  
  
11. Haga clic en **Contoso** en el cuadro de texto. Escriba **Fabrikam** y presione ENTRAR para cambiar el valor de la variable `name` durante la depuración. También puede cambiar su valor en la ventana **Variables locales**. Observe que el valor del parámetro se muestra ahora en rojo, indicando que ha cambiado.  
  
12. Presione F10 para recorrer paso a paso por procedimientos el código restante.  
  
13. En el Explorador de objetos de SQL Server, actualice el nodo de base de datos **TradeDev** para ver el nuevo contenido en la vista de datos de la tabla **Product**.  
  
14. En el Explorador de objetos de SQL Server, en el nodo **Local**, busque la tabla **Product** de la base de datos **TradeDev**.  
  
15. Haga clic con el botón derecho en la tabla **Product** y seleccione **Ver datos**. Observe que se ha agregado la nueva fila a la tabla.  
  
