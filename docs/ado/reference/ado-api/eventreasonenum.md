---
description: EventReasonEnum
title: EventReasonEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EventReasonEnum
helpviewer_keywords:
- EventReasonEnum enumeration [ADO]
ms.assetid: 7d4a5496-ec2d-4936-b36a-7049a82be4b4
author: rothja
ms.author: jroth
ms.openlocfilehash: d8789e0c2819bfd4a976dfa047b6081284e7292d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034185"
---
# <a name="eventreasonenum"></a>EventReasonEnum
Especifica la razón por la que se produjo un evento.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adRsnAddNew**|1|Una operación agregó un nuevo registro.|  
|**adRsnClose**|9|Una operación cerró el **conjunto de registros**.|  
|**adRsnDelete**|2|Una operación eliminó un registro.|  
|**adRsnFirstChange**|11|Una operación realizó el primer cambio en un registro.|  
|**adRsnMove**|10|Una operación ha despasado el puntero de registro en el **conjunto de registros**.|  
|**adRsnMoveFirst**|12|Una operación ha pasado el puntero de registro al primer registro del **conjunto de registros**.|  
|**adRsnMoveLast**|15|Una operación ha pasado el puntero de registro al último registro del **conjunto de registros**.|  
|**adRsnMoveNext**|13|Una operación ha pasado el puntero de registro al siguiente registro del **conjunto de registros**.|  
|**adRsnMovePrevious**|14|Una operación ha pasado el puntero de registro al registro anterior del **conjunto de registros**.|  
|**adRsnRequery**|7|Una operación volvió a consultar el [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md).|  
|**adRsnResynch**|8|Una operación vuelve a sincronizar el **conjunto de registros** con la base de datos.|  
|**adRsnUndoAddNew**|5|Una operación invirtió la adición de un nuevo registro.|  
|**adRsnUndoDelete**|6|Una operación invirtió la eliminación de un registro.|  
|**adRsnUndoUpdate**|4|Una operación invirtió la actualización de un registro.|  
|**adRsnUpdate**|3|Una operación actualizó un registro existente.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. EventReason. ADDNEW|  
|AdoEnums. EventReason. CLOSE|  
|AdoEnums. EventReason. DELETE|  
|AdoEnums. EventReason. FIRSTCHANGE|  
|AdoEnums. EventReason. MOVE|  
|AdoEnums. EventReason. MOVEFIRST|  
|AdoEnums. EventReason. MoveLast|  
|AdoEnums. EventReason. MOVENEXT|  
|AdoEnums. EventReason. MOVEPREVIOUS|  
|AdoEnums. EventReason. Requery|  
|AdoEnums. EventReason. resincronizar|  
|AdoEnums. EventReason. UNDOADDNEW|  
|AdoEnums. EventReason. UNDODELETE|  
|AdoEnums. EventReason. UNDOUPDATE|  
|AdoEnums. EventReason. UPDATE|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Eventos WillChangeRecord y RecordChangeComplete (ADO)](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)  

        [Eventos WillChangeRecordset y RecordsetChangeComplete (ADO)](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)  
    :::column-end:::
    :::column:::
        [Eventos WillMove y MoveComplete (ADO)](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md)  
    :::column-end:::
:::row-end:::
