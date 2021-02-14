---
title: Administración de paquetes con la extensión Machine Learning
description: Obtenga información sobre la administración de paquetes de Python o R de la base de datos con la extensión Machine Learning para Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 05/19/2020
ms.openlocfilehash: c874317acf39e954a0e640c1954e3a0dac6e9f43
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051966"
---
# <a name="manage-packages-in-database-with-machine-learning-extension-for-azure-data-studio-preview"></a>Administración de paquetes de la base de datos con la extensión de Machine Learning para Azure Data Studio (versión preliminar)

Obtenga información sobre la administración de paquetes de Python o R de la base de datos con la [extensión Machine Learning para Azure Data Studio](machine-learning-extension.md).

> [!IMPORTANT]
> La administración de paquetes de la base de datos con la extensión Machine Learning actualmente solo admite [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) en SQL Server 2019.

## <a name="prerequisites"></a>Requisitos previos

- Instale y configure la [extensión Machine Learning para Azure Data Studio](machine-learning-extension.md). Debe especificar las [rutas de instalación de Python o R en la configuración de la extensión](machine-learning-extension.md#settings) para que la administración de paquetes funcione.

- El paquete **sqlmlutils**. Si el paquete todavía no está instalado, la extensión Machine Learning le pedirá que lo instale.

- Por SQL Server en Linux, no se admite la autenticación de Windows. Por lo tanto, debe usar la autenticación de SQL para conectarse a su SQL Server en Linux.

## <a name="manage-python-packages"></a>Administración de paquetes de Python

Puede instalar y desinstalar paquetes de Python con la extensión Machine Learning.

### <a name="install-new-python-package"></a>Instalación de paquetes de Python adicionales

Siga los pasos que se indican a continuación para instalar paquetes de Python en la base de datos.

1. Seleccione **Administrar paquetes de la base de datos**.

1. Si se le pide que instale **sqlmlutils**, seleccione **Sí**.

1. En la pestaña **Instalado**, seleccione **Python** en **Tipo de paquete** y seleccione la base de datos en **Ubicación**.

1. Seleccione la pestaña **Agregar nuevo**.

1. Escriba el paquete de Python que desea instalar en **Buscar paquetes de Python** y seleccione **Buscar**.

1. Compruebe que el paquete aparece en **Nombre del paquete** y que la versión deseada aparece en **Versión del paquete**.

1. Seleccione **Instalar**.

1. Seleccione **Cerrar** y compruebe que el paquete se ha instalado correctamente en **Tareas**.

### <a name="uninstall-a-python-package"></a>Desinstalación de un paquete de Python

Siga los pasos que se indican a continuación para desinstalar paquetes de Python de la base de datos.

> [!NOTE]
> Solo se pueden desinstalar los paquetes que se han instalado en la base de datos. No es posible desinstalar un paquete que se ha instalado previamente con SQL Server Machine Learning Services.

1. Seleccione **Administrar paquetes de la base de datos**.

1. Si se le pide que instale **sqlmlutils**, seleccione **Sí**.

1. En la pestaña **Instalado**, seleccione **Python** en **Tipo de paquete** y seleccione la base de datos en **Ubicación**.

1. Seleccione los paquetes que desea desinstalar.

1. Seleccione **Desinstalar paquetes seleccionados**.

1. Seleccione **Cerrar** y compruebe que el paquete se ha instalado correctamente en **Tareas**.

## <a name="manage-r-packages"></a>Administración de paquetes de R

Puede instalar y desinstalar paquetes de R con la extensión Machine Learning.

### <a name="install-new-r-package"></a>Instalación de paquetes de R adicionales

Siga los pasos que se indican a continuación para instalar paquetes de Python en la base de datos.

1. Seleccione **Administrar paquetes de la base de datos**.

1. Si se pide que instale **sqlmlutils**, seleccione **Sí**.

1. En la pestaña **Instalado**, seleccione **R** en **Tipo de paquete** y seleccione la base de datos en **Ubicación**.

1. Seleccione la pestaña **Agregar nuevo**.

1. Escriba el paquete de R que desea instalar en **Buscar paquetes de R** y seleccione **Buscar**.

1. Compruebe que el paquete aparece en **Nombre del paquete** y que la versión deseada aparece en **Versión del paquete**.

1. Seleccione **Instalar**.

1. Seleccione **Cerrar** y compruebe que el paquete se ha instalado correctamente en **Tareas**.

### <a name="uninstall-an-r-package"></a>Desinstalación de un paquete de R

Siga los pasos que se indican a continuación para desinstalar paquetes de R de la base de datos.

> [!NOTE]
> Solo se pueden desinstalar los paquetes que se han instalado en la base de datos. No es posible desinstalar un paquete que se ha instalado previamente con SQL Server Machine Learning Services.

1. Seleccione **Administrar paquetes de la base de datos**.

1. Si se pide que instale **sqlmlutils**, seleccione **Sí**.

1. En la pestaña **Instalado**, seleccione **R** en **Tipo de paquete** y seleccione la base de datos en **Ubicación**.

1. Seleccione los paquetes que desea desinstalar.

1. Seleccione **Desinstalar paquetes seleccionados**.

1. Seleccione **Cerrar** y compruebe que el paquete se ha instalado correctamente en **Tareas**.

## <a name="next-steps"></a>Pasos siguientes

- [Extensión Machine Learning en Azure Data Studio](machine-learning-extension.md)
- [Realización de predicciones](machine-learning-extension-predictions.md)
- [Importación o visualización de modelos](machine-learning-extension-import-view-models.md)
- [Cuadernos en Azure Data Studio](../notebooks/notebooks-guidance.md)
- [Documentación del aprendizaje automático en SQL](../../machine-learning/index.yml)