---
description: 'Solución alternativa para habilitar la copia desde la ventana para buscar y reemplazar '
title: Habilitar la copia desde la ventana de buscar y reemplazar
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: seo-lt-2019
ms.date: 11/03/2020
ms.openlocfilehash: f7a11c952fa20b720ad37abc204c7dbb31f0e96f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058610"
---
# <a name="workaround-to-enable-copying-from-find-and-replace-window"></a>Solución alternativa para habilitar la copia desde la ventana para buscar y reemplazar

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

Se requiere una solución alternativa para habilitar la copia desde la ventana para buscar y reemplazar.  Si ve que no puede copiar desde la ventana para buscar y reemplazar en SQL Server Management Studio, pruebe con la [solución alternativa](#workaround) que se indica a continuación.

## <a name="error-message"></a>Mensaje de error

Al intentar copiar texto de la ventana para buscar y reemplazar en SQL Server Management Studio, se obtiene un mensaje de error.

> Los documentos no guardados no se pueden cortar ni copiar en el Portapapeles desde el proyecto de archivos varios. Debe guardar los documentos no guardados antes de cortarlos o copiarlos.

![Cuadro de diálogo de error para: Los documentos no guardados no se pueden cortar ni copiar en el Portapapeles desde el proyecto de archivos varios. Debe guardar los documentos no guardados antes de cortarlos o copiarlos.](../media/troubleshoot/unable-copy-find-replace-window.png)

## <a name="workaround"></a>Solución alternativa

Para habilitar la copia de texto de la ventana para buscar y reemplazar, siga estos pasos:

1. En el menú **Herramientas**, abra **Opciones**.

2. En **Entorno**>**Documentos**, desactive la opción "Mostrar archivos varios en el Explorador de soluciones".

3. Cierre y vuelva a abrir SQL Server Management Studio.

![Ventana Opciones con la opción "Mostrar archivos varios en el Explorador de soluciones" desactivada](../media/troubleshoot/fix-copy-find-replace-window.png)

