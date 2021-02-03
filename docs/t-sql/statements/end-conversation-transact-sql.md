---
description: END CONVERSATION (Transact-SQL)
title: END CONVERSATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- END DIALOG
- END CONVERSATION
- END_DIALOG_TSQL
- END_CONVERSATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- errors [Service Broker], conversations
- dialogs [Service Broker], ending
- closing conversations
- END CONVERSATION statement
- conversations [Service Broker], ending
- ending conversations [SQL Server]
ms.assetid: 4415a126-cd22-4a5e-b84a-d8c68515c83b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 83d0acb45d13442e2c538878d3336688513e6519
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182999"
---
# <a name="end-conversation-transact-sql"></a>END CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Finaliza un extremo de una conversación existente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
END CONVERSATION conversation_handle  
   [   [ WITH ERROR = failure_code DESCRIPTION = 'failure_text' ]  
     | [ WITH CLEANUP ]  
    ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *conversation_handle*  
 Es el identificador de conversación que va a finalizar.  
  
 WITH ERROR =*failure_code*  
 Es el código de error. El *failure_code* es de tipo **int**. El código de error es un código definido por el usuario que se incluye en el mensaje de error enviado al otro extremo de la conversación. El código de error debe ser mayor que 0.  
  
 DESCRIPTION =*failure_text*  
 Es el mensaje de error. El *failure_text* es de tipo **nvarchar (3000)**. El texto del error es texto definido por el usuario que se incluye en el mensaje de error enviado al otro extremo de la conversación.  
  
 WITH CLEANUP  
 Quita todos los mensajes y todas las entradas de la vista de catálogo para un lado de una conversación que no se puede completar con normalidad. No se le informa de la limpieza al otro lado de la conversación. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quita el extremo de conversación, todos los mensajes de la conversación que estén en la cola de transmisión y todos los mensajes de la conversación que estén en la cola de servicio. Los administradores pueden utilizar esta opción para quitar las conversaciones que no se completan con normalidad. Por ejemplo, si el servicio remoto se ha quitado de manera permanente, un administrador puede utilizar WITH CLEANUP para quitar las conversaciones con ese servicio. No use WITH CLEANUP en el código de una aplicación de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Si se ejecuta END CONVERSATION WITH CLEANUP antes de que el extremo receptor confirme la recepción de un mensaje, el extremo de envío enviará el mensaje de nuevo. Esto podría volver a ejecutar el cuadro de diálogo.  
  
## <a name="remarks"></a>Observaciones  
 Al finalizar una conversación, se bloquea el grupo de conversaciones al que pertenece el *conversation_handle* suministrado. Cuando finaliza una conversación, [!INCLUDE[ssSB](../../includes/sssb-md.md)] quita todos los mensajes de la conversación de la cola de servicio.  
  
 Después de que una conversación finaliza, ninguna aplicación puede enviar o recibir mensajes para esa conversación. Los dos participantes de una conversación deben llamar a END CONVERSATION para que la conversación finalice. Si [!INCLUDE[ssSB](../../includes/sssb-md.md)] no ha recibido ningún mensaje de fin de diálogo ni ningún mensaje de error del otro participante de la conversación, [!INCLUDE[ssSB](../../includes/sssb-md.md)] notifica al otro participante que la conversación ha finalizado. En este caso, aunque el identificador de conversación ya no es válido, el extremo de la conversación sigue activo hasta que la instancia que hospeda el servicio remoto confirma el mensaje.  
  
 Si [!INCLUDE[ssSB](../../includes/sssb-md.md)] no ha procesado ningún mensaje de fin de diálogo ni ningún mensaje de error para la conversación, [!INCLUDE[ssSB](../../includes/sssb-md.md)] notifica al extremo remoto que la conversación ha finalizado. Los mensajes que [!INCLUDE[ssSB](../../includes/sssb-md.md)] envía al servicio remoto dependen de las opciones especificadas:  
  
-   Si la conversación finaliza sin errores y la conversación con el servicio remoto sigue activa, [!INCLUDE[ssSB](../../includes/sssb-md.md)] envía un mensaje de tipo `https://schemas.microsoft.com/SQL/ServiceBroker/EndDialog` al servicio remoto. [!INCLUDE[ssSB](../../includes/sssb-md.md)] agrega este mensaje a la cola de transmisión de acuerdo con el orden de la conversación. Antes de enviar este mensaje, [!INCLUDE[ssSB](../../includes/sssb-md.md)] envía todos los mensajes de esta conversación que se encuentren en la cola de transmisión.  
  
-   Si la conversación finaliza con un error y la conversación con el servicio remoto sigue activa, [!INCLUDE[ssSB](../../includes/sssb-md.md)] envía un mensaje de tipo `https://schemas.microsoft.com/SQL/ServiceBroker/Error` al servicio remoto. [!INCLUDE[ssSB](../../includes/sssb-md.md)] quita los demás mensajes de esta conversación que se encuentren en la cola de transmisión.  
  
-   La cláusula WITH CLEANUP permite al administrador de la base de datos quitar las conversaciones que no finalizan con normalidad. Esta opción quita todos los mensajes y todas las entradas de la vista de catálogo para la conversación. Observe que, en este caso, el extremo remoto de la conversación no recibe ninguna indicación de que la conversación ha finalizado y puede que no reciba los mensajes enviados por una aplicación que todavía no se hayan transmitido a través de la red. Evite esta opción a menos que la conversación no pueda finalizar con normalidad.  
  
 Una vez finalizada una conversación, una instrucción SEND de [!INCLUDE[tsql](../../includes/tsql-md.md)] que especifique el identificador de conversación provocará un error de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Si los mensajes de esta conversación proceden del otro extremo de la conversación, [!INCLUDE[ssSB](../../includes/sssb-md.md)] los descarta.  
  
 Si una conversación finaliza pero el servicio remoto todavía tiene mensajes sin enviar para la conversación, el servicio remoto quita los mensajes no enviados. Esto no se considera un error, por lo que el servicio remoto no recibirá ninguna notificación de que se han quitado los mensajes.  
  
 Los códigos de error especificados en la cláusula WITH ERROR deben ser números positivos. Los números negativos se reservan para los mensajes de error de [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 END CONVERSATION no es válido en una función definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Para finalizar una conversación activa, el usuario actual debe ser el propietario de la conversación, miembro del rol fijo de servidor sysadmin o miembro del rol fijo de base de datos db_owner.  
  
 Los miembros del rol fijo de servidor sysadmin o del rol fijo de base de datos db_owner pueden utilizar WITH CLEANUP para quitar los metadatos de una conversación que ya ha finalizado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-ending-a-conversation"></a>A. Finalizar una conversación  
 El ejemplo siguiente finaliza el cuadro de diálogo que especifica `@dialog_handle`.  
  
```sql 
END CONVERSATION @dialog_handle ;  
```  
  
### <a name="b-ending-a-conversation-with-an-error"></a>B. Finalizar una conversación con un error  
 En el ejemplo siguiente se finaliza con un error el cuadro de diálogo especificado por `@dialog_handle` si la instrucción que se está procesando genera un error. Tenga en cuenta que éste es un planteamiento simplista del control de errores y puede no ser adecuado para algunas aplicaciones.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER,  
        @ErrorSave INT,  
        @ErrorDesc NVARCHAR(100) ;  
BEGIN TRANSACTION ;  
  
<receive and process message>  
  
SET @ErrorSave = @@ERROR ;  
  
IF (@ErrorSave <> 0)  
  BEGIN  
      ROLLBACK TRANSACTION ;  
      SET @ErrorDesc = N'An error has occurred.' ;  
      END CONVERSATION @dialog_handle   
      WITH ERROR = @ErrorSave DESCRIPTION = @ErrorDesc ;  
  END  
ELSE  
  
COMMIT TRANSACTION ;  
```  
  
### <a name="c-cleaning-up-a-conversation-that-cannot-complete-normally"></a>C. Limpiar una conversación que no puede finalizar con normalidad  
 El ejemplo siguiente finaliza el cuadro de diálogo que especifica `@dialog_handle`. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quita inmediatamente todos los mensajes de la cola de servicio y la cola de transmisión, sin notificárselo al servicio remoto. Dado que, cuando un cuadro de diálogo finaliza con la opción de limpieza, el servicio remoto no recibe ninguna notificación, esta opción se debe usar únicamente cuando el servicio remoto no esté disponible para recibir un mensaje **EndDialog** o **Error**.  
  
```sql  
END CONVERSATION @dialog_handle WITH CLEANUP ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [BEGIN CONVERSATION TIMER &#40;Transact-SQL&#41;](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
