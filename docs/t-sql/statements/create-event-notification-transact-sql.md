---
description: CREATE EVENT NOTIFICATION (Transact-SQL)
title: CREATE EVENT NOTIFICATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_EVENT_NOTIFICATION_TSQL
- NOTIFICATION_TSQL
- EVENT
- NOTIFICATION
- CREATE EVENT NOTIFICATION
- EVENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EVENT NOTIFICATION statement
- events [SQL Server], notifications
- event notifications [SQL Server], creating
ms.assetid: dbbff0e8-9e25-4f12-a1ba-e12221d16ac2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2f4f4863a468f5f7d0da39cc578c8f4f865f7828
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186872"
---
# <a name="create-event-notification-transact-sql"></a>CREATE EVENT NOTIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un objeto que envía información sobre un evento de servidor o base de datos para un servicio de Service Broker. Las notificaciones de eventos solo se pueden crear mediante instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE EVENT NOTIFICATION event_notification_name   
ON { SERVER | DATABASE | QUEUE queue_name }   
[ WITH FAN_IN ]  
FOR { event_type | event_group } [ ,...n ]  
TO SERVICE 'broker_service' , { 'broker_instance_specifier' | 'current database' }  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *event_notification_name*  
 Es el nombre de la notificación de eventos. El nombre de una notificación de eventos debe cumplir las reglas para [identificadores](../../relational-databases/databases/database-identifiers.md) y debe ser único dentro del ámbito en el que se creó: SERVER, DATABASE u *object_name*.  
  
 SERVER  
 Aplica el ámbito de la notificación de eventos a la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si se especifica, la notificación se activa cada vez que el evento especificado en la cláusula FOR se produce en cualquier lugar de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 DATABASE  
 Aplica el ámbito de la notificación de eventos a la base de datos actual. Si se especifica, la notificación se activa cada vez que el evento especificado en la cláusula FOR se produce en la base de datos actual.  
  
 QUEUE  
 Aplica el ámbito de la notificación a la cola especificada en la base de datos actual. Se puede especificar QUEUE solo si también se especifica FOR QUEUE_ACTIVATION o FOR BROKER_QUEUE_DISABLED.  
  
 *queue_name*  
 Es el nombre de la cola a la que se aplica la notificación de eventos. Solo se puede especificar *queue_name* si se ha establecido QUEUE.  
  
 WITH FAN_IN   
 Indica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que solo envíe un mensaje por evento a un servicio especificado para todas las notificaciones de eventos que:  
  
-   Se crean en el mismo evento.  
  
-   Son creadas por la misma entidad de seguridad (identificada por el mismo SID).  
  
-   Especifica el mismo servicio y *broker_instance_specifier*.  
  
