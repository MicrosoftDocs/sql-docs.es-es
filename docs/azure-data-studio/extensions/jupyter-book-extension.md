---
title: Creación de una extensión de libro de Jupyter
description: Obtenga información sobre cómo empaquetar un libro de Jupyter en una extensión mediante el generador de extensiones.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 37875ecc79d471d1e7226337cec65c3323e19875
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052066"
---
# <a name="create-a-jupyter-book-extension"></a>Creación de una extensión de libro de Jupyter

En este tutorial se muestra cómo crear una extensión de libro de Jupyter de Azure Data Studio. La extensión envía un libro de Jupyter de ejemplo que se puede abrir y ejecutar en Azure Data Studio.

En este artículo, aprenderá a:

> [!div class="checklist"]
> - Crear un proyecto de extensión.
> - Instalar el generador de extensiones.
> - Crear la extensión de libro de Jupyter.
> - Ejecutar la extensión.
> - Empaquetar la extensión.
> - Publicar la extensión en marketplace.

## <a name="apis-used"></a>API utilizadas

- `bookTreeView.openBook`

### <a name="extension-use-cases"></a>Casos de uso de la extensión

Hay varias razones para crear una extensión de libro de Jupyter:

- Compartir documentación interactiva y organizada en secciones
- Compartir un libro completo (similar a un libro electrónico, pero distribuido mediante Azure Data Studio)
- Crear versiones y realizar el seguimiento de las actualizaciones del libro de Jupyter

La diferencia principal entre un libro de Jupyter y una extensión de cuaderno es que un libro de Jupyter proporciona organización. Se pueden dividir decenas de cuadernos en capítulos diferentes de un libro de Jupyter, pero la extensión de cuadernos está diseñada para enviar una cantidad reducida de cuadernos individuales.

## <a name="prerequisites"></a>Prerrequisitos

Azure Data Studio se basa en el mismo marco que Visual Studio Code, por lo que las extensiones para Azure Data Studio se crean con Visual Studio Code. Para comenzar, necesitará los siguientes componentes:

