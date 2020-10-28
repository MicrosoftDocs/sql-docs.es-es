---
description: Implementar notificaciones de eventos
title: Implementar notificaciones de eventos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- event notifications [SQL Server], target service
- target service [SQL Server]
- event notifications [SQL Server], creating
ms.assetid: 29ac8f68-a28a-4a77-b67b-a8663001308c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 91eef4cfd2a1097b7879c507cbccb0b3d3a7e909
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92679006"
---
# <a name="implement-event-notifications"></a>Implementar notificaciones de eventos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Para implementar una notificación de eventos, debe crear primero un servicio de destino para que reciba las notificaciones de eventos y, a continuación, crear la notificación de eventos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssSB](../../includes/sssb-md.md)] se debe configurar para las notificaciones de eventos que envíen mensajes a un Service Broker en un servidor remoto. La seguridad del diálogo debe configurarse manualmente según el modelo de seguridad completa.  
  
## <a name="creating-the-target-service"></a>Crear el servicio de destino  
 No es necesario que cree un servicio de inicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)]debido a que [!INCLUDE[ssSB](../../includes/sssb-md.md)] incluye el siguiente tipo de mensaje y contrato para notificaciones de eventos:  
  
```  
https://schemas.microsoft.com/SQL/Notifications/PostEventNotification  
```  
  
 El servicio de destino que recibe notificaciones de eventos debe respetar este contrato preexistente.  
  
 **Para crear un servicio de destino** :  
  
1.  Cree una cola para recibir mensajes.  
  
    > [!NOTE]  
    >  La cola recibe el siguiente tipo de mensaje: `http://schemas.microsoft.com/SQL/Notifications/QueryNotification`.  
  
2.  Cree un servicio en la cola que hace referencia al contrato de notificaciones de eventos.  
  
3.  Cree una ruta en el servicio para definir la dirección a la que [!INCLUDE[ssSB](../../includes/sssb-md.md)] envía los mensajes de ese servicio. Para las notificaciones de eventos que tengan como destino un servicio en la misma base de datos, especifique `ADDRESS = 'LOCAL'`.  
  
    > [!NOTE]  
    >  [!INCLUDE[ssSB](../../includes/sssb-md.md)] determina el servicio que recibe los mensajes de notificación. Si la notificación de eventos tiene como destino un servicio en un servidor remoto, tanto el servidor de origen como el servidor de destino deben tener rutas definidas en este servidor para garantizar que se produce comunicación en los dos sentidos.  
  
 En el ejemplo siguiente se crea una cola, un servicio en la cola y una ruta en el servicio para procesar los mensajes del contrato de notificaciones de eventos.  
  
```  
CREATE QUEUE NotifyQueue ;  
GO  
CREATE SERVICE NotifyService  
ON QUEUE NotifyQueue  
(  
[http://schemas.microsoft.com/SQL/Notifications/PostEventNotification]  
);  
GO  
CREATE ROUTE NotifyRoute  
WITH SERVICE_NAME = 'NotifyService',  
ADDRESS = 'LOCAL';  
GO  
```  
  
## <a name="creating-the-event-notification"></a>Crear la notificación de eventos  
 Las notificaciones de eventos se crean mediante la instrucción CREATE EVENT NOTIFICATION de [!INCLUDE[tsql](../../includes/tsql-md.md)] y se quitan con DROP EVENT NOTIFICATION STATEMENT. Para modificar una notificación de eventos, debe quitarla y volver a crearla.  
  
 En el ejemplo siguiente se crea la notificación de eventos `CreateDatabaseNotification`. Esta notificación envía un mensaje acerca de cualquier evento `CREATE_DATABASE` que se produzca en el servidor en el servicio `NotifyService` que se creó anteriormente.  
  
```  
CREATE EVENT NOTIFICATION CreateDatabaseNotification  
ON SERVER  
FOR CREATE_DATABASE  
TO SERVICE 'NotifyService', '8140a771-3c4b-4479-8ac0-81008ab17984' ;  
```  
  
> [!CAUTION]  
>  Las notificaciones de eventos reconocen los eventos CREATE_SCHEMA y las definiciones <schema_element> de las instrucciones CREATE SCHEMA como eventos independientes. Por ejemplo, una notificación de eventos se crea en los dos eventos CREATE_SCHEMA y CREATE_TABLE, y se ejecuta el siguiente lote.  
>   
>  `CREATE SCHEMA s`  
>   
>  `CREATE TABLE t1 (col1 int)`  
>   
>  En este caso, el evento se notifica dos veces: una vez cuando se produce el evento CREATE_SCHEMA y otra vez cuando ocurre el evento CREATE_TABLE. Es recomendable que no cree notificaciones de eventos en los eventos CREATE_SCHEMA ni en los textos <schema_element> de las definiciones CREATE SCHEMA correspondientes, y que no genere lógica en la aplicación para evitar capturar datos de eventos no deseados.  
  
 **Para crear una notificación de eventos**  
  
-   [CREATE EVENT NOTIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-notification-transact-sql.md)  
  
 **Para quitar una notificación de eventos**  
  
-   [DROP EVENT NOTIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-notification-transact-sql.md)  
  
## <a name="see-also"></a>Vea también  
 [Obtener información sobre notificaciones de eventos](../../relational-databases/service-broker/get-information-about-event-notifications.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
