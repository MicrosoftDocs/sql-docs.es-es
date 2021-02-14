---
title: Depuración de objetos de base de datos
description: Aprenda a depurar procedimientos almacenados, funciones y desencadenadores. Vea cómo activar la depuración, establecer puntos de interrupción y ejecutar una prueba unitaria de SQL Server en modo de depuración.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f5d4584f-e85f-4558-b056-83681c365978
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 9bc07e55af111adca6c8a995e2f40994ed2d9e5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018255"
---
# <a name="how-to--debug-database-objects"></a>Procedimientos:  Depuración de objetos de base de datos

Una prueba unitaria de SQL Server consta de lo siguiente:  
  
-   El código de la prueba unitaria escrito en Visual C\# o Visual Basic. Este código, que lo genera el Diseñador de pruebas unitarias de SQL Server, es el responsable de ejecutar el script de Transact\-SQL que conforma el cuerpo de la prueba.  
  
-   Una o varias condiciones de prueba, que se escriben en Visual C\# o Visual Basic. Para depurar condiciones de prueba, siga el procedimiento para depurar pruebas unitarias que se describe en [Cómo: Depurar mientras se ejecuta una prueba (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182484(v=vs.100)) o [Cómo: Depurar mientras se ejecuta una prueba (Visual Studio 2012)](/previous-versions/ms182484(v=vs.140)).  
  
-   Uno o más scripts de Transact\-SQL que se ejecutan en objetos de la base de datos que se está probando. Estos scripts de Transact\-SQL no se pueden depurar.  
  
En los procedimientos de este tema se describe cómo depurar objetos de base de datos específicos, como los procedimientos almacenados, las funciones y los desencadenadores de la base de datos que se está probando. Para depurar un objeto de base de datos, siga estos procedimientos en este orden:  
  
1.  Habilite la depuración de SQL Server en el proyecto de prueba.  
  
2.  Habilite la depuración de la aplicación en la instancia de SQL Server que hospeda la base de datos que se está probando.  
  
3.  Establezca puntos de interrupción en el script de Transact\-SQL de los objetos de base de datos que se van a depurar.  
  
4.  Depure la prueba unitaria. En este procedimiento, ejecute la prueba en modo de depuración.  
  
### <a name="to-enable-sql-debugging-on-your-test-project"></a>Para habilitar la depuración de SQL en el proyecto de prueba  
  
1.  Abra el **Explorador de soluciones**.  
  
2.  En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto de prueba y haga clic en **Propiedades**.  
  
    Se abrirá una página de propiedades con el mismo nombre que el proyecto de prueba.  
  
3.  En la página de propiedades, haga clic en **Depurar**.  
  
4.  En **Habilitar depuradores**, haga clic en **Habilitar depuración de SQL Server**.  
  
5.  Guarde los cambios.  
  
### <a name="to-set-an-increased-execution-context-timeout-to-enable-debugging-for-your-test-project"></a>Para establecer un tiempo de espera del contexto de ejecución mayor para habilitar la depuración del proyecto de prueba  
  
1.  En el menú **Archivo**, seleccione **Abrir** y haga clic en **Archivo**.  
  
2.  Busque la carpeta que contiene el proyecto de prueba y haga doble clic en el archivo app.config.  
  
    Se abre el archivo app.config en el editor.  
  
3.  Modifique el nodo ExecutionContext para agregar un tiempo de espera de comando, como en el ejemplo siguiente:  
  
    ```  
    <ExecutionContext CommandTimeout ="300" Provider="System.Data.SqlClient" ConnectionString="Data Source=TargetServerName\TargetInstanceName;Initial Catalog=TargetDatabaseName;Integrated Security=True;Pooling=False" />  
    ```  
  
4.  Guarde los cambios.  
  
5.  Recompile el proyecto de prueba unitaria.  
  
> [!IMPORTANT]  
> Si no recompila el proyecto, los cambios realizados en app.config no se aplicarán al ejecutar las pruebas unitarias y la depuración producirá un error.  
  
### <a name="to-add-breakpoints-to-your-transact-sql-script"></a>Para agregar puntos de interrupción al script de Transact\-SQL  
  
1.  En el menú **Ver**, abra el **Explorador de objetos de SQL Server**.  
  
2.  Debajo de **Conexiones de datos**, expanda el nodo de la base de datos que desea probar.  
  
3.  Si aparece una 'x' roja pequeña al lado del icono de la base de datos, la conexión a la base de datos está cerrada. En ese caso, haga clic con el botón derecho en la base de datos y haga clic en **Actualizar**. Podría tener que proporcionar las credenciales para abrir la conexión a la base de datos.  
  
4.  Expanda el nodo **Vistas**, **Procedimientos almacenados** o **Funciones** para buscar el objeto que desea depurar.  
  
5.  Haga doble clic en el objeto que desee depurar.  
  
6.  Haga clic en la barra lateral deshabilitada para establecer un punto de interrupción.  
  
### <a name="to-debug-your-sql-server-unit-test"></a>Para depurar la prueba unitaria de SQL Server  
  
1.  En Visual Studio 2010, abra la ventana **Vista de pruebas** (Prueba -> Ventanas). En Visual Studio 2012, abra la ventana **Explorador de pruebas**.  
  
2.  Haga clic con el botón derecho en la prueba cuyo script Transact\-SQL ejercite el objeto de base de datos en el que ha establecido puntos de interrupción y seleccione **Depurar selección**.  
  
    La prueba se ejecuta en modo de depuración hasta que se encuentra un punto de interrupción en el objeto de base de datos.  
  
3.  (Opcional) Para abrir otra ventana de depuración, abra el menú **Depuración**, seleccione **Ventanas** y haga clic en **Puntos de interrupción**, **Resultado** o **Inmediato**.  
  
## <a name="see-also"></a>Consulte también  
[Ejecutar pruebas unitarias de SQL Server](../ssdt/running-sql-server-unit-tests.md)  
[Depuración de Transact-SQL (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/zefbf0t6(v=vs.100))  
