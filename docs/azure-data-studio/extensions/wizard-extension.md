---
title: Creación de una extensión de asistente
description: En este tutorial se muestra cómo crear una extensión de asistente para agregar funcionalidad personalizada a Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 0fc56805427b820dc8ccb8cafedb0c04eb34ab6d
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495742"
---
# <a name="create-an-azure-data-studio-wizard-extension"></a>Creación de una extensión de asistente de Azure Data Studio

En este tutorial se muestra cómo crear una **extensión de asistente de Azure Data Studio**. La extensión contribuye con un asistente para interactuar con los usuarios en Azure Data Studio.

En este artículo, aprenderá a:
> [!div class="checklist"]
> - Instalación del generador de extensiones
> - Creación de la extensión
> - Agregar un asistente personalizado a la extensión
> - Prueba de la extensión
> - Empaquetado de la extensión
> - Publicación de la extensión en Marketplace

## <a name="prerequisites"></a>Prerrequisitos

Azure Data Studio se basa en el mismo marco que Visual Studio Code, por lo que las extensiones para Azure Data Studio se crean con Visual Studio Code. Para comenzar, necesitará los siguientes componentes:

- [Node.js](https://nodejs.org) instalado y disponible en su `$PATH`. Node.js incluye [npm](https://www.npmjs.com/), el administrador de paquetes de Node.js, que se usa para instalar el generador de extensiones.
- [Visual Studio Code](https://code.visualstudio.com) para depurar la extensión.
- La [extensión Debug](https://marketplace.visualstudio.com/items?itemName=ms-mssql.sqlops-debug) de Azure Data Studio (opcional). Esto permite probar la extensión sin necesidad de empaquetarla e instalarla en Azure Data Studio.
- Asegúrese de que `azuredatastudio` está en su ruta de acceso. En el caso de Windows, asegúrese de elegir la opción `Add to Path` de setup.exe. En Mac o Linux, ejecute la opción **Install 'azuredatastudio' command in PATH** (Instalar el comando "azuredatastudio" en PATH) desde la paleta de comandos de Azure Data Studio.

## <a name="install-the-extension-generator"></a>Instalación del generador de extensiones

Para simplificar el proceso de creación de extensiones, hemos compilado [un generador de extensiones](https://code.visualstudio.com/docs/extensions/yocode) con Yeoman. Para instalarlo, ejecute lo siguiente desde el símbolo del sistema:

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-wizard-extension"></a>Creación de la extensión de asistente

### <a name="introduction-to-wizards"></a>Introducción a los asistentes

Los asistentes son un tipo de interfaz de usuario que presenta páginas paso a paso para que los usuarios las rellenen, con el fin de llevar a cabo una tarea. Algunos ejemplos comunes son los asistentes para la instalación de software y los de solución de problemas. Por ejemplo:

:::image type="content" source="media/wizard-extension/dacpac-wizard.gif" alt-text="Asistente DACPAC":::

Como los asistentes suelen ser útiles al trabajar con datos y extender la funcionalidad de Azure Data Studio, Azure Data Studio ofrece API para crear asistentes personalizados propios. Se le guiará por el modo de generar una plantilla de asistente y modificarla para crear un asistente personalizado propio.

### <a name="run-the-extension-generator"></a>Ejecución del generador de extensiones

Para crear una extensión:

1. Inicie el generador de extensiones con el siguiente comando:

   `yo azuredatastudio`

2. Elija **Nuevo asistente o cuadro de diálogo** en la lista de tipos de extensiones. Después, seleccione **Asistente** y la **plantilla Introducción**.

3. Siga los pasos para rellenar el nombre de la extensión (en este tutorial, use **Mi extensión de prueba**) y agregue una descripción.

    :::image type="content" source="media/wizard-extension/extension-generator.png" alt-text="Generador de extensiones":::

Al completar los pasos anteriores, se crea una carpeta. Abra la carpeta en Visual Studio Code y estará listo para crear una extensión de asistente propia.

### <a name="run-the-extension"></a>Ejecución de la extensión

Ahora se ejecutará la extensión para ver qué proporciona la plantilla de asistente. Antes de ejecutarla, asegúrese de que la **extensión Debug de Azure Data Studio** está instalada en Visual Studio Code.

Presione **F5** en VS Code para iniciar Azure Data Studio en modo de depuración con la extensión en ejecución. Después, en Azure Data Studio, ejecute el comando **Iniciar asistente** desde la paleta de comandos (Ctrl+Mayús+P) en la nueva ventana. Esto iniciará el asistente predeterminado que aporta esta extensión:

:::image type="content" source="media/wizard-extension/wizard-template.gif" alt-text="Plantilla del asistente":::

A continuación, se verá cómo modificar este asistente predeterminado.

### <a name="develop-the-wizard"></a>Desarrollo del asistente

Los archivos más importantes para empezar a trabajar con el desarrollo de extensiones son `package.json`, `src/main.ts`y `vsc-extension-quickstart.md`:

- `package.json`: este es el archivo de manifiesto, donde se registra el comando **Iniciar asistente**. Aquí también es donde `main.ts` se declara como punto de entrada del programa principal.
- `main.ts`: contiene el código para agregar elementos de interfaz de usuario al asistente, como páginas, texto y botones.
- `vsc-extension-quickstart.md`: contiene documentación técnica que puede ser una referencia útil durante el desarrollo.

Ahora se realizará un cambio en el asistente: se agregará una cuarta página en blanco. Modifique `src/main.ts` como se muestra a continuación; debería ver una página adicional al iniciar el asistente.

:::image type="content" source="media/wizard-extension/wizard-main.png" alt-text="Asistente principal":::
Una vez que esté familiarizado con la plantilla, estas son algunas ideas adicionales que puede probar:

- Agregar un botón con un ancho de 300 a la página nueva.
- Agregar un componente flexible para colocar el botón.
- Agregar una acción al botón. Por ejemplo, al hacer clic en el botón, inicie un cuadro de diálogo de apertura de archivos o abra un editor de consultas.

## <a name="package-your-extension"></a>Empaquetado de la extensión

Para compartirla con otros usuarios, ha de empaquetar la extensión en un solo archivo. Este se puede publicar en el Marketplace de extensiones de Azure Data Studio o compartir entre su equipo o comunidad. Para ello, debe instalar otro paquete npm desde la línea de comandos:

```console
npm install -g vsce
```

Edite `README.md` como prefiera, vaya hasta el directorio base de la extensión y ejecute `vsce package`. Opcionalmente, puede vincular un repositorio con la extensión o continuar sin ninguno. Para agregar uno, agregue una línea similar al archivo `package.json`.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/anjalia/my-test-extension.git"
}
```

Después de agregar estas líneas, se crea el archivo my-test-extension-0.0.1.vsix, listo para instalarlo en Azure Data Studio.

:::image type="content" source="media/wizard-extension/install-vsix.png" alt-text="Instalación de VSIX":::

## <a name="publish-your-extension-to-the-marketplace"></a>Publicación de la extensión en Marketplace

El Marketplace de extensiones de Azure Data Studio aún no está totalmente implementado, pero el proceso actual consiste en hospedar la extensión VSIX en algún lugar (por ejemplo, una página de versiones de GitHub) y luego enviar una solicitud de incorporación de cambios [al archivo JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) con la información de la extensión.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:
> [!div class="checklist"]
> - Instalación del generador de extensiones
> - Creación de la extensión
> - Agregar un asistente personalizado a la extensión
> - Prueba de la extensión
> - Empaquetado de la extensión
> - Publicación de la extensión en Marketplace

Confiamos en que después de leer esto se sienta inspirado para compilar su propia extensión para Azure Data Studio. Tenemos compatibilidad con información del panel (gráficos atractivos que se ejecutan en la instancia de SQL Server), varias API específicas de SQL y un gran conjunto existente de puntos de extensión heredados de Visual Studio Code.

Si tiene una idea, pero no está seguro de cómo empezar, abra una incidencia o envíe un Tweet al equipo: [azuredatastudio](https://twitter.com/azuredatastudio).

Siempre puede hacer referencia a la [guía de extensiones de Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview) porque aborda todas las API y los patrones existentes.

Para obtener información sobre cómo trabajar con T-SQL en Azure Data Studio, siga el tutorial del editor de T-SQL sobre el:

> [!div class="nextstepaction"]
> [usar el editor de Transact-SQL para crear objetos de la base de datos](../tutorial-sql-editor.md)