-   Especifique WITH FAN_IN.  
  
 Por ejemplo, se crean tres notificaciones de evento. Todas las notificaciones de evento especifican FOR ALTER_TABLE, WITH FAN_IN, la misma cláusula TO SERVICE y se crean con el mismo SID. Cuando se ejecuta una instrucción ALTER TABLE, los mensajes que crean estas tres notificaciones de evento se mezclan en uno solo. Por tanto, el servicio de destino recibe un solo mensaje del evento.  
  
 *event_type*  
 Es el nombre del tipo de evento que ejecuta la notificación de eventos. *event_type* puede ser un tipo de evento DDL de [!INCLUDE[tsql](../../includes/tsql-md.md)], un tipo de evento de Seguimiento SQL o un tipo de evento de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Para obtener una lista de tipos de eventos DDL de [!INCLUDE[tsql](../../includes/tsql-md.md)] posibles, vea [Eventos DDL](../../relational-databases/triggers/ddl-events.md). Los tipos de evento de [!INCLUDE[ssSB](../../includes/sssb-md.md)] son QUEUE_ACTIVATION y BROKER_QUEUE_DISABLED. Para más información, consulte [Event Notifications](../../relational-databases/service-broker/event-notifications.md).  
  
 *event_group*  
 Es el nombre de un grupo predefinido de tipos de evento de [!INCLUDE[tsql](../../includes/tsql-md.md)] o de Seguimiento de SQL. Se puede activar una notificación de eventos tras la ejecución de cualquier evento que pertenece a un grupo de eventos. Para obtener una lista de grupos de eventos DDL, los eventos de [!INCLUDE[tsql](../../includes/tsql-md.md)] que cubren y el ámbito en el que se pueden definir, vea [Grupos de eventos DDL](../../relational-databases/triggers/ddl-event-groups.md).  
  
 *event_group* también actúa como una macro, cuando finaliza la instrucción CREATE EVENT NOTIFICATION, al agregar los tipos de evento que cubre a la vista de catálogo **sys.events**.  
  
 **'** *broker_service* **'**  
 Especifica el servicio de destino que recibe los datos de instancia de evento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abre una o más conversaciones al servicio de destino para la notificación de eventos. Este servicio debe respetar el mismo contrato y tipo de mensaje de eventos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se utilizan en el envío del mensaje.  
  
 Las conversaciones permanecen abiertas hasta que se quita la notificación de eventos. Algunos errores pueden hacer que las conversaciones se cierren antes. Es posible que la finalización de algunas o todas las conversaciones explícitamente evite que el servicio de destino reciba más mensajes.  
  
 { **'** _broker\_instance\_specifier_ **'**  |  **'current database'** }  
 Especifica una instancia de Service Broker en la que se resuelve *broker_service*. Se puede adquirir el valor de un Service Broker específico al realizar una consulta en la columna **service_broker_guid** de la vista de catálogo **sys.databases**. Utilice **'current database'** para especificar la instancia de Service Broker en la base de datos actual. **'current database'** es un literal de cadena que no distingue mayúsculas de minúsculas.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
## <a name="remarks"></a>Observaciones  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] incluye un tipo de mensaje y un contrato específicos para notificaciones de evento. Por lo tanto, no se debe crear un servicio de inicio de Service Broker debido a que ya existe uno que especifica el nombre de contrato siguiente: `https://schemas.microsoft.com/SQL/Notifications/PostEventNotification`.  
  
 El servicio de destino que recibe notificaciones de eventos debe respetar este contrato preexistente.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssSB](../../includes/sssb-md.md)] se debe configurar para las notificaciones de eventos que envíen mensajes a un Service Broker en un servidor remoto. La seguridad del diálogo debe configurarse manualmente según el modelo de seguridad completa. Para obtener más información, consulte [Configurar la seguridad de diálogo para notificaciones de eventos](../../relational-databases/service-broker/configure-dialog-security-for-event-notifications.md).  
  
 Si se revierte una transacción de evento que activa una notificación, también se revierte el envío de la notificación de eventos. Las notificaciones de eventos no son activadas por una acción definida en un desencadenador cuando se confirma o revierte la transacción en el desencadenador. Dado que los eventos de seguimiento no están enlazados mediante transacciones, se envían las notificaciones de eventos basadas en los eventos de seguimiento, independientemente de si se revierte la transacción que los activa.  
  
 Si se interrumpe la conversación entre el servidor y el servicio de destino tras la activación de una notificación de eventos, se informa un error y se quita la notificación de eventos.  
  
 Tanto si se envía la notificación de eventos como si no, la notificación de eventos que originalmente inició la notificación no se ve afectada.  
  
 Se guarda un registro de cualquier error en el envío de una notificación de eventos.  
  
## <a name="permissions"></a>Permisos  
 Para crear una notificación de eventos en el ámbito de la base de datos (ON DATABASE), se requiere el permiso CREATE DATABASE DDL EVENT NOTIFICATION en la base de datos actual.  
  
 Para crear una notificación de eventos en una instrucción DDL incluida en el ámbito del servidor (ON SERVER), se requiere el permiso CREATE DDL EVENT NOTIFICATION en el servidor.  
  
 Para crear una notificación de eventos en un evento de seguimiento, se requiere el permiso CREATE TRACE EVENT NOTIFICATION en el servidor.  
  
 Para crear una notificación de eventos en el ámbito de una cola, se requiere el permiso ALTER en la cola.  
  
