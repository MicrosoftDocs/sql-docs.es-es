---
description: Control de eventos de ADO
title: Controlar eventos de ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- events [ADO]
- ADO, events
- event handlers [ADO]
ms.assetid: e9003457-0762-48b3-942f-0820266b158f
author: rothja
ms.author: jroth
ms.openlocfilehash: e3bcc0d4d436afcc10b4d079938adf2bad237d88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037415"
---
# <a name="handling-ado-events"></a>Control de eventos de ADO
El modelo de eventos de ADO admite ciertas operaciones de ADO sincrónicas y asincrónicas que emiten *eventos*, o notificaciones, antes de que se inicie la operación o después de que se complete. Un evento es realmente una llamada a una rutina del controlador de eventos que se define en la aplicación.  
  
 Si proporciona funciones de controlador o procedimientos para el grupo de eventos que se producen antes de que se inicie la operación, puede examinar o modificar los parámetros que se pasaron a la operación. Como aún no se ha ejecutado, puede cancelar la operación o permitir que se complete.  
  
 Los eventos que se producen después de completarse una operación son especialmente importantes si usa ADO de forma asincrónica. Por ejemplo, una aplicación que inicia una operación asincrónica [Recordset. Open](../../reference/ado-api/open-method-ado-recordset.md) se notifica mediante un evento de ejecución completada cuando concluye la operación.  
  
 El uso del modelo de eventos de ADO agrega cierta sobrecarga a la aplicación, pero proporciona mucha más flexibilidad que otros métodos de trabajar con operaciones asincrónicas, como la supervisión de la propiedad [State](../../reference/ado-api/state-property-ado.md) de un objeto con un bucle.  
  
> [!NOTE]
>  Para controlar los eventos, ADO debe tener un bombeo de mensajes o usarse en un modelo de contenedor uniproceso (STA). Los eventos de ADO se administran internamente mediante la creación de una ventana oculta. ADO envía mensajes a esta ventana cuando es necesario desencadenar eventos. Esto se hace para asegurarse de que los eventos se envían al subproceso que llamó a **IConnectionPoint:: Advise** en el punto de conexión. Esta arquitectura puede producir problemas cuando el subproceso que debe recibir las notificaciones no bombea mensajes de ventana. Entre los posibles problemas se incluyen los eventos de ADO que no se entregan al subproceso y las difusiones de la ventana global agotan el tiempo de espera y posiblemente ralentizan todo el sistema porque las ventanas ocultas no procesan los mensajes. Normalmente, los subprocesos STA tienen un bombeo de mensajes en ejecución, por lo que este problema no se manifiesta en los subprocesos STA. Sin embargo, los subprocesos MTA no tienen normalmente un bombeo de mensajes, por lo que el problema normalmente se manifiesta en subprocesos MTA.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Conexión ADO y los eventos de conjunto de registros](./ado-event-handler-summary.md)  
  
-   [Tipos de eventos](./types-of-events.md)  
  
-   [Parámetros de eventos](./event-parameters.md)  
  
-   [Cómo funcionan conjuntamente los controladores de eventos](./how-event-handlers-work-together.md)  
  
-   [Creación de instancias de eventos de ADO según el lenguaje](./ado-event-instantiation-by-language.md)  
  
## <a name="see-also"></a>Consulte también  
 [Resumen del controlador de eventos de ADO](./ado-event-handler-summary.md)   
 [Creación de instancias de eventos de ADO por lenguaje](./ado-event-instantiation-by-language.md)   
 [Eventos de ADO](../../reference/ado-api/ado-events.md)   
 [Parámetros de evento](./event-parameters.md)   
 [Tipos de eventos](./types-of-events.md)