- [Node.js](https://nodejs.org) instalado y disponible en su `$PATH`. Node.js incluye [npm](https://www.npmjs.com/), el administrador de paquetes de Node.js, que se usa para instalar el generador de extensiones.
- [Visual Studio Code](https://code.visualstudio.com) para realizar cambios en la extensión y depurarla.
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

1. Elija **New Jupyter Book** (Nuevo libro de Jupyter) en la lista de tipos de extensiones.

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-extension-generator.png" alt-text="Captura de pantalla que muestra el generador de extensiones.":::

1. Siga los pasos para rellenar el nombre de la extensión. Use **Test Book** (Libro de prueba) en este tutorial. A continuación, especifique el nombre del publicador. Use **Microsoft** en este tutorial. Finalmente, agregue una descripción.

Puede elegir entre proporcionar un libro de Jupyter existente, usar un libro de ejemplo proporcionado, o bien trabajar para crear uno. Se muestran las tres opciones.

### <a name="provide-an-existing-book"></a>Empleo de un libro existente

Si quiere enviar un libro que ya ha creado, proporcione la ruta de acceso de archivo absoluta a la carpeta en la que reside el contenido del libro. Después, estará listo para continuar y obtener información sobre la extensión y cómo enviarla.

:::image type="content" source="media/jupyter-book-extension/jupyter-book-existing-book.png" alt-text="Captura de pantalla que muestra un libro existente.":::

### <a name="use-the-sample-book"></a>Uso del libro de ejemplo

Si no tiene un libro ni cuadernos existentes, puede usar el ejemplo proporcionado en el generador.

:::image type="content" source="media/jupyter-book-extension/jupyter-book-sample-path.png" alt-text="Captura de pantalla que muestra un libro de Jupyter de ejemplo.":::

El libro de ejemplo muestra el aspecto de un libro de Jupyter sencillo. Si quiere obtener información sobre cómo personalizar un libro de Jupyter, consulte la sección siguiente sobre cómo crear un nuevo libro con cuadernos existentes.

### <a name="create-a-new-book"></a>Creación de un nuevo libro

Si tiene cuadernos que le gustaría empaquetar en un libro de Jupyter, puede hacerlo. El generador le preguntará si quiere incluir capítulos en el libro y, en ese caso, cuántos y sus títulos. Puede ver el aspecto del proceso de selección a continuación. Use la barra espaciadora para seleccionar los cuadernos que quiere colocar en cada capítulo.

:::image type="content" source="media/jupyter-book-extension/jupyter-book-create-book.png" alt-text="Captura de pantalla que muestra la creación de un libro de Jupyter.":::

Al completar los pasos anteriores, se crea una carpeta con el nuevo libro de Jupyter. Abra la carpeta en Visual Studio Code y estará listo para enviar la extensión de libro de Jupyter.

## <a name="understand-your-extension"></a>Descripción de la extensión

Este es el aspecto que debe tener el proyecto:

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-file-structure-generator.png" alt-text="Captura de pantalla que muestra una estructura de archivos de extensión.":::

El archivo `vsc-extension-quickstart.md` le proporciona una referencia de los archivos importantes. En el archivo `README.md` puede proporcionar documentación para la nueva extensión. Observe los archivos `package.json`, `jupyter-book.ts`, `content` y `toc.yml`. La carpeta `content` contiene todos los archivos Markdown o de cuaderno. El archivo `toc.yml` estructura el libro de Jupyter y se genera automáticamente si ha optado por crear un libro de Jupyter personalizado mediante el generador de extensiones.

Si ha creado un libro con el generador y ha optado por incluir capítulos, la estructura de carpetas tendrá un aspecto ligeramente distinto. En lugar de que los archivos de marcado y de los cuadernos de Jupyter estén en la carpeta `content`, habría subcarpetas correspondientes a los títulos elegidos para los capítulos.

Si hay archivos o carpetas que no quiere publicar, puede incluir sus nombres en el archivo `.vscodeignore`.

Ahora se examinará `jupyter-book.ts` para comprender lo que hace la extensión recién creada.

```javascript
// This function is called when you run the command `Launch Book: Test Book` from the
// command palette in Azure Data Studio. If you want any additional functionality
// to occur when you launch the book, add it to the activate function.
export function activate(context: vscode.ExtensionContext) {
    context.subscriptions.push(vscode.commands.registerCommand('launchBook.test-book', () => {
        processNotebooks();
    }));

    // Add other code here if you want to register another command.
}
```

La función `activate` es la acción principal de la extensión. Los comandos que quiera registrar deben aparecer dentro de la función `activate`, similar al comando `launchBook.test-book`. Dentro de la función `processNotebooks`, se encuentra la carpeta de extensión que contiene el libro de Jupyter y se llama a `bookTreeView.openBook` con la carpeta de la extensión como parámetro.

El archivo `package.json` también desempeña un papel importante en el registro del comando de la extensión.

```json
"activationEvents": [
        "onCommand:launchBook.test-book"
    ],
    "main": "./out/notebook.js",
    "contributes": {
        "commands": [
            {
                "command": "launchBook.test-book",
                "title": "Launch Book: Test Book"
            }
        ]
    }
```

El evento de activación, `onCommand`, desencadena la función que se ha registrado al invocar el comando. Hay otros eventos de activación posibles para tareas de personalización adicionales. Para más información, consulte [Eventos de activación](https://code.visualstudio.com/api/references/activation-events).

## <a name="package-your-extension"></a>Empaquetado de la extensión

Para compartirla con otros usuarios, tendrá que empaquetar la extensión en un único archivo. La extensión se puede publicar en el marketplace de extensiones de Azure Data Studio, o bien compartir con el equipo o la comunidad. Para realizar este paso, debe instalar otro paquete npm desde la línea de comandos.

```console
npm install -g vsce
```

Edite el archivo `README.md` a su gusto. Vaya al directorio base de la extensión y ejecute `vsce package`. Opcionalmente, puede vincular un repositorio con la extensión o continuar sin ninguno. Para agregar uno, agregue una línea similar al archivo `package.json`.

```json
"repository": {
    "type": "git",
    "url": "https://github.com/laurajjiang/testbook.git"
}
```

Una vez agregadas estas líneas, se crea el archivo `my test-book-0.0.1.vsix` y está listo para su instalación en Azure Data Studio.

## <a name="run-your-extension"></a>Ejecución de la extensión

Para ejecutar y probar la extensión, abra Azure Data Studio y abra la paleta de comandos mediante **Ctrl+Shift+P**. Busque el comando **Extensiones: Instalar desde VSIX** y vaya a la carpeta que contiene la nueva extensión. Ahora debería aparecer en el panel de extensiones de Azure Data Studio.

   :::image type="content" source="media/jupyter-book-extension/install-vsix.png" alt-text="Captura de pantalla que muestra la instalación con VSIX.":::

Vuelva a abrir la paleta de comandos y busque el comando que ha registrado, **Iniciar libro: cuaderno de prueba**. Al ejecutarlo, debe abrir el libro de Jupyter que ha empaquetado con la extensión.

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-launch-ads.png" alt-text="Captura de pantalla que muestra el comando del cuaderno.":::

¡Enhorabuena! Ha creado la primera extensión de libro de Jupyter y ya puede enviarla. Para obtener más información sobre los libros de Jupyter, vea [Libros con Jupyter](https://jupyterbook.org/intro.html).

## <a name="publish-your-extension-to-the-marketplace"></a>Publicación de la extensión en marketplace

El marketplace de extensiones de Azure Data Studio está en construcción. Para publicar, hospede la extensión VSIX en alguna parte, por ejemplo, en una página de versiones de GitHub. Envíe una solicitud de incorporación de cambios que actualice [este archivo JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) con la información de la extensión.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:
> [!div class="checklist"]
> - Crear un proyecto de extensión.
> - Instalar el generador de extensiones.
> - Crear la extensión de libro de Jupyter.
> - Empaquetar la extensión.
> - Publicar la extensión en marketplace.

Esperamos que, después de leer este artículo, tendrá ideas sobre libros de Jupyter que le gustaría compartir con la comunidad de Azure Data Studio.

Si tiene una idea, pero no está seguro de cómo empezar, abra una incidencia o envíe un tweet al equipo a [azuredatastudio](https://twitter.com/azuredatastudio).

Para más información, la [guía de extensiones de Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview) trata sobre todas las API y patrones existentes.
