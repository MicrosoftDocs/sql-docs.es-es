---
description: Actualizar y conservar datos
title: Actualización y persistencia de datos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- updating data [ADO]
- data updates [ADO]
- ADO, updating data
ms.assetid: 8dc27274-4f96-43d1-913c-4ff7d01b9a27
author: rothja
ms.author: jroth
ms.openlocfilehash: 90ea623d429a17db86040675b1b0112cda4237e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032327"
---
# <a name="updating-and-persisting-data"></a>Actualizar y conservar datos
En los capítulos anteriores se ha explicado cómo usar ADO para obtener datos de un origen de datos, Cómo desplazarse por los datos e incluso cómo editar los datos. Por supuesto, si el objetivo de la aplicación es permitir que los usuarios realicen cambios en los datos, deberá saber cómo guardar los cambios. Puede conservar los cambios del **conjunto de registros** en un archivo mediante el método **Save** o puede devolver los cambios al origen de datos para su almacenamiento mediante los métodos **Update** o **UpdateBatch** .  
  
 En los capítulos anteriores, ha cambiado los datos en varias filas del **conjunto de registros**. ADO admite dos nociones básicas relacionadas con la adición, eliminación y modificación de filas de datos.  
  
 La primera noción es que los cambios no se realizan inmediatamente en el **conjunto de registros**; en su lugar, se realizan en un *búfer de copia* interno. Si decide que no desea que se realicen los cambios, se descartan las modificaciones en el búfer de copia. Si decide conservar los cambios, los cambios en el búfer de copia se aplican al **conjunto de registros**.  
  
 La segunda noción es que los cambios se propagan al origen de datos en cuanto se declare el trabajo en una fila completa (es decir, modo *inmediato* ) o se recopilen todos los cambios en un conjunto de filas hasta que se declare el trabajo del conjunto completo (es decir, el modo *por lotes* ). La propiedad **LockType** determina cuándo se realizan los cambios en el origen de datos subyacente. **adLockOptimistic** o **adLockPessimistic** especifican el modo inmediato, mientras que **adLockBatchOptimistic** especifica el modo por lotes. La propiedad **CursorLocation** puede afectar a la configuración de **LockType** que está disponible. Por ejemplo, el valor **adLockPessimistic** no se admite si la propiedad **CursorLocation** está establecida en **adUseClient**.  
  
 En el modo inmediato, cada invocación del método **Update** propaga los cambios al origen de datos. En el modo por lotes, cada invocación de **actualización** o movimiento de la posición de la fila actual guarda los cambios en el búfer de copia, pero solo el método **UpdateBatch** propaga los cambios al origen de datos.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Actualización de datos](../../../ado/guide/data/updating-data.md)  
  
-   [Conservar los datos](../../../ado/guide/data/persisting-data.md)
