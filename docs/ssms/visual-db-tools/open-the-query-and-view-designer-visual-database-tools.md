---
title: Abrir el Diseñador de consultas y vistas
description: Abrir el Diseñador de consultas y vistas (Visual Database Tools)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- opening View Designer
- View Designer, opening
- Query Designer [SQL Server], opening
- opening Query Designer
ms.assetid: b473f258-d53c-41c0-9ad9-528a2ff141f4
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: ea9bd06205846401d4e6bd65690f0af4e34bdf63
ms.sourcegitcommit: 894c1a23e922dc29b82c1d2c34c7b0ff28b38654
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067362"
---
# <a name="open-the-query-and-view-designer-visual-database-tools"></a>Abrir el Diseñador de consultas y vistas (Visual Database Tools)

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/applies-to-version/sql-asdb.md)]

El Diseñador de consultas y vistas se ejecuta cuando se abre la definición de una vista, cuando se muestran los resultados de una consulta o una vista, o cuando se crea o se abre una consulta. Se compone de cuatro paneles diferentes:  
  
-   El panel Diagrama muestra una presentación gráfica de las tablas o de los objetos con valores de tabla que ha seleccionado en la conexión de datos. También muestra todas las relaciones de combinación entre ellos.  
  
-   El panel Criterios permite definir las opciones de consulta (como qué columnas de datos se van a mostrar, cómo se van a ordenar los resultados y qué filas se van a seleccionar); para ello, las opciones se deben escribir en una cuadrícula con forma de hoja de cálculo.  
  
-   Puede utilizar el panel SQL para crear su propia instrucción SQL o puede hacerlo en los paneles Criterios y Diagrama (en ese caso, las instrucciones SQL se crearán en el panel SQL). A medida que crea la consulta, el panel SQL actualiza la instrucción y le vuelve a dar formato automáticamente para facilitar su lectura.  
  
-   El panel Resultados muestra los resultados de la consulta Select que se ha ejecutado más recientemente (Los resultados de otros tipos de consultas se muestran en cuadros de mensaje).  
  
-   Estos paneles resultan útiles al trabajar con consultas y vistas.  
  
-   Cuando abra una vista o una consulta, se abrirán algunos o todos los paneles. Los paneles se abrirán en función de los valores establecidos en el cuadro de diálogo **Opciones** y del sistema de administración de base de datos a la que está conectado. De forma predeterminada, se abren los cuatro.  
  
### <a name="to-open-the-query-and-view-designer-for-a-view"></a>Para abrir el Diseñador de consultas y vistas para una vista  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en la vista que desea abrir y, a continuación, haga clic en **Diseño** o en **Abrir vista**.  
  
    Si elige **Diseño** , se abrirán los paneles del Diseñador de consultas y vistas en función de las opciones seleccionadas en el cuadro de diálogo **Opciones** . Si elige **Abrir vista** , se abre solo el panel Resultados de forma predeterminada.  
  
### <a name="to-open-the-query-and-view-designer-for-an-existing-query"></a>Para abrir el Diseñador de consultas y vistas para una consulta existente  
  
1.  En el Explorador de soluciones, expanda la carpeta **Consultas** .  
  
2.  Haga doble clic en la consulta que desee abrir.  
  
3.  Resalte las instrucciones de la consulta, haga clic con el botón derecho en el área resaltada y, a continuación, haga clic en **Diseñar consulta en el editor**.  
  
## <a name="see-also"></a>Consulte también  
[Temas de procedimientos de diseño de consultas y vistas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Herramientas Diseñador de consultas y vistas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md)  
  
