---
title: Incorporación de extensiones
description: Obtenga información sobre cómo agregar funcionalidad a Azure Data Studio mediante la selección y la instalación de extensiones proporcionadas por Microsoft y terceros.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: ''
ms.date: 10/03/2019
ms.openlocfilehash: 3cc965ffd169d53e59706ca4ba68f2bcdddb5aa9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052125"
---
# <a name="extend-the-functionality-of-azure-data-studio"></a>Extensión de la funcionalidad de Azure Data Studio

Las extensiones de Azure Data Studio proporcionan una manera sencilla de agregar más funcionalidad a la instalación base de Azure Data Studio.

Las extensiones las proporciona el equipo de Azure Data Studio (Microsoft), así como la comunidad de terceros (los usuarios). Para más información sobre la creación de extensiones, vea [Creación de extensiones](./extension-authoring.md).

## <a name="add-azure-data-studio-extensions"></a>Incorporación de extensiones de Azure Data Studio

1. Para acceder a las extensiones disponibles, seleccione el icono Extensiones o **Extensiones** en el menú **Ver**. Puede usar el comando **Vista: Mostrar extensiones**, disponible en la **paleta de comandos** (F1 o `Ctrl+Shift+P`).

    ![Icono del Administrador de extensiones](media/add-extensions/extension-manager-icon.png)

    También puede acceder rápidamente al administrador de extensiones si presiona `Ctrl+Shift+X` (Windows/Linux) o `Command+Shift+X` (Mac).

2. Seleccione una extensión disponible para ver sus detalles.

    ![detalles de la extensión](media/add-extensions/extension-details.png)

3. Seleccione la extensión que quiera e **instálela**.

4. Una vez instalada, seleccione **Recargar** para habilitar la extensión en Azure Data Studio (solo es necesario cuando se instala una extensión por primera vez).

