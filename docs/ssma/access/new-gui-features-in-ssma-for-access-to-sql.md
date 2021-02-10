---
description: Nuevas características de la interfaz gráfica de usuario de SSMA para el acceso a SQL
title: Nuevas características de la interfaz gráfica de usuario de SSMA para el acceso a SQL | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 95b95de2-db05-4422-825d-43968ecfd01c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 75bddda078465537e7d68047d6c9c690beafaea4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066580"
---
# <a name="new-gui-features-in-ssma-for-access-to-sql"></a>Nuevas características de la interfaz gráfica de usuario de SSMA para el acceso a SQL
En este capítulo se describen las nuevas características de la interfaz de usuario de SSMA  
  
## <a name="layouts"></a>Diseños  
Esta característica permite elegir una de las dos predefinidas de Windows o crear su propio diseño. Para tener acceso al submenú diseño, en el menú Ver, seleccione diseños. Puede elegir uno de los diseños existentes, agregar el diseño actual o administrar los diseños.  
  
### <a name="add-current-layout"></a>Agregar diseño actual  
Para guardar el diseño de Windows actual, en el menú Ver, seleccione diseños y, a continuación, haga clic en agregar diseño actual.  
  
### <a name="choose-predefined-layout"></a>Elegir el diseño predefinido  
Para elegir uno de los diseños predefinidos, en el menú Ver, seleccione diseños y, a continuación, haga clic en diseño predeterminado o sin exploradores. También puede usar los métodos abreviados Ctrl + Alt + 1 o Ctrl + Alt + 2 para los diseños predefinidos respectivamente.  
  
### <a name="choose-user-defined-layout"></a>Elegir el diseño definido por el usuario  
Para elegir el diseño definido por el usuario, en el menú Ver, seleccione diseños y, a continuación, haga clic en uno de los diseños definidos por el usuario. También puede usar accesos directos definidos para los diseños.  
  
### <a name="manage-layouts"></a>Administrar diseños  
Para abrir el cuadro de diálogo administrar diseños, en el menú Ver, seleccione diseños y haga clic en administrar diseños. En el cuadro de diálogo administrar diseños encontrará una lista de los diseños existentes en el lado izquierdo del cuadro de diálogo. Puede seleccionar el diseño para cambiar su configuración. También puede cambiar el orden de diseño en la lista o eliminar el diseño con los botones situados en la parte superior de la lista. En el lado derecho del cuadro de diálogo puede cambiar la siguiente configuración de diseño:  
  
-   Nombre del diseño  
  
-   Sincronización de exploradores de metadatos  
  
-   Visibilidad y ancho de los exploradores de metadatos de origen y de destino  
  
-   Visibilidad de las ventanas de origen o de destino y sus tamaños  
  
-   Visibilidad y alto de las ventanas auxiliares  
  
## <a name="bookmarks"></a>Marcadores  
Esta característica permite establecer uno o varios marcadores en el código de origen o de destino, encontrar rápidamente un marcador mediante accesos directos, administrar los marcadores con un cuadro de diálogo descriptivo.  
  
### <a name="toggle-bookmark"></a>Alternar marcador  
Puede establecer o quitar un marcador de las siguientes maneras:  
  
-   Usar el botón Alternar marcador en la parte superior de la ventana SQL de origen o de destino  
  
-   Haga clic en el área gris a la izquierda de la ventana SQL.  
  
-   Use Ctrl + Mayús + &lt; 0.. 9 &gt; para establecer el marcador numerado  
  
### <a name="bookmark-navigation"></a>Navegación por marcadores  
Puede recorrer los marcadores de las siguientes maneras:  
  
-   Usar botones marcador siguiente, marcador anterior en la parte superior de la ventana SQL  
  
-   Use Ctrl + &lt; 0.. 9 &gt; para buscar el marcador numerado  
  
-   Usar los botones ir a o ver código fuente en el cuadro de diálogo administrar marcadores  
  
### <a name="removing-bookmark"></a>Quitar marcador  
Puede quitar un marcador de las siguientes maneras:  
  
-   Use el botón borrar en la parte superior de la ventana de SQL para quitar todos los marcadores del documento actual.  
  
-   Usar botones quitar o quitar todo en el cuadro de diálogo administrar marcadores  
  
### <a name="manage-bookmarks"></a>Administración de marcadores  
Para abrir el cuadro de diálogo administrar marcadores, en el menú Edición, haga clic en administrar marcadores. En el cuadro de diálogo verá una lista de marcadores existentes. Puede usar los botones que se encuentran en el lado derecho del cuadro de diálogo para administrar los marcadores.  
  
## <a name="object-history"></a>Historial de objetos  
El historial de objetos GUI le permite las siguientes ventajas al navegar por objetos:  
  
-   Puede usar los botones ir atrás y adelante para navegar por los objetos que ya ha visitado  
  
-   Cuando vuelva al objeto, volverá a la misma pestaña que ha dejado.  
  
-   Cuando vuelva al objeto y la pestaña sea SQL, volverá a la misma posición del cursor que ha dejado.  
  
## <a name="advanced-search-capabilities"></a>Funcionalidades de búsqueda avanzada  
Las funcionalidades de búsqueda avanzada proporcionan características de búsqueda eficaces y flexibles, y permiten buscar declaraciones de objetos, obtener información de objetos, realizar búsquedas rápidas, realizar búsquedas de objetos avanzadas en categorías mediante patrones, etc.  
  
### <a name="get-quick-information"></a>Obtener información rápida  
Puede obtener información rápida sobre el objeto en la posición del cursor de las siguientes maneras:  
  
-   Haga clic en la información rápida del botón en la parte superior de la ventana de SQL  
  
-   Seleccione información rápida en el menú emergente que aparece al hacer clic con el botón secundario  
  
-   Presione Ctrl + Mayús + barra espaciadora  
  
### <a name="find-declaration"></a>Buscar declaración  
Puede ir a la declaración del objeto en la posición del cursor de las siguientes maneras:  
  
-   Haga clic en el botón ir a declaración en la parte superior de la ventana SQL  
  
-   Seleccione ir a declaración en el menú emergente que aparece al hacer clic con el botón secundario.  
  
-   Presione F12  
  
### <a name="quick-search"></a>Búsqueda rápida  
Puede realizar una búsqueda de texto rápida con las siguientes características:  
  
-   Puede iniciar la búsqueda con el método abreviado CTRL + F.  
  
-   Puede repetir la última búsqueda hacia delante mediante F3  
  
-   Puede repetir la última búsqueda hacia atrás con MAYÚS + F3  
  
-   Puede buscar la siguiente aparición de la palabra en la posición del cursor mediante CTRL + F3  
  
-   Puede buscar la repetición anterior de la palabra en la posición del cursor mediante CTRL + MAYÚS + F3.  
  
-   También puede realizar todas estas acciones con elementos de menú.  
  
### <a name="advanced-search"></a>Búsqueda avanzada  
Para abrir el cuadro de diálogo búsqueda avanzada, en el menú Edición, busque y, a continuación, haga clic en búsqueda avanzada. En el cuadro de diálogo, podrá encontrar cualquier objeto que use pattern. En la parte superior del cuadro de diálogo puede elegir área de búsqueda y categorías de objetos.  
  
