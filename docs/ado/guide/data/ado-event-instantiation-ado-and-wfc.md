---
description: 'Creación de instancias de eventos de ADO: ADO y WFC'
title: 'Creación de instancias de eventos de ADO: ADO y WFC | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 9ee4be21-657b-407a-afa4-0b27a6b096ce
author: rothja
ms.author: jroth
ms.openlocfilehash: b7ed585adcfc9542e3d396f2b0fd1a82d65cdcf4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028357"
---
# <a name="ado-event-instantiation-ado-and-wfc"></a>Creación de instancias de eventos de ADO: ADO y WFC
ADO para Windows Foundation Classes (ADO/WFC) se basa en el modelo de eventos de ADO y presenta una interfaz de programación de aplicaciones simplificada. En general, ADO/WFC intercepta eventos de ADO, consolida los parámetros de evento en una única clase de eventos y, a continuación, llama al controlador de eventos.  
  
### <a name="to-use-ado-events-in-adowfc"></a>Para usar eventos de ADO en ADO/WFC  
  
1.  Defina su propio controlador de eventos para procesar un evento. Por ejemplo, si desea procesar el evento **ConnectComplete** en la familia **ConnectionEvent** , puede usar este código:  
  
    ```  
    public void onConnectComplete(Object sender,ConnectionEvent e)  
    {  
        System.out.println("onConnectComplete:" + e);  
    }  
    ```  
  
2.  Defina un objeto de controlador para representar el controlador de eventos. El objeto de controlador debe ser del tipo de datos **ConnectEventHandler** para un evento de tipo **ConnectionEvent**, o el tipo de datos **RecordsetEventHandler** para un evento de tipo **RecordsetEvent**. Por ejemplo, codifique lo siguiente para el controlador de eventos de **ConnectComplete** :  
  
    ```  
    ConnectionEventHandler handler =   
        new ConnectionEventHandler(this, "onConnectComplete");  
    ```  
  
     El primer argumento del constructor **ConnectionEventHandler** es una referencia a la clase que contiene el controlador de eventos denominado en el segundo argumento.  
  
3.  Agregue el controlador de eventos a una lista de controladores designados para procesar un tipo de evento determinado. Use el método con un nombre como **addOn**_eventName_(*handler*).  
  
4.  ADO/WFC implementa internamente todos los controladores de eventos de ADO. Por lo tanto, un evento provocado por una operación de **conexión** o de **conjunto de registros** es interceptado por un controlador de eventos ADO/wfc.  
  
     El controlador de eventos ADO/WFC pasa los parámetros **ConnectionEvent** de ADO en una instancia de la clase **ConnectionEvent** de ADO/wfc, o los parámetros **RecordsetEvent** de ADO en una instancia de la clase **RecordsetEvent** de ADO/wfc. Estas clases ADO/WFC consolidan los parámetros de evento de ADO; es decir, cada clase ADO/WFC contiene un miembro de datos para cada parámetro único en todos los métodos **ConnectionEvent** o **RecordsetEvent** de ADO.  
  
5.  A continuación, ADO/WFC llama al controlador de eventos con el objeto de evento ADO/WFC. Por ejemplo, el controlador **onConnectComplete** tiene una firma similar a la siguiente:  
  
    ```  
    public void onConnectComplete(Object sender,ConnectionEvent e)  
    ```  
  
     El primer argumento es el tipo de objeto que envió el evento ([conexión](../../reference/ado-api/connection-object-ado.md) o [conjunto de registros](../../reference/ado-api/recordset-object-ado.md)) y el segundo argumento es el objeto de evento ADO/wfc (**ConnectionEvent** o **RecordsetEvent**).  
  
     La firma del controlador de eventos es más sencilla que un evento de ADO. Sin embargo, todavía debe comprender el modelo de eventos de ADO para saber qué parámetros se aplican a un evento y cómo responder.  
  
6.  Vuelva del controlador de eventos al controlador de ADO/WFC para el evento de ADO. ADO/WFC copia los miembros de datos de eventos ADO/WFC pertinentes de nuevo a los parámetros de evento de ADO y, a continuación, devuelve el controlador de eventos de ADO.  
  
7.  Cuando haya terminado el procesamiento, quite el controlador de la lista de controladores de eventos ADO/WFC. Use el método con un **nombre como renameon**_eventName_(*controlador*).  
  
## <a name="see-also"></a>Consulte también  
 [Resumen del controlador de eventos de ADO](./ado-event-handler-summary.md)   
 [ADO: índice de sintaxis de WFC](../../reference/ado-api/ado-wfc-syntax-index.md)   
 [Parámetros de evento](./event-parameters.md)   
 [Cómo funcionan conjuntamente los controladores de eventos](./how-event-handlers-work-together.md)   
 [Tipos de eventos](./types-of-events.md)