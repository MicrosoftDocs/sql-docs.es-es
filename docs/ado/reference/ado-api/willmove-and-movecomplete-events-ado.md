---
description: Eventos WillMove y MoveComplete (ADO)
title: Eventos WillMove y MoveComplete (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset::MoveComplete
- WillMove
- MoveComplete
- Recordset::WillMove
helpviewer_keywords:
- MoveComplete event [ADO]
- WillMove event [ADO]
ms.assetid: 1a3d1042-4f30-4526-a0c7-853c242496db
author: rothja
ms.author: jroth
ms.openlocfilehash: a2476447e5da8baa89a2df326eba3d41225c698e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056060"
---
# <a name="willmove-and-movecomplete-events-ado"></a>Eventos WillMove y MoveComplete (ADO)
Se llama al evento **WillMove** antes de que una operación pendiente cambie la posición actual en el [conjunto de registros](./recordset-object-ado.md). Se llama al evento **MoveComplete** después de que cambie la posición actual en el **conjunto de registros** .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
WillMove adReason, adStatus, pRecordset  
MoveComplete adReason, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Parámetros  
 *adReason*  
 Valor [EventReasonEnum](./eventreasonenum.md) que especifica el motivo de este evento. Su valor puede ser **adRsnMoveFirst**, **adRsnMoveLast**, **adRsnMoveNext**, **adRsnMovePrevious**, **adRsnMove** o **adRsnRequery**.  
  
 *pError*  
 Un objeto de [error](./error-object.md) . Describe el error que se produjo si el valor de *adStatus* es **adStatusErrorsOccurred**; de lo contrario, no se establece el parámetro.  
  
 *adStatus*  
 Valor de estado de [EventStatusEnum](./eventstatusenum.md) .  
  
 Cuando se llama a **WillMove** , este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente. Se establece en **adStatusCantDeny** si este evento no puede solicitar la cancelación de la operación pendiente.  
  
 Cuando se llama a **MoveComplete** , este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente, o en **adStatusErrorsOccurred** si se produjo un error en la operación.  
  
 Antes de que **WillMove** vuelva, establezca este parámetro en **adStatusCancel** para solicitar la cancelación de la operación pendiente o establezca este parámetro en **adStatusUnwantedEvent** para evitar las notificaciones posteriores.  
  
 Antes de que se devuelva **MoveComplete** , establezca este parámetro en **adStatusUnwantedEvent** para evitar las notificaciones posteriores.  
  
 *pRecordset*  
 Objeto de [conjunto de registros](./recordset-object-ado.md) . **Conjunto de registros** para el que se produjo este evento.  
  
## <a name="remarks"></a>Observaciones  
 Puede producirse un evento **WillMove** o **MoveComplete** debido a las siguientes operaciones de **conjunto de registros** : [Open](./open-method-ado-recordset.md), [Move](./move-method-ado.md), [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [MoveLast](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [MoveNext](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), [AddNew](./addnew-method-ado.md)y [Requery](./requery-method.md). Estos eventos pueden producirse debido a las siguientes propiedades: [Filter](./filter-property.md), [index](./index-property.md), [Bookmark](./bookmark-property-ado.md), [AbsolutePage](./absolutepage-property-ado.md)y [AbsolutePosition](./absoluteposition-property-ado.md). Estos eventos también se producen si un **conjunto de registros** secundario tiene eventos de **conjunto de registros** conectados y se mueve el **conjunto de registros** primario.  
  
 Debe establecer el parámetro *adStatus* en **adStatusUnwantedEvent** para cada valor de *adReason* posible con el fin de detener por completo la notificación de eventos para cualquier evento que incluya un parámetro *adReason* .  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de modelo de eventos de ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Resumen del controlador de eventos de ADO](../../guide/data/ado-event-handler-summary.md)   
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)