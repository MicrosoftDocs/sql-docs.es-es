---
title: Creación de una extensión de distribución de teclado
description: En este tutorial se muestra cómo crear una extensión de distribución de teclado para agregar funcionalidad personalizada a Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 1cf1b48b3293f3315c11328b5b5c4beb7ab751ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048565"
---
# <a name="create-an-azure-data-studio-keymap-extension"></a>Creación de una extensión de distribución de teclado de Azure Data Studio

En este tutorial se muestra cómo crear una extensión de Azure Data Studio. La extensión crea enlaces de teclado de SSMS conocidos en Azure Data Studio.

En este artículo, aprenderá a:
> [!div class="checklist"]
> - Creación de un proyecto de extensión
> - Instalación del generador de extensiones
> - Creación de la extensión
> - Agregar enlaces de clave personalizados a la extensión
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

Para simplificar el proceso de creación de extensiones, hemos compilado [un generador de extensiones](https://code.visualstudio.com/docs/extensions/yocode) con Yeoman. Para instalarlo, ejecute el código en el siguiente símbolo del sistema:

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-keymap-extension"></a>Creación de la extensión de distribución de teclado

Para crear una extensión:

1. Inicie el generador de extensiones con el siguiente comando:

   `yo azuredatastudio`

2. Elija **New Keymap** de la lista de tipos de extensiones:

   :::image type="content" source="media/keymap-extension/extension-generator.png" alt-text="Generador de extensiones":::

3. Siga los pasos para rellenar el nombre de la extensión (en este tutorial, use **ssmskeymap2**) y agregue una descripción.

Al completar los pasos anteriores, se crea una carpeta. Abra la carpeta en Visual Studio Code y estará listo para crear su propia extensión de enlace de teclado.

### <a name="add-a-keyboard-shortcut"></a>Adición de un método abreviado de teclado

**Paso 1: Búsqueda de los accesos directos que se van a reemplazar**

Ahora que la extensión está lista para usar, agregue algunos métodos abreviados de teclado (o enlaces de teclado) de SSMS en Azure Data Studio. Utilicé la [hoja de referencia de Andy Mallon](https://am2.co/2018/02/updated-cheat-sheet/) y la lista de métodos abreviados de teclado de RedGate como inspiración.

Lo más importante que noté que faltaba fue:

- Ejecutar una consulta con el plan de ejecución real habilitado. Se usa **Ctrl+M** en SSMS, que no tiene un enlace en Azure Data Studio.
- Tener **CTRL+MAYÚS+E** como segunda forma de ejecutar una consulta. Los comentarios de los usuarios indicaron que faltaba.
- Tener **ALT+F1** para ejecutar `sp_help`. Lo agregamos a Azure Data Studio pero como ese enlace ya estaba en uso, lo asignamos a **ALT+F2** en su lugar.
- Alternar pantalla completa (**MAYÚS+ALT+ENTRAR**).
- **F8** para mostrar **Explorador de objetos** / **Vista de servidores**.

Resulta fácil encontrar y reemplazar estos enlaces de teclado. Ejecute *Abrir métodos abreviados de teclado* para mostrar la pestaña **Métodos abreviados de teclado** en Azure Data Studio, busque *consulta* y elija **Cambiar enlace de teclado**. Una vez que haya terminado de cambiar el enlace de teclado, puede ver la asignación actualizada en el archivo keybindings.json (ejecute *Abrir métodos abreviados de teclado* para verla).

:::image type="content" source="media/keymap-extension/keyboard-shortcuts.png" alt-text="Métodos abreviados de teclado":::

:::image type="content" source="media/keymap-extension/key-bindings-json.png" alt-text="Extensión Keybindings.json":::

**Paso 2: Adición de accesos directos a la extensión**

Para agregar accesos directos a la extensión, abra el archivo *package.json* (en la extensión) y reemplace la sección `contributes` por el siguiente código:

```json
"contributes": {
  "keybindings": [
    {
      "key": "shift+cmd+e",
      "command": "runQueryKeyboardAction"
    },
    {
      "key": "ctrl+cmd+e",
      "command": "workbench.view.explorer"
    },
    {
      "key": "alt+f1",
      "command": "workbench.action.query.shortcut1"
    },
    {
      "key": "shift+alt+enter",
      "command": "workbench.action.toggleFullScreen"
    },
    {
      "key": "f8",
      "command": "workbench.view.connections"
    },
    {
      "key": "ctrl+m",
      "command": "runCurrentQueryWithActualPlanKeyboardAction"
    }
  ]
}
```

## <a name="test-your-extension"></a>Prueba de la extensión

Para asegurarse de que `azuredatastudio` se encuentra en la ruta de acceso, ejecute la opción de instalación del comando azuredatastudio de PATH en Azure Data Studio.

Asegúrese de que la extensión Debug de Azure Data Studio está instalada en Visual Studio Code.

Seleccione **F5** para iniciar Azure Data Studio en modo de depuración con la extensión en ejecución:

:::image type="content" source="media/keymap-extension/install-extension.png" alt-text="Instalación de la extensión":::

:::image type="content" source="media/keymap-extension/test-extension.png" alt-text="prueba de la extensión":::

Las asignaciones de teclas son una de las extensiones más rápidas de crear, por lo que la nueva extensión ahora debería funcionar correctamente y estar lista para compartir.

## <a name="package-your-extension"></a>Empaquetado de la extensión

Para compartirla con otros usuarios, tendrá que empaquetar la extensión en un único archivo. Este se puede publicar en el marketplace de extensiones de Azure Data Studio, o bien compartir entre el equipo o la comunidad. Para ello, debe instalar otro paquete npm desde la línea de comandos:

```console
npm install -g vsce
```

Navegue hasta el directorio base de la extensión y ejecute `vsce package`. Tuve que agregar un par de líneas adicionales para evitar reclamaciones de la herramienta *vsce*:

```json
"repository": {
    "type": "git",
    "url": "https://github.com/kevcunnane/ssmskeymap.git"
},
"bugs": {
    "url": "https://github.com/kevcunnane/ssmskeymap/issues"
},
```

Una vez hecho esto, el archivo ssmskeymap-0.1.0.vsix se ha creado y está listo para instalarlo y compartirlo con el mundo.

:::image type="content" source="media/keymap-extension/extensions.png" alt-text="Instalación":::

## <a name="publish-your-extension-to-the-marketplace"></a>Publicación de la extensión en marketplace

El marketplace de extensiones de Azure Data Studio está en construcción, pero el proceso actual consiste en hospedar la extensión VSIX en algún lugar (por ejemplo, una página de versiones de GitHub) y luego enviar una solicitud de incorporación de cambios a [este archivo JSON](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json) con la información de la extensión.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:
> [!div class="checklist"]
> - Creación de un proyecto de extensión
> - Instalación del generador de extensiones
> - Creación de la extensión
> - Agregar enlaces de clave personalizados a la extensión
> - Prueba de la extensión
> - Empaquetado de la extensión
> - Publicación de la extensión en Marketplace

Confiamos en que después de leer esto se sienta inspirado para compilar su propia extensión para Azure Data Studio. Tenemos compatibilidad con información del panel (gráficos atractivos que se ejecutan en la instancia de SQL Server), varias API específicas de SQL y un gran conjunto existente de puntos de extensión heredados de Visual Studio Code.

Si tiene una idea, pero no está seguro de cómo empezar, abra una incidencia o envíe un tweet al equipo [azuredatastudio](https://twitter.com/azuredatastudio).

Siempre puede hacer referencia a la [guía de extensiones de Visual Studio Code](https://code.visualstudio.com/docs/extensions/overview) porque aborda todas las API y los patrones existentes.

Para obtener información sobre cómo trabajar con T-SQL en Azure Data Studio, siga el tutorial del editor de T-SQL sobre el:

> [!div class="nextstepaction"]
> [usar el editor de Transact-SQL para crear objetos de la base de datos](../tutorial-sql-editor.md)
