---
title: Creación de una extensión de panel
description: En este tutorial se muestra cómo crear una extensión de panel para agregar funcionalidad personalizada a Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 400dae97922503d37b1ca1c2b726264032fa12cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048605"
---
# <a name="create-an-azure-data-studio-dashboard-extension"></a>Creación de una extensión de panel de Azure Data Studio

En este tutorial se muestra cómo crear una nueva extensión de panel de Azure Data Studio. La extensión contribuye al panel de conexión de Azure Data Studio, para que pueda ampliar la funcionalidad de Azure Data Studio de una manera fácilmente visible para los usuarios.

En este artículo, aprenderá a:

> [!div class="checklist"]
> - Instalar el generador de extensiones.
> - Crear la extensión.
> - Contribuir al panel en la extensión.
> - Probar la extensión.
> - Empaquetar la extensión.
> - Publicar la extensión en marketplace.

## <a name="prerequisites"></a>Prerrequisitos

Azure Data Studio se basa en el mismo marco que Visual Studio Code, por lo que las extensiones para Azure Data Studio se crean con Visual Studio Code. Para comenzar, necesitará los siguientes componentes:

- [Node.js](https://nodejs.org) instalado y disponible en su `$PATH`. Node.js incluye [npm](https://www.npmjs.com/), el administrador de paquetes de Node.js, que se usa para instalar el generador de extensiones.
- [Visual Studio Code](https://code.visualstudio.com) para depurar la extensión.
- La [extensión Debug](https://marketplace.visualstudio.com/items?itemName=ms-mssql.sqlops-debug) de Azure Data Studio (opcional). La extensión de depuración permite probar la extensión sin necesidad de empaquetarla e instalarla en Azure Data Studio.
- Asegúrese de que `azuredatastudio` está en su ruta de acceso. En el caso de Windows, asegúrese de elegir la opción **Agregar a PATH** de setup.exe. En Mac o Linux, ejecute la opción **Install 'azuredatastudio' command in PATH** (Instalar el comando "azuredatastudio" en PATH) desde la paleta de comandos de Azure Data Studio.

## <a name="install-the-extension-generator"></a>Instalación del generador de extensiones

Para simplificar el proceso de creación de extensiones, hemos compilado [un generador de extensiones](https://code.visualstudio.com/docs/extensions/yocode) con Yeoman. Para instalarlo, ejecute el comando siguiente desde el símbolo del sistema:

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-dashboard-extension"></a>Creación de la extensión de panel

### <a name="introduction-to-the-dashboard"></a>Introducción al panel

El panel de conexión de Azure Data Studio es una herramienta eficaz que resume y proporciona conclusiones sobre las conexiones de un usuario.

Hay dos variantes del panel. El *panel de servidor*, en el que se resume todo el servidor, y el *panel de base de datos*, en el que se resume una base de datos individual. Para acceder a cualquiera de ambos paneles, haga clic con el botón derecho en un servidor o una base de datos en el viewlet **Connections** (Conexiones) de Azure Data Studio y haga clic en **Manage** (Administrar).

:::image type="content" source="media/dashboard-extension/dashboard-summary.gif" alt-text="Captura de pantalla que muestra una presentación de los paneles.":::

Hay tres puntos de contribución principales para que las extensiones agreguen funcionalidad al panel:

1. **Pestaña de panel completo**: una pestaña independiente en el panel de la extensión. Se puede agregar a un panel de servidor o de base de datos. Se puede personalizar con widgets, una barra de herramientas y una sección de navegación.
2. **Acciones de la página principal**: botones de acción en la parte superior de la barra de herramientas de conexión.
3. **Widgets**: grafos que se ejecutan en la instancia de SQL Server.

   :::image type="content" source="media/dashboard-extension/dashboard-contrib-points.png" alt-text="Captura de pantalla que muestra los puntos de contribución.":::

### <a name="run-the-extension-generator"></a>Ejecución del generador de extensiones

Para crear una extensión:

1. Inicie el generador de extensiones con el siguiente comando:

   `yo azuredatastudio`

1. Elija **Nuevo panel** en la lista de tipos de extensión.

1. Rellene los mensajes, como se muestra, para crear una extensión que contribuye con una pestaña al panel del servidor.

   :::image type="content" source="media/dashboard-extension/dashboard-generator.png" alt-text="Captura de pantalla que muestra el generador de extensiones.":::

   Hay varios mensajes, por lo que se muestra a continuación un poco más de información sobre lo que significa cada pregunta:

   :::image type="content" source="media/dashboard-extension/dashboard-flowchart.png" alt-text="Captura de pantalla que muestra un diagrama de flujo del panel.":::

Al completar los pasos anteriores, se crea una carpeta. Abra la carpeta en Visual Studio Code y estará listo para crear una extensión de panel propia.

### <a name="run-the-extension"></a>Ejecución de la extensión

Ahora se ejecutará la extensión para ver qué proporciona la plantilla del panel. Antes de ejecutarla, asegúrese de que la **extensión de depuración de Azure Data Studio** está instalada en Visual Studio Code.

Pulse **F5** en Visual Studio Code para iniciar Azure Data Studio en modo de depuración con la extensión en ejecución. Después, puede ver cómo contribuye al panel esta plantilla predeterminada.

A continuación, se verá cómo modificar este panel predeterminado.

### <a name="develop-the-dashboard"></a>Desarrollo del panel

El archivo más importante para empezar a trabajar con el desarrollo de extensiones es `package.json`. Este es el archivo de manifiesto, en el que se registran las contribuciones del panel. Observe las secciones `dashboard.tabs`, `dashboard.insights` y `dashboard.containers`.

Puede probar con estos cambios:

- Experimente con los tipos de información, como barras, barras horizontales y series temporales.
- Escriba sus propias consultas para ejecutarlas en la conexión de SQL Server.
- Consulte este [tutorial de información de ejemplo](../tutorial-qds-sql-server.md) o [este tutorial](../tutorial-table-space-sql-server.md) para obtener tutoriales sobre información específica.

## <a name="package-your-extension"></a>Empaquetado de la extensión

Para compartirla con otros usuarios, tendrá que empaquetar la extensión en un único archivo. La extensión se puede publicar en el marketplace de extensiones de Azure Data Studio, o bien compartir con el equipo o la comunidad. Para realizar este paso, debe instalar otro paquete npm desde la línea de comandos.

```console
npm install -g vsce
```

Edite el archivo `README.md` a su gusto. Vaya al directorio base de la extensión y ejecute `vsce package`. Opcionalmente, puede vincular un repositorio con la extensión o continuar sin ninguno. Para agregar uno, agregue una línea similar al archivo `package.json`.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/anjalia/my-test-extension.git"
}
```

Una vez agregadas estas líneas, se crea el archivo `my-test-extension-0.0.1.vsix` y está listo para su instalación en Azure Data Studio.

:::image type="content" source="media/dashboard-extension/install-vsix.png" alt-text="Captura de pantalla que muestra la instalación con VSIX.":::

## <a name="publish-your-extension-to-the-marketplace"></a>Publicación de la extensión en Marketplace

El marketplace de extensiones de Azure Data Studio está en construcción. El proceso actual consiste en hospedar el archivo VSIX de la extensión en alguna parte, por ejemplo, en una página de versiones de GitHub. A continuación, envíe una solicitud de incorporación de cambios que actualice [este archivo JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) con la información de la extensión.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:
> [!div class="checklist"]
> - Instalar el generador de extensiones.
> - Crear la extensión.
> - Contribuir al panel en la extensión.
> - Probar la extensión.
> - Empaquetar la extensión.
> - Publicar la extensión en marketplace.

Confiamos en que después de leer este artículo se sienta inspirado para crear su propia extensión para Azure Data Studio. Tenemos compatibilidad con información del panel (gráficos atractivos que se ejecutan en la instancia de SQL Server), varias API específicas de SQL y un gran conjunto existente de puntos de extensión heredados de Visual Studio Code.

Si tiene una idea, pero no está seguro de cómo empezar, abra una incidencia o envíe un tweet al equipo a [azuredatastudio](https://twitter.com/azuredatastudio).

Para más información, la [guía de extensiones de Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview) trata sobre todas las API y patrones existentes.

Para obtener información sobre cómo trabajar con T-SQL en Azure Data Studio, siga el tutorial del editor de T-SQL sobre el:

> [!div class="nextstepaction"]
> [usar el editor de Transact-SQL para crear objetos de la base de datos](../tutorial-sql-editor.md)
