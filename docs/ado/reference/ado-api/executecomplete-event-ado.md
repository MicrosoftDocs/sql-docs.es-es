---
description: Evento ExecuteComplete (ADO)
title: Evento ExecuteComplete (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection::ExecuteComplete
- ExecuteComplete
helpviewer_keywords:
- ExecuteComplete event [ADO]
ms.assetid: 62470d42-e511-494c-bec4-ad4591734b7b
author: rothja
ms.author: jroth
ms.openlocfilehash: e6a4311090ca5402d4f036c6f3eb83b80a17aacb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034115"
---
# <a name="executecomplete-event-ado"></a>Evento ExecuteComplete (ADO)
Se llama al evento **ExecuteComplete** una vez finalizada la ejecución de un comando.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
ExecuteComplete RecordsAffected, pError, adStatus, pCommand, pRecordset, pConnection  
```  
  
#### <a name="parameters"></a>Parámetros  
 *RecordsAffected*  
 Un valor **largo** que indica el número de registros afectados por el comando.  
  
 *pError*  
 Un objeto de [error](../../../ado/reference/ado-api/error-object.md) . Describe el error que se produjo si el valor de **adStatus** es **adStatusErrorsOccurred**; de lo contrario, no se establece.  
  
 *adStatus*  
 Valor de estado de [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md) . Cuando se llama a este evento, este parámetro se establece en **adStatusOK** si la operación que causó el evento se realizó correctamente, o en **adStatusErrorsOccurred** si se produjo un error en la operación.  
  
 Antes de que se devuelva este evento, establezca este parámetro en **adStatusUnwantedEvent** para evitar notificaciones posteriores.  
  
 *pRecordset*  
 Objeto de [comando](../../../ado/reference/ado-api/command-object-ado.md) que se ha ejecutado. Contiene un objeto **Command** incluso cuando se llama a **Connection.Exe** de la función. Open o a **Recordset. Open** sin crear explícitamente un **comando**, en cuyo caso ADO crea internamente el objeto de **comando** .  
  
 *pRecordset*  
 Objeto de [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) que es el resultado del comando ejecutado. Este **conjunto de registros** puede estar vacío. Nunca debe destruir este objeto de conjunto de registros desde este controlador de eventos. Si lo hace, se producirá una infracción de acceso cuando ADO intente obtener acceso a un objeto que ya no existe.  
  
 *pConnection*  
 Objeto de [conexión](../../../ado/reference/ado-api/connection-object-ado.md) . La conexión en la que se ejecutó la operación.  
  
## <a name="remarks"></a>Observaciones  
 Puede producirse un evento **ExecuteComplete** debido a la **conexión.** [Ejecutar](../../../ado/reference/ado-api/execute-method-ado-connection.md), **comando.** [Ejecutar](../../../ado/reference/ado-api/execute-method-ado-command.md), **conjunto de registros.** [Abra](../../../ado/reference/ado-api/open-method-ado-recordset.md), **conjunto de registros.** [Requery](../../../ado/reference/ado-api/requery-method.md)o **Recordset.** Métodos [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md) .  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de modelo de eventos de ADO (VC + +)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Conexión ADO y los eventos de conjunto de registros](../../../ado/guide/data/ado-event-handler-summary.md)