## <a name="examples"></a>Ejemplos  
  
> [!NOTE]  
>  En los ejemplos A y B siguientes, el GUID de la cláusula `TO SERVICE 'NotifyService'` (8140a771-3c4b-4479-8ac0-81008ab17984') es específico para el equipo en el que se configuró el ejemplo. Para esa instancia, ese era el GUID para la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
>   
>  Para copiar y ejecutar estos ejemplos, necesita reemplazar este GUID por uno de su equipo y su instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tal como se ha explicado en la sección Argumentos anterior, puede adquirir el **'** _broker\_instance\_specifier_ **'** consultando la columna de service_broker_guid de la vista de catálogo sys.databases.  
  
### <a name="a-creating-an-event-notification-that-is-server-scoped"></a>A. Crear una notificación de eventos en el ámbito de un servidor  
 En el ejemplo siguiente se crean los objetos necesarios para configurar un servicio de destino utilizando [!INCLUDE[ssSB](../../includes/sssb-md.md)]. El servicio de destino hace referencia al contrato y tipo de mensaje del servicio de inicio específicamente para notificaciones de eventos. Después, se crea una notificación de eventos en el servicio de destino que envía una notificación cada vez que tiene lugar un evento de seguimiento `Object_Created` en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
--Create a queue to receive messages.  
CREATE QUEUE NotifyQueue ;  
GO  

--Create a service on the queue that references  
--the event notifications contract.  
CREATE SERVICE NotifyService  
ON QUEUE NotifyQueue  
([https://schemas.microsoft.com/SQL/Notifications/PostEventNotification]);  
GO  

--Create a route on the service to define the address   
--to which Service Broker sends messages for the service.  
CREATE ROUTE NotifyRoute  
WITH SERVICE_NAME = 'NotifyService',  
ADDRESS = 'LOCAL';  
GO 

--Create the event notification.  
CREATE EVENT NOTIFICATION log_ddl1   
ON SERVER   
FOR Object_Created   
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984' ;  
```  
  
### <a name="b-creating-an-event-notification-that-is-database-scoped"></a>B. Crear una notificación de eventos en el ámbito de una base de datos  
 El ejemplo siguiente crea una notificación de eventos en el mismo servicio de destino que el ejemplo anterior. La notificación de eventos se activa después de un evento `ALTER_TABLE` en la base de datos de ejemplo [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
CREATE EVENT NOTIFICATION Notify_ALTER_T1  
ON DATABASE  
FOR ALTER_TABLE  
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984';  
```  
  
### <a name="c-getting-information-about-an-event-notification-that-is-server-scoped"></a>C. Obtener información sobre una notificación de eventos en el ámbito de un servidor  
 El ejemplo siguiente realiza una consulta en la vista de catálogo `sys.server_event_notifications` respecto de metadatos sobre la notificación de eventos `log_ddl1` creada en el ámbito de un servidor.  
  
```sql  
SELECT * FROM sys.server_event_notifications  
WHERE name = 'log_ddl1';  
```  
  
### <a name="d-getting-information-about-an-event-notification-that-is-database-scoped"></a>D. Obtener información sobre una notificación de eventos en el ámbito de una base de datos  
 El ejemplo siguiente realiza una consulta en la vista de catálogo `sys.event_notifications` respecto de metadatos sobre la notificación de eventos `Notify_ALTER_T1` creada en el ámbito de una base de datos.  
  
```sql  
SELECT * FROM sys.event_notifications  
WHERE name = 'Notify_ALTER_T1';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Notificaciones de eventos](../../relational-databases/service-broker/event-notifications.md)   
 [DROP EVENT NOTIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-notification-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.event_notifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md)   
 [sys.server_event_notifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-notifications-transact-sql.md)   
 [sys.events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-events-transact-sql.md)   
 [sys.server_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-events-transact-sql.md)  
  
  
