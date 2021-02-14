---
description: Cómo colaborar en la documentación de SQL Server
title: Cómo colaborar en la documentación de SQL Server | Microsoft Docs
ms.date: 08/13/2018
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: a0f3f665421c04208af1123e659aadcfe513d407
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080316"
---
# <a name="how-to-contribute-to-sql-server-documentation"></a>Cómo colaborar en la documentación de SQL Server

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Cualquier usuario puede colaborar en la documentación de SQL Server. Implica la corrección de errores tipográficos, la sugerencia de explicaciones mejoradas y la mejora de la precisión técnica. En este artículo se explica cómo empezar con las colaboraciones de contenido y cómo funciona el proceso.

Hay dos flujos de trabajo principales que se pueden aplicar para colaborar:

|Flujo de trabajo|Descripción|
|---|---|
| [Edición en el navegador](#githubui) | Es útil para ediciones rápidas y breves de cualquier artículo. |
| [Edición local con herramientas](#tools) | Es útil para ediciones más complejas, ediciones que afecten a varios artículos y colaboraciones frecuentes en docs.microsoft.com. |

El equipo de contenido de SQL valida todas las contribuciones públicas para mejorar la precisión técnica y la coherencia. 

## <a name="edit-in-your-browser"></a><a id="githubui"></a> Edición en el navegador

Puede realizar modificaciones sencillas en el contenido de SQL Server en el explorador y, después, enviarlas a Microsoft. Para más información, puede ver la [Guía para colaboradores de Microsoft Docs: información general](/contribute/#quick-edits-to-existing-documents). 

El proceso se resume en los pasos siguientes: 

1. En la página sobre la que tiene comentarios, seleccione el vínculo **Editar** en la parte superior derecha.
1. En la página siguiente, seleccione el icono de **lápiz** situado en la parte superior derecha.
1. En la página siguiente, en la ventana de texto **Editar archivo**, haga las modificaciones directamente en el texto que quiera cambiar.
    Si necesita ayuda para dar formato al texto nuevo o modificado, consulte [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) (Hoja de referencia de Markdown).
1. Una vez realizadas las modificaciones, en **Confirmar cambios**:
    1. En el primer cuadro de texto, escriba una breve descripción del cambio hecho.
    1. En el cuadro **Agregar una descripción amplia adicional**, proporcione una breve explicación del cambio.
1. Seleccione **Propose file change** (Proponer cambio de archivo).
1. En la página **Comparing changes** (Comparación de cambios), seleccione **Crear solicitud de incorporación de cambios**. 
1. En la página **Open a pull request** (Abrir una solicitud de incorporación de cambios), seleccione **Crear solicitud de incorporación de cambios**. 

En el GIF siguiente se muestra el proceso completo para enviar los cambios en el explorador:

![Cómo colaborar en la documentación de SQL Server](media/sql-server-docs-navigation-guide/edit-sql-docs.gif)

## <a name="edit-locally-with-tools"></a><a id="tools"></a> Edición local con herramientas

Otra opción de edición consiste en bifurcar el repositorio **sql-docs** o **azure-docs** y clonarlo localmente en el equipo. Después puede usar un editor de Markdown y un cliente de Git para enviar los cambios. Este flujo de trabajo es útil para las ediciones que son más complejas o que afectan a varios archivos. También es práctico para los colaboradores frecuentes de docs.microsoft.com.

Para colaborar con este método, consulte los siguientes artículos:

- [Creación de una cuenta de GitHub](/contribute/get-started-setup-github)
- [Instalación de herramientas de creación de contenido](/contribute/get-started-setup-tools)
- [Configuración local de un repositorio de Git](/contribute/get-started-setup-local)
- [Uso de herramientas para la colaboración](/contribute/how-to-write-workflows-major)

Si envía una solicitud de incorporación de cambios con cambios importantes en la documentación, recibirá un comentario en GitHub en el que se le pedirá que envíe un **contrato de licencia de colaboración (CLA)** en línea. Debe cumplimentar el formulario en línea para que se pueda aceptar la solicitud de incorporación de cambios.

## <a name="recognition"></a>Reconocimiento

Si se aceptan los cambios, será reconocido como colaborador en la parte superior del artículo.

![Reconocimiento de la colaboración en contenido](./media/sql-server-docs-contribute/contribution-recognition.png)

## <a name="sql-docs-overview"></a>información general de sql-docs

En esta sección se muestran instrucciones adicionales sobre cómo trabajar en el repositorio **sql-docs**.

> [!IMPORTANT]
> La información de esta sección es específica de **sql-docs**. Si edita un artículo de SQL en la documentación de Azure, vea [el archivo Léame del repositorio azure-docs en GitHub](https://github.com/MicrosoftDocs/azure-docs/blob/master/README.md).

El repositorio [sql-docs](https://github.com/MicrosoftDocs/sql-docs) usa muchas carpetas estándares para organizar el contenido.

| Carpeta | Descripción |
|---|---|
| [docs](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs) | Contiene todo el contenido publicado de SQL Server. Las subcarpetas organizan de forma lógica distintas áreas del contenido. |
| [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) | Contiene archivos de inclusión. Estos archivos son bloques de contenido que se pueden incluir en uno o más temas. |
| **./media** | Cada carpeta puede tener una subcarpeta **media** para imágenes de artículos. La carpeta **media** tiene a su vez subcarpetas con el mismo nombre que los temas en los que aparece la imagen. Las imágenes deben ser archivos .png con todas las letras en minúsculas y sin espacios en blanco. |
| **TOC.MD** | Archivo de tabla de contenidos. Cada subcarpeta tiene la opción de usar un archivo TOC.MD. |

#### <a name="applies-to-includes"></a>Archivos de inclusión applies-to

Todos los artículos de SQL Server contienen un archivo de inclusión **applies-to** al final del título. Indica las áreas o las versiones de SQL Server a las que se aplica el artículo.

Observe el siguiente ejemplo de Markdown, que incorpora el archivo de inclusión **appliesto-ss-asdb-asdw-pdw-md.md**.

```Markdown
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
```

De esta manera se agrega el siguiente texto en la parte superior del artículo:

![Texto "Se aplica a"](./media/sql-server-docs-contribute/applies-to.png)

Para buscar el archivo de inclusión applies-to correcto para su artículo, aplique las siguientes sugerencias:

- Para obtener una lista de los archivos de inclusión más utilizados, vea ["Applies to" e "Includes" de SQL](applies-to-includes.md).
- Consulte otros artículos que aborden la misma función o una tarea relacionada. Si edita ese mismo artículo, puede copiar el Markdown del vínculo del archivo de inclusión applies-to (puede cancelar la edición sin enviarla).
- Busque el directorio [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) de los archivos que contienen el texto "applies-to". Puede usar el botón **Find** (Buscar) en GitHub para filtrar rápidamente. Haga clic en el archivo para ver cómo se representa.
- Preste atención a la convención de nomenclatura. Si hay x en el nombre, suelen ser marcadores de posición que indican la falta de compatibilidad de un servicio. Por ejemplo, **appliesto-xx-xxxx-asdw-xxx-md.md** indica compatibilidad solo con Azure Synapse Analytics, porque solo se especifica **asdw** y los demás campos contienen x.
- Algunos archivos de inclusión especifican un número de versión, como **tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md**. Use estos archivos de inclusión únicamente si sabe que la característica se ha introducido con una versión específica de SQL Server.

## <a name="contributor-resources"></a>Recursos para los colaboradores

- [Guía del colaborador de docs.microsoft.com](/contribute/)
- [Guía de estilo de Microsoft](/teamblog/style-guide)
- [Markdown basics](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) (Conceptos básicos de Markdown)

> [!TIP]
> Si tiene comentarios sobre el producto en vez de comentarios sobre la documentación, [aquí puede enviar comentarios sobre el producto de SQL Server](https://feedback.azure.com/forums/908035-sql-server).

## <a name="next-steps"></a>Pasos siguientes

Explore el [repositorio sql-docs](https://github.com/MicrosoftDocs/sql-docs) en GitHub.

Busque un artículo, envíe un cambio y ayude a la comunidad de SQL Server. 

Gracias.