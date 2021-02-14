---
title: Control de código fuente
description: Azure Data Studio admite Git para la administración del control de código fuente (SCM). Aprenda a abrir un repositorio de Git existente y cómo inicializar uno nuevo.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18
ms.date: 09/24/2019
ms.openlocfilehash: 014376dd56045b9e5ef440054ae2c1d1df836cad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040025"
---
# <a name="source-control-in-azure-data-studio"></a>Control de código fuente en Azure Data Studio

Azure Data Studio admite Git para el control de código fuente y de versiones.

## <a name="git-support-in-azure-data-studio"></a>Compatibilidad de Git en Azure Data Studio

Azure Data Studio se entrega con un administrador de control de código fuente (SCM) de Git, pero sigue siendo necesario [instalar Git (versión 2.0.0 o posterior)](https://git-scm.com/download) para que estas características estén disponibles.

## <a name="open-an-existing-git-repository"></a>Apertura de un repositorio de Git existente

1. En el menú **Archivo**, seleccione **Abrir carpeta...**
2. Vaya a la carpeta que contiene los archivos a los que Git realiza un seguimiento y elija **Seleccionar carpeta**. Aquí puede seleccionar las subcarpetas de su repositorio local.

## <a name="initialize-a-new-git-repository"></a>Inicialización de un nuevo repositorio de Git

1. Seleccione **Control de código fuente** y después seleccione el icono de Git.

   ![Inicio de Git del control de código fuente](media/source-control/source-control.png)

1. Escriba la ruta de acceso a la carpeta que quiera inicializar como un repositorio de Git y presione **Entrar**.

   ![Inicialización del repositorio de Git](media/source-control/initialize-git-repository.png)

## <a name="working-with-git-repositories"></a>Trabajo con repositorios de Git

Azure Data Studio hereda su implementación de Git de VS Code, pero actualmente no admite proveedores de SCM adicionales. Para obtener información detallada sobre cómo trabajar con Git después de abrir o inicializar un repositorio, consulte [Compatibilidad de Git en VS Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support).

## <a name="additional-resources"></a>Recursos adicionales

- [Documentación de Git](https://git-scm.com/documentation)
