---
description: RECEIVE (Transact-SQL)
title: RECEIVE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RECEIVE_TSQL
- RECEIVE
dev_langs:
- TSQL
helpviewer_keywords:
- queues [Service Broker], message retrieval
- messages [Service Broker], retrieving
- RECEIVE statement
- receiving messages
- retrieving messages
ms.assetid: 878c6c14-37ab-4b87-9854-7f8f42bac7dd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e1665c7fa5a0cfcbd1d3d14fc83dbcfc82697cef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187216"
---
# <a name="receive-transact-sql"></a>RECEIVE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Recupera uno o más mensajes de una cola. En función del valor de retención de la cola, se quita el mensaje de la cola o se actualiza el estado del mensaje de la cola.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
[ WAITFOR ( ]  
    RECEIVE [ TOP ( n ) ]   
        <column_specifier> [ ,...n ]  
        FROM <queue>  
        [ INTO table_variable ]  
        [ WHERE {  conversation_handle = conversation_handle  
                 | conversation_group_id = conversation_group_id } ]  
[ ) ] [ , TIMEOUT timeout ]  
[ ; ]  
  
<column_specifier> ::=  
{    *   
  |  { column_name | [ ] expression } [ [ AS ] column_alias ]  
  |  column_alias = expression   
}     [ ,...n ]   
  
<queue> ::=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 WAITFOR  
 Especifica que la instrucción RECEIVE espera a que llegue un mensaje a la cola en caso de no haber ningún mensaje.  
  
 TOP( *n* )  
 Especifica el número máximo de mensajes que se van a devolver. Si no se especifica esta cláusula, se devuelven todos los mensajes que cumplen los criterios de la instrucción.  
  
 \*  
 Especifica que el conjunto de resultados contiene todas las columnas de la cola.  
  
 *column_name*  
 Nombre de la columna que se va a incluir en el conjunto de resultados.  
  
 *expression*  
 Nombre de columna, constante, función o combinación de nombres de columnas, constantes y funciones conectados mediante un operador.  
  
 *column_alias*  
 Nombre alternativo para reemplazar el nombre de la columna en el conjunto de resultados.  
  
 FROM  
 Especifica la cola que contiene los mensajes que se van a recuperar.  
  
 *database_name*  
 Nombre de la base de datos que contiene la cola desde la que se van a recibir los mensajes. Si no se proporciona *database_name*, el valor predeterminado es la base de datos actual.  
  
 *schema_name*  
 Nombre del esquema al que pertenece la cola desde la que se van a recibir los mensajes. Si no se proporciona *schema_name*, se usa el esquema predeterminado del usuario actual.  
  
 *queue_name*  
 Nombre de la cola desde la que se reciben los mensajes.  
  
 INTO *table_variable*  
 Especifica la variable de tabla en la que RECEIVE coloca los mensajes. La variable de tabla debe tener el mismo número de columnas que los mensajes. El tipo de datos de cada columna en la variable de tabla debe ser implícitamente convertible al tipo de datos de la columna correspondiente en los mensajes. Si no se especifica INTO, los mensajes se devuelven como un conjunto de resultados.  
  
 WHERE  
 Especifica la conversación o grupo de conversación para los mensajes recibidos. Si se omite, se devuelven los mensajes del siguiente grupo de conversación disponible.  
  
 conversation_handle = *conversation_handle*  
 Especifica la conversación para los mensajes recibidos. El valor de *conversation_handle* proporcionado debe ser un **uniqueidentifier** o un tipo que pueda convertirse en **uniqueidentifier**.  
  
 conversation_group_id = *conversation_group_id*  
 Especifica el grupo de conversación para los mensajes recibidos. El valor de *conversation_group_id* proporcionado debe ser un **uniqueidentifier** o un tipo convertible en **uniqueidentifier**.  
  
 TIMEOUT *tiempo_de_espera*  
 Especifica el tiempo, en milisegundos, durante el que la instrucción espera un mensaje. Esta cláusula solo se puede utilizar con la cláusula WAITFOR. Si no se especifica esta cláusula o el tiempo de espera es -**1**, el tiempo de espera es ilimitado. Si se agota el tiempo de espera, RECEIVE devuelve un conjunto de resultados vacío.  
  
## <a name="remarks"></a>Comentarios  
  
> [!IMPORTANT]  
>  Si la instrucción RECEIVE no es la primera instrucción de un lote o de un procedimiento almacenado, la instrucción anterior deberá finalizar con un punto y coma (;).  
  
 La instrucción RECEIVE lee los mensajes de una cola y devuelve un conjunto de resultados. El conjunto de resultados consta de cero o más filas, cada una de las cuales contiene un mensaje. Si no se usa la cláusula INTO y *column_specifier* no asigna valores a las variables locales, la instrucción devuelve un conjunto de resultados al programa que realiza la llamada.  
  
 Los mensajes devueltos por la instrucción RECEIVE pueden ser de distintos tipos de mensaje. Las aplicaciones pueden usar la columna **message_type_name** para enrutar cada mensaje al código que administra el tipo de mensaje asociado. Hay dos clases de tipos de mensaje:  
  
-   Tipos de mensaje definidos por la aplicación que se crearon utilizando la instrucción CREATE MESSAGE TYPE. El contrato de [!INCLUDE[ssSB](../../includes/sssb-md.md)] que se especifica para la conversación define el conjunto de tipos de mensaje definidos por la aplicación que se admiten en una conversación.  
  
-   Mensajes de sistema de [!INCLUDE[ssSB](../../includes/sssb-md.md)] que devuelven información de estado o error.  
  
 La instrucción RECEIVE quita los mensajes recibidos de la cola a menos que la cola especifique la retención de mensajes. Si el valor RETENTION para la cola es ON, la instrucción RECEIVE actualiza la columna **status** a **0** y deja los mensajes en la cola. Si se revierte una transacción que contiene la instrucción RECEIVE, también se revierten todos los cambios de la cola de la transacción y se devuelven los mensajes a la cola.  
  
 Todos los mensajes devueltos por una instrucción RECEIVE pertenecen al mismo grupo de conversación. La instrucción RECEIVE bloquea el grupo de conversación para los mensajes devueltos hasta que se termina la transacción que contiene la instrucción. Una instrucción RECEIVE devuelve mensajes que tienen un **status** de **1**. El conjunto de resultados devuelto por la instrucción RECEIVE se ordena de forma implícita:  
  
-   Si los mensajes de varias conversaciones cumplen las condiciones de la cláusula WHERE, la instrucción RECEIVE devuelve todos los mensajes de una conversación antes de devolver los mensajes para otras conversaciones. Las conversaciones se procesan en orden de nivel de prioridad descendente.  
  
-   Para una conversación determinada, una instrucción RECEIVE devuelve los mensajes en orden **message_sequence_number** ascendente.  
  
 La cláusula WHERE de la instrucción RECEIVE solo puede contener una condición de búsqueda que use **conversation_handle** o **conversation_group_id**. La condición de búsqueda no puede contener una o más columnas de las demás columnas de la cola. **conversation_handle** o **conversation_group_id** no pueden ser una expresión. El conjunto de mensajes que se devuelve depende de las condiciones que se especifican en la cláusula WHERE:  
  
-   Si se especifica **conversation_handle**, RECEIVE devuelve todos los mensajes de la conversación especificada que están disponibles en la cola.  
  
-   Si se especifica **conversation_group_id**, RECEIVE devuelve todos los mensajes que están disponibles en la cola de cualquier conversación que forme parte del grupo de conversación especificado.  
  
-   Si no hay ninguna cláusula WHERE, RECEIVE determina qué grupo de conversación:  
  
    -   Tiene uno o más mensajes en la cola.  
  
    -   No ha sido bloqueado por otra instrucción RECEIVE.  
  
    -   Tiene el nivel de prioridad más alto de todos los grupos de conversación que cumplen estos criterios.  
  
     A continuación, RECEIVE devuelve todos los mensajes disponibles en la cola de cualquier conversación que forme parte del grupo de conversación seleccionado.  
  
 Si el identificador de conversación o el identificador del grupo de conversación especificado en la cláusula WHERE no existe o no está asociado a la cola especificada, la instrucción RECEIVE devuelve un error.  
  
 Si la cola especificada en la instrucción RECEIVE tiene el estado de cola establecido en OFF, la instrucción genera un error de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Si se especifica la cláusula WAITFOR, la instrucción espera a que se agote el tiempo de espera especificado o hasta que haya un conjunto de resultados disponible. Si se quita la cola o su estado se establece en OFF mientras la instrucción está esperando, dicha instrucción devuelve un error de inmediato. Si la instrucción RECEIVE especifica un grupo de conversación o un identificador de conversación y se quita o se mueve el servicio para dicha conversación a otra cola, la instrucción RECEIVE notifica de un error de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 RECEIVE no tiene validez en una función definida por el usuario.  
  
 La instrucción RECEIVE no tiene ninguna prevención de colapso de la prioridad. Si una instrucción RECEIVE única bloquea un grupo de conversación y recupera muchos mensajes de conversaciones de prioridad baja, no se podrá recibir ningún mensaje de las conversaciones de prioridad alta del grupo. Para evitar esto, cuando recupere mensajes de conversaciones de prioridad baja, utilice la cláusula TOP para limitar el número de mensajes recuperados por cada instrucción RECEIVE.  
  
## <a name="queue-columns"></a>Poner las columnas en una cola  
 La siguiente tabla enumera las columnas que contiene una cola:  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**status**|**tinyint**|Estado del mensaje. Para los mensajes devueltos por el comando RECEIVE, el estado es siempre **0**. Los mensajes de la cola pueden contener uno de los siguiente valores:<br /><br /> **0**=Listo **1**=Mensaje recibido **2**=Sin completar **3**=Mensaje enviado retenido|  
|**priority**|**tinyint**|Nivel de prioridad de la conversación que se aplica al mensaje.|  
|**queuing_order**|**bigint**|Número de orden del mensaje en la cola.|  
|**conversation_group_id**|**uniqueidentifier**|Identificador para el grupo de conversación al que pertenece este mensaje.|  
|**conversation_handle**|**uniqueidentifier**|Identificador para la conversación de la que forma parte este mensaje.|  
|**message_sequence_number**|**bigint**|Número de secuencia del mensaje en la conversación.|  
|**service_name**|**nvarchar(512)**|Nombre del servicio al que se destina la conversación.|  
|**service_id**|**int**|Identificador de objeto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del servicio al que se destina la conversación.|  
|**service_contract_name**|**nvarchar(256)**|Nombre del contrato por el que se rige la conversación.|  
|**service_contract_id**|**int**|Identificador de objeto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del contrato por el que se rige la conversación.|  
|**message_type_name**|**nvarchar(256)**|Nombre del tipo de mensaje que describe el formato del mensaje. Los mensajes pueden ser tipos de mensaje de aplicación o mensajes del sistema del agente.|  
|**message_type_id**|**int**|Identificador de objeto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del tipo de mensaje que describe el mensaje.|  
|**validation**|**nchar(2)**|Validación utilizada para el mensaje.<br /><br /> **E**=Vacío **N**=Ninguno **X**=XML|  
|**message_body**|**varbinary(MAX)**|Contenido del mensaje.|  
  
## <a name="permissions"></a>Permisos  
 Para recibir un mensaje, el usuario actual debe tener el permiso RECEIVE en la cola.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-receiving-all-columns-for-all-messages-in-a-conversation-group"></a>A. Recibir todas las columnas para todos los mensajes en un grupo de conversación  
 En el siguiente ejemplo se reciben todos los mensajes disponibles para el próximo grupo de conversación disponible desde la cola `ExpenseQueue`. La instrucción devuelve los mensajes como un conjunto de resultados.  
  
```sql  
RECEIVE * FROM ExpenseQueue ;  
```  
  
### <a name="b-receiving-specified-columns-for-all-messages-in-a-conversation-group"></a>B. Recibir las columnas especificadas para todos los mensajes en un grupo de conversación  
 En el siguiente ejemplo se reciben todos los mensajes disponibles para el próximo grupo de conversación disponible desde la cola `ExpenseQueue`. La instrucción devuelve los mensajes como un conjunto de resultados que contiene las columnas `conversation_handle`, `message_type_name` y `message_body`.  
  
```sql  
RECEIVE conversation_handle, message_type_name, message_body  
FROM ExpenseQueue ;  
```  
  
### <a name="c-receiving-the-first-available-message-in-the-queue"></a>C. Recibir el primer mensaje disponible en la cola  
 En el siguiente ejemplo se recibe el primer mensaje disponible en la cola `ExpenseQueue` como un conjunto de resultados.  
  
```sql  
RECEIVE TOP (1) * FROM ExpenseQueue ;  
```  
  
### <a name="d-receiving-all-messages-for-a-specified-conversation"></a>D. Recibir todos los mensajes para una conversación especificada  
 En el siguiente ejemplo se reciben todos los mensajes disponibles para la conversación especificada desde la cola `ExpenseQueue` como un conjunto de resultados.  
  
```sql  
DECLARE @conversation_handle UNIQUEIDENTIFIER ;  
  
SET @conversation_handle = <retrieve conversation from database> ;  
  
RECEIVE *  
FROM ExpenseQueue  
WHERE conversation_handle = @conversation_handle ;  
```  
  
### <a name="e-receiving-messages-for-a-specified-conversation-group"></a>E. Recibir mensajes para un grupo de conversación especificado  
 En el siguiente ejemplo se reciben todos los mensajes disponibles para el grupo de conversación especificado desde la cola `ExpenseQueue` como un conjunto de resultados.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_group_id =   
    <retrieve conversation group ID from database> ;  
  
RECEIVE *  
FROM ExpenseQueue  
WHERE conversation_group_id = @conversation_group_id ;  
```  
  
### <a name="f-receiving-into-a-table-variable"></a>F. Recibir en una variable de tabla  
 En el siguiente ejemplo se reciben todos los mensajes disponibles para el grupo de conversación especificado desde la cola `ExpenseQueue` en una variable de tabla.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
DECLARE @procTable TABLE(  
     service_instance_id UNIQUEIDENTIFIER,  
     handle UNIQUEIDENTIFIER,  
     message_sequence_number BIGINT,  
     service_name NVARCHAR(512),  
     service_contract_name NVARCHAR(256),  
     message_type_name NVARCHAR(256),  
     validation NCHAR,  
     message_body VARBINARY(MAX)) ;  
  
SET @conversation_group_id = <retrieve conversation group ID from database> ;  
  
RECEIVE TOP (1)  
    conversation_group_id,  
    conversation_handle,  
    message_sequence_number,  
    service_name,  
    service_contract_name,  
    message_type_name,  
    validation,  
    message_body  
FROM ExpenseQueue  
INTO @procTable  
WHERE conversation_group_id = @conversation_group_id ;  
```  
  
### <a name="g-receiving-messages-and-waiting-indefinitely"></a>G. Recibir mensajes y esperar indefinidamente  
 En el siguiente ejemplo se reciben todos los mensajes disponibles para el próximo grupo de conversación disponible en la cola `ExpenseQueue`. La instrucción espera hasta que se encuentre disponible un mensaje como mínimo y devuelve a continuación un conjunto de resultados que contiene todas las columnas de mensajes.  
  
```sql  
WAITFOR (  
    RECEIVE *  
    FROM ExpenseQueue) ;  
```  
  
### <a name="h-receiving-messages-and-waiting-for-a-specified-interval"></a>H. Recibir mensajes y esperar durante un intervalo especificado  
 En el siguiente ejemplo se reciben todos los mensajes disponibles para el próximo grupo de conversación disponible en la cola `ExpenseQueue`. La instrucción espera 60 segundos o hasta que está disponible un mensaje como mínimo (lo que suceda en primer lugar). La instrucción devuelve un conjunto de resultados que contiene todas las columnas de mensajes, si hay como mínimo un mensaje disponible. En caso contrario, la instrucción devuelve un conjunto de resultados vacío.  
  
```sql  
WAITFOR (  
    RECEIVE *  
    FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="i-receiving-messages-modifying-the-type-of-a-column"></a>I. Recibir mensajes y modificar el tipo de columna  
 En el siguiente ejemplo se reciben todos los mensajes disponibles para el próximo grupo de conversación disponible en la cola `ExpenseQueue`. Si el tipo de mensaje indica que el mensaje contiene un documento XML, la instrucción convierte el cuerpo del mensaje a XML.  
  
```sql  
WAITFOR (  
    RECEIVE message_type_name,  
        CASE  
            WHEN validation = 'X' THEN CAST(message_body as XML)  
            ELSE NULL  
         END AS message_body   
         FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="j-receiving-a-message-extracting-data-from-the-message-body-retrieving-conversation-state"></a>J. Recibir un mensaje, extraer los datos del cuerpo del mensaje y recuperar el estado de la conversación  
 En el siguiente ejemplo se recibe el siguiente mensaje disponible para el próximo grupo de conversación disponible en la cola `ExpenseQueue`. Si el mensaje es de tipo `//Adventure-Works.com/Expenses/SubmitExpense`, la instrucción extrae el Id. de empleado y una lista de los elementos del cuerpo del mensaje. La instrucción recupera además el estado de la conversación de la tabla `ConversationState`.  
  
```sql  
WAITFOR(  
    RECEIVE   
    TOP(1)  
      message_type_name,  
      COALESCE(  
           (SELECT TOP(1) ConversationState  
            FROM CurrentConversations AS cc  
            WHERE cc.ConversationHandle = conversation_handle),  
           'NEW')  
      AS ConversationState,  
      COALESCE(  
          (SELECT TOP(1) ErrorCount  
           FROM CurrentConversations AS cc  
           WHERE cc.ConversationHandle = conversation_handle),   
           0)  
      AS ConversationErrors,  
      CASE WHEN message_type_name = N'//Adventure-Works.com/Expenses/SubmitExpense'  
          THEN CAST(message_body AS XML).value(  
                'declare namespace rpt = "https://Adventure-Works.com/schemas/expenseReport"  
                   (/rpt:ExpenseReport/rpt:EmployeeID)[1]', 'nvarchar(20)')  
         ELSE NULL  
      END AS EmployeeID,  
      CASE WHEN message_type_name = N'//Adventure-Works.com/Expenses/SubmitExpense'  
          THEN CAST(message_body AS XML).query(  
                'declare namespace rpt = "https://Adventure-Works.com/schemas/expenseReport"   
                     /rpt:ExpenseReport/rpt:ItemDetail')  
          ELSE NULL  
      END AS ItemList  
    FROM ExpenseQueue   
), TIMEOUT 60000 ;  
```  
  
## <a name="see-also"></a>Vea también  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [BEGIN CONVERSATION TIMER &#40;Transact-SQL&#41;](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [CREATE CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [SEND &#40;Transact-SQL&#41;](../../t-sql/statements/send-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [ALTER QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-queue-transact-sql.md)   
 [DROP QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-queue-transact-sql.md)  
  
  
