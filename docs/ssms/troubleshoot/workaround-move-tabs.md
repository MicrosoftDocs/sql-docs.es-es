---
description: Solución alternativa para mover pestañas
title: Habilitación del movimiento de pestañas sin que SSMS se bloquee
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
ms.openlocfilehash: ae7c79792d962ce578059e672de7165f477f1c4a
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "100654640"
---
# <a name="workaround-to-move-tabs"></a>Solución alternativa para mover pestañas

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

Es posible que se requiera una solución alternativa para poder mover las pestañas del editor de consultas, ya sea dentro de la ventana o para acoplar una pestaña quitada previamente.  Si ha aplicado todas las actualizaciones de Windows disponibles y no puede manipular las pestañas del editor de consultas sin que se produzca un bloqueo, siga la [solución alternativa](#workaround) que se indica a continuación.

## <a name="applicable-environments"></a>Entornos aplicables
Las actualizaciones de .NET Framework de Windows introdujeron un problema conocido que provoca un bloqueo de la aplicación SQL Server Management Studio (SSMS) al acoplar pestañas o dividir la ventana.  Para consultar la información más reciente, vea los [comentarios de los usuarios de SQL Server](https://feedback.azure.com/forums/908035/suggestions/42651556).

## <a name="workaround"></a>Solución alternativa

Si el bloqueo persiste después de aplicar todas las actualizaciones disponibles de Windows, siga estos pasos para mitigar el problema:

1. Cierre todas las instancias de SQL Server Management Studio (SSMS).

2. Busque el archivo de aplicación de SSMS (exe).  Normalmente se encuentra en `C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE`.

3. Abra el archivo `Ssms.exe.config` en el Bloc de notas como administrador.

4. Busque el nodo `AppContextSwitchOverrides` y anexe estas dos propiedades al valor.
    ```
    ;Switch.System.Windows.Interop.MouseInput.OptOutOfMoveToChromedWindowFix=true; Switch.System.Windows.Interop.MouseInput.DoNotOptOutOfMoveToChromedWindowFix=true
    ```

    ![Edición del archivo ssms.exe.config](../media/troubleshoot/execonfig-edit.png)

5. Guarde el archivo de configuración y vuelva a abrir SSMS.
