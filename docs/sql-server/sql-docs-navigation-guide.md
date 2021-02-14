---
title: Sugerencias de navegación por la documentación de SQL Server
description: Sugerencias y trucos para navegar por la documentación técnica de SQL Server, donde se explican aspectos como la página central, la tabla de contenido, el encabezado y cómo usar las rutas de navegación y cómo usar el filtro de versión.
ms.date: 08/12/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: fc749f636a4d1b7b2e737e54d9ecd9418321c548
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082560"
---
# <a name="sql-server-docs-navigation-guide"></a>Guía de navegación de documentos de SQL Server

En este tema se proporcionan algunas sugerencias y trucos para navegar por el espacio de documentación técnica de SQL Server.  

## <a name="hub-page"></a>Página central

La página central de SQL Server se puede encontrar en [https://aka.ms/sqldocs](../index.yml?WT.mc_id=akams) y es el punto de entrada para buscar contenido pertinente a SQL Server.

Siempre puede volver a esta página seleccionando **Documentación de SQL** en el encabezado situado en la parte superior de todas las páginas del conjunto de documentación técnica de SQL Server: 

![Documentación de SQL en el encabezado](media/sql-server-docs-navigation-guide/sql-docs-in-header.png)

## <a name="offline-documentation"></a>Documentación sin conexión

Si desea ver la documentación de SQL Server en un sistema sin conexión, tiene dos opciones para hacerlo. Puede crear un archivo PDF cuando se encuentre en la documentación técnica de SQL Server, o bien puede descargar el contenido sin conexión usando [Visor de Ayuda y ayuda sin conexión de SQL Server](./sql-server-offline-documentation.md). 

Si quiere crear un archivo PDF, seleccione el vínculo **Descargar PDF** que se encuentra en la parte inferior de cada tabla de contenido.


![Descargar PDF](media/sql-server-docs-navigation-guide/download-pdf.png)

## <a name="toc-symbols"></a>Símbolos de la TDC 

Las entradas de la tabla de contenido (TDC) que tienen un `>` al final de la entrada indican que se le dirigirá a documentación técnica con una tabla de contenido diferente. 

![Signos de mayor que sencillos en la TDC](media/sql-server-docs-navigation-guide/single-carrots-in-sql-docs-toc.png)

Las entradas de la TDC que tienen un `>>` indican que saldrá de docs.microsoft.com. 

![Marcadores de navegación de la TDC](media/sql-server-docs-navigation-guide/double-carrots-in-sql-docs-toc.png)

Si navega a una de estas páginas y quiere volver a la página técnica principal de SQL Server y a la tabla de contenido, seleccione la entrada "Bienvenido a SQL Server >" que se encuentra en la parte superior de cada una de estas tablas de contenido. 

![Volver a la TDC de SQL](media/sql-server-docs-navigation-guide/navigate-back-to-sql-toc.png)

## <a name="toc-search"></a>Búsqueda de la TDC 
En docs.microsoft.com, puede buscar en el contenido de la tabla de contenido mediante el cuadro de búsqueda de filtro de la parte superior: 

![Uso del cuadro de filtro](media/sql-server-docs-navigation-guide/sql-docs-toc-filter.gif)

## <a name="version-filter"></a>Filtro de la versión
La documentación técnica de SQL Server proporciona contenido para varias versiones admitidas y tipos de SQL Server. Las características pueden variar entre las versiones y los tipos de SQL Server, y por tanto, a veces el contenido puede variar. 

Puede usar el [filtro de versión](versioning-system-monikers-ui-sql-server.md) para asegurarse de estar viendo el contenido de la versión y el tipo adecuados de SQL Server: 

![Filtro de versión de documentación de SQL](media/sql-server-docs-navigation-guide/sql-docs-version-filter.gif)

Al seleccionar **Todo SQL** \> **Hide nothing** (No ocultar nada), se asegura de que todo el contenido esté visible y que no haya nada oculto detrás del filtro de la versión. La opción **Hide nothing** (No ocultar nada) puede revelar contenido pertinente para varias versiones diferentes de SQL Server en el mismo artículo, lo que puede ser contradictorio o confuso. Por tanto, no se recomienda activar la opción [**Hide nothing** (No ocultar nada) para el uso rutinario](versioning-system-monikers-ui-sql-server.md#anchor-allsql-hidenothing). 

## <a name="breadcrumbs"></a>Rutas de navegación

Las rutas de navegación se pueden encontrar debajo del encabezado y encima de la tabla de contenido, e indican dónde se encuentra el artículo actual en la tabla de contenido.  Esto no solo ayuda a establece el contexto del tipo de contenido que está leyendo, sino que también permite navegar hacia atrás en el árbol de la tabla de contenido:

![Rutas de navegación de documentación de SQL](media/sql-server-docs-navigation-guide/sql-docs-bread-crumbs.gif)

## <a name="article-section-navigation"></a>Navegación por las secciones de un artículo

El panel de navegación de la derecha le permite desplazarse rápidamente a las secciones de un artículo, así como identificar su ubicación en el artículo.  

![Navegación a la derecha](media/sql-server-docs-navigation-guide/sql-docs-right-hand-navigation.gif)


## <a name="submit-docs-feedback"></a>Enviar comentarios de los documentos

Si encuentra algún problema dentro de un artículo, puede enviar comentarios al equipo de contenido de SQL para ese artículo; para ello, desplácese hacia abajo hasta el final de la página y seleccione **Comentarios sobre el contenido**.

![Comentarios del contenido de problemas de GIT](media/sql-server-get-help/git-issues.png)

También puede enviar comentarios y sugerencias generales sobre la documentación en [https://aka.ms/sqldocsfeedback](https://aka.ms/sqldocsfeedback). 

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![Cómo colaborar en la documentación de SQL Server](media/sql-server-docs-navigation-guide/edit-sql-docs.gif)

## <a name="next-steps"></a>Pasos siguientes

- Introducción a la [documentación técnica de SQL Server](index.yml).
- Para más información sobre el envío de comentarios o la obtención de ayuda con SQL Server, consulte la página [Obtener ayuda](sql-server-get-help.md). 
- Para acceder rápidamente a todos los tutoriales e inicios rápidos, consulte [Recursos educativos de SQL](../sql-server/educational-sql-resources.yml).