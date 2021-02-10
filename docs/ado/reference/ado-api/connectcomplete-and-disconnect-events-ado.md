---
description: Eventos ConnectComplete y Disconnect (ADO)
title: Eventos ConnectComplete y Disconnect (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Disconnect
- Connection::ConnectComplete
- ConnectComplete
- Connection::Disconnect
helpviewer_keywords:
- Disconnect event [ADO]
- ConnectComplete event [ADO]
ms.assetid: 568f5252-d069-4d99-a01b-2ada87ad1304
author: rothja
ms.author: jroth
ms.openlocfilehash: 73966677eb224c2549954f80525d88ee5622dd0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026573"
---
# <a name="connectcomplete-and-disconnect-events-ado"></a>Eventos ConnectComplete y Disconnect (ADO)
El evento **ConnectComplete** se llama después de que se inicie una conexión. Se llama al evento **Disconnect** una vez finalizada la conexión.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
ConnectComplete pError, adStatus, pConnection  
Disconnect adStatus, pConnection  
```  
  
#### <a name="parameters"></a>Parámetros  
 *pError*  
 Un objeto de [error](./error-object.md) . Describe el error que se produjo si el valor de *adStatus* es **adStatusErrorsOccurred**; de lo contrario, no se establece.  
  
 *adStatus*  
 Valor de [EventStatusEnum](./eventstatusenum.md) que siempre devuelve **adStatusOK**.  
  
 Cuando se llama a **ConnectComplete** , este parámetro se establece en **adStatusCancel** si un evento **WillConnect** ha solicitado la cancelación de la conexión pendiente.  
  
 Antes de que se devuelva un evento, establezca este parámetro en **adStatusUnwantedEvent** para evitar las notificaciones posteriores. Sin embargo, al cerrar y volver a abrir la [conexión](./connection-object-ado.md) , se vuelven a producir estos eventos.  
  
 *pConnection*  
 Objeto de **conexión** para el que se aplica este evento.  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de modelo de eventos de ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Conexión ADO y los eventos de conjunto de registros](../../guide/data/ado-event-handler-summary.md)