---
description: Evento InfoMessage (ADO)
title: Evento InfoMessage (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection::InfoMessage
- InfoMessage
helpviewer_keywords:
- InfoMessage event [ADO]
ms.assetid: 468c87dd-e3bc-4084-9941-94d10743d4e9
author: rothja
ms.author: jroth
ms.openlocfilehash: 68121629c35c6924304e81d9401dcbd53b51ddb9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020653"
---
# <a name="infomessage-event-ado"></a>Evento InfoMessage (ADO)
Se llama al evento **InfoMessage** siempre que se produce una advertencia durante una operación **ConnectionEvent** .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
InfoMessage pError, adStatus, pConnection  
```  
  
#### <a name="parameters"></a>Parámetros  
 *pError*  
 Un objeto de [error](./error-object.md) . Este parámetro contiene los errores que se devuelven. Si se devuelven varios errores, enumere la colección de **errores** para encontrarlos.  
  
 *adStatus*  
 Valor de estado de [EventStatusEnum](./eventstatusenum.md) . Si se produce una advertencia, *adStatus* se establece en **AdStatusOK** y el *perror* contiene la advertencia.  
  
 Antes de que se devuelva este evento, establezca este parámetro en **adStatusUnwantedEvent** para evitar notificaciones posteriores.  
  
 *pConnection*  
 Objeto de [conexión](./connection-object-ado.md) . La conexión para la que se produjo la advertencia. Por ejemplo, se pueden producir advertencias al abrir un objeto de **conexión** o ejecutar un [comando](./command-object-ado.md) en una **conexión**.  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de modelo de eventos de ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Resumen del controlador de eventos de ADO](../../guide/data/ado-event-handler-summary.md)   
 [Objeto de conexión (ADO)](./connection-object-ado.md)