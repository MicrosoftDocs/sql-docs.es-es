---
title: Creación de extensiones
description: Puede agregar funcionalidad a Azure Data Studio con una extensión. Aprenda a crear una extensión y a publicarla en la galería de extensiones.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: ''
ms.date: 08/26/2020
ms.openlocfilehash: a7cb4ede1c8ef11d55a40b5b72d825ba02c5a550
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048575"
---
# <a name="extend-functionality-by-creating-azure-data-studio-extensions"></a>Extensión de la funcionalidad mediante la creación de extensiones de Azure Data Studio

Las extensiones de Azure Data Studio proporcionan una manera sencilla de agregar más funcionalidad a la instalación base de Azure Data Studio.

Las extensiones las proporciona el equipo de Azure Data Studio (Microsoft), así como la comunidad de terceros (los usuarios).

## <a name="author-an-extension"></a>Creación de una extensión

Si está interesado en las extensiones de Azure Data Studio, puede crear su propia extensión y publicarla en la galería de extensiones.

### <a name="write-an-extension"></a>Escritura de una extensión

#### <a name="prerequisites"></a>Prerrequisitos

Para desarrollar una extensión ha de tener [Node.js](https://nodejs.org/) instalado y disponible en `$PATH`. Node.js incluye npm, el administrador de paquetes de Node.js, que se usa para instalar el generador de extensiones.

Para crear la extensión, puede usar el generador de extensiones de Azure Data Studio. El [generador de extensiones](https://www.npmjs.com/package/generator-azuredatastudio) de Yeoman es un punto de partida beneficioso para los proyectos de extensión. Para iniciar el generador, escriba el siguiente comando en un símbolo del sistema:

```console
npm install -g yo generator-azuredatastudio
yo azuredatastudio
```

Para obtener instrucciones detalladas sobre cómo empezar a usar la plantilla de extensiones, consulte [Extensión de asignación de teclado](keymap-extension.md), que le guía durante la creación de una extensión.

### <a name="extensibility-references"></a>Referencias de extensibilidad

Para más información sobre la extensibilidad de Azure Data Studio, consulte el artículo sobre [introducción a la extensibilidad](../extensibility.md). También puede ver ejemplos de uso de la API en [ejemplos](https://github.com/Microsoft/azuredatastudio/tree/main/samples) existentes.

## <a name="debug-an-extension"></a>Depuración de una extensión

Puede depurar la nueva extensión mediante la extensión [Debug de Azure Data Studio](https://github.com/kevcunnane/sqlops-debug) de Visual Studio Code.

Para depurar la extensión:

1. Abra la extensión con [Visual Studio Code](https://code.visualstudio.com/).
2. Instale la extensión Debug de Azure Data Studio.
3. Seleccione **F5** o seleccione el icono **Depurar** y, a continuación, seleccione **Iniciar**.
4. Se inicia una nueva instancia de Azure Data Studio en un modo especial (Host de desarrollo de la extensión). Esta nueva instancia tiene ahora en cuenta la extensión.

## <a name="create-an-extension-package"></a>Creación de un paquete de extensión

Después de escribir la extensión, debe crear un paquete VSIX para poder instalarlo en Azure Data Studio. Puede usar [vscode-vsce](https://github.com/Microsoft/vscode-vsce) (extensiones de Visual Studio Code) para crear el paquete VSIX.

```console
npm install -g vsce
cd myExtensionName
vsce package
# The myExtensionName.vsix file has now been generated
```

Con un paquete VSIX, puede compartir la extensión de forma local y privada si comparte el archivo .vsix y usa el comando **Extensiones: Instalar desde archivo VSIX** de la paleta de comandos con el fin de instalar la extensión en Azure Data Studio.

## <a name="publish-an-extension"></a>Publicación de una extensión

Para publicar la nueva extensión en Azure Data Studio:

1. Agregue la extensión a la [galería de extensiones](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json).
2. Actualmente no se admite el hospedaje de extensiones de terceros. En lugar de descargar la extensión, Azure Data Studio tiene la opción de ir a una página de descarga. Para establecer una página de descarga para la extensión, establezca el valor del recurso **Microsoft.AzureDataStudio.DownloadPage**.
3. Cree una solicitud de incorporación de cambios en la rama de versión/extensiones.
4. Envíe una solicitud de revisión al equipo.

La extensión se revisará y se agregará a la galería de extensiones.

### <a name="publish-extension-updates"></a>Publicación de actualizaciones de extensión

El proceso de publicación de actualizaciones es similar al de publicación de una extensión. Asegúrese de que la versión está actualizada en package.json.

## <a name="next-steps"></a>Pasos siguientes

Consulte uno de los siguientes tutoriales sobre creación de extensiones para obtener instrucciones paso a paso sobre cómo empezar:

- [Tutorial de la extensión de distribución de teclado](keymap-extension.md)
- [Tutorial de la extensión de panel](dashboard-extension.md)
- [Tutorial de la extensión de Notebook](notebook-extension.md)
- [Tutorial de la extensión de libro de Jupyter](jupyter-book-extension.md)
- [Tutorial de la extensión de asistente](wizard-extension.md)
