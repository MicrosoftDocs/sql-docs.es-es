---
description: Eventos WillChangeRecord y RecordChangeComplete (ADO)
title: Eventos WillChangeRecord y RecordChangeComplete (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- RecordChangeComplete
- Recordset::WillChangeRecord
- WillChangeRecord
- Recordset::RecordChangeComplete
helpviewer_keywords:
- WillChangeRecord event [ADO]
- recordchangecomplete event [ADO]
ms.assetid: cbc369fd-63af-4a7d-96ae-efa91b78ca69
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a713c39746db1d6a5317a4884ac0a2c5cbb23fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166275"
---
# <a name="willchangerecord-and-recordchangecomplete-events-ado"></a>Eventos WillChangeRecord y RecordChangeComplete (ADO)
Se llama al evento **WillChangeRecord** antes de que cambien uno o más registros (filas) del [conjunto de registros](./recordset-object-ado.md) . Se llama al evento **RecordChangeComplete** después de que uno o varios registros cambien.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
WillChangeRecord adReason, cRecords, adStatus, pRecordset  
RecordChangeCompleteadReason, cRecords, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Parámetros  
 *adReason*  
 Valor [EventReasonEnum](./eventreasonenum.md) que especifica el motivo de este evento. Su valor puede ser **adRsnAddNew**, **adRsnDelete**, **adRsnUpdate**, **adRsnUndoUpdate**, **adRsnUndoAddNew**, **adRsnUndoDelete** o **adRsnFirstChange**.  
  
 *cRecords*  
 Un valor **Long** que indica el número de registros que cambian (se ven afectados).  
  
 *pError*  
 Un objeto de [error](./error-object.md) . Describe el error que se produjo si el valor de *adStatus* es **adStatusErrorsOccurred**; de lo contrario, no se establece.  
  
 *adStatus*  
 Valor de estado de [EventStatusEnum](./eventstatusenum.md) .  
  
 Cuando se llama a **WillChangeRecord** , este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente. Se establece en **adStatusCantDeny** si este evento no puede solicitar la cancelación de la operación pendiente.  
  
 Cuando se llama a **RecordChangeComplete** , este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente, o en **adStatusErrorsOccurred** si se produjo un error en la operación.  
  
 Antes de que se devuelva **WillChangeRecord** , establezca este parámetro en **adStatusCancel** para solicitar la cancelación de la operación que provocó este evento o establecer este parámetro en **adStatusUnwantedEvent** para evitar notificaciones posteriores.  
  
 Antes de que se devuelva **RecordChangeComplete** , establezca este parámetro en **adStatusUnwantedEvent** para evitar notificaciones posteriores.  
  
 *pRecordset*  
 Objeto de **conjunto de registros** . **Conjunto de registros** para el que se produjo este evento.  
  
## <a name="remarks"></a>Observaciones  
 Es posible que se produzca un evento **WillChangeRecord** o **RecordChangeComplete** para el primer campo modificado de una fila debido a las siguientes operaciones de **conjunto de registros** : [Update](./update-method.md), [Delete](./delete-method-ado-recordset.md), [CancelUpdate](./cancelupdate-method-ado.md), [AddNew](./addnew-method-ado.md), [UpdateBatch](./updatebatch-method.md)y [CancelBatch](./cancelbatch-method-ado.md). El valor de [CursorType](./cursortype-property-ado.md) del **conjunto de registros** determina qué operaciones hacen que se produzcan los eventos.  
  
 Durante el evento **WillChangeRecord** , la propiedad del [filtro](./filter-property.md) del **conjunto de registros** se establece en **adFilterAffectedRecords**. No se puede cambiar esta propiedad mientras se procesa el evento.  
  
 Debe establecer el parámetro **adStatus** en **adStatusUnwantedEvent** para cada valor de **adReason** posible para detener por completo la notificación de eventos para cualquier evento que incluya un parámetro **adReason** .  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de modelo de eventos de ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Conexión ADO y los eventos de conjunto de registros](../../guide/data/ado-event-handler-summary.md)