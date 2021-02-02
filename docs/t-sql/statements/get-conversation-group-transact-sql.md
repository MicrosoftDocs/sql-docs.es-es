---
description: GET CONVERSATION GROUP (Transact-SQL)
title: GET CONVERSATION GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GET
- CONVERSATION_GROUP_TSQL
- GET_TSQL
- GET_CONVERSATION_GROUP_TSQL
- GET CONVERSATION GROUP
- CONVERSATION GROUP
- GET CONVERSATION
- GET_CONVERSATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GET CONVERSATION GROUP statement
- conversations [Service Broker], groups
ms.assetid: 4da8a855-33c0-43b2-a49d-527487cb3b5c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 37c96250e3cbee346ac9be545e258c4afbcbe9b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159122"
---
# <a name="get-conversation-group-transact-sql"></a>GET CONVERSATION GROUP (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve el identificador del grupo de conversación del siguiente mensaje que se va a recibir y bloquea el grupo de la conversación que contiene el mensaje. El identificador del grupo de conversación se puede utilizar para recuperar información del estado de la conversación antes de recuperar el propio mensaje.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql 
[ WAITFOR ( ]  
   GET CONVERSATION GROUP @conversation_group_id  
      FROM <queue>  
[ ) ] [ , TIMEOUT timeout ]  
[ ; ]  
  
<queue> ::=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 WAITFOR  
 Especifica que la instrucción GET CONVERSATION GROUP espera a que llegue un mensaje a la cola en caso de no haber ningún mensaje.  
  
 *\@conversation_group_id*  
 Es una variable que se utiliza para almacenar el Id. de grupo de conversación devuelto por la instrucción GET CONVERSATION GROUP. La variable debe ser de tipo **uniqueidentifier**. Si no está disponible ningún grupo de conversación, la variable se establece en NULL.  
  
 FROM  
 Especifica la cola de la que se obtiene el grupo de conversación.  
  
 *database_name*  
 Es el nombre de la base de datos que contiene la cola de la que obtiene el grupo de conversación. Si no se proporciona *database_name*, el valor predeterminado es la base de datos actual.  
  
 *schema_name*  
 Es el nombre del esquema propietario de la cola de la que obtiene el grupo de conversación. Si no se proporciona *schema_name*, se usa el esquema predeterminado del usuario actual.  
  
 *queue_name*  
 Es el nombre de la cola de la que se obtiene el grupo de conversación.  
  
 TIMEOUT *tiempo_de_espera*  
 Especifica el tiempo, en milisegundos, que espera Service Broker a que llegue un mensaje a la cola. Esta cláusula solo se puede usar con la cláusula WAITFOR. Si una instrucción que usa WAITFOR no incluye esta cláusula o *tiempo_de_espera* es -1, el tiempo de espera es ilimitado. Si se agota el tiempo de espera, GET CONVERSATION GROUP establece la variable *\@conversation_group_id* en NULL.  
  
## <a name="remarks"></a>Comentarios  
  
> [!IMPORTANT]  
>  Si la instrucción GET CONVERSATION GROUP no es la primera de un lote o un procedimiento almacenado, la instrucción anterior debe terminar en un punto y coma (**;**), que es el terminador de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Si la cola especificada en la instrucción GET CONVERSATION GROUP no está disponible, la instrucción genera un error de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Esta instrucción devuelve el siguiente grupo de conversación donde se dan todas las condiciones siguientes:  
  
-   El grupo de conversación se puede bloquear correctamente.  
  
-   El grupo de conversación tiene mensajes disponibles en la cola.  
  
-   El grupo de conversación tiene el nivel de prioridad más alto de todos los grupos de conversación que cumplen los criterios mencionados anteriormente. El nivel de prioridad de un grupo de conversación es el nivel de prioridad más alto asignado a cualquier conversación que sea miembro del grupo y que tenga mensajes en la cola.  
  
 Las sucesivas llamadas a GET CONVERSATION GROUP en la misma transacción pueden bloquear más de un grupo de conversación. Si no está disponible ningún grupo de conversación, la instrucción devuelve NULL como identificador del grupo de conversación.  
  
 Si se especifica la cláusula WAITFOR, la instrucción espera a que se agote el tiempo de espera especificado o hasta que haya un grupo de conversación disponible. Si se quita la cola mientras la instrucción está esperando, dicha instrucción devuelve un error inmediatamente.  
  
 GET CONVERSATION GROUP no es válido en una función definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Para obtener un identificador de grupo de conversación de una cola, el usuario actual debe tener el permiso RECEIVE en la cola.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-getting-a-conversation-group-waiting-indefinitely"></a>A. Obtener un grupo de conversación, esperando indefinidamente  
 En el siguiente ejemplo se establece `@conversation_group_id` en el identificador de grupo de conversación para el siguiente mensaje disponible en `ExpenseQueue`. El comando espera hasta que un mensaje esté disponible.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
WAITFOR (  
 GET CONVERSATION GROUP @conversation_group_id  
     FROM ExpenseQueue  
) ;  
```  
  
### <a name="b-getting-a-conversation-group-waiting-one-minute"></a>B. Obtener un grupo de conversación, esperando un minuto  
 En el siguiente ejemplo se establece `@conversation_group_id` en el identificador de grupo de conversación para el siguiente mensaje disponible en `ExpenseQueue`. Si en un minuto no está disponible ningún mensaje, GET CONVERSATION GROUP devuelve `@conversation_group_id` sin cambiar su valor.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER  
  
WAITFOR (  
    GET CONVERSATION GROUP @conversation_group_id   
    FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="c-getting-a-conversation-group-returning-immediately"></a>C. Obtener un grupo de conversación, devolviendo un valor inmediatamente  
 En el siguiente ejemplo se establece `@conversation_group_id` en el identificador de grupo de conversación para el siguiente mensaje disponible en `ExpenseQueue`. Si no está disponible ningún mensaje, `GET CONVERSATION GROUP` devuelve inmediatamente `@conversation_group_id` sin cambiar su valor.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
GET CONVERSATION GROUP @conversation_group_id  
FROM AdventureWorks.dbo.ExpenseQueue ;  
```  
  
## <a name="see-also"></a>Vea también  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [MOVE CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/move-conversation-transact-sql.md)  
  
  
