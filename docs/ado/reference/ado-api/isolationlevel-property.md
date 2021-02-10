---
description: Propiedad IsolationLevel
title: Propiedad IsolationLevel | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::IsolationLevel
helpviewer_keywords:
- IsolationLevel property
ms.assetid: ea84e4b2-fbf2-4eef-b9ce-796b22e21800
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e6b818ef937328d75d5c5425815dbd931ebdd2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044315"
---
# <a name="isolationlevel-property"></a>Propiedad IsolationLevel
Indica el nivel de aislamiento para un objeto de [conexión](./connection-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de [IsolationLevelEnum](./isolationlevelenum.md) . El valor predeterminado es **adXactReadCommitted**.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **IsolationLevel** para establecer el nivel de aislamiento de un objeto de **conexión** . La configuración no surte efecto hasta la próxima vez que se llame al método [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) . Si el nivel de aislamiento solicitado no está disponible, el proveedor puede devolver el siguiente nivel mayor de aislamiento sin actualizar la propiedad **IsolationLevel** .  
  
 La propiedad **IsolationLevel** es de lectura y escritura.  
  
> [!NOTE]
>  **Uso del servicio de datos remotos** Cuando se usa en un objeto de **conexión** del lado cliente, la propiedad **IsolationLevel** solo se puede establecer en **adXactUnspecified**. Dado que los usuarios trabajan con objetos de **conjunto de registros** desconectados en una caché del lado cliente, puede haber problemas con los usuarios. Por ejemplo, cuando dos usuarios diferentes intentan actualizar el mismo registro, el servicio de datos remotos simplemente permite al usuario que actualiza el registro primero a "Win". Se producirá un error en la solicitud de actualización del segundo usuario.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conexión (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades IsolationLevel y MODE (VB)](./isolationlevel-and-mode-properties-example-vb.md)   
 [Ejemplo de las propiedades IsolationLevel y MODE (VC + +)](./isolationlevel-and-mode-properties-example-vc.md)