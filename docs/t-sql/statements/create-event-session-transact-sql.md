---
description: CREATE EVENT SESSION (Transact-SQL)
title: CREATE EVENT SESSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE EVENT SESSION
- SESSION
- EVENT SESSION
- SESSION_TSQL
- EVENT_SESSION_TSQL
- CREATE_EVENT_SESSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event sessions [SQL Server]
- CREATE EVENT SESSION statement
ms.assetid: 67683027-2b0f-47aa-b223-604731af8b4d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7284b9c3d574a55dc67c99e500c7c7da69400e29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186859"
---
# <a name="create-event-session-transact-sql"></a>CREATE EVENT SESSION (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crea una sesión de eventos extendidos que identifica el origen de los eventos, los destinos de la sesión de eventos y las opciones de la sesión de eventos.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Sintaxis

```syntaxsql
CREATE EVENT SESSION event_session_name
ON { SERVER | DATABASE }
{  
    <event_definition> [ ,...n]
    [ <event_target_definition> [ ,...n] ]
    [ WITH ( <event_session_options> [ ,...n] ) ]
}
;

<event_definition>::=
{
    ADD EVENT [event_module_guid].event_package_name.event_name
         [ ( {
                 [ SET { event_customizable_attribute = <value> [ ,...n] } ]
                 [ ACTION ( { [event_module_guid].event_package_name.action_name [ ,...n] } ) ]
                 [ WHERE <predicate_expression> ]
        } ) ]
}

<predicate_expression> ::=
{
    [ NOT ] <predicate_factor> | {( <predicate_expression> ) }
    [ { AND | OR } [ NOT ] { <predicate_factor> | ( <predicate_expression> ) } ]
    [ ,...n ]
}  
  
<predicate_factor>::=
{
    <predicate_leaf> | ( <predicate_expression> )
}

<predicate_leaf>::=
{
      <predicate_source_declaration> { = | < > | ! = | > | > = | < | < = } <value>
    | [event_module_guid].event_package_name.predicate_compare_name ( <predicate_source_declaration>, <value> )
}

<predicate_source_declaration>::=
{
    event_field_name | ( [event_module_guid].event_package_name.predicate_source_name )
}

<value>::=
{
    number | 'string'
}

<event_target_definition>::=
{
    ADD TARGET [event_module_guid].event_package_name.target_name
        [ ( SET { target_parameter_name = <value> [ ,...n] } ) ]
}

<event_session_options>::=
{  
    [    MAX_MEMORY = size [ KB | MB ] ]
    [ [,] EVENT_RETENTION_MODE = { ALLOW_SINGLE_EVENT_LOSS | ALLOW_MULTIPLE_EVENT_LOSS | NO_EVENT_LOSS } ]
    [ [,] MAX_DISPATCH_LATENCY = { seconds SECONDS | INFINITE } ]
    [ [,] MAX_EVENT_SIZE = size [ KB | MB ] ]
    [ [,] MEMORY_PARTITION_MODE = { NONE | PER_NODE | PER_CPU } ]
    [ [,] TRACK_CAUSALITY = { ON | OFF } ]
    [ [,] STARTUP_STATE = { ON | OFF } ]
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*event_session_name* es el nombre definido por el usuario para la sesión de eventos. *event_session_name* es alfanumérico, puede tener hasta 128 caracteres, debe ser único dentro de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y debe cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).

ADD EVENT [ *event_module_guid* ].*event_package_name*.*event_name* es el evento que se va a asociar con la sesión de eventos, donde:

- *event_module_guid* es el GUID del módulo que contiene el evento.
- *event_package_name* es el paquete que contiene el objeto de la acción.
- *event_name* es el objeto de evento.

Los eventos aparecen en la vista sys.dm_xe_objects como object_type "event".

SET { *atributos_personalizables_de_evento*= \<value> [ ,...*n*] } Permite establecer atributos personalizables para el evento. Los atributos personalizables aparecen en la vista sys.dm_xe_object_columns como column_type "customizable" y object_name = *event_name*.

ACTION ( { [*event_module_guid*].*event_package_name*.*action_name* [ **,** ...*n*] }) es la acción que se va a asociar con la sesión de eventos, donde:

- *event_module_guid* es el GUID del módulo que contiene el evento.
- *event_package_name* es el paquete que contiene el objeto de la acción.
- *action_name* es el objeto de la acción.

Las acciones aparecen en la vista sys.dm_xe_objects como object_type 'action'.

WHERE \<predicate_expression> Especifica la expresión de predicado usada para determinar si se debe procesar un evento. Si \<predicate_expression> es true, las acciones y los destinos de la sesión siguen procesando el evento. Si \<predicate_expression> es false, la sesión quita el evento antes de que las acciones y los destinos de la sesión lo procesen. Las expresiones de predicado se limitan a 3.000 caracteres, lo que limita los argumentos de cadena.

*event_field_name* es el nombre del campo de evento que identifica el origen del predicado.

[*event_module_guid*].*event_package_name*.*predicate_source_name* es el nombre del origen del predicado global, donde:

- *event_module_guid* es el GUID del módulo que contiene el evento.
- *event_package_name* es el paquete que contiene el objeto del predicado.
- *predicate_source_name* se define en la vista sys.dm_xe_objects como object_type "pred_source".

[*event_module_guid*].*event_package_name*.*predicate_compare_name* es el nombre del objeto del predicado que se va a asociar con el evento, donde:

- *event_module_guid* es el GUID del módulo que contiene el evento.
- *event_package_name* es el paquete que contiene el objeto del predicado.
- *predicate_compare_name* es un origen global definido en la vista sys.dm_xe_objects como object_type "pred_compare".

*number* es cualquier tipo numérico, incluido el tipo **decimal**. Las limitaciones son la falta de memoria física disponible o un número demasiado grande para ser representado como un entero de 64 bits.

"*string*", ya sea una cadena ANSI o Unicode según lo requerido por la comparación de predicado. No se realiza ninguna conversión implícita de tipos de cadena para las funciones de comparación de predicado. Si se pasa el tipo incorrecto se producirá un error.

ADD TARGET [*event_module_guid*].*event_package_name*.*target_name* es el destino que se va a asociar con la sesión de eventos, donde:

- *event_module_guid* es el GUID del módulo que contiene el evento.
- *event_package_name* es el paquete que contiene el objeto de la acción.
- *target_name* es el destino. Los destinos aparecen en la vista sys.dm_xe_objects como object_type 'target'.

SET { *nombre_parámetro_de_destino*= \<value> [, ...*n*] } Establece un parámetro de destino. Los parámetros de destino aparecen en la vista sys.dm_xe_object_columns como column_type 'customizable' y object_name = *target_name*.

> [!IMPORTANT]
> Si usa el destino de búfer en anillo, le recomendamos que establezca el parámetro de destino max_memory en 2048 kilobytes (KB) para intentar evitar el truncamiento de los datos en la salida XML. Para más información sobre cuándo usar los diferentes tipos de destino, vea [Destinos para eventos extendidos en SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).

WITH ( \<event_session_options> [ ,...*n*] ) Especifica las opciones que se usarán con la sesión de eventos.

MAX_MEMORY =*size* [ KB | **MB** ] especifica la cantidad máxima de memoria para asignar a la sesión para el almacenamiento en búfer de los eventos. El valor predeterminado es 4 MB. *size* es un número entero y puede expresarse en kilobytes (KB) o en megabytes (MB). La cantidad máxima no puede superar los 2 GB (menos de 2048 MB). Sin embargo, no se recomienda usar valores de memoria en el intervalo de GB.

EVENT_RETENTION_MODE = { **ALLOW_SINGLE_EVENT_LOSS** | ALLOW_MULTIPLE_EVENT_LOSS | NO_EVENT_LOSS } especifica el modo de retención de eventos que se usa para controlar las pérdidas de eventos.

**ALLOW_SINGLE_EVENT_LOSS**, puede perderse un evento de la sesión. Se elimina un único evento solo cuando todos los búferes de eventos están llenos. La pérdida de un único evento cuando los búferes de eventos están llenos permite un rendimiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aceptable, al mismo tiempo que minimiza las pérdidas de datos en el flujo de eventos procesado.

ALLOW_MULTIPLE_EVENT_LOSS, en la sesión pueden perderse búferes completos de eventos que contienen varios eventos. El número de eventos perdidos depende del tamaño de la memoria asignada a la sesión, del particionamiento de la memoria y del tamaño de los eventos del búfer. Esta opción minimiza el impacto en el rendimiento del servidor si los búferes de eventos se llenan rápidamente, pero se puede perder un gran número de eventos de la sesión.

NO_EVENT_LOSS, no se permite ninguna pérdida de eventos. Esta opción asegura que se van a retener todos los eventos que aparezcan. Al utilizar esta opción, se obliga a todas las tareas que activan eventos a esperar hasta que haya espacio disponible en un búfer de eventos. Esto puede producir problemas detectables de rendimiento mientras la sesión de eventos está activa. Las conexiones de usuario pueden detenerse a la espera de que se quiten eventos del búfer.

MAX_DISPATCH_LATENCY = { *seconds* SECONDS | **INFINITE** }, especifica el tiempo que los eventos se almacenarán en memoria antes de enviarse a los destinos de la sesión de eventos. De forma predeterminada, este valor está establecido en 30 segundos.

*seconds* SECONDS. El tiempo, en segundos, que hay que esperar antes de que empiecen a variarse los búferes en los destinos. *seconds* es un número entero. El valor de latencia mínimo es de 1 segundo. Sin embargo, puede usarse el valor 0 para especificar la latencia INFINITE.

**INFINITE**. Los búferes se vacían en los destinos solo si están llenos o cuando se cierra la sesión de eventos.

> [!NOTE]
> MAX_DISPATCH_LATENCY = 0 SECONDS es equivalente a MAX_DISPATCH_LATENCY = INFINITE.

MAX_EVENT_SIZE =*size* [ KB | **MB** ] especifica el tamaño máximo permitido para los eventos. MAX_EVENT_SIZE solo se debe establecer para permitir los eventos únicos mayores que MAX_MEMORY; al establecerlo en un valor menor que MAX_MEMORY, se producirá un error. *size* es un número entero y puede expresarse en kilobytes (KB) o en megabytes (MB). Si *size* se especifica en kilobytes, el tamaño mínimo permitido es 64 KB. Cuando MAX_EVENT_SIZE se establece, se crean dos búferes de *size*, además de MAX_MEMORY. Esto significa que la memoria total utilizada en búferes de eventos es MAX_MEMORY + 2 * MAX_EVENT_SIZE.

MEMORY_PARTITION_MODE = { **NONE** | PER_NODE | PER_CPU } especifica la ubicación en la que se van a crear los búferes de eventos.

**NONE**: se crea un conjunto único de búferes dentro de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

PER_NODE A: se crea un conjunto de búferes para cada nodo NUMA.

PER_CPU: se crea un conjunto de búferes para cada CPU.

TRACK_CAUSALITY = { ON | **OFF** } especifica si se va a realizar el seguimiento de la causalidad. Si está habilitado, la causalidad permite correlacionar eventos relacionados en las diferentes conexiones con el servidor.

STARTUP_STATE = { ON | **OFF** } especifica si esta sesión de eventos se inicia automáticamente o no cuando se inicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> Si `STARTUP_STATE = ON`, la sesión de eventos se iniciará solo si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se detiene y después se reinicia.

ON: la sesión de eventos se inicia en el inicio.

**OFF**: la sesión de eventos no se inicia en el inicio.

## <a name="remarks"></a>Observaciones

El orden de prioridad de los operadores lógicos es `NOT` (el más alto), seguido de `AND` y `OR`.

## <a name="permissions"></a>Permisos

En SQL Server, se requiere el permiso `ALTER ANY EVENT SESSION`.
En SQL Database, se requiere el permiso `ALTER ANY DATABASE EVENT SESSION` en la base de datos.

## <a name="examples"></a>Ejemplos

### <a name="sql-server-example"></a>Ejemplo de SQL Server

En el ejemplo siguiente se muestra cómo crear una sesión de eventos denominada `test_session`. En este ejemplo se agregan dos eventos y utiliza el Seguimiento de eventos para Windows.

```sql
IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='test_session')
    DROP EVENT session test_session ON SERVER;
GO
CREATE EVENT SESSION test_session
ON SERVER
    ADD EVENT sqlos.async_io_requested,
    ADD EVENT sqlserver.lock_acquired
    ADD TARGET package0.etw_classic_sync_target
        (SET default_etw_session_logfile_path = N'C:\demo\traces\sqletw.etl' )
    WITH (MAX_MEMORY=4MB, MAX_EVENT_SIZE=4MB);
GO
```
### <a name="sql-database-example"></a>Ejemplo de SQL Database

Si quiere ver un ejemplo de Azure SQL Database, consulte el ejemplo que aparece en el [código de destino del archivo de evento para eventos extendidos en SQL Database](/azure/sql-database/sql-database-xevent-code-event-file#transact-sql-code).

### <a name="code-examples-can-differ-for-azure-sql-database"></a>Ejemplos de código que pueden diferir de Azure SQL Database

[!INCLUDE[sql-on-premises-vs-azure-similar-sys-views-include.](../../includes/paragraph-content/sql-on-premises-vs-azure-similar-sys-views-include.md)]

## <a name="see-also"></a>Consulte también

- [ALTER EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-event-session-transact-sql.md)
- [DROP EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-session-transact-sql.md)
- [sys.server_event_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)
- [sys.dm_xe_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)
- [sys.dm_xe_object_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)