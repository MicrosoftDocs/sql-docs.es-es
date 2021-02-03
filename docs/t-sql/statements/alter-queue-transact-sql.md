---
description: ALTER QUEUE (Transact-SQL)
title: ALTER QUEUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_QUEUE_TSQL
- ALTER QUEUE
dev_langs:
- TSQL
helpviewer_keywords:
- number of queue readers
- modifying queues
- ALTER QUEUE statement
- queue readers [SQL Server]
- queues [Service Broker], modifying
- unavailable queues [SQL Server]
- activation stored procedures [Service Broker]
ms.assetid: d54aa325-8761-4cd4-8da7-acf33df12296
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e1a858efef550632d3d5c8913222ab86a22a9543
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237771"
---
# <a name="alter-queue-transact-sql"></a>ALTER QUEUE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cambia las propiedades de una cola.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER QUEUE <object>   
   queue_settings  
   | queue_action  
[ ; ]  
  
<object> : :=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }
  
<queue_settings> : :=  
WITH  
   [ STATUS = { ON | OFF } [ , ] ]  
   [ RETENTION = { ON | OFF } [ , ] ]  
   [ ACTIVATION (  
       { [ STATUS = { ON | OFF } [ , ] ]   
         [ PROCEDURE_NAME = <procedure> [ , ] ]  
         [ MAX_QUEUE_READERS = max_readers [ , ] ]  
         [ EXECUTE AS { SELF | 'user_name'  | OWNER } ]  
       |  DROP }  
          ) [ , ]]  
         [ POISON_MESSAGE_HANDLING (  
          STATUS = { ON | OFF } )  
         ]   
  
<queue_action> : :=  
   REBUILD [ WITH <query_rebuild_options> ]  
   | REORGANIZE [ WITH (LOB_COMPACTION = { ON | OFF } ) ]  
   | MOVE TO { file_group | "default" }  
  
<procedure> : :=  
{ database_name.schema_name.stored_procedure_name | schema_name.stored_procedure_name | stored_procedure_name }
  
<queue_rebuild_options> : :=  
{  
   ( MAXDOP = max_degree_of_parallelism )  
}  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *database_name* (object)  
 Es el nombre de la base de datos que contiene la cola que se va a cambiar. Si no se proporciona *database_name*, el valor predeterminado es la base de datos actual.  
  
 *schema_name* (object)  
 Nombre del esquema al que pertenece la nueva cola. Si no se proporciona *schema_name*, se usa el esquema predeterminado del usuario actual.  
  
 *queue_name*  
 Es el nombre de la cola que se va a cambiar.  
  
 STATUS (cola)  
 Especifica si la cola está disponible (ON) o no (OFF). Cuando la cola no está disponible, no pueden agregarse mensajes a ella ni tampoco quitarse.  
  
 RETENTION  
 Especifica la configuración de retención para la cola. Si RETENTION = ON, todos los mensajes enviados o recibidos relativos a conversaciones que utilizan esta cola se retendrán hasta que las conversaciones finalicen. Esto permite retener mensajes con fines de auditoría o para realizar transacciones de compensación si se produce un error.  
  
> [!NOTE]  
>  El rendimiento puede disminuir si se establece RETENTION en ON. Esta configuración solo debe utilizarse si es necesario satisfacer el acuerdo de nivel de servicio para la aplicación.  
  
 ACTIVATION  
 Especifica información acerca del procedimiento almacenado que se activa para procesar mensajes que llegan a esta cola.  
  
 STATUS (activación)  
 Especifica si una cola activa o no el procedimiento almacenado. Si STATUS = ON, la cola inicia el procedimiento almacenado especificado con PROCEDURE_NAME cuando el número de procedimientos que se ejecutan actualmente es menor que MAX_QUEUE_READERS y cuando los mensajes llegan a la cola antes de que los procedimientos almacenados reciban mensajes. Si STATUS = OFF, la cola no activa el procedimiento almacenado.  
  
 REBUILD [ WITH \<queue_rebuild_options> ]  
 **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.  
  
 Vuelve a compilar todos los índices de la tabla interna de colas. Use esta función cuando experimente problemas de fragmentación debido a una carga elevada. MAXDOP es la única opción de recompilación de cola admitida. REBUILD siempre es una operación sin conexión.  
  
 REORGANIZE [ WITH ( LOB_COMPACTION = { ON | OFF } ) ]  
 **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.  
  
 Vuelve a organizar todos los índices de la tabla interna de colas.   
A diferencia de REORGANIZE en tablas de usuario, REORGANIZE en una cola siempre se realiza como una operación sin conexión porque los bloqueos de nivel de página se deshabilitan explícitamente en las colas.  
  
> [!TIP]  
>  Para obtener información general sobre la fragmentación del índice, cuando la fragmentación se encuentre entre el 5 % y el 30 %, reorganice el índice. Cuando la fragmentación sea superior al 30 %, recompile el índice. Aun así, estas cifras solo aportan información general que se puede tomar como punto de partida para el entorno. Para determinar la cantidad de fragmentación del índice, use [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md). Vea el ejemplo G del artículo para obtener ejemplos.  
  
 MOVE TO { *file_group* | "default" }  
 **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.  
  
 Mueve la tabla interna de colas (con sus índices) a un grupo de archivos especificado por el usuario.  El nuevo grupo de archivos no debe ser de solo lectura.  
  
 PROCEDURE_NAME = \<procedure>  
 Especifica el nombre del procedimiento almacenado que se va a activar cuando la cola contiene mensajes para procesar. Este valor debe ser un identificador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *database_name* (procedimiento)  
 Nombre de la base de datos que contiene el procedimiento almacenado.  
  
 *schema_name* (procedimiento)  
 Nombre del esquema que tiene la propiedad del procedimiento almacenado.  
  
 *stored_procedure_name*  
 Es el nombre del procedimiento almacenado.  
  
 MAX_QUEUE_READERS =*max_reader*  
 Especifica el número máximo de instancias del procedimiento almacenado de activación que la cola inicia simultáneamente. El valor de *max_readers* debe ser un número comprendido entre 0 y 32 767.  
  
 EXECUTE AS  
 Especifica la cuenta de usuario de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que se ejecuta el procedimiento almacenado de activación. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe poder comprobar los permisos de este usuario en el momento en que la cola inicia el procedimiento almacenado. Para un usuario de dominio de Windows, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe estar conectado al dominio y debe poder validar los permisos del usuario especificado cuando se active el procedimiento o la activación genere un error. En usuarios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el servidor siempre puede comprobar los permisos.  
  
 SELF  
 Especifica que el procedimiento almacenado se ejecuta como el usuario actual. Es la entidad de seguridad de base de datos que ejecuta esta instrucción ALTER QUEUE.  
  
 '*user_name*'  
 Es el nombre de usuario con el que se ejecuta el procedimiento almacenado. *user_name* debe ser un usuario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] válido especificado como identificador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El usuario actual debe tener el permiso IMPERSONATE para el valor de *user_name* especificado.  
  
 OWNER  
 Especifica que el procedimiento almacenado se ejecuta como el propietario de la cola.  
  
 DROP  
 Elimina toda la información de activación asociada a la cola.  
  
 POISON_MESSAGE_HANDLING  
 Especifica si se ha habilitado controlar mensajes dudosos. El valor predeterminado es ON.  
  
 Una cola que tenga configurado en OFF el control de mensajes dudosos no se deshabilitará después de cinco reversiones de transacción consecutivas. Esto permite que el sistema de control de mensajes dudosos sea definido por la aplicación.  
  
## <a name="remarks"></a>Observaciones  
 Cuando una cola que tiene especificado un procedimiento almacenado de activación contiene mensajes, al cambiar el estado de activación de OFF a ON, se activa inmediatamente el procedimiento almacenado de activación. Cambiar el estado de activación de ON a OFF impide que el agente active instancias del procedimiento almacenado, pero las instancias que están en ejecución no se detienen.  
  
 Modificar una cola para agregar un procedimiento almacenado de activación no afecta al estado de activación de la cola. Modificar el procedimiento almacenado de activación de la cola no afecta a las instancias del procedimiento almacenado de activación que se estén ejecutando en el momento actual.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] comprueba el número máximo de lectores de cola como parte del proceso de activación. Por tanto, si se modifica una cola para aumentar el número máximo de lectores de cola, [!INCLUDE[ssSB](../../includes/sssb-md.md)] iniciará inmediatamente más instancias del procedimiento almacenado de activación. Modificar una cola para reducir el número máximo de lectores de cola no afecta a las instancias del procedimiento almacenado de activación que se estén ejecutando en el momento actual. No obstante, [!INCLUDE[ssSB](../../includes/sssb-md.md)] no inicia ninguna nueva instancia del procedimiento almacenado hasta que el número de instancias del procedimiento almacenado de activación sea inferior al número máximo configurado.  
  
 Cuando una cola no está disponible, [!INCLUDE[ssSB](../../includes/sssb-md.md)] retiene los mensajes de los servicios que usan la cola en la cola de transmisión de la base de datos. La vista de catálogo [sys.transmission_queue](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md) proporciona una vista de la cola de transmisión.  
  
 Si las instrucciones RECEIVE o GET CONVERSATION GROUP especifican una cola que no está disponible, generarán un error de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, se concede permiso para modificar una cola al propietario de la cola, a los miembros de los roles fijos de base de datos db_ddladmin o db_owner y a los miembros del rol fijo de servidor sysadmin.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-making-a-queue-unavailable"></a>A. Hacer que una cola no esté disponible  
 En el ejemplo siguiente se hace que la cola de `ExpenseQueue` no esté disponible para recibir mensajes.  
  
```sql  
ALTER QUEUE ExpenseQueue WITH STATUS = OFF ;  
```  
  
### <a name="b-changing-the-activation-stored-procedure"></a>B. Cambiar el procedimiento almacenado de activación  
 En el siguiente ejemplo se muestra cómo cambiar el procedimiento almacenado iniciado por la cola. El procedimiento almacenado se ejecuta como el usuario que ejecutó la instrucción `ALTER QUEUE`.  
  
```sql  
ALTER QUEUE ExpenseQueue  
    WITH ACTIVATION (  
        PROCEDURE_NAME = new_stored_proc,  
        EXECUTE AS SELF) ;  
```  
  
### <a name="c-changing-the-number-of-queue-readers"></a>C. Cambiar el número de lectores de cola  
 En el ejemplo siguiente se establece en `7` el número máximo de instancias de procedimiento almacenado que inicia [!INCLUDE[ssSB](../../includes/sssb-md.md)] para esta cola.  
  
```sql  
ALTER QUEUE ExpenseQueue WITH ACTIVATION (MAX_QUEUE_READERS = 7) ;  
```  
  
### <a name="d-changing-the-activation-stored-procedure-and-the-execute-as-account"></a>D. Cambiar el procedimiento almacenado de activación y la cuenta EXECUTE AS  
 En el ejemplo siguiente se cambia el procedimiento almacenado iniciado por [!INCLUDE[ssSB](../../includes/sssb-md.md)]. El procedimiento almacenado se ejecuta como el usuario `SecurityAccount`.  
  
```sql  
ALTER QUEUE ExpenseQueue  
    WITH ACTIVATION (  
        PROCEDURE_NAME = AdventureWorks2012.dbo.new_stored_proc ,  
        EXECUTE AS 'SecurityAccount') ;  
```  
  
### <a name="e-setting-the-queue-to-retain-messages"></a>E. Configurar la cola para retener mensajes  
 En el siguiente ejemplo se muestra cómo configurar la cola para retener mensajes. La cola retiene todos los mensajes enviados a o desde los servicios que utilizan la cola hasta que la conversación que contiene el mensaje finaliza.  
  
```sql  
ALTER QUEUE ExpenseQueue WITH RETENTION = ON ;  
```  
  
### <a name="f-removing-activation-from-a-queue"></a>F. Quitar la activación de una cola  
 En el siguiente ejemplo se muestra cómo quitar toda la información de activación de la cola.  
  
```  
ALTER QUEUE ExpenseQueue WITH ACTIVATION (DROP) ;  
```  
  
### <a name="g-rebuilding-queue-indexes"></a>G. Recompilar índices de cola  
  
**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.  
  
 En el ejemplo siguiente se recompilan índices de cola.  
  
```sql  
ALTER QUEUE ExpenseQueue REBUILD WITH (MAXDOP = 2)   
```  
  
### <a name="h-reorganizing-queue-indexes"></a>H. Reorganizar índices de cola  
  
**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.  
  
 En el ejemplo siguiente se reorganizan índices de cola.  
  
```sql  
ALTER QUEUE ExpenseQueue REORGANIZE   
```  
  
### <a name="i-moving-queue-internal-table-to-another-filegroup"></a>I: Mover una tabla interna de colas a otro grupo de archivos  
  
**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.  
  
```sql  
ALTER QUEUE ExpenseQueue MOVE TO [NewFilegroup]   
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [DROP QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-queue-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
  
  
