---
description: Eventos BeginTransComplete, CommitTransComplete y RollbackTransComplete (ADO)
title: Eventos BeginTrans, CommitTrans y RollbackTrans (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CommitTransComplete
- Connection::BeginTransComplete
- Connection::RollbackTransComplete
- Connection::CommitTransComplete
- RollbackTransComplete
- BeginTransComplete
helpviewer_keywords:
- CommitTransComplete event [ADO]
- RollbackTransComplete event [ADO]
- BeginTransComplete event [ADO]
ms.assetid: ec4e4b38-e9c6-4757-b2ef-4e468ae5f1d8
author: rothja
ms.author: jroth
ms.openlocfilehash: 013df2c56a973d8df6b51434f016ee7642fc4f71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164786"
---
# <a name="begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado"></a>Eventos BeginTransComplete, CommitTransComplete y RollbackTransComplete (ADO)
Se llamará a estos eventos después de que la operación asociada en el objeto de [conexión](./connection-object-ado.md) termine de ejecutarse.  
  
-   Se llama a **BeginTransComplete** después de la operación [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) .  
  
-   Se llama a **CommitTransComplete** después de la operación [CommitTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) .  
  
-   Se llama a **RollbackTransComplete** después de la operación [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
BeginTransComplete TransactionLevel, pError, adStatus, pConnection  
CommitTransComplete pError, adStatus, pConnection  
RollbackTransComplete pError, adStatus, pConnection  
```  
  
#### <a name="parameters"></a>Parámetros  
 *TransactionLevel*  
 Un valor **Long** que contiene el nuevo nivel de transacción de los **BeginTrans** que produjeron este evento.  
  
 *pError*  
 Un objeto de [error](./error-object.md) . Describe el error que se produjo si el valor de EventStatusEnum es **adStatusErrorsOccurred**; de lo contrario, no se establece.  
  
 *adStatus*  
 Valor de estado de [EventStatusEnum](./eventstatusenum.md) . Cuando se llama a cualquiera de estos eventos, este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente, o en **adStatusErrorsOccurred** si se produjo un error en la operación.  
  
 Estos eventos pueden impedir las siguientes notificaciones si se establece este parámetro en **adStatusUnwantedEvent** antes de que el evento vuelva.  
  
 *pConnection*  
 Objeto de **conexión** para el que se produjo este evento.  
  
## <a name="remarks"></a>Observaciones  
 En Visual C++, varias **conexiones** pueden compartir el mismo método de control de eventos. El método utiliza el objeto de **conexión** devuelto para determinar qué objeto provocó el evento.  
  
 Si la propiedad [attributes](./attributes-property-ado.md) se establece en **adXactCommitRetaining** o **adXactAbortRetaining**, se inicia una nueva transacción después de confirmar o revertir una transacción. Use el evento **BeginTransComplete** para omitir todos los eventos, excepto el primer evento de inicio de la transacción.  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de modelo de eventos de ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Ejemplo de los métodos BeginTrans, CommitTrans y RollbackTrans (VB)](./begintrans-committrans-and-rollbacktrans-methods-example-vb.md)   
 [Resumen del controlador de eventos de ADO](../../guide/data/ado-event-handler-summary.md)   
 [Métodos BeginTrans, CommitTrans y RollbackTrans (ADO)](./begintrans-committrans-and-rollbacktrans-methods-ado.md)