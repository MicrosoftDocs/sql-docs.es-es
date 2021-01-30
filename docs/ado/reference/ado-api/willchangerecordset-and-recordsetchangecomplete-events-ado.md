---
description: Eventos WillChangeRecordset y RecordsetChangeComplete (ADO)
title: Eventos WillChangeRecordset y RecordsetChangeComplete (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset::RecordsetChangeComplete
- RecordsetChangeComplete
- Recordset::WillChangeRecordset
- WillChangeRecordset
helpviewer_keywords:
- RecordsetChangeComplete event [ADO]
- WillChangeRecordset event [ADO]
ms.assetid: d5d44659-e0d9-46d9-a297-99c43555082f
author: rothja
ms.author: jroth
ms.openlocfilehash: 6246eaa8dbdd92236a891fa56189e104b594b64c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172362"
---
# <a name="willchangerecordset-and-recordsetchangecomplete-events-ado"></a>Eventos WillChangeRecordset y RecordsetChangeComplete (ADO)
Se llama al evento **WillChangeRecordset** antes de que una operación pendiente cambie el [conjunto de registros](./recordset-object-ado.md). El evento **RecordsetChangeComplete** se llama después de que el **conjunto de registros** haya cambiado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
WillChangeRecordset adReason, adStatus, pRecordset  
RecordsetChangeComplete adReason, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Parámetros  
 *adReason*  
 Valor [EventReasonEnum](./eventreasonenum.md) que especifica el motivo de este evento. Su valor puede ser **adRsnRequery**, **adRsnResynch**, **adRsnClose**, **adRsnOpen**.  
  
 *adStatus*  
 Valor de estado de [EventStatusEnum](./eventstatusenum.md) .  
  
 Cuando se llama a **WillChangeRecordset** , este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente. Se establece en **adStatusCantDeny** si este evento no puede solicitar la cancelación de la operación pendiente.  
  
 Cuando se llama a **RecordsetChangeComplete** , este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente, **adStatusErrorsOccurred** si se produjo un error en la operación o **adStatusCancel** si se ha cancelado la operación asociada al evento **WillChangeRecordset** previamente aceptado.  
  
 Antes de que **WillChangeRecordset** vuelva, establezca este parámetro en **adStatusCancel** para solicitar la cancelación de la operación pendiente o establezca este parámetro en adStatusUnwantedEvent para evitar notificaciones posteriores.  
  
 Antes de que se devuelva **WillChangeRecordset** o **RecordsetChangeComplete** , establezca este parámetro en **adStatusUnwantedEvent** para evitar notificaciones posteriores.  
  
 *pError*  
 Un objeto de [error](./error-object.md) . Describe el error que se produjo si el valor de *adStatus* es **adStatusErrorsOccurred**; de lo contrario, no se establece.  
  
 *pRecordset*  
 Objeto de **conjunto de registros** . **Conjunto de registros** para el que se produjo este evento.  
  
## <a name="remarks"></a>Observaciones  
 Puede producirse un evento **WillChangeRecordset** o **RecordsetChangeComplete** debido a la [reconsulta](./requery-method.md) o a los métodos [abiertos](./open-method-ado-recordset.md) del **conjunto de registros** .  
  
 Si el proveedor no admite marcadores, se produce una notificación de evento **RecordsetChange** cada vez que se recuperan nuevas filas del proveedor. La frecuencia de este evento depende de la propiedad **RecordsetCacheSize** .  
  
 Debe establecer el parámetro **adStatus** en **adStatusUnwantedEvent** para cada valor de **adReason** posible para detener por completo la notificación de eventos para cualquier evento que incluya un parámetro **adReason** .  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de modelo de eventos de ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Conexión ADO y los eventos de conjunto de registros](../../guide/data/ado-event-handler-summary.md)