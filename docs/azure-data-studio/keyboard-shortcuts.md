---
title: Creación y personalización de métodos abreviados de teclado
description: Obtenga información sobre cómo ver, editar y crear métodos abreviados de teclado en Azure Data Studio mediante una funcionalidad basada en la incluida en Visual Studio Code.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 26d16927004a9a34e0b0d50ca5ebe28abac599a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040165"
---
# <a name="keyboard-shortcuts-in-azure-data-studio"></a>Métodos abreviados de teclado de Azure Data Studio

En este artículo se proporcionan los pasos para ver, editar y crear rápidamente métodos abreviados de teclado en Azure Data Studio.

Como Azure Data Studio hereda su funcionalidad de enlace de teclado de Visual Studio Code, la información detallada sobre las personalizaciones avanzadas, el uso de diferentes distribuciones de teclado, etc., se encuentra en el artículo sobre [Enlaces de teclado de Visual Studio Code](https://code.visualstudio.com/docs/getstarted/keybindings). Es posible que algunas características de enlace de teclado no estén disponibles (por ejemplo, en Azure Data Studio no se admiten las extensiones de distribución de teclado).

## <a name="open-the-keyboard-shortcuts-editor"></a>Abra el editor de métodos abreviados de teclado

Para ver todos los métodos abreviados de teclado actualmente definidos:

Abra el editor de **métodos abreviados de teclado** desde el menú **Archivo**: **Archivo** > **Preferencias** > **Métodos abreviados de teclado** (**Azure Data Studio** > **Preferencias** > **Métodos abreviados de teclado** en Mac).

Además de mostrar los enlaces de teclado actuales, el editor de **métodos abreviados de teclado** enumera los comandos disponibles que no tienen definido un método abreviado de teclado. El editor de **métodos abreviados de teclado** permite cambiar, quitar, restablecer y definir fácilmente nuevos enlaces de teclado.  

## <a name="edit-existing-keyboard-shortcuts"></a>Edición de los métodos abreviados de teclado existentes

Para cambiar el enlace de teclado de un método abreviado de teclado existente:

1. Busque el método abreviado de teclado que quiera cambiar con el cuadro de búsqueda o desplazándose por la lista.
   > [!TIP]
   > Busque por clave, comando, por origen, etc. para devolver todos los métodos abreviados de teclado pertinentes.

2. Haga clic con el botón derecho en la entrada deseada y seleccione **Change Key binding** (Cambiar enlace de teclado).

   ![editar método abreviado de teclado](media/keyboard-shortcuts/change-keybinding.png)

3. Presione la combinación de teclas deseada y, después, presione **Entrar** para guardarla. 

   ![guardar método abreviado de teclado](media/keyboard-shortcuts/save-keybinding.png)

## <a name="create-new-keyboard-shortcuts"></a>Creación de nuevos métodos abreviados de teclado

Para crear nuevos métodos abreviados de teclado:

1. Haga clic con el botón derecho en un comando que no tenga ningún enlace de teclado y seleccione **Add Key binding** (Agregar enlace de teclado).

   ![crear método abreviado de teclado](media/keyboard-shortcuts/add-keybinding.png)

2. Presione la combinación de teclas deseada y, después, presione **Entrar** para guardarla.