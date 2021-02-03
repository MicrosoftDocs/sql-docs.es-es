---
description: BEGIN DIALOG CONVERSATION (Transact-SQL)
title: BEGIN DIALOG CONVERSATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DIALOG CONVERSATION
- DIALOG
- BEGIN_DIALOG_TSQL
- BEGIN_DIALOG_CONVERSATION_TSQL
- DIALOG_CONVERSATION_TSQL
- DIALOG_TSQL
- BEGIN DIALOG
- BEGIN DIALOG CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker]
- beginning dialogs
- dialogs [Service Broker], beginning
- BEGIN DIALOG CONVERSATION statement
- cryptography [SQL Server], conversations
- opening conversations
- encryption [SQL Server], conversations
- starting conversations
ms.assetid: 8e814f9d-77c1-4906-b8e4-668a86fc94ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fdf0f646d2df5c8b0c2fc748973378426a0b96b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184460"
---
# <a name="begin-dialog-conversation-transact-sql"></a>BEGIN DIALOG CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Inicia un diálogo de un servicio a otro servicio. Un diálogo es una conversación que proporciona una función de mensajería exactamente una vez por orden entre dos servicios.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
BEGIN DIALOG [ CONVERSATION ] @dialog_handle  
   FROM SERVICE initiator_service_name  
   TO SERVICE 'target_service_name'  
       [ , { 'service_broker_guid' | 'CURRENT DATABASE' }]   
   [ ON CONTRACT contract_name ]  
   [ WITH  
   [  { RELATED_CONVERSATION = related_conversation_handle   
      | RELATED_CONVERSATION_GROUP = related_conversation_group_id } ]   
   [ [ , ] LIFETIME = dialog_lifetime ]   
   [ [ , ] ENCRYPTION = { ON | OFF }  ] ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 **@** _dialog_handle_  
 Se trata de una variable utilizada para almacenar el identificador de diálogo generado por el sistema para el nuevo diálogo devuelto por la instrucción BEGIN DIALOG CONVERSATION. La variable debe ser de tipo **uniqueidentifier**.  
  
 FROM SERVICE *initiator_service_name*  
 Especifica el servicio que inicia el diálogo. El nombre especificado debe ser el nombre de un servicio de la base de datos actual. La cola especificada para el servicio iniciador recibe los mensajes devueltos por el servicio de destino y los mensajes creados por Service Broker para esta conversación.  
  
 TO SERVICE **'** _target_service_name_ **'**  
 Especifica el servicio de destino con el se inicia el diálogo. *target_service_name* es de tipo **nvarchar(256)**. [!INCLUDE[ssSB](../../includes/sssb-md.md)] usa una comparación byte a byte para buscar una coincidencia con la cadena *target_service_name*. Es decir, la comparación distingue mayúsculas de minúsculas y no tiene en cuenta la intercalación actual.  
  
 *service_broker_guid*  
 Especifica la base de datos que hospeda el servicio de destino. Si hay varias bases de datos que hospedan una instancia del servicio de destino, puede comunicarse con una base de datos específica si proporciona *service_broker_guid*.  
  
 *service_broker_guid* es de tipo **nvarchar(128)**. Para buscar el *service_broker_guid* de una base de datos, ejecute la siguiente consulta en la base de datos:  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID() ;  
```  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 **'** CURRENT DATABASE **'**  
 Especifica que la conversación debe usar el *service_broker_guid* de la base de datos actual.  
  
 ON CONTRACT *contract_name*  
 Especifica el contrato que sigue la conversación. El contrato debe existir en la base de datos actual. Si el servicio de destino no acepta conversaciones nuevas en el contrato especificado, [!INCLUDE[ssSB](../../includes/sssb-md.md)] devuelve un mensaje de error en la conversación. Si se omite esta cláusula, la conversación sigue el contrato llamado **DEFAULT**.  
  
 RELATED_CONVERSATION **=** _related_conversation_handle_  
 Especifica el grupo de conversación existente al que se agrega el diálogo nuevo. Si esta cláusula está presente, el diálogo nuevo pertenece al mismo grupo de conversación que el diálogo especificado por *related_conversation_handle*. *related_conversation_handle* debe ser de un tipo que se pueda convertir implícitamente al tipo **uniqueidentifier**. La instrucción genera un error si *related_conversation_handle* no hace referencia a un diálogo existente.  
  
 RELATED_CONVERSATION_GROUP **=** _related_conversation_group_id_  
 Especifica el grupo de conversación existente al que se agrega el diálogo nuevo. Si esta cláusula está presente, el nuevo diálogo se agrega al grupo de conversación especificado por *related_conversation_group_id*. *related_conversation_group_id* debe ser de un tipo que se pueda convertir implícitamente al tipo **uniqueidentifier**. Si *related_conversation_group_id* no hace referencia a un grupo de conversación existente, Service Broker crea un grupo de conversación nuevo con la especificación de *related_conversation_group_id* y relaciona el diálogo nuevo con dicho grupo de conversación.  
  
 LIFETIME **=** _dialog_lifetime_  
 Especifica la cantidad máxima de tiempo durante el que el diálogo permanece abierto. Para que el diálogo finalice correctamente, ambos extremos deben finalizar explícitamente el diálogo antes del final de la vigencia. El valor de *dialog_lifetime* se debe expresar en segundos. LIFETIME es de tipo **int**. Si no se especifica la cláusula LIFETIME, la vigencia del diálogo equivale al valor máximo del tipo de datos **int**.  
  
 ENCRYPTION  
 Especifica si se deben cifrar o no los mensajes enviados y recibidos en este diálogo en caso de enviarse fuera de una instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un diálogo que se debe cifrar es un *diálogo protegido*. Si no se han configurado ENCRYPTION = ON y los certificados necesarios para admitir el cifrado, [!INCLUDE[ssSB](../../includes/sssb-md.md)] devuelve un mensaje de error en la conversación. Si ENCRYPTION = OFF, el cifrado se usa si se ha configurado un enlace de servicio remoto para *target_service_name*; en caso contrario, los mensajes se envían sin cifrar. Si no está presente esta cláusula, el valor predeterminado es ON.  
  
> [!NOTE]  
>  Los mensajes intercambiados con servicios en la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se cifran en ningún caso. No obstante, la clave maestra de la base de datos y los certificados de cifrado siguen siendo necesarios para las conversaciones que utilizan el cifrado si los servicios para la conversación están en distintas bases de datos. Esto permite que las conversaciones continúen en caso de que una de las bases de datos se mueva a otra instancia mientras la conversación está en curso.  
  
## <a name="remarks"></a>Observaciones  
 Todos los mensajes forman parte de una conversación. Por lo tanto, el servicio iniciador debe iniciar una conversación con el servicio de destino antes de enviar un mensaje al servicio de destino. La información especificada en la instrucción BEGIN DIALOG CONVERSATION es similar a la dirección de una carta; [!INCLUDE[ssSB](../../includes/sssb-md.md)] utiliza la información para entregar mensajes al servicio correcto. El servicio especificado en la cláusula TO SERVICE es la dirección a la que se envían los mensajes. El servicio especificado en la cláusula FROM SERVICE es la dirección de retorno utilizada para responder a los mensajes.  
  
 El destino de una conversación no necesita llamar a BEGIN DIALOG CONVERSATION. [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea una conversación en la base de datos de destino cuando llega del iniciador el primer mensaje de la conversación.  
  
 Al iniciarse un diálogo se crea un extremo de conversación en la base de datos para el servicio iniciador, pero no se crea una conexión de red a la instancia que hospeda el servicio de destino. [!INCLUDE[ssSB](../../includes/sssb-md.md)] no establecerá comunicación con el destino del diálogo hasta que se haya enviado el primer mensaje.  
  
 Si la instrucción BEGIN DIALOG CONVERSATION no especifica ninguna conversación o ningún grupo de conversación relacionado, [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea otro grupo de conversación para la conversación nueva.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] no permite agrupaciones arbitrarias de conversaciones. Todas las conversaciones de un grupo de conversación deben tener el servicio especificado en la cláusula FROM como iniciador o como destino de la conversación.  
  
 El comando BEGIN DIALOG CONVERSATION bloquea el grupo de conversación que contiene la devolución de *dialog_handle*. Si el comando incluye una cláusula RELATED_CONVERSATION_GROUP, el grupo de conversación para *dialog_handle* es el grupo de conversación especificado en el parámetro *related_conversation_group_id*. Si el comando incluye una cláusula RELATED_CONVERSATION, el grupo de conversación para *dialog_handle* es el grupo de conversación asociado al *related_conversation_handle* especificado.  
  
 BEGIN DIALOG CONVERSATION no tiene validez en una función definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Para iniciar un diálogo, el usuario actual debe tener el permiso RECEIVE en la cola para el servicio especificado en la cláusula FROM del comando y el permiso REFERENCES para el contrato especificado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-beginning-a-dialog"></a>A. Iniciar un diálogo  
 El ejemplo siguiente comienza una conversación del diálogo y almacena un identificador para el diálogo en `@dialog_handle.` El servicio `//Adventure-Works.com/ExpenseClient` es el iniciador para el diálogo, y el servicio `//Adventure-Works.com/Expenses` es el destino del diálogo. El diálogo sigue el contrato `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
### <a name="b-beginning-a-dialog-with-an-explicit-lifetime"></a>B. Iniciar un diálogo con una vigencia explícita  
 En el ejemplo siguiente se inicia una conversación de diálogo y se almacena un identificador para el diálogo en `@dialog_handle`. El servicio `//Adventure-Works.com/ExpenseClient` es el iniciador del diálogo y el servicio `//Adventure-Works.com/Expenses` es el destino del diálogo. El diálogo sigue el contrato `//Adventure-Works.com/Expenses/ExpenseSubmission`. Si el diálogo no se ha cerrado mediante el comando END CONVERSATION en `60` segundos, el broker finaliza el diálogo con un error.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH LIFETIME = 60 ;  
```  
  
### <a name="c-beginning-a-dialog-with-a-specific-broker-instance"></a>C. Iniciar un diálogo con una instancia de broker específica  
 En el ejemplo siguiente se inicia una conversación de diálogo y se almacena un identificador para el diálogo en `@dialog_handle`. El servicio `//Adventure-Works.com/ExpenseClient` es el iniciador del diálogo y el servicio `//Adventure-Works.com/Expenses` es el destino del diálogo. El diálogo sigue el contrato `//Adventure-Works.com/Expenses/ExpenseSubmission`. El broker enruta mensajes de este diálogo al broker identificado por el GUID `a326e034-d4cf-4e8b-8d98-4d7e1926c904.`  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses',   
              'a326e034-d4cf-4e8b-8d98-4d7e1926c904'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
### <a name="d-beginning-a-dialog-and-relating-it-to-an-existing-conversation-group"></a>D. Iniciar un diálogo y relacionarlo con un grupo de conversación existente  
 En el ejemplo siguiente se inicia una conversación de diálogo y se almacena un identificador para el diálogo en `@dialog_handle`. El servicio `//Adventure-Works.com/ExpenseClient` es el iniciador del diálogo y el servicio `//Adventure-Works.com/Expenses` es el destino del diálogo. El diálogo sigue el contrato `//Adventure-Works.com/Expenses/ExpenseSubmission`. El broker asocia el diálogo al grupo de conversación identificado por `@conversation_group_id` en lugar de crear un grupo de conversación.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_group_id = <retrieve conversation group ID from database>  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH RELATED_CONVERSATION_GROUP = @conversation_group_id ;  
```  
  
### <a name="e-beginning-a-dialog-with-an-explicit-lifetime-and-relating-the-dialog-to-an-existing-conversation"></a>E. Iniciar un diálogo con una vigencia explícita y relacionarlo con una conversación existente  
 En el ejemplo siguiente se inicia una conversación de diálogo y se almacena un identificador para el diálogo en `@dialog_handle`. El servicio `//Adventure-Works.com/ExpenseClient` es el iniciador del diálogo y el servicio `//Adventure-Works.com/Expenses` es el destino del diálogo. El diálogo sigue el contrato `//Adventure-Works.com/Expenses/ExpenseSubmission`. El nuevo diálogo pertenece al mismo grupo de conversación al que pertenece `@existing_conversation_handle`. Si el diálogo no se ha cerrado mediante el comando END CONVERSATION en `600` segundos, [!INCLUDE[ssSB](../../includes/sssb-md.md)] finaliza el diálogo con un error.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER  
DECLARE @existing_conversation_handle UNIQUEIDENTIFIER  
  
SET @existing_conversation_handle = <retrieve conversation handle from database>  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH RELATED_CONVERSATION = @existing_conversation_handle  
   LIFETIME = 600 ;  
```  
  
### <a name="f-beginning-a-dialog-with-optional-encryption"></a>F. Iniciar un diálogo con cifrado opcional  
 En el ejemplo siguiente se inicia un diálogo y se almacena un identificador para el diálogo en `@dialog_handle`. El servicio `//Adventure-Works.com/ExpenseClient` es el iniciador del diálogo y el servicio `//Adventure-Works.com/Expenses` es el destino del diálogo. El diálogo sigue el contrato `//Adventure-Works.com/Expenses/ExpenseSubmission`. La conversación de este ejemplo permite que el mensaje viaje por la red sin cifrado en caso de no estar disponible.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH ENCRYPTION = OFF ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [BEGIN CONVERSATION TIMER &#40;Transact-SQL&#41;](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [MOVE CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/move-conversation-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
