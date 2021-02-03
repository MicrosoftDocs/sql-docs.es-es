---
description: BEGIN CONVERSATION TIMER (Transact-SQL)
title: BEGIN CONVERSATION TIMER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONVERSATION
- BEGIN_CONVERSATION_TSQL
- TIMER_TSQL
- TIMER
- CONVERSATION TIMER
- CONVERSATION_TIMER_TSQL
- BEGIN CONVERSATION TIMER
- BEGIN_CONVERSATION_TIMER_TSQL
- CONVERSATION_TSQL
- BEGIN CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CONVERSATION TIMER statement
- DialogTimer message
- dialogs [Service Broker], beginning
- timeouts [Service Broker]
- timer messages [Service Broker]
- conversations [Service Broker], timers
- starting timers [Service Broker]
- https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer message
ms.assetid: 98e49b3f-a38f-4180-8171-fa9cb30db4cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8212a0d9589c325eb716ea2491fd7c78551929fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184484"
---
# <a name="begin-conversation-timer-transact-sql"></a>BEGIN CONVERSATION TIMER (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Inicia un temporizador. Cuando expira el tiempo de espera, [!INCLUDE[ssSB](../../includes/sssb-md.md)] coloca un mensaje del tipo `https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer` en la cola local para la conversación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
BEGIN CONVERSATION TIMER ( conversation_handle )  
   TIMEOUT = timeout   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 BEGIN CONVERSATION TIMER **(** _conversation\_handle_ **)**  
 Especifica la conversación cuyo tiempo se va controlar. El *identificador_de_conversación* debe ser de tipo **uniqueidentifier**.  
  
 TIMEOUT  
 Especifica la cantidad de tiempo en segundos que se va esperar antes de colocar el mensaje en la cola.  
  
## <a name="remarks"></a>Observaciones  
 Un temporizador de conversación proporciona a la aplicación un método para recibir un mensaje en una conversación tras una cantidad de tiempo específica. Si se llama a BEGIN CONVERSATION TIMER en una conversación antes de que el temporizador haya expirado, el tiempo de espera se establece en el valor nuevo. A diferencia de la duración de la conversación, cada parte de la conversación cuenta con un temporizador de conversación independiente. El mensaje **DialogTimer** llega a la cola local sin afectar a la parte remota de la conversación. Por lo tanto, una aplicación puede utilizar un mensaje del temporizador con cualquier fin.  
  
 Por ejemplo, puede utilizar el temporizador de conversación para evitar que la aplicación espere durante demasiado tiempo una respuesta atrasada. Si tiene previsto que la aplicación complete el diálogo en 30 segundos, debe establecer el temporizador de conversación para dicho diálogo en 60 segundos (30 segundos más un período de gracia de 30 segundos). Si el diálogo sigue abierto después de 60 segundos, la aplicación recibe un mensaje de tiempo de espera agotado en la cola para dicho diálogo.  
  
 O bien, la aplicación puede utilizar un temporizador de conversación para solicitar la activación en un momento concreto. Por ejemplo, puede crear un servicio que informe del número de conexiones activas cada pocos minutos o un servicio que informe del número de pedidos de compra abiertos cada noche. El servicio establece que el temporizador de conversación expire en el momento deseado; si el temporizador expira, [!INCLUDE[ssSB](../../includes/sssb-md.md)] envía un mensaje **DialogTimer**. El mensaje **DialogTimer** hace que [!INCLUDE[ssSB](../../includes/sssb-md.md)] inicie el procedimiento almacenado de activación para la cola. El procedimiento almacenado envía un mensaje al servicio remoto y reinicia el temporizador de conversación.  
  
 BEGIN CONVERSATION TIMER no tiene validez en una función definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 El permiso para configurar un temporizador de conversación se concede de forma predeterminada a los usuarios que tienen permisos SEND en el servicio para la conversación, a los miembros del rol fijo de servidor **sysadmin** y a los miembros del rol fijo de base de datos **db_owner**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se establece un tiempo de espera de dos minutos en el diálogo identificado por `@dialog_handle`.  
  
```sql 
-- @dialog_handle is of type uniqueidentifier and  
-- contains a valid conversation handle.  
  
BEGIN CONVERSATION TIMER (@dialog_handle)  
TIMEOUT = 120 ;  
```  
  
## <a name="see-also"></a>Vea también  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)  
  
  
