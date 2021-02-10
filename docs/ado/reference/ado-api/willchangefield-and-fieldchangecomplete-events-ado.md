---
description: Eventos WillChangeField y FieldChangeComplete (ADO)
title: Eventos WillChangeField y FieldChangeComplete (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldChangeComplete
- Recordset::WillChangeField
- Recordset::FieldChangeComplete
- WillChangeField
helpviewer_keywords:
- WillChangeField event [ADO]
- fieldchangecomplete event [ADO]
ms.assetid: 3e49fb89-c45b-4d39-823e-3cc887c59b37
author: rothja
ms.author: jroth
ms.openlocfilehash: 4aef5291d7e4c539200eb0cfd34edf3174c44c0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056140"
---
# <a name="willchangefield-and-fieldchangecomplete-events-ado"></a>Eventos WillChangeField y FieldChangeComplete (ADO)
Se llama al evento **WillChangeField** antes de que una operación pendiente cambie el valor de uno o más objetos de [campo](./field-object.md) en el [conjunto de registros](./recordset-object-ado.md). Se llama al evento **FieldChangeComplete** una vez cambiado el valor de uno o más objetos **Field** .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
WillChangeField cFields, Fields, adStatus, pRecordset  
FieldChangeComplete cFields, Fields, pError, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Parámetros  
 *cFields*  
 Un **valor de tipo Long** que indica el número de objetos de **campo** en *los campos*.  
  
 *Fields*  
 En el caso de **WillChangeField**, el parámetro *Fields* es una matriz de **variantes** que contiene objetos de **campo** con los valores originales. En **FieldChangeComplete**, el parámetro *Fields* es una matriz de **variantes** que contiene objetos de **campo** con los valores modificados.  
  
 *pError*  
 Un objeto de [error](./error-object.md) . Describe el error que se produjo si el valor de *adStatus* es **adStatusErrorsOccurred**; de lo contrario, no se establece.  
  
 *adStatus*  
 Valor de estado de [EventStatusEnum](./eventstatusenum.md) .  
  
 Cuando se llama a **WillChangeField** , este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente. Se establece en **adStatusCantDeny** si este evento no puede solicitar la cancelación de la operación pendiente.  
  
 Cuando se llama a **FieldChangeComplete** , este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente, o en **adStatusErrorsOccurred** si se produjo un error en la operación.  
  
 Antes de que se devuelva **WillChangeField** , establezca este parámetro en **adStatusCancel** para solicitar la cancelación de la operación pendiente.  
  
 Antes de que **FieldChangeComplete** vuelva, establezca este parámetro en **adStatusUnwantedEvent** para evitar notificaciones posteriores.  
  
 *pRecordset*  
 Objeto de **conjunto de registros** . **Conjunto de registros** para el que se produjo este evento.  
  
## <a name="remarks"></a>Observaciones  
 Puede producirse un evento **WillChangeField** o **FieldChangeComplete** al establecer la propiedad [Value](./value-property-ado.md) y llamar al método [Update](./update-method.md) con parámetros de matriz de valores y campos.  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de modelo de eventos de ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Conexión ADO y los eventos de conjunto de registros](../../guide/data/ado-event-handler-summary.md)