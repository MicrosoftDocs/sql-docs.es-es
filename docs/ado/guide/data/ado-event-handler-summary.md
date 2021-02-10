---
description: Eventos Connection y Recordset de ADO
title: Resumen del controlador de eventos ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- events [ADO], about event handlers
- event handlers [ADO]
ms.assetid: b34f4472-5e04-4a2c-ab64-38d6eca31a69
author: rothja
ms.author: jroth
ms.openlocfilehash: a447c149769d892149096c980b35e77e49de05da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028367"
---
# <a name="ado-connection-and-recordset-events"></a>Eventos Connection y Recordset de ADO
Dos objetos ADO pueden generar eventos: el objeto de [conexión](../../reference/ado-api/connection-object-ado.md) y el objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) . La familia **ConnectionEvent** pertenece a las operaciones en el objeto de **conexión** y la familia **RecordsetEvent** pertenece a las operaciones en el objeto de conjunto de **registros** .

-   **Eventos de conexión**: los eventos se emiten cuando se inicia una transacción en una conexión, se confirma o se revierte. Cuando se ejecuta un [comando](../../reference/ado-api/command-object-ado.md) ; Cuando se produce una advertencia durante una operación de **evento de conexión** ; o cuando se inicia o finaliza una **conexión** .

-   **Eventos de conjunto de registros**: los eventos se emiten en torno a las operaciones de captura asincrónicas y al navegar por las filas de un objeto de **conjunto de registros** , cambiar un campo de una fila de un **conjunto** de registros, cambiar una fila de un **conjunto de registros**, abrir un conjunto de **registros** con un cursor de servidor, cerrar un **conjunto de registros** o realizar cualquier cambio en el **conjunto**

 En las tablas siguientes se resumen los eventos y sus descripciones.

|ConnectionEvent|Descripción|
|---------------------|-----------------|
|[BeginTransComplete, CommitTransComplete, RollbackTransComplete](../../reference/ado-api/begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|**Administración de transacciones** : notificación de que la transacción actual en la conexión se ha iniciado, confirmado o revertido.|
|[WillConnect](../../reference/ado-api/willconnect-event-ado.md), [ConnectComplete, Disconnect](../../reference/ado-api/connectcomplete-and-disconnect-events-ado.md)|**Administración de conexiones** : notificación de que la conexión actual se inicia, se ha iniciado o ha finalizado.|
|[WillExecute](../../reference/ado-api/willexecute-event-ado.md), [ExecuteComplete](../../reference/ado-api/executecomplete-event-ado.md)|**Administración de ejecución de comandos** : notificación de que la ejecución del comando actual en la conexión se iniciará o ha finalizado.|
|[InfoMessage](../../reference/ado-api/infomessage-event-ado.md)|**Informativo** : notificación de que hay información adicional sobre la operación actual.|

|RecordsetEvent|Descripción|
|--------------------|-----------------|
|[FetchProgress](../../reference/ado-api/fetchprogress-event-ado.md), [FetchComplete](../../reference/ado-api/fetchcomplete-event-ado.md)|**Estado de recuperación** : notificación del progreso de una operación de recuperación de datos o de que se ha completado la operación de recuperación. Estos eventos solo están disponibles si el **conjunto de registros** se ha abierto con un cursor del lado cliente.|
|[WillChangeField, FieldChangeComplete](../../reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)|**Administración de cambios de campo** : notificación de que el valor del campo actual cambiará o ha cambiado.|
|[WillMove, MoveComplete](../../reference/ado-api/willmove-and-movecomplete-events-ado.md), [EndOfRecordset](../../reference/ado-api/endofrecordset-event-ado.md)|**Administración de navegación** : notificación de que la posición de la fila actual en un **conjunto de registros** cambia, ha cambiado o ha alcanzado el final del **conjunto de registros**.|
|[WillChangeRecord, RecordChangeComplete](../../reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)|**Administración de cambios de fila** : notificación de que un elemento de la fila actual del **conjunto de registros** cambiará o ha cambiado.|
|[WillChangeRecordset, RecordsetChangeComplete](../../reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)|**Administración de cambios de conjunto de registros** : notificación de que un elemento del **conjunto de registros** actual cambiará o ha cambiado.|

## <a name="see-also"></a>Consulte también
 [Creación de instancias de eventos de ADO con](./ado-event-instantiation-by-language.md) parámetros de eventos de [eventos de ADO](../../reference/ado-api/ado-events.md) de lenguaje cómo [los](./event-parameters.md) [controladores de eventos trabajan juntos](./how-event-handlers-work-together.md) [tipos de eventos](./types-of-events.md)