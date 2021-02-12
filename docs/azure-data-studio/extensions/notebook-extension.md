---
title: Creación de una extensión de cuaderno de Jupyter Notebook
description: Obtenga información sobre cómo empaquetar cuadernos en una extensión mediante el generador de extensiones.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: e8480ff67a5afc175d311ca3529c1e679f360106
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048555"
---
# <a name="create-a-jupyter-notebook-extension"></a>Creación de una extensión de cuaderno de Jupyter Notebook

En este tutorial se muestra cómo crear una extensión de Jupyter Notebook para Azure Data Studio. La extensión envía un cuaderno de Jupyter Notebook de ejemplo que se puede abrir y ejecutar en Azure Data Studio.

En este artículo, aprenderá a:

> [!div class="checklist"]
> - Crear un proyecto de extensión.
> - Instalar el generador de extensiones.
> - Crear la extensión de cuaderno.
> - Ejecutar la extensión.
> - Empaquetar la extensión.
> - Publicar la extensión en marketplace.

## <a name="apis-used"></a>API utilizadas

- `azdata.nb.showNotebookDocument`

### <a name="extension-use-cases"></a>Casos de uso de la extensión

Hay varias razones para crear una extensión de cuaderno:
- Compartir documentación interactiva
- Guardar y tener acceso constante a ese cuaderno
- Proporcionar problemas de codificación para que los usuarios los sigan
- Crear versiones y realizar el seguimiento de las actualizaciones del cuaderno

## <a name="prerequisites"></a>Prerrequisitos

Azure Data Studio se basa en el mismo marco que Visual Studio Code, por lo que las extensiones para Azure Data Studio se crean con Visual Studio Code. Para comenzar, necesitará los siguientes componentes:

- [Node.js](https://nodejs.org) instalado y disponible en su `$PATH`. Node.js incluye [npm](https://www.npmjs.com/), el administrador de paquetes de Node.js, que se usa para instalar el generador de extensiones.
- [Visual Studio Code](https://code.visualstudio.com) para depurar la extensión.
- Asegúrese de que `azuredatastudio` está en su ruta de acceso. En el caso de Windows, asegúrese de elegir la opción **Agregar a PATH** de setup.exe. En Mac o Linux, ejecute la opción **Install 'azuredatastudio' command in PATH** (Instalar el comando "azuredatastudio" en PATH) desde la paleta de comandos de Azure Data Studio.

## <a name="install-the-extension-generator"></a>Instalación del generador de extensiones

Para simplificar el proceso de creación de extensiones, hemos compilado [un generador de extensiones](https://www.npmjs.com/package/generator-azuredatastudio) con Yeoman. Para instalarlo, ejecute el comando siguiente desde el símbolo del sistema:

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-extension"></a>Creación de la extensión

Para crear una extensión:

1. Inicie el generador de extensiones con el siguiente comando:

   `yo azuredatastudio`

2. Elija **Nuevo cuaderno (individual)** en la lista de tipos de extensiones.

   :::image type="content" source="media/notebook-extension/notebook-extension-generator.png" alt-text="Generador de extensiones de cuaderno":::

3. Siga los pasos para rellenar el nombre de la extensión. Use **Cuaderno de prueba** en este tutorial. A continuación, especifique el nombre del publicador. Use **Microsoft** en este tutorial. Finalmente, agregue una descripción.

En este punto, existe más de una opción. Puede agregar cuadernos de Jupyter Notebook que ya ha creado, o bien puede usar cuadernos de ejemplo que le haya proporcionado el generador.

En este tutorial, se usará un cuaderno de Python de ejemplo:

   :::image type="content" source="media/notebook-extension/notebook-sample-generator.png" alt-text="Selección del ejemplo de Python":::

Si tiene cuadernos que le interesa enviar, responda que ya tiene cuadernos que desea enviar. Proporcione la ruta de acceso absoluta del archivo donde se encuentran todos los cuadernos o archivos Markdown.

Al completar los pasos anteriores, se crea una carpeta con el cuaderno de ejemplo. Abra la carpeta en Visual Studio Code y estará listo para enviar la nueva extensión de cuaderno.

## <a name="understand-your-extension"></a>Descripción de la extensión

Este es el aspecto que debe tener el proyecto:

   :::image type="content" source="media/notebook-extension/notebook-file-structure-generator.png" alt-text="Estructura de los archivos de la extensión":::

El archivo `vsc-extension-quickstart.md` le proporciona una referencia de los archivos importantes. En el archivo `README.md` puede proporcionar documentación para la nueva extensión. Observe los archivos `package.json`, `notebook.ts` y `pySample.ipynb`.

Si hay archivos o carpetas que no quiere publicar, puede incluir sus nombres en el archivo `.vscodeignore`.

Ahora se examinará `notebook.ts` para comprender lo que hace la extensión recién creada.

```javascript
// This function is called when you run the command `Launch Notebooks: Test Notebook` from the
// command palette in Azure Data Studio. If you want any additional functionality
// to occur when you launch the book, add it to the activate function.
export function activate(context: vscode.ExtensionContext) {
    context.subscriptions.push(vscode.commands.registerCommand('launchNotebooks.test-notebook', () => {
        let notebooksToDisplay: Array<string> = processNotebooks();
        notebooksToDisplay.forEach(name => {
            azdata.nb.showNotebookDocument(vscode.Uri.file(name));
        });
    }));

    // Add other code here if you want to register another command.
}
```

Esta es la función principal de `notebook.ts` a la que se llama cada vez que se ejecuta la extensión mediante el comando, **Iniciar cuadernos: cuaderno de prueba**. El nuevo comando se crea mediante la API `vscode.commands.registerCommand`. La siguiente definición entre llaves es el código que se ejecuta cada vez que se llama a nuestro comando. Cada cuaderno que se encuentre en la función `processNotebooks` se abre en Azure Data Studio mediante `azdata.nb.showNotebookDocument`.

El archivo `package.json` también desempeña un papel importante en el registro del comando, **Iniciar cuadernos: cuaderno de prueba**.

```json
"activationEvents": [
        "onCommand:launchNotebooks.test-notebook"
    ],
    "main": "./out/notebook.js",
    "contributes": {
        "commands": [
            {
                "command": "launchNotebooks.test-notebook",
                "title": "Launch Notebooks: Test Notebook"
            }
        ]
    }
```

Hay un evento de activación para el comando y también se han agregado puntos de contribución específicos. Estos se muestran en el marketplace de extensiones, donde se publican las extensiones, cuando los usuarios consultan la extensión. Si quiere agregar comandos adicionales, asegúrese de hacerlo en el campo `activationEvents`. Para obtener más opciones, consulte [Eventos de activación](https://code.visualstudio.com/api/references/activation-events).

## <a name="package-your-extension"></a>Empaquetado de la extensión

Para compartirla con otros usuarios, tendrá que empaquetar la extensión en un único archivo. La extensión se puede publicar en el marketplace de extensiones de Azure Data Studio, o bien compartir con el equipo o la comunidad. Para realizar este paso, debe instalar otro paquete npm desde la línea de comandos.

```console
npm install -g vsce
```

Edite el archivo `README.md` a su gusto. Vaya al directorio base de la extensión y ejecute `vsce package`. Opcionalmente, puede vincular un repositorio con la extensión o continuar sin ninguno. Para agregar uno, agregue una línea similar al archivo `package.json`.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/laurajjiang/testnotebook.git"
}
```

Una vez agregadas estas líneas, se crea un archivo `my test-notebook-0.0.1.vsix` y está listo para instalarlo y compartirlo con el mundo.

## <a name="run-your-extension"></a>Ejecución de la extensión

Para ejecutar y probar la extensión, abra Azure Data Studio y abra la paleta de comandos mediante **Ctrl+Shift+P**. Busque el comando **Extensiones: Instalar desde VSIX** y vaya a la carpeta que contiene la nueva extensión.

   :::image type="content" source="media/notebook-extension/install-vsix.png" alt-text="Instalación de VSIX":::

La extensión debería aparecer en el panel de extensiones de Azure Data Studio. Vuelva a abrir la paleta de comandos y encontrará el nuevo comando que se ha creado con la extensión, **Iniciar libro: libro de prueba**. Al ejecutarlo, debe abrir el libro de Jupyter que ha empaquetado con la extensión.

   :::image type="content" source="media/notebook-extension/notebook-launch-ads.png" alt-text="Comando Notebook":::

¡Enhorabuena! Ha creado la primera extensión de Jupyter Notebook y ya puede enviarla.

## <a name="publish-your-extension-to-the-marketplace"></a>Publicación de la extensión en Marketplace

El marketplace de extensiones de Azure Data Studio está en construcción. Para publicar, hospede la extensión VSIX en alguna parte, por ejemplo, en una página de versiones de GitHub. A continuación, envíe una solicitud de incorporación de cambios que actualice [este archivo JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) con la información de la extensión.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:
> [!div class="checklist"]
> - Crear un proyecto de extensión.
> - Instalar el generador de extensiones.
> - Crear la extensión de cuaderno.
> - Crear la extensión.
> - Empaquetar la extensión.
> - Publicar la extensión en marketplace.

Confiamos en que después de leer este artículo se sienta inspirado para crear su propia extensión para Azure Data Studio.

Si tiene una idea, pero no está seguro de cómo empezar, abra una incidencia o envíe un tweet al equipo a [azuredatastudio](https://twitter.com/azuredatastudio).

Para más información, la [guía de extensiones de Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview) trata sobre todas las API y patrones existentes.