Si tiene problemas para acceder al Administrador de extensiones en Azure Data Studio, puede descargar la extensión que necesita en nuestra [Wiki de GitHub](https://github.com/microsoft/azuredatastudio/wiki/List-of-Extensions).

## <a name="manage-extensions"></a>Administración de extensiones

### <a name="list-installed-extensions"></a>Enumeración de las extensiones instaladas

La vista Extensiones predeterminada muestra las extensiones que están habilitadas actualmente, todas las extensiones recomendadas y un contenedor contraído de todas las extensiones deshabilitadas actualmente. El comando **Vista: Mostrar extensiones instaladas**, disponible en la **paleta de comandos** o en el menú desplegable **Más acciones** `(...)`, muestra una lista de todas las extensiones instaladas, incluidas las extensiones deshabilitadas.

### <a name="uninstall-an-extension"></a>Desinstalación de una extensión

Para desinstalar una extensión, haga clic en el icono de engranaje situado a la derecha de una entrada de extensión y elija **Desinstalar** en el menú desplegable. La extensión seleccionada se desinstalará y tendrá que volver a cargar Azure Data Studio.

 ![desplegable de expresión](media/add-extensions/extension-gear-dropdown.png)

### <a name="disable-an-extension"></a>Deshabilitación de una extensión

En lugar de quitar una extensión de forma permanente, puede deshabilitarla temporalmente. Puede deshabilitar una extensión en todas las sesiones de Azure Data Studio (**Deshabilitar**) o solo en el área de trabajo actual (**Deshabilitar [área de trabajo]** ). También puede deshabilitar todas las extensiones instaladas actualmente a través de la **paleta de comandos** con los comandos **Extensiones: Deshabilitar todas las extensiones** y **Extensiones: Deshabilitar todas las extensiones (área de trabajo)** .

### <a name="enable-an-extension"></a>Habilitación de extensiones

Si se ha deshabilitado una extensión, se encontrará en la sección **Deshabilitado** de la lista de extensiones, marcada como **_Deshabilitada_ *_. Puede volver a habilitarla con los comandos _* Habilitar** o **Habilitar (área de trabajo)** del menú desplegable. La **paleta de comandos** también permite habilitar todas las extensiones con los comandos **Extensiones: Habilitar todas las extensiones** y **Extensiones: Habilitar todas las extensiones (área de trabajo)** .

![habilitación de extensión](media/add-extensions/extensions-enable.png)

### <a name="updating-an-extension"></a>Actualización de una extensión

Azure Data Studio busca e instala automáticamente las actualizaciones de sus extensiones instaladas. Si quiere desactivar la característica de actualización automática, puede hacerlo con el comando **Extensiones: Disable Auto Updating Extensions** (Deshabilitar la actualización automática de extensiones).

Para actualizar manualmente una extensión, puede comprobar si hay actualizaciones de extensión con el comando **Extensiones: Mostrar extensiones obsoletas**, que busca en la lista de extensiones mediante el filtro `@outdated`. Esta acción mostrará todas las actualizaciones disponibles para las extensiones instaladas actualmente. Haga clic en el botón **Actualizar** en una extensión obsoleta y se instalará la actualización. A continuación, se le pedirá que vuelva a cargar Azure Data Studio. También puede actualizar todas las extensiones obsoletas de manera simultánea con el comando **Extensiones: Actualizar todas las extensiones**.

El comando **Extensiones: Buscar actualizaciones de la extensión** es otra manera de comprobar qué extensiones tienen actualizaciones disponibles.

## <a name="install-from-a-vsix"></a>Instalación desde un VSIX

Para instalar manualmente una extensión de Azure Data Studio empaquetada en un archivo `.vsix`, use el comando **Instalar desde VSIX** de la lista desplegable de comandos de la vista Extensiones, o bien use el comando **Extensiones: Instalar desde VSIX** de la paleta de comandos, y apunte al archivo `.vsix` de la extensión.

## <a name="access-installed-azure-data-studio-extensions"></a>Acceso a las extensiones instaladas de Azure Data Studio

Cada extensión mejora la experiencia en Azure Data Studio de manera diferente. Como resultado, el punto de entrada de las extensiones puede variar. Consulte la documentación individual de la extensión instalada para obtener información sobre cómo se puede acceder a sus características una vez instalada.

## <a name="extensions-view-filters"></a>Filtros de la vista Extensiones

El cuadro de búsqueda de la vista Extensiones admite filtros que le ayudarán a encontrar y administrar las extensiones. Los comandos **Mostrar extensiones instaladas** y **Mostrar extensiones recomendadas** usan filtros como `@installed` y `@recommended` en el cuadro de búsqueda.

Para ver una lista completa de todos los filtros y comandos de ordenación, escriba @ en el cuadro de búsqueda de extensiones y navegue por las sugerencias:

![ordenación de extensiones](media/add-extensions/extension-sort.png)

Estos son los filtros de la vista Extensiones:

- `@builtin`: muestra las extensiones que están integradas en Azure Data Studio. Están agrupadas por tipo (lenguajes de programación, temas, etc.).
- `@disabled`: muestra las extensiones instaladas que están deshabilitadas.
- `@enabled`: muestra las extensiones instaladas que están habilitadas. Las extensiones se pueden habilitar y deshabilitar de forma individual.
- `@installed`: muestra las extensiones instaladas.
- `@outdated`: muestra las extensiones instaladas que están obsoletas y que tienen una versión más reciente disponible en el marketplace.
- `@recommended`: muestra las extensiones recomendadas. Están agrupadas como de uso general o específico del área de trabajo.
- `@category`: muestra las extensiones que pertenecen a la categoría especificada. A continuación, se muestran algunas de las categorías admitidas. Para ver la lista completa, escriba @category y siga las opciones de la lista de sugerencias:
    - `@category:themes`
    - `@category:formatters`
    - `@category:snippets` (Estos filtros también se pueden combinar. Por ejemplo, `@installed @category:themes` muestra todos los temas instalados).

Si no se proporciona ningún filtro, la vista Extensiones muestra las extensiones actualmente instaladas y las recomendadas.

### <a name="sorting"></a>Ordenación

Puede ordenar las extensiones con el filtro `@sort`, que acepta los siguientes valores:

- `installs`: orden descendente según el número de instalaciones de la galería de extensiones.
- `rating`: orden descendente según la clasificación de la galería de extensiones (1-5 estrellas).
- `name`: orden alfabético según el nombre de la extensión.

## <a name="common-questions"></a>Preguntas frecuentes

### <a name="where-are-extensions-installed"></a>¿Dónde se instalan las extensiones?

Las extensiones se instalan en una carpeta de extensiones que depende del usuario. En función de la plataforma, la ubicación se encuentra en la siguiente carpeta:

- Windows `%USERPROFILE%\.azuredatastudio\extensions`
- macOS `~/.azuredatastudio/extensions`
- Linux `~/.azuredatastudio/extensions`
