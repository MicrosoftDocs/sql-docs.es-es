---
description: CREATE QUEUE (Transact-SQL)
title: CREATE QUEUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- QUEUE_TSQL
- CREATE QUEUE
- QUEUE
- CREATE_QUEUE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE QUEUE statement
- internal activation [Service Broker]
- stored procedure activation [Service Broker]
- message retention [Service Broker]
- unavailable queues [Service Broker]
- activation stored procedures [Service Broker]
- queues [Service Broker], creating
ms.assetid: fce80faf-2bdc-475d-8ca1-31438ed41fb0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 42b5b42ee88c6d562f1ebf8c03f92a1bd4be54f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188539"
---
# <a name="create-queue-transact-sql"></a>CREATE QUEUE (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Crea una nueva cola en una base de datos. Las colas almacenan mensajes. Cuando llega un mensaje para un servicio, [!INCLUDE[ssSB](../../includes/sssb-md.md)] lo coloca en la cola asociada a ese servicio.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
CREATE QUEUE <object>
   [ WITH
     [ STATUS = { ON | OFF } [ , ] ]
     [ RETENTION = { ON | OFF } [ , ] ]
     [ ACTIVATION (
         [ STATUS = { ON | OFF } , ]
           PROCEDURE_NAME = <procedure> ,
           MAX_QUEUE_READERS = max_readers ,
           EXECUTE AS { SELF | 'user_name' | OWNER }
            ) [ , ] ]
     [ POISON_MESSAGE_HANDLING (
         [ STATUS = { ON | OFF } ] ) ]
    ]
     [ ON { filegroup | [ DEFAULT ] } ]
[ ; ]

<object> ::=
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }

<procedure> ::=
{ database_name.schema_name.stored_procedure_name | schema_name.stored_procedure_name | stored_procedure_name }

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*database_name* (objeto) Nombre de la base de datos en la que se crea la nueva cola. *database_name* debe especificar el nombre de una base de datos existente. Si no se proporciona *database_name*, la cola se crea en la base de datos actual.

*schema_name* (objeto) Nombre del esquema al que pertenece la nueva cola. El valor predeterminado del esquema es el esquema predeterminado del usuario que ejecuta la instrucción. Si la instrucción CREATE QUEUE es ejecutada por un miembro del rol fijo de servidor sysadmin o por un miembro de los roles fijos de base de datos db_dbowner o db_ddladmin en la base de datos especificada por *database_name*, *schema_name* puede especificar un esquema distinto del asociado con el inicio de sesión de la conexión actual. De no ser así, *schema_name* debe ser el esquema predeterminado del usuario que ejecuta la instrucción.

*queue_name* Nombre de la cola que se va a crear. Este nombre debe cumplir las directrices de los identificadores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

STATUS (Cola) Especifica si la cola está disponible (ON) o no (OFF). Cuando la cola no está disponible, no pueden agregarse mensajes a ella ni tampoco quitarse. Puede crear la cola en un estado de no disponibilidad para impedir que lleguen mensajes a ésta hasta que la cola esté disponible mediante una instrucción ALTER QUEUE. Si se pasa por alto esta cláusula, el valor predeterminado es ON y la cola estará disponible.

RETENTION Especifica la configuración de retención para la cola. Si RETENTION = ON, todos los mensajes enviados o recibidos relativos a conversaciones que utilizan esta cola se retendrán en ella hasta que finalicen las conversaciones. Esto permite retener mensajes con fines de auditoría o para realizar transacciones de compensación si se produce un error. Si no se especifica esta cláusula, el valor predeterminado de retención es OFF.

> [!NOTE]
> El rendimiento puede disminuir si se establece RETENTION en ON. Utilice este valor solo si lo requiere la aplicación.

ACTIVATION Especifica información sobre qué procedimiento almacenado es necesario iniciar para procesar los mensajes de esta cola.

STATUS (activación) Especifica si [!INCLUDE[ssSB](../../includes/sssb-md.md)] inicia el procedimiento almacenado. Si STATUS = ON, la cola inicia el procedimiento almacenado especificado con PROCEDURE_NAME cuando el número de procedimientos que se ejecutan actualmente es menor que MAX_QUEUE_READERS y cuando los mensajes llegan a la cola antes de que los procedimientos almacenados reciban mensajes. Si STATUS = OFF, la cola no inicia el procedimiento almacenado. Si no se especifica esta cláusula, el valor predeterminado es ON.

PROCEDURE_NAME = \<procedure> Especifica el nombre del procedimiento almacenado que se va a iniciar para procesar mensajes en esta cola. Este valor debe ser un identificador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

*database_name*(procedimiento) Nombre de la base de datos que contiene el procedimiento almacenado.

*schema_name*(procedimiento) Nombre del esquema que contiene el procedimiento almacenado.

*procedure_name* Nombre del procedimiento almacenado.

MAX_QUEUE_READERS =*max_readers* Especifica el número máximo de instancias del procedimiento almacenado de activación que la cola inicia al mismo tiempo. El valor de *max_readers* debe ser un número comprendido entre **0** y **32767**.

EXECUTE AS Especifica la cuenta de usuario de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que se ejecuta el procedimiento almacenado de activación. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe poder comprobar los permisos de este usuario en el momento en que la cola inicia el procedimiento almacenado. En el caso de un usuario de dominio, el servidor debe estar conectado al dominio cuando se inicie el procedimiento o se producirá un error en la activación. En usuarios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el servidor siempre puede comprobar los permisos.

SELF Especifica que el procedimiento almacenado se ejecuta como el usuario actual. Es la entidad de seguridad de base de datos que ejecuta esta instrucción CREATE QUEUE.

"*user_name*" Nombre del usuario con el que se ejecuta el procedimiento almacenado. El parámetro *user_name* debe ser un usuario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] válido especificado como identificador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El usuario actual debe tener el permiso IMPERSONATE para el valor de *user_name* especificado.

OWNER Especifica que el procedimiento almacenado se ejecuta como el propietario de la cola.

POISON_MESSAGE_HANDLING Especifica si está habilitado para la cola el control de mensajes dudosos. El valor predeterminado es ON.

Una cola que tenga configurado en OFF el control de mensajes dudosos no se deshabilitará después de cinco reversiones de transacción consecutivas. Esto permite que el sistema de control de mensajes dudosos sea definido por la aplicación.

*grupo de archivos ON |* [**DEFAULT**] Especifica el grupo de archivos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el que se va a crear esta cola. Puede usar el parámetro *filegroup* para identificar un grupo de archivos o emplear el identificador DEFAULT para usar el grupo de archivos predeterminado de la base de datos de Service Broker. En el contexto de esta cláusula, DEFAULT no es una palabra clave y debe delimitarse como un identificador. Si no se especifica ningún grupo de archivos, la cola utiliza el grupo de archivos predeterminado de la base de datos.

## <a name="remarks"></a>Observaciones

Una cola puede ser el destino de una instrucción SELECT. No obstante, el contenido de una cola solo puede modificarse mediante instrucciones que funcionan en conversaciones de [!INCLUDE[ssSB](../../includes/sssb-md.md)], como SEND, RECEIVE y END CONVERSATION. Una cola no puede ser el destino de una instrucción INSERT, UPDATE, DELETE o TRUNCATE.

Una cola no puede ser un objeto temporal. Por lo tanto, los nombres de cola que empiezan por **#** no son válidos.

Si crea una cola en estado inactivo, puede obtener la infraestructura de un servicio antes de permitir que los mensajes se reciban en la cola.

[!INCLUDE[ssSB](../../includes/sssb-md.md)] no detiene los procedimientos almacenados de activación si no hay ningún mensaje en la cola. Un procedimiento almacenado de activación debe finalizar cuando no haya ningún mensaje disponible en la cola durante un breve período de tiempo.

Los permisos para el procedimiento almacenado de activación se comprueban cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] inicia el procedimiento almacenado, no cuando se crea la cola. La instrucción CREATE QUEUE no comprueba que el usuario especificado en la cláusula EXECUTE AS tenga permiso para ejecutar el procedimiento almacenado especificado en la cláusula PROCEDURE NAME.

Cuando una cola no está disponible, [!INCLUDE[ssSB](../../includes/sssb-md.md)] retiene los mensajes de los servicios que usan la cola en la cola de transmisión de la base de datos. La vista de catálogo sys.transmission_queue proporciona una vista de la cola de transmisión.

Una cola es un objeto propiedad de un esquema. Las colas aparecen en la vista de catálogo sys.objects.

La siguiente tabla contiene las columnas de una cola.

|Nombre de la columna|Tipo de datos|Descripción|
|-----------------|---------------|-----------------|
|status|**tinyint**|Estado del mensaje. La instrucción RECEIVE devuelve todos los mensajes que tienen **1** como estado. Si la retención de los mensajes está activada, el estado se establece en 0. Si está desactivada, el mensaje se elimina de la cola. Los mensajes de la cola pueden contener uno de los valores siguientes:<br /><br /> **0**=Mensaje recibido retenido<br /><br /> **1**=Listo para recibir<br /><br /> **2**=Sin completar<br /><br /> **3**=Mensaje enviado retenido|
|priority|**tinyint**|Nivel de prioridad asignado a este mensaje.|
|queuing_order|**bigint**|Número de orden del mensaje en la cola.|
|conversation_group_id|**uniqueidentifier**|Identificador para el grupo de conversación al que pertenece este mensaje.|
|conversation_handle|**uniqueidentifier**|Identificador para la conversación de la que forma parte este mensaje.|
|message_sequence_number|**bigint**|Número de secuencia del mensaje en la conversación.|
|service_name|**nvarchar(512)**|Nombre del servicio al que se destina la conversación.|
|service_id|**int**|Identificador de objeto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del servicio al que se destina la conversación.|
|service_contract_name|**nvarchar(256)**|Nombre del contrato por el que se rige la conversación.|
|service_contract_id|**int**|Identificador de objeto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del contrato por el que se rige la conversación.|
|message_type_name|**nvarchar(256)**|Nombre del tipo de mensaje que describe el mensaje.|
|message_type_id|**int**|Identificador de objeto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del tipo de mensaje que describe el mensaje.|
|validation|**nchar(2)**|Validación utilizada para el mensaje.<br /><br /> E=Empty<br /><br /> N=None<br /><br /> X=XML|
|message_body|**varbinary(max)**|Contenido del mensaje.|
|message_id|**uniqueidentifier**|Identificador único para el mensaje.|

## <a name="permissions"></a>Permisos

Tienen permiso para crear una cola los miembros de los roles fijos de base de datos `db_ddladmin` o `db_owner` o el rol fijo de servidor `sysadmin`.

De forma predeterminada, se concede permiso `REFERENCES` para una cola al propietario de la cola, a los miembros de los roles fijos de base de datos `db_ddladmin` o `db_owner` o a los miembros del rol fijo de servidor `sysadmin`.

De forma predeterminada, se concede el permiso `RECEIVE` para una cola al propietario de la cola, a los miembros del rol fijo de base de datos `db_owner` o a los miembros del rol fijo de servidor `sysadmin`.

## <a name="examples"></a>Ejemplos

### <a name="a-creating-a-queue-with-no-parameters"></a>A. Crear una cola sin parámetros

En el siguiente ejemplo se crea una cola que está disponible para recibir mensajes. No se especifica ningún procedimiento almacenado de activación para la cola.

```sql
CREATE QUEUE ExpenseQueue ;
```

### <a name="b-creating-an-unavailable-queue"></a>B. Crear una cola no disponible

En el siguiente ejemplo se crea una cola que no está disponible para recibir mensajes. No se especifica ningún procedimiento almacenado de activación para la cola.

```sql
CREATE QUEUE ExpenseQueue WITH STATUS=OFF ;
```

### <a name="c-creating-a-queue-and-specify-internal-activation-information"></a>C. Crear una cola y especificar información de activación interna

En el siguiente ejemplo se crea una cola que está disponible para recibir mensajes. La cola inicia el procedimiento almacenado `expense_procedure` cuando un mensaje entra en la cola. El procedimiento almacenado se ejecuta como el usuario `ExpenseUser`. La cola inicia un máximo de `5` instancias del procedimiento almacenado.

```sql
CREATE QUEUE ExpenseQueue
    WITH STATUS=ON,
    ACTIVATION (
        PROCEDURE_NAME = expense_procedure
        , MAX_QUEUE_READERS = 5
        , EXECUTE AS 'ExpenseUser' ) ;
```

### <a name="d-creating-a-queue-on-a-specific-filegroup"></a>D. Crear una cola en un grupo de archivos específico

En el siguiente ejemplo se crea una cola en el grupo de archivos `ExpenseWorkFileGroup`.

```sql
CREATE QUEUE ExpenseQueue
    ON ExpenseWorkFileGroup ;
```

### <a name="e-creating-a-queue-with-multiple-parameters"></a>E. Crear una cola con varios parámetros

En el siguiente ejemplo se crea una cola en el grupo de archivos `DEFAULT`. La cola no está disponible. Los mensajes se retienen en la cola hasta que finaliza la conversación a la que pertenecen. Cuando la cola pasa a estar disponible mediante ALTER QUEUE, la cola inicia el procedimiento almacenado `2008R2.dbo.expense_procedure` para procesar los mensajes. El procedimiento almacenado se ejecuta como el usuario que ejecutó la instrucción `CREATE QUEUE`. La cola inicia un máximo de `10` instancias del procedimiento almacenado.

```sql
CREATE QUEUE ExpenseQueue
    WITH STATUS = OFF
      , RETENTION = ON
      , ACTIVATION (
          PROCEDURE_NAME = AdventureWorks2012.dbo.expense_procedure
          , MAX_QUEUE_READERS = 10
          , EXECUTE AS SELF )
    ON [DEFAULT];
```

## <a name="see-also"></a>Consulte también

- [ALTER QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-queue-transact-sql.md)
- [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)
- [DROP QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-queue-transact-sql.md)
- [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
