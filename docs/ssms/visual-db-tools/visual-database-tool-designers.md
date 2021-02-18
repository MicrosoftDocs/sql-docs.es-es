---
description: Diseñadores de Visual Database Tools
title: Diseñadores de Visual Database Tools
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- data sources [SQL Server]
- View Designer
- Visual Database Tools [SQL Server]
- Database Diagram Designer
- Query Designer [SQL Server]
- design tools [Visual Database Tools]
- Table Designer
- Visual Database Tools [SQL Server], designers
- Properties window [Visual Database Tools]
ms.assetid: bd0ca68e-6f69-42dd-bcb5-ce511673769c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 129de2adfa1b3b033551d3b41db4eac8f34891fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338771"
---
# <a name="visual-database-tool-designers"></a>Diseñadores de Visual Database Tools
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Visual Database Tools es una combinación de herramientas de diseño que se pueden utilizar para trabajar con un origen de datos. Puede utilizar estas herramientas para crear consultas, diseñar o modificar la estructura de una base de datos o actualizar datos. Estas herramientas son: Diseñador de diagramas de base de datos, Diseñador de tablas y Diseñador de consultas y vistas.  
  
## <a name="properties-window"></a>Ventana Propiedades  
La ventana Propiedades no es exclusiva de Visual Database Tools, pero aquí es donde se pueden hacer gran parte de las modificaciones. En ella se muestran las propiedades de los elementos seleccionados (por ejemplo, una tabla) y se pueden editar dichas propiedades (desde el nombre de las propiedades hasta la intercalación de una columna). Algunas propiedades pueden verse en la ventana Propiedades, pero deben modificarse en otra herramienta.  
  
## <a name="database-diagram-designer"></a>Diseñador de diagramas de base de datos  
El Diseñador de diagramas de base de datos proporciona una ventana en la que de forma visual se pueden crear, editar y mostrar las tablas y las relaciones de una base de datos.  
  
Para abrir el Diseñador de diagramas de base de datos, abra un diagrama existente o haga clic con el botón derecho en el nodo Base de datos en el Explorador de objetos y elija **Nuevo diagrama de base de datos** en el menú desplegable.  
  
Una vez que se ha abierto el diseñador, aparece el menú **Diagrama de base de datos** en el menú principal. Este menú es el punto de acceso a las características especiales del diseñador.  
  
> [!NOTE]  
> Este diseñador trabaja con bases de datos de Microsoft SQL Server.  
>   
> Esta versión de Visual Database Tools no admite Microsoft SQL Server 7 ni versiones anteriores.  
  
## <a name="table-designer"></a>Diseñador de tablas  
El Diseñador de tablas es una herramienta visual que permite diseñar y ver una sola tabla en una base de datos de Microsoft SQL Server a la que está conectado.  
  
El Diseñador de tablas consta de dos paneles. La parte superior muestra una cuadrícula en la que cada fila describe una columna de la base de datos. La cuadrícula muestra los atributos fundamentales de cada columna de la base de datos: nombre de columna, tipo de datos, longitud y configuración de valores NULL permitidos.  
  
En la parte inferior del Diseñador de tablas está la pestaña Propiedades de columna, donde se muestran otros atributos de la columna que esté seleccionada en la parte superior.  
  
En el Diseñador de tablas, también puede hacer clic con el botón secundario en la parte de la cuadrícula para obtener acceso a cuadros de diálogos mediante los que podrá crear y modificar las relaciones, las restricciones, los índices y las claves de la tabla.  
  
Para abrir el Diseñador de tablas, abra una tabla existente o haga clic con el botón derecho en el nodo **Tablas** en el Explorador de objetos y elija **Agregar nueva tabla** en el menú desplegable.  
  
Una vez que se ha abierto el diseñador, aparece el menú Diseñador de tablas en el menú principal. Este menú es el punto de acceso a las características especiales del diseñador.  
  
> [!NOTE]  
> Este diseñador trabaja con bases de datos de Microsoft SQL Server.  
>   
> Esta versión de Visual Database Tools no admite Microsoft SQL Server 7 ni versiones anteriores.  
  
## <a name="query-and-view-designer"></a>Diseñador de consultas y vistas  
El Diseñador de consultas y vistas se compone de dos herramientas que funcionan de forma similar. Algunas de las principales diferencias son:  
  
-   Las vistas se guardan con la base de datos mientras que las consultas se guardan con un proyecto de base de datos de Visual Studio.  
  
-   El Diseñador de consultas trabaja con casi cualquier origen de datos, mientras que el Diseñador de vistas solamente funciona con SQL Server.  
  
-   El Diseñador de consultas permite diseñar instrucciones SELECT, INSERT, UPDATE y DELETE DML, mientras que las vistas solo pueden contener instrucciones SELECT.  
  
### <a name="view-designer"></a>Diseñador de vistas  
El Diseñador de vistas permite diseñar y visualizar una vista existente o crear una nueva en una base de datos de Microsoft SQL Server a la que esté conectado.  
  
El Diseñador de vistas tiene cuatro paneles: el panel Diagrama, el panel Criterios, el panel de SQL y el panel Resultados. Para más información detallada sobre cada uno de estos paneles, consulte [Herramientas Diseñador de consultas y vistas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md).  
  
Para abrir el Diseñador de vistas, abra una vista existente o haga clic con el botón derecho en el nodo **Vista** en el Explorador de objetos y elija **Agregar nueva vista** en el menú desplegable.  
  
Una vez que se ha abierto el diseñador, aparece el menú **Diseñador de consultas** en el menú principal. Este menú es el punto de acceso a las características especiales del diseñador.  
  
> [!NOTE]  
> Este diseñador trabaja con bases de datos de Microsoft SQL Server.  
>   
> Esta versión de Visual Database Tools no admite Microsoft SQL Server 7 ni versiones anteriores.  
  
## <a name="see-also"></a>Vea también  
[Diseñar diagramas de base de datos &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-database-diagrams-visual-database-tools.md)  
[Diseñar tablas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-tables-visual-database-tools.md)  
[Temas de procedimientos de diseño de consultas y vistas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
