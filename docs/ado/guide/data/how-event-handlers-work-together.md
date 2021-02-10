---
description: Cómo funcionan conjuntamente los controladores de eventos
title: Cómo funcionan conjuntamente los controladores de eventos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- events [ADO], about event handlers
- unpaired event handlers [ADO]
- paired event handlers [ADO]
- single event handlers [ADO]
- event handlers [ADO]
- multiple object event handlers [ADO]
ms.assetid: a86c8a02-dd69-420d-8a47-0188b339858d
author: rothja
ms.author: jroth
ms.openlocfilehash: dd6950ec37ddf974470e29ae554603723b299ef5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037355"
---
# <a name="how-event-handlers-work-together"></a>Cómo funcionan conjuntamente los controladores de eventos
A menos que esté programando en Visual Basic, se deben implementar todos los controladores de eventos para los eventos **Connection** y **Recordset** , independientemente de si realmente procesa todos los eventos. La cantidad de trabajo de implementación que tiene que hacer depende del lenguaje de programación. Para obtener más información, vea [creación de instancias de eventos de ADO por lenguaje](./ado-event-instantiation-by-language.md).  
  
## <a name="paired-event-handlers"></a>Controladores de eventos emparejados  
 Cada controlador de eventos tiene asociado un controlador de eventos **completo** . Por ejemplo, cuando la aplicación cambia el valor de un campo, se llama al controlador de eventos **WillChangeField** . Si el cambio es aceptable, la aplicación deja el parámetro **adStatus** sin modificar y se realiza la operación. Una vez finalizada la operación, un evento **FieldChangeComplete** notifica a la aplicación que la operación ha finalizado. Si se completó correctamente, **adStatus** contiene **adStatusOK**; de lo contrario, **adStatus** contiene **adStatusErrorsOccurred** y debe comprobar el objeto de **error** para determinar la causa del error.  
  
 Cuando se llama a **WillChangeField** , puede determinar que no se debe realizar el cambio. En ese caso, establezca **adStatus** en **adStatusCancel.** La operación se cancela y el evento **FieldChangeComplete** recibe un valor de **adStatus** de **adStatusErrorsOccurred**. El objeto **error** contiene **adErrOperationCancelled** para que el controlador **FieldChangeComplete** sepa que se canceló la operación. No obstante, debe comprobar el valor del parámetro **adStatus** antes de cambiarlo, ya que el valor de **adStatus** en **adStatusCancel** no tiene ningún efecto si el parámetro se estableció en **adStatusCantDeny** en la entrada al procedimiento.  
  
 A veces, una operación puede producir más de un evento. Por ejemplo, el objeto de **conjunto de registros** tiene eventos emparejados para cambios de **campo** y cambios de **registro** . Cuando la aplicación cambia el valor de un **campo**, se llama al controlador de eventos **WillChangeField** . Si determina que la operación puede continuar, también se genera el controlador de eventos **WillChangeRecord** . Si este controlador también permite que el evento continúe, se realiza el cambio y se llama a los controladores de eventos **FieldChangeComplete** y **RecordChangeComplete** . El orden en el que se llama a los controladores de eventos para una operación determinada no está definido, por lo que debe evitar escribir código que dependa de llamar a los controladores en una secuencia determinada.  
  
 En las instancias de cuando se producen varios eventos, uno de los eventos podría cancelar la operación pendiente. Por ejemplo, cuando la aplicación cambia el valor de un **campo**, normalmente se llamaría a los controladores de eventos **WillChangeField** y **WillChangeRecord** . Sin embargo, si la operación se cancela en el primer controlador de eventos, se llama inmediatamente a su controlador **completo** asociado con **adStatusOperationCancelled**. Nunca se llama al segundo controlador. Sin embargo, si el primer controlador de eventos permite que continúe el evento, se llamará al otro controlador de eventos. Si después cancela la operación, se llamará a ambos eventos **Complete** como en los ejemplos anteriores.  
  
## <a name="unpaired-event-handlers"></a>Controladores de eventos no emparejados  
 Siempre que el estado que se pasa al evento no sea **adStatusCantDeny**, se pueden desactivar las notificaciones de eventos para cualquier evento devolviendo **adStatusUnwantedEvent** en el parámetro *status* . Por ejemplo, cuando se llama por primera vez a un controlador de eventos **completo** , puede devolver **adStatusUnwantedEvent**. **Recibirá posteriormente solo eventos** . Sin embargo, algunos eventos se pueden desencadenar por más de una razón. En ese caso, el evento tendrá un parámetro *Reason* . Cuando devuelva **adStatusUnwantedEvent**, dejará de recibir notificaciones para ese evento solo cuando se produzcan por ese motivo concreto. Dicho de otro modo, recibirá una notificación por cada posible motivo por el que se puede desencadenar el evento.  
  
 Los controladores **de eventos únicos** pueden ser útiles si desea examinar los parámetros que se utilizarán en una operación. Puede modificar los parámetros de la operación o cancelar la operación.  
  
 O bien, deje habilitada la notificación de eventos **completa** . La primera vez que se llama al controlador de eventos, devuelve **adStatusUnwantedEvent**. Posteriormente, solo recibirá eventos **completos** .  
  
 Los controladores de eventos **completos** pueden ser útiles para administrar operaciones asincrónicas. Cada operación asincrónica tiene un evento **completo** adecuado.  
  
 Por ejemplo, puede tardar mucho tiempo en rellenar un objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) grande. Si la aplicación se ha escrito correctamente, puede iniciar una `Recordset.Open(...,adAsyncExecute)` operación y continuar con otro procesamiento. Finalmente recibirá una notificación cuando el **conjunto de registros** se rellene con un evento **ExecuteComplete** .  
  
## <a name="single-event-handlers-and-multiple-objects"></a>Controladores de eventos únicos y varios objetos  
 La flexibilidad de un lenguaje de programación como Microsoft Visual C++® permite que un controlador de eventos procese eventos de varios objetos. Por ejemplo, podría tener un controlador de eventos de **desconexión** que procese eventos de varios objetos de **conexión** . Si una de las conexiones ha finalizado, se llamará al controlador de eventos **Disconnect** . Podría indicar qué conexión causó el evento porque el parámetro de objeto de controlador de eventos se establecería en el objeto de **conexión** correspondiente.  
  
> [!NOTE]
>  Esta técnica no se puede usar en Visual Basic porque ese lenguaje solo puede correlacionar un objeto con un controlador de eventos.  
  
## <a name="see-also"></a>Consulte también  
 [Resumen del controlador de eventos de ADO](./ado-event-handler-summary.md)   
 [Creación de instancias de eventos de ADO por lenguaje](./ado-event-instantiation-by-language.md)   
 [Parámetros de evento](./event-parameters.md)   
 [Tipos de eventos](./types-of-events.md)