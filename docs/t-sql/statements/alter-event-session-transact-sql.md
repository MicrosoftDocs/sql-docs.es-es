---
description: ALTER EVENT SESSION (Transact-SQL)
title: ALTER EVENT SESSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER EVENT SESSION
- ALTER_EVENT_SESSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event sessions [SQL Server]
- extended events [SQL Server], Transact-SQL
- ALTER EVENT SESSION statement
ms.assetid: da006ac9-f914-4995-a2fb-25b5d971cd90
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a48973fcbd4aa030d41b5bdce3e0c10f37ce8efc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204277"
---
# <a name="alter-event-session-transact-sql"></a>ALTER EVENT SESSION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Inicia o detiene una sesión de eventos, o cambia la configuración de una sesión de eventos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER EVENT SESSION event_session_name  
ON SERVER  
{  
    [ [ {  <add_drop_event> [ ,...n] }     
       | { <add_drop_event_target> [ ,...n ] } ]   
    [ WITH ( <event_session_options> [ ,...n ] ) ]  
    ]  
    | [ STATE = { START | STOP } ]  
}  
  
<add_drop_event>::=  
{  
    [ ADD EVENT <event_specifier>   
         [ ( {   
                 [ SET { event_customizable_attribute = <value> [ ,...n ] } ]  
                 [ ACTION ( { [event_module_guid].event_package_name.action_name [ ,...n ] } ) ]  
                 [ WHERE <predicate_expression> ]  
        } ) ]  
   ]   
   | DROP EVENT <event_specifier> }  
  
<event_specifier> ::=  
{  
[event_module_guid].event_package_name.event_name  
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
  
<add_drop_event_target>::=  
{  
    ADD TARGET <event_target_specifier>  
        [ ( SET { target_parameter_name = <value> [ ,...n] } ) ]  
    | DROP TARGET <event_target_specifier>  
}  
  
<event_target_specifier>::=  
{  
    [event_module_guid].event_package_name.target_name  
}  
  
<event_session_options>::=  
{  
    [    MAX_MEMORY = size [ KB | MB] ]  
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
  
|Término|Definición|  
|-|-|
|*event_session_name*|El nombre de una sesión de eventos existente.|  
|STATE = START &#124; STOP|Inicia o detiene la sesión de eventos. Este argumento solo es válido cuando ALTER EVENT SESSION se aplica a un objeto de sesión de eventos.|  
|ADD EVENT \<event_specifier>|Asocia el evento que identifica \<event_specifier> con la sesión de eventos.|
|[*event_module_guid*] *.event_package_name.event_name*|El nombre de un evento en un paquete de eventos, donde.<br /><br /> -   *event_module_guid* es el GUID del módulo que contiene el evento.<br />-   *event_package_name* es el paquete que contiene el objeto de la acción.<br />-   *event_name* es el objeto de evento.<br /><br /> Los eventos aparecen en la vista sys.dm_xe_objects como object_type "event".|  
|SET { *event_customizable_attribute*= \<value> [ ,...*n*] }|Especifica atributos personalizables del evento. Los atributos personalizables aparecen en la vista sys.dm_xe_object_columns como column_type "customizable" y object_name = *event_name*.|  
|ACTION ( { [*event_module_guid*] *.event_package_name.action_name* [ **,** ...*n*] } )|Es la acción que se va a asociar a la sesión de eventos, donde:<br /><br /> -   *event_module_guid* es el GUID del módulo que contiene el evento.<br />-   *event_package_name* es el paquete que contiene el objeto de la acción.<br />-   *action_name* es el objeto de la acción.<br /><br /> Las acciones aparecen en la vista sys.dm_xe_objects como object_type 'action'.|  
|WHERE \<predicate_expression>|Especifica la expresión de predicado usada para determinar si debe procesarse un evento. Si \<predicate_expression> es true, las acciones y los destinos de la sesión siguen procesando el evento. Si \<predicate_expression> es false, la sesión quita el evento antes de que las acciones y los destinos de la sesión lo procesen. Las expresiones de predicado se limitan a 3.000 caracteres, lo que limita los argumentos de cadena.|
|*event_field_name*|Es el nombre del campo de evento que identifica el origen del predicado.|  
|[event_module_guid].event_package_name.predicate_source_name|Es el nombre del origen del predicado global, donde:<br /><br /> -   *event_module_guid* es el GUID del módulo que contiene el evento.<br />-   *event_package_name* es el paquete que contiene el objeto del predicado.<br />-   *predicate_source_name* se define en la vista sys.dm_xe_objects como object_type "pred_source".|  
|[*event_module_guid*].*event_package_name*.*predicate_compare_name*|Es el nombre del objeto de predicado que se va a asociar al evento, donde:<br /><br /> -   *event_module_guid* es el GUID del módulo que contiene el evento.<br />-   *event_package_name* es el paquete que contiene el objeto del predicado.<br />-   *predicate_compare_name* es un origen global definido en la vista sys.dm_xe_objects como object_type "pred_compare".|  
|DROP EVENT \<event_specifier>|Quita el evento que identifica *\<event_specifier>* . \<event_specifier> debe ser válido en la sesión de eventos.|  
|ADD TARGET \<event_target_specifier>|Asocia el destino que identifica \<event_target_specifier> con la sesión de eventos.|
|[*event_module_guid*].*event_package_name*.*target_name*|El nombre de un destino en la sesión de eventos, donde:<br /><br /> -   *event_module_guid* es el GUID del módulo que contiene el evento.<br />-   *event_package_name* es el paquete que contiene el objeto de la acción.<br />-   *target_name* es la acción. Las acciones aparecen en la vista sys.dm_xe_objects como object_type 'target'.|  
|SET { *target_parameter_name*= \<value> [, ...*n*] }|Establece un parámetro de destino. Los parámetros de destino aparecen en la vista sys.dm_xe_object_columns como column_type 'customizable' y object_name = *target_name*.<br /><br /> **NOTA** Si usa el destino de búfer en anillo, le recomendamos que establezca el parámetro de destino max_memory en 2048 kilobytes (KB) para intentar evitar el truncamiento de los datos en la salida XML. Para más información sobre cuándo usar los diferentes tipos de destino, vea [Destinos para eventos extendidos en SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).|  
|DROP TARGET \<event_target_specifier>|Quita el destino que identifica \<event_target_specifier>. \<event_target_specifier> debe ser válido en la sesión de eventos.|  
|EVENT_RETENTION_MODE = { **ALLOW_SINGLE_EVENT_LOSS** &#124; ALLOW_MULTIPLE_EVENT_LOSS &#124; NO_EVENT_LOSS }|Especifica el modo de retención de eventos usado para controlar las pérdidas de eventos.<br /><br /> **ALLOW_SINGLE_EVENT_LOSS**<br /> Puede perderse un evento de la sesión. Se elimina un único evento solo cuando todos los búferes de eventos están llenos. La pérdida de un único evento cuando los búferes de eventos están llenos permite un rendimiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aceptable, al mismo tiempo que minimiza las pérdidas de datos en el flujo de eventos procesado.<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS<br /> En la sesión pueden perderse búferes completos de eventos que contienen varios eventos. El número de eventos perdidos depende del tamaño de la memoria asignada a la sesión, del particionamiento de la memoria y del tamaño de los eventos del búfer. Esta opción minimiza el impacto en el rendimiento del servidor si los búferes de eventos se llenan rápidamente, pero se puede perder un gran número de eventos de la sesión.<br /><br /> NO_EVENT_LOSS<br /> No se permite ninguna pérdida de eventos. Esta opción asegura que se van a retener todos los eventos que aparezcan. Al utilizar esta opción, se obliga a todas las tareas que activan eventos a esperar hasta que haya espacio disponible en un búfer de eventos. Esto puede producir problemas detectables de rendimiento mientras la sesión de eventos está activa. Las conexiones de usuario pueden detenerse a la espera de que se quiten eventos del búfer.|  
|MAX_DISPATCH_LATENCY = { *seconds* SECONDS &#124; **INFINITE** }|Especifica el tiempo que los eventos se almacenan en memoria antes de enviarse a los destinos de la sesión de eventos. El valor de latencia mínimo es de 1 segundo. Sin embargo, puede usarse el valor 0 para especificar la latencia INFINITE. De forma predeterminada, este valor está establecido en 30 segundos.<br /><br /> *seconds* SECONDS<br /> Tiempo, en segundos, que hay que esperar antes de que empiecen a vaciarse los búferes en los destinos. *seconds* es un número entero.<br /><br /> **INFINITE**<br /> Los búferes se vacían en los destinos solo si están llenos o cuando se cierra la sesión de eventos.<br /><br /> **NOTA** MAX_DISPATCH_LATENCY = 0 SECONDS es equivalente a MAX_DISPATCH_LATENCY = INFINITE.|  
|MAX_EVENT_SIZE =*size* [ KB &#124; **MB** ]|Especifica el tamaño máximo permitido para los eventos. MAX_EVENT_SIZE solo se debe establecer para permitir los eventos únicos mayores que MAX_MEMORY; al establecerlo en un valor menor que MAX_MEMORY, se producirá un error. *size* es un número entero y puede expresarse en kilobytes (KB) o en megabytes (MB). Si *size* se especifica en kilobytes, el tamaño mínimo permitido es 64 KB. Cuando MAX_EVENT_SIZE se establece, se crean dos búferes de *size*, además de MAX_MEMORY. Esto significa que la memoria total utilizada en búferes de eventos es MAX_MEMORY + 2 * MAX_EVENT_SIZE.|  
|MEMORY_PARTITION_MODE = { **NONE** &#124; PER_NODE &#124; PER_CPU }|Especifica la ubicación en la que se van a crear los búferes de eventos.<br /><br /> **NONE**<br /> Se crea un conjunto único de búferes dentro de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> PER_NODE: se crea un conjunto de búferes por cada nodo NUMA.<br /><br /> PER_CPU: se crea un conjunto de búferes por cada CPU.|  
|TRACK_CAUSALITY = { ON &#124; **OFF** }|Especifica si se va a realizar el seguimiento de la causalidad. Si está habilitado, la causalidad permite correlacionar eventos relacionados en las diferentes conexiones con el servidor.|  
|STARTUP_STATE = { ON &#124; **OFF** }|Especifica si esta sesión de eventos se inicia automáticamente cuando se inicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Si STARTUP_STATE=ON, la sesión de eventos se iniciará solo si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se detiene y se reinicia a continuación.<br /><br /> ON = La sesión de eventos comienza en el inicio.<br /><br /> **OFF** = La sesión de eventos NO comienza en el inicio.|  
  
## <a name="remarks"></a>Observaciones  
 Los argumentos `ADD` y `DROP` no pueden usarse en la misma instrucción.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `ALTER ANY EVENT SESSION`.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se inicia una sesión de eventos, se obtienen algunas estadísticas de la sesión activa y, a continuación, se agregan dos eventos a la sesión existente.  
  
```sql  
-- Start the event session  
ALTER EVENT SESSION test_session ON SERVER  
STATE = start;  
GO  

-- Obtain live session statistics   
SELECT * FROM sys.dm_xe_sessions;  
SELECT * FROM sys.dm_xe_session_events;  
GO  
  
-- Add new events to the session  
ALTER EVENT SESSION test_session ON SERVER  
ADD EVENT sqlserver.database_transaction_begin,  
ADD EVENT sqlserver.database_transaction_end;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md)   
 [DROP EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-session-transact-sql.md)   
 [Destinos de SQL Server Extended Events](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))   
 [sys.server_event_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)   
 [sys.dm_xe_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)   
 [sys.dm_xe_object_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)  
  
