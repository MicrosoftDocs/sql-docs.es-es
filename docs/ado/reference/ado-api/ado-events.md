---
description: Eventos de ADO
title: Eventos de ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- events [ADO]
- ADO, events
ms.assetid: 0ded5ad9-8f83-4224-95af-38512783b972
author: rothja
ms.author: jroth
ms.openlocfilehash: 343d24455490674a9c926d94a902906992679a2c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164926"
---
# <a name="ado-events"></a>Eventos de ADO

|Evento|Descripción|  
|-|-|  
|[BeginTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Se llama después de la operación **BeginTrans** .|  
|[CommitTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Se llama después de la operación **CommitTrans** .|  
|[ConnectComplete](./connectcomplete-and-disconnect-events-ado.md)|Se llama después de que se inicie una conexión.|  
|[Desconexión](./connectcomplete-and-disconnect-events-ado.md)|Se llama después de que finalice una conexión.|  
|[EndOfRecordset](./endofrecordset-event-ado.md)|Se llama cuando se intenta moverse a una fila más allá del final del conjunto de **registros**.|  
|[ExecuteComplete](./executecomplete-event-ado.md)|Se llama después de que se haya terminado de ejecutar un comando.|  
|[FetchComplete](./fetchcomplete-event-ado.md)|Se llama después de que todos los registros de una operación asincrónica larga se hayan recuperado en el **conjunto de registros**.|  
|[FetchProgress](./fetchprogress-event-ado.md)|Se llama periódicamente durante una operación asincrónica larga para informar del número de filas que se han recuperado actualmente en el **conjunto de registros**.|  
|[FieldChangeComplete](./willchangefield-and-fieldchangecomplete-events-ado.md)|Se llama después de cambiar el valor de uno o más objetos **Field** .|  
|[InfoMessage](./infomessage-event-ado.md)|Se llama siempre que se produce una advertencia durante una operación **ConnectionEvent** .|  
|[MoveComplete](./willmove-and-movecomplete-events-ado.md)|Se llama después de que cambie la posición actual del **conjunto de registros** .|  
|[RecordChangeComplete](./willchangerecord-and-recordchangecomplete-events-ado.md)|Se llama después de que uno o varios registros cambien.|  
|[RecordsetChangeComplete](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Se llama después de que el **conjunto de registros** haya cambiado.|  
|[RollbackTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Se llama después de la operación **RollbackTrans** .|  
|[WillChangeField](./willchangefield-and-fieldchangecomplete-events-ado.md)|Se llama antes de que una operación pendiente cambie el valor de uno o más objetos de **campo** en el **conjunto de registros**.|  
|[WillChangeRecord](./willchangerecord-and-recordchangecomplete-events-ado.md)|Se llama antes de que se cambien uno o más registros (filas) del **conjunto de registros** .|  
|[WillChangeRecordset](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Se llama antes de que una operación pendiente cambie el **conjunto de registros**.|  
|[WillConnect](./willconnect-event-ado.md)|Se llama antes de que se inicie una conexión.|  
|[WillExecute](./willexecute-event-ado.md)|Se llama justo antes de que un comando pendiente se ejecute en esta conexión y permite al usuario examinar y modificar los parámetros de ejecución pendientes.|  
|[WillMove](./willmove-and-movecomplete-events-ado.md)|Se  llama al evento WillMove *antes* de que una operación pendiente cambie la posición actual en el **conjunto de registros**.|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ADO](./ado-api-reference.md)   
 [Colecciones de ADO](./ado-collections.md)   
 [Propiedades dinámicas de ADO](./ado-dynamic-properties.md)   
 [Constantes enumeradas de ADO](./ado-enumerated-constants.md)   
 [Apéndice B: errores de ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Controlar eventos de ADO](../../guide/data/handling-ado-events.md)   
 [Métodos de ADO](./ado-methods.md)   
 [Modelo de objetos ADO](./ado-object-model.md)   
 [Objetos e interfaces de ADO](./ado-objects-and-interfaces.md)   
 [Propiedades de ADO](./ado-properties.md)