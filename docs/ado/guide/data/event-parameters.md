---
description: Parámetros de eventos
title: Parámetros de evento | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Error parameter [ADO]
- Object parameter [ADO]
- Status parameter [ADO]
- events [ADO], parameters
- Reason parameter [ADO]
- event parameters [ADO]
ms.assetid: bd5c5afa-d301-4899-acda-40f98a6afa4d
author: rothja
ms.author: jroth
ms.openlocfilehash: 7f60d32ccd968c39da4fcea3978a46389e49ae49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033215"
---
# <a name="event-parameters"></a>Parámetros de eventos
Cada controlador de eventos tiene un parámetro de estado que controla el controlador de eventos. En el caso de los eventos completos, este parámetro también se utiliza para indicar si la operación que generó el evento se ha realizado correctamente o no. Los eventos más completos también tienen un parámetro de error para proporcionar información sobre cualquier error que se pudiera haber producido y uno o varios parámetros de objeto que hacen referencia a los objetos ADO utilizados para realizar la operación. Por ejemplo, el evento [ExecuteComplete](../../reference/ado-api/executecomplete-event-ado.md) incluye parámetros de objeto para los objetos de **comando**, **conjunto de registros** y **conexión** asociados al evento. En el siguiente ejemplo de Microsoft® Visual Basic®, puede ver los objetos pCommand, pRecordset y pConnection que representan los objetos de **comando**, **conjunto de registros** y **conexión** utilizados por el método **Execute** .  
  
```  
Private Sub connEvent_ExecuteComplete(ByVal RecordsAffected As Long, _  
     ByVal pError As ADODB.Error, _  
     adStatus As ADODB.EventStatusEnum, _  
     ByVal pCommand As ADODB.Command, _  
     ByVal pRecordset As ADODB.Recordset, _  
     ByVal pConnection As ADODB.Connection)  
```  
  
 Excepto en el caso del objeto de **error** , los mismos parámetros se pasan a los eventos Events. Esto le permite examinar cada uno de los objetos que se usarán en la operación pendiente y determinar si se debe permitir que finalice la operación.  
  
 Algunos controladores de eventos tienen un parámetro *Reason* , que proporciona información adicional sobre el motivo del evento. Por ejemplo, los eventos **WillMove** y **MoveComplete** pueden producirse debido a que se llama a uno de los métodos de navegación (**MoveNext**, **MovePrevious**, etc.) o como resultado de una nueva consulta.  
  
## <a name="status-parameter"></a>Parámetro status  
 Cuando se llama a la rutina del controlador de eventos, el parámetro *status* se establece en uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**adStatusOK**|Se pasa a los eventos y se completan. Este valor significa que la operación que causó el evento se completó correctamente.|  
|**adStatusErrorsOccurred**|Se pasa solo a eventos de finalización. Este valor significa que la operación que causó el evento no se ha realizado correctamente o que un evento se cancelará la operación. Compruebe el parámetro de *error* para obtener más detalles.|  
|**adStatusCantDeny**|Solo se pasa a eventos de. Este valor significa que la operación no puede ser cancelada por el evento. Debe realizarse.|  
  
 Si determina en el evento se producirá que la operación debe continuar, deje el parámetro *status* sin modificar. Sin embargo, siempre que el parámetro de estado de entrada no se haya establecido en **adStatusCantDeny**, puede cancelar la operación pendiente cambiando el *Estado* a **adStatusCancel**. Al hacerlo, el evento complete asociado a la operación tiene su parámetro *status* establecido en **adStatusErrorsOccurred**. El objeto de **error** que se pasa al evento complete contendrá el valor **adErrOperationCancelled**.  
  
 Si ya no desea procesar un evento, puede establecer el *Estado* en **adStatusUnwantedEvent** y la aplicación ya no recibirá la notificación de ese evento. Sin embargo, recuerde que algunos eventos se pueden generar por más de un motivo. En ese caso, debe especificar **adStatusUnwantedEvent** para cada posible motivo. Por ejemplo, para dejar de recibir la notificación de eventos pendientes de **RecordChange** , debe establecer el parámetro *status* en **adStatusUnwantedEvent** para **adRsnAddNew**, **adRsnDelete**, **adRsnUpdate**, **adRsnUndoUpdate**, **adRsnUndoAddNew**, **adRsnUndoDelete** y **adRsnFirstChange** a medida que se producen.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**adStatusUnwantedEvent**|Solicite que este controlador de eventos no reciba más notificaciones.|  
|**adStatusCancel**|Solicitar la cancelación de la operación que está a punto de producirse.|  
  
## <a name="error-parameter"></a>Parámetro de error  
 El parámetro *error* es una referencia a un objeto de [error](../../reference/ado-api/error-object.md) de ADO. Cuando el parámetro *status* se establece en **adStatusErrorsOccurred**, el objeto **error** contiene información sobre por qué se produjo un error en la operación. Si el evento que va a asociar a un evento completo ha cancelado la operación estableciendo el parámetro *status* en **adStatusCancel**, el objeto de error siempre se establece en **adErrOperationCancelled**.  
  
## <a name="object-parameter"></a>Parámetro de objeto  
 Cada evento recibe uno o más objetos que representan los objetos implicados en la operación. Por ejemplo, el evento **ExecuteComplete** recibe un objeto de **comando** , un objeto de **conjunto de registros** y un objeto de **conexión** .  
  
## <a name="reason-parameter"></a>Parámetro Reason  
 El parámetro *Reason* , *adReason*, proporciona información adicional acerca de por qué se produjo el evento. Se puede llamar varias veces a los eventos con un parámetro *adReason* , incluso para la misma operación, por un motivo diferente cada vez. Por ejemplo, se llama al controlador de eventos **WillChangeRecord** para las operaciones que están a punto de realizar o deshacer la inserción, eliminación o modificación de un registro. Si desea procesar un evento solo cuando se produce por un motivo determinado, puede usar el parámetro *adReason* para filtrar las repeticiones que no le interesan. Por ejemplo, si desea procesar eventos de cambio de registro solo cuando se producen porque se agregó un registro, puede usar algo parecido a lo siguiente.  
  
```  
' BeginEventExampleVB01  
Private Sub rsTest_WillChangeRecord(ByVal adReason As ADODB.EventReasonEnum, ByVal cRecords As Long, adStatus As ADODB.EventStatusEnum, ByVal pRecordset As ADODB.Recordset)  
   If adReason = adRsnAddNew Then  
       ' Process event  
       '...  
   Else  
       ' Cancel event notification for all  
       ' other possible adReason values.  
       adStatus = adStatusUnwantedEvent  
   End If  
End Sub  
' EndEventExampleVB01  
```  
  
 En este caso, la notificación puede producirse por cada uno de los demás motivos. Sin embargo, solo se producirá una vez por cada motivo. Una vez que la notificación se ha producido una vez por cada motivo, recibirá una notificación solo para la adición de un nuevo registro.  
  
 Por el contrario, debe establecer el valor de *adStatus* en **adStatusUnwantedEvent** solo una vez para solicitar que un controlador de eventos sin un parámetro **adReason** dejen de recibir notificaciones de eventos.  
  
## <a name="see-also"></a>Consulte también  
 [Resumen del controlador de eventos de ADO](./ado-event-handler-summary.md)   
 [Creación de instancias de eventos de ADO por lenguaje](./ado-event-instantiation-by-language.md)   
 [Cómo funcionan conjuntamente los controladores de eventos](./how-event-handlers-work-together.md)   
 [Tipos de eventos](./types-of-events.md)