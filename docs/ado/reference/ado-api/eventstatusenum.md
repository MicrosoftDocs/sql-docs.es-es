---
description: EventStatusEnum
title: EventStatusEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EventStatusEnum
helpviewer_keywords:
- EventStatusEnum enumeration [ADO]
ms.assetid: ebfd4cda-4017-4873-9d28-38b1c7db12a8
author: rothja
ms.author: jroth
ms.openlocfilehash: ace9f64003241cbf16e0ed957bdce77a1187813c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171181"
---
# <a name="eventstatusenum"></a>EventStatusEnum
Especifica el estado actual de la ejecución de un evento.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adStatusCancel**|4|Solicita la cancelación de la operación que hizo que se produjera el evento.|  
|**adStatusCantDeny**|3|Indica que la operación no puede solicitar la cancelación de la operación pendiente.|  
|**adStatusErrorsOccurred**|2|Indica que la operación que causó el evento no se pudo realizar debido a un error o a errores.|  
|**adStatusOK**|1|Indica que la operación que causó el evento se realizó correctamente.|  
|**adStatusUnwantedEvent**|5|Impide las notificaciones posteriores antes de que se haya terminado de ejecutar el método de evento.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. EventStatus. CANCEL|  
|AdoEnums. EventStatus. CANTDENY|  
|AdoEnums. EventStatus. ERRORSOCCURRED|  
|AdoEnums. EventStatus. OK|  
|AdoEnums. EventStatus. UNWANTEDEVENT|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Eventos BeginTransComplete, CommitTransComplete y RollbackTransComplete (ADO)](../../../ado/reference/ado-api/begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)  

        [Eventos ConnectComplete y Disconnect (ADO)](../../../ado/reference/ado-api/connectcomplete-and-disconnect-events-ado.md)  

        [Evento EndOfRecordset (ADO)](../../../ado/reference/ado-api/endofrecordset-event-ado.md)  

        [Evento ExecuteComplete (ADO)](../../../ado/reference/ado-api/executecomplete-event-ado.md)  
    :::column-end:::
    :::column:::
        [Evento FetchComplete (ADO)](../../../ado/reference/ado-api/fetchcomplete-event-ado.md)  

        [Evento InfoMessage (ADO)](../../../ado/reference/ado-api/infomessage-event-ado.md)  

        [Eventos WillChangeField y FieldChangeComplete (ADO)](../../../ado/reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)  

        [Eventos WillChangeRecord y RecordChangeComplete (ADO)](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)  
    :::column-end:::
    :::column:::
        [Eventos WillChangeRecordset y RecordsetChangeComplete (ADO)](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)  

        [Evento WillConnect (ADO)](../../../ado/reference/ado-api/willconnect-event-ado.md)  

        [Evento WillExecute (ADO)](../../../ado/reference/ado-api/willexecute-event-ado.md)  

        [Eventos WillMove y MoveComplete (ADO)](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md)  
    :::column-end:::
:::row-end:::
