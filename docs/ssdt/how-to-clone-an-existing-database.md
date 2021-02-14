---
title: Clon de una base de datos existente
description: Aprenda a clonar una base de datos. Vea los pasos implicados en la creación de una base de datos, la duplicación de su esquema y la replicación de sus datos.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: aad3594a-11cf-4e68-a622-071a93d43875
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 9dd341612f075c3a92cd01b5cfca1133910f2c57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018455"
---
# <a name="how-to-clone-an-existing-database"></a>Procedimientos: Clon de una base de datos existente

Esta tarea usa algunos de los pasos que ha aprendido en procedimientos anteriores para crear una nueva base de datos y transportar datos existentes. Además, utiliza los pasos descritos en [Uso de Comparación de esquemas para comparar distintas definiciones de base de datos](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md) para sincronizar el esquema de una base de datos de origen y de un proyecto.  
  
Mediante estos pasos, puede crear fácilmente una base de datos de desarrollo o de prueba a partir de una base de datos de producción con un esquema y datos idénticos. Después, puede seguir desarrollando la base de datos de prueba en un modo conectado o puede crear un proyecto de base de datos para su desarrollo y pruebas sin conexión, todo ello sin interrumpir el funcionamiento de la base de datos de producción.  
  
> [!WARNING]  
> Los procedimientos siguientes usan entidades creadas en procedimientos anteriores de la sección [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md).  
  
### <a name="to-create-a-development-database"></a>Para crear una base de datos de desarrollo  
  
1.  En el **Explorador de objetos de SQL Server**, en el nodo **SQL Server**, expanda la instancia de servidor a la que se ha conectado.  
  
2.  Haga clic con el botón derecho en el nodo **Bases de datos** y seleccione **Agregar nueva base de datos**.  
  
3.  Cambie el nombre de la nueva base de datos a **TradeDev**.  
  
4.  Haga clic con el botón derecho en la base de datos **Trade** en el **Explorador de objetos de SQL Server** y seleccione **Comparación de esquemas**. Siga los pasos del tema [Uso de Comparación de esquemas para comparar distintas definiciones de base de datos](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md), eligiendo la base de datos **Trade** original como origen y la nueva base de datos **TradeDev** como destino. Esto actualizará **TradeDev** con el esquema de **Trade**.  
  
### <a name="to-replicate-data"></a>Para replicar datos  
  
1.  El paso anterior solo ha duplicado el esquema de la base de datos de producción en la base de datos de desarrollo. En este procedimiento, duplicará datos de producción en la base de datos de desarrollo.  
  
    Haga clic con el botón derecho en la tabla **Suppliers** de la base de datos **Trade** y seleccione **Ver datos**. Se abrirá el Editor de datos.  
  
2.  Haga clic en el botón **Script** situado junto a **Máximo de filas** en la barra de herramientas.  
  
3.  Cuando se abra la ventana de script, asegúrese de que aparezca Conectado en la barra de estado bajo el panel de scripts de Transact\-SQL. Si se muestra Desconectado, haga clic en el botón **Conectar** (el situado más a la izquierda de la barra de herramientas) y especifique la información del servidor y las credenciales.  
  
4.  En el menú desplegable **Base de datos** que hay junto a los botones **Conectar**/**Desconectar**, seleccione **TradeDev**. Esto es similar a la instrucción Transact\-SQL`USE` y garantizará que el script del editor de código se ejecute en la base de datos **TradeDev**.  
  
5.  Haga clic en el botón **Ejecutar consulta** para ejecutar las instrucciones `INSERT`. Esto insertará todas las filas de la tabla `Suppliers` de la base de datos `Trade` en la tabla `Suppliers` de la base de datos `TradeDev`.  
  
6.  Repita los pasos anteriores para todas las tablas de la base de datos `Trade`, de forma que se repliquen en la base de datos `TradeDev`.  
  
7.  Use el Editor de datos para comprobar que todas las tablas de la nueva base de datos `TradeDev` se han rellenado.  
  
## <a name="see-also"></a>Consulte también  
[Cómo: Usar Comparación de esquemas para comparar distintas definiciones de base de datos](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
