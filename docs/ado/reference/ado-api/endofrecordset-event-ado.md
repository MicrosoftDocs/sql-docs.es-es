---
description: Evento EndOfRecordset (ADO)
title: EndOfRecordset (evento) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EndOfRecordset
- Recordset::EndOfRecordset
helpviewer_keywords:
- EndOfRecordset event [ADO]
ms.assetid: 475de5e2-f634-4954-9edf-0027a6ba38d6
author: rothja
ms.author: jroth
ms.openlocfilehash: 9369b22c012773627c17da55a336aac116dc4e06
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171224"
---
# <a name="endofrecordset-event-ado"></a>Evento EndOfRecordset (ADO)
Se llama al evento **EndOfRecordset** cuando se intenta moverse a una fila más allá del final del [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
EndOfRecordset fMoreData, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Parámetros  
 *fMoreData*  
 Un valor **VARIANT_BOOL** que, si se establece en VARIANT_TRUE, indica que se han agregado más filas al **conjunto de registros**.  
  
 *adStatus*  
 Valor de estado de [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md) .  
  
 Cuando se llama a **EndOfRecordset** , este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente. Se establece en **adStatusCantDeny** si este evento no puede solicitar la cancelación de la operación que provocó este evento.  
  
 Antes de que **EndOfRecordset** vuelva, establezca este parámetro en **adStatusUnwantedEvent** para evitar las notificaciones posteriores.  
  
 *pRecordset*  
 Objeto de **conjunto de registros** . **Conjunto de registros** para el que se produjo este evento.  
  
## <a name="remarks"></a>Observaciones  
 Un evento **EndOfRecordset** puede producirse si se produce un error en la operación [MoveNext](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md) .  
  
 Se llama a este controlador de eventos cuando se realiza un intento de moverse más allá del final del objeto de **conjunto de registros** , quizás como resultado de llamar a **MoveNext**. Sin embargo, mientras que en este evento, podría recuperar más registros de una base de datos y anexarlos al final del **conjunto de registros**. En ese caso, establezca *fMoreData* en VARIANT_TRUE y vuelva de **EndOfRecordset**. A continuación, llame de nuevo a **MoveNext** para acceder a los registros recién recuperados.  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de modelo de eventos de ADO (VC + +)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Conexión ADO y los eventos de conjunto de registros](../../../ado/guide/data/ado-event-handler-summary.md)
