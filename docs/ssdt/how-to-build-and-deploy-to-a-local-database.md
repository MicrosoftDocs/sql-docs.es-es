---
title: Compilación e implementación de una base de datos local
description: Obtenga información sobre la instancia del servidor local que proporciona SQL Server 2012. Vea cómo usar esta instancia para compilar, probar y depurar proyectos de desarrollo.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ebca8ff8-9a09-4207-8979-9d577af7c1d5
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 598c75f5ef502d1679bd8ed7f17cb44c0f536abc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063540"
---
# <a name="how-to-build-and-deploy-to-a-local-database"></a>Procedimientos: Compilación e implementación de una base de datos local

Microsoft SQL Server 2012 proporciona una instancia de servidor local a petición, denominada SQL Server Express Local Database Runtime, que se activa al depurar un proyecto de base de datos de SQL Server. Esta instancia de servidor local se puede usar como espacio aislado para compilar, probar y depurar un proyecto. Es independiente de cualquiera de las instancias instaladas de SQL Server y no es accesible fuera de SQL Server Data Tools (SSDT). Esa organización es ideal para los desarrolladores que tienen acceso limitado o no tienen acceso a bases de datos de producción pero desean probar sus proyectos localmente antes de que el personal autorizado los implemente en producción. Además, cuando desarrolle una solución de base de datos para SQL Azure, puede usar la comodidad que ofrece este servidor local para desarrollar y probar su proyecto de base de datos localmente, antes de implementarlo en la nube.  
  
> [!WARNING]  
> Una base de datos bajo el nodo de base de datos local en el Explorador de objetos de SQL Server es un reflejo de su proyecto de base de datos correspondiente y no está relacionada con la base de datos del mismo nombre en una instancia de servidor conectado.  
  
> [!WARNING]  
> Los procedimientos siguientes usan entidades creadas en procedimientos anteriores de las secciones [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md) y [Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-use-the-local-database"></a>Para usar la base de datos local  
  
1.  Tenga en cuenta que en el **Explorador de objetos de SQL Server**, en el nodo **SQL Server**, aparece un nuevo nodo denominado **Local**. Esta es la instancia de base de datos local.  
  
2.  Expanda los nodos **Local** y **Bases de datos**. Observe que aparece una base de datos con el mismo nombre que el proyecto TradeDev. Expanda los nodos bajo esta base de datos. La ventana **Operaciones de Data Tools** muestra el estado de las operaciones de expansión/importación en curso en cualquier base de datos del nodo **Local**. Observe que no contienen ninguna de las tablas y entidades que creamos en procedimientos anteriores.  
  
3.  Presione F5 para depurar el proyecto de base de datos TradeDev.  
  
    De forma predeterminada, SSDT usará la instancia de servidor de bases de datos local para la depuración de proyectos de base de datos. En este caso, SSDT intentará primero compilar el proyecto y, si no hay ningún error, el proyecto (y sus entidades) se implementará en la base de datos local. Si depura el mismo proyecto más tarde, SSDT detectará los cambios realizados desde la última sesión de depuración y solo implementará esos cambios en la base de datos local.  
  
4.  Vuelva a expandir los nodos bajo **TradeDev** del servidor de bases de datos **Local**. Esta vez, observe que las tablas, vistas y funciones se han implementado en el servidor de bases de datos local.  
  
5.  Haga clic con el botón derecho en el nodo **TradeDev** y seleccione **Nueva consulta**.  
  
6.  En el panel de scripts, pegue este código y haga clic en el botón **Ejecutar consulta** para ejecutar la consulta.  
  
    ```  
    select * from dbo.GetProductsBySupplier(1)  
    ```  
  
7.  El panel **Mensaje** muestra (0 filas afectadas) y el panel de **resultados** no devuelve ninguna fila. Esto se debe a que estamos consultando la base de datos local en lugar de la base de datos conectada que contiene datos reales.  
  
    Para confirmarlo, haga clic con el botón derecho en la tabla **Products** bajo esta base de datos local **TradeDev** y seleccione **Ver datos**. Observe que la tabla está vacía.  
  
### <a name="to-replicate-real-data-to-the-local-database"></a>Para replicar datos reales en la base de datos local  
  
1.  En el **Explorador de objetos de SQL Server**, expanda la instancia de SQL Server conectada y busque la base de datos **TradeDev**.  
  
    Haga clic con el botón derecho en la tabla **Suppliers** y seleccione **Ver datos**.  
  
2.  Haga clic en el botón **Script** (el segundo botón desde la derecha) situado encima del Editor de datos. Copie las instrucciones `INSERT` del script.  
  
3.  Expanda la instancia de servidor **Local**, haga clic con el botón derecho en el nodo **TradeDev** y seleccione **Nueva consulta**.  
  
4.  Pegue las instrucciones `INSERT` en esta ventana de consulta y ejecute la consulta.  
  
5.  Repita los pasos anteriores para replicar datos de las tablas **Products** y **Fruits** de la base de datos **TradeDev** conectada en la base de datos **TradeDev** local.  
  
6.  Haga clic con el botón derecho en la instancia de servidor **Local** y seleccione **Actualizar**. Examine las tablas usando **Ver datos** para comprobar que la base de datos local se ha rellenado.  
  
7.  Haga clic con el botón derecho en el nodo **TradeDev** de la instancia de servidor Local y seleccione **Nueva consulta**.  
  
8.  En el panel de scripts, pegue este código y haga clic en el botón **Ejecutar consulta** para ejecutar la consulta.  
  
    ```  
    select * from dbo.GetProductsBySupplier(1)  
    ```  
  
9. En el panel de **resultados** bajo el panel del Editor de Transact\-SQL, verá que se devuelven las filas Apples y Potato Chips de la tabla `Products`.  
  
