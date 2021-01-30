---
description: sp_trace_setevent (Transact-SQL)
title: sp_trace_setevent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_setevent_TSQL
- sp_trace_setevent
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_setevent
ms.assetid: 7662d1d9-6d0f-443a-b011-c901a8b77a44
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fffcbb5ec35fb329fa0ed3dac4782f0f1417ce82
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209730"
---
# <a name="sp_trace_setevent-transact-sql"></a>sp_trace_setevent (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega o quita un evento o una columna de evento en un seguimiento. **sp_trace_setevent** solo se puede ejecutar en seguimientos existentes que estén detenidos (el *Estado* es **0**). Se devuelve un error si este procedimiento almacenado se ejecuta en un seguimiento que no existe o cuyo *Estado* no es **0**.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use eventos extendidos en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_trace_setevent [ @traceid = ] trace_id   
          , [ @eventid = ] event_id  
          , [ @columnid = ] column_id  
          , [ @on = ] on  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @traceid = ] trace_id` Es el ID. del seguimiento que se va a modificar. *trace_id* es de **tipo int** y no tiene ningún valor predeterminado. El usuario emplea este valor *trace_id* para identificar, modificar y controlar el seguimiento.  
  
`[ @eventid = ] event_id` Es el identificador del evento que se va a activar. *event_id* es de **tipo int** y no tiene ningún valor predeterminado.  
  
 Esta tabla muestra una lista de los eventos que pueden agregarse o quitarse de un seguimiento.  
  
|Número de evento|Nombre del evento|Descripción|  
|------------------|----------------|-----------------|  
|0-9|Reservado|Reservado|  
|10|RPC:Completed|Se produce cuando se ha completado una llamada a procedimiento remoto (RPC).|  
|11|RPC:Starting|Se produce cuando se ha iniciado una RPC.|  
|12|SQL:BatchCompleted|Se produce cuando se ha completado un proceso por lotes de [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|13|SQL:BatchStarting|Se produce cuando se ha iniciado un proceso por lotes de [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|14|Auditoría de inicio de sesión|Se produce cuando un usuario inicia una sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correctamente.|  
|15|Auditoría de cierre de sesión|Se produce cuando un usuario cierra la sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|16|Atención|Se produce cuando tienen lugar eventos de atención como, por ejemplo, las solicitudes de interrupción de clientes o las conexiones de cliente interrumpidas.|  
|17|ExistingConnection|Detecta toda la actividad de los usuarios conectados a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antes del inicio del seguimiento.|  
|18|Audit Server Starts and Stops|Se produce cuando se modifica el estado del servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|19|DTCTransaction|Realiza un seguimiento de las transacciones coordinadas del Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC) entre dos o más bases de datos.|  
|20|Audit Login Failed|Indica que un intento de inicio de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde un cliente ha sido erróneo.|  
|21|EventLog|Indica que los eventos se han grabado en el registro de aplicación de Windows.|  
|22|ErrorLog|Indica que se han registrado eventos de error en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|23|Lock:Released|Indica que se ha liberado un bloqueo en un recurso, como una página.|  
|24|Lock:Acquired|Indica la adquisición de un bloqueo en un recurso, como una página de datos.|  
|25|Lock:Deadlock|Indica que dos transacciones simultáneas se han interbloqueado mutuamente al intentar obtener bloqueos incompatibles en recursos que son propiedad de la otra transacción.|  
|26|Lock:Cancel|Indica la anulación de la adquisición de un bloqueo en un recurso (por ejemplo, debido a un interbloqueo).|  
|27|Lock:Timeout|Indica que una solicitud de un bloqueo en un recurso, como una página, ha agotado el tiempo de espera debido a que existía otra transacción que mantenía un bloqueo en el recurso requerido. El tiempo de espera viene determinado por la @LOCK_TIMEOUT función @ y se puede establecer con la instrucción set LOCK_TIMEOUT.|  
|28|Degree of Parallelism Event (7.0 Insert)|Se produce antes de ejecutarse una instrucción SELECT, INSERT o UPDATE.|  
|29-31|Reservado|Utilice el evento 28 en su lugar.|  
|32|Reservado|Reservado|  
|33|Excepción|Indica que se ha producido una excepción en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|34|SP:CacheMiss|Indica que no se ha encontrado un procedimiento almacenado en la caché de procedimientos.|  
|35|SP:CacheInsert|Indica que se ha insertado un elemento en la caché de procedimientos.|  
|36|SP:CacheRemove|Indica que se ha eliminado un elemento de la caché de procedimientos.|  
|37|SP:Recompile|Indica que se ha vuelto a compilar un procedimiento almacenado.|  
|38|SP:CacheHit|Indica que se ha encontrado un procedimiento almacenado en la caché de procedimientos.|  
|39|Obsoleto|Obsoleto|  
|40|SQL:StmtStarting|Se produce cuando se ha iniciado la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|41|SQL:StmtCompleted|Se produce cuando se ha completado la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|42|SP:Starting|Indica el inicio de un procedimiento almacenado.|  
|43|SP:Completed|Indica la conclusión de un procedimiento almacenado.|  
|44|SP:StmtStarting|Indica que se ha iniciado la ejecución de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] de un procedimiento almacenado.|  
|45|SP:StmtCompleted|Indica que se ha finalizado la ejecución de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] de un procedimiento almacenado.|  
|46|Object:Created|Indica que se ha creado un objeto, como para las instrucciones CREATE INDEX, CREATE TABLE o CREATE DATABASE.|  
|47|Object:Deleted|Indica que se ha eliminado un objeto, como en las instrucciones DROP INDEX o DROP TABLE.|  
|48|Reservado||  
|49|Reservado||  
|50|SQL Transaction|Realiza un seguimiento de las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] BEGIN, COMMIT, SAVE y ROLLBACK TRANSACTION.|  
|51|Scan:Started|Indica que se ha iniciado un recorrido de tabla o de índice.|  
|52|Scan:Stopped|Indica que se ha detenido un recorrido de tabla o de índice.|  
|53|CursorOpen|Indica cuándo ODBC, OLE DB o DB-Library ha abierto un cursor en una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|54|TransactionLog|Realiza un seguimiento cuando se escriben transacciones en el registro de transacciones.|  
|55|Hash Warning|Indica que una operación de hash (por ejemplo, combinación hash, agregado hash, unión hash o distinción hash) que no se procesa en una partición de búfer ha vuelto a un plan alternativo. Esto puede producirse debido a la profundidad de repetición, el sesgo de datos, las marcas de seguimiento o el recuento de bits.|  
|56-57|Reservado||  
|58|Auto Stats|Indica una actualización automática de las estadísticas indizadas.|  
|59|Lock:Deadlock Chain|Se produce para cada evento que lleva a un interbloqueo.|  
|60|Lock:Escalation|Indica que un bloqueo específico se ha convertido en un bloqueo general (por ejemplo, un bloqueo de página se ha concentrado o convertido en un bloqueo de tabla o de HoBT).|  
|61|OLE DB Errors|Indica un error OLE DB.|  
|62-66|Reservado||  
|67|Execution Warnings|Indica las advertencias producidas durante la ejecución de una instrucción o un procedimiento almacenado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|68|Showplan Text (Unencoded)|Muestra el árbol del plan de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] ejecutado.|  
|69|Sort Warnings|Indica operaciones de orden que no caben en la memoria. No incluye aquellas operaciones de orden que implican la creación de índices, solo las operaciones de orden dentro de una consulta (como las de una cláusula ORDER BY en una instrucción SELECT).|  
|70|CursorPrepare|Indica cuándo se prepara un cursor en una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] para que lo utilicen ODBC, OLE DB o DB-Library.|  
|71|Prepare SQL|ODBC, OLE DB o DB-Library ha preparado una o varias instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] para su uso.|  
|72|Exec Prepared SQL|ODBC, OLE DB o DB-Library ha ejecutado una o varias instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] preparadas.|  
|73|Unprepare SQL|ODBC, OLE DB o DB-Library ha cancelado la preparación de (eliminado) una o varias instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] preparadas.|  
|74|CursorExecute|Se ejecuta un cursor anteriormente preparado en una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] mediante ODBC, OLE DB o DB-Library.|  
|75|CursorRecompile|Un cursor abierto en una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] mediante ODBC o DB-Library se ha vuelto a compilar directamente o debido a un cambio de esquema.<br /><br /> Se desencadena para cursores ANSI y no ANSI.|  
|76|CursorImplicitConversion|[!INCLUDE[tsql](../../includes/tsql-md.md)] convierte un cursor de una instrucción [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de un tipo a otro.<br /><br /> Se desencadena para cursores ANSI y no ANSI.|  
|77|CursorUnprepare|Se cancela la preparación de un cursor (se elimina) preparado en una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] mediante ODBC, OLE DB o DB-Library.|  
|78|CursorClose|Se cierra un cursor anteriormente abierto en una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] mediante ODBC, OLE DB o DB-Library.|  
|79|Missing Column Statistics|No están disponibles las estadísticas de columna que podrían haber resultado útiles para el optimizador.|  
|80|Missing Join Predicate|Se está ejecutando una consulta que no tiene ningún predicado de combinación. Esto podría dar como resultado una consulta de ejecución prolongada.|  
|81|Server Memory Change|El uso de la memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha aumentado o ha disminuido en 1 megabyte (MB) o el 5 por ciento de la memoria máxima del servidor, el valor que sea más alto.|  
|82-91|Configurable por el usuario (0-9)|Datos de eventos definidos por el usuario.|  
|92|Data File Auto Grow|Indica que el servidor ha extendido automáticamente un archivo de datos.|  
|93|Log File Auto Grow|Indica que el servidor ha extendido automáticamente un archivo de registro.|  
|94|Data File Auto Shrink|Indica que el servidor ha reducido automáticamente un archivo de datos.|  
|95|Log File Auto Shrink|Indica que el servidor ha reducido automáticamente un archivo de registro.|  
|96|Showplan Text|Muestra el árbol del plan de consulta de la instrucción SQL desde el optimizador de consultas. Tenga en cuenta que la columna **TextData** no contiene el plan de presentación para este evento.|  
|97|Showplan All|Muestra el plan de consulta con detalles completos del tiempo de compilación de la instrucción SQL ejecutada. Tenga en cuenta que la columna **TextData** no contiene el plan de presentación para este evento.|  
|98|Showplan Statistics Profile|Muestra el plan de consulta con detalles completos del tiempo de ejecución de la instrucción SQL ejecutada. Tenga en cuenta que la columna **TextData** no contiene el plan de presentación para este evento.|  
|99|Reservado||  
|100|RPC Output Parameter|Produce valores de salida de los parámetros para cada RPC.|  
|101|Reservado||  
|102|Audit Database Scope GDR|Se produce siempre que un usuario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] emite una instrucción GRANT, DENY o REVOKE para acciones exclusivas de base de datos como la concesión de permisos en una base de datos.|  
|103|Audit Object GDR Event|Se produce cada vez que un usuario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] emite GRANT, DENY, REVOKE para un permiso de objeto.|  
|104|Audit AddLogin Event|Se produce cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se agrega o se quita un inicio de sesión; por **sp_addlogin** y **sp_droplogin**.|  
|105|Audit Login GDR Event|Se produce cuando se agrega o se quita un derecho de inicio de sesión de Windows. por **sp_grantlogin**, **sp_revokelogin** y **sp_denylogin**.|  
|106|Audit Login Change Property Event|Se produce cuando se modifica una propiedad de un inicio de sesión, excepto las contraseñas; para **sp_defaultdb** y **sp_defaultlanguage**.|  
|107|Audit Login Change Password Event|Se produce cuando se cambia una contraseña de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Las contraseñas no se registran.|  
|108|Audit Add Login to Server Role Event|Se produce cuando se agrega o se quita un inicio de sesión de un rol fijo de servidor; por **sp_addsrvrolemember** y **sp_dropsrvrolemember**.|  
|109|Audit Add DB User Event|Se produce cuando se agrega o se quita un inicio de sesión como un usuario de base de datos (Windows o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ) en una base de datos; por **sp_grantdbaccess**, **sp_revokedbaccess**, **sp_adduser** y **sp_dropuser**.|  
|110|Audit Add Member to DB Role Event|Se produce cuando se agrega o se quita un inicio de sesión como un usuario de base de datos (fijo o definido por el usuario) en una base de datos; por **sp_addrolemember**, **sp_droprolemember** y **sp_changegroup**.|  
|111|Audit Add Role Event|Se produce cuando se agrega o se quita un inicio de sesión como un usuario de base de datos en una base de datos; para **sp_addrole** y **sp_droprole**.|  
|112|Audit App Role Change Password Event|Se produce cuando se cambia una contraseña de un rol de aplicación.|  
|113|Audit Statement Permission Event|Se produce cuando se utiliza un permiso de instrucción (como CREATE TABLE).|  
|114|Audit Schema Object Access Event|Se produce cuando se utiliza un permiso de objeto (como SELECT), tanto con éxito como con error.|  
|115|Audit Backup/Restore Event|Se produce cuando se emite un comando BACKUP o RESTORE.|  
|116|Audit DBCC Event|Se produce cuando se emiten comandos DBCC.|  
|117|Audit Change Audit Event|Se produce cuando se realizan modificaciones en el seguimiento de auditoría.|  
|118|Audit Object Derived Permission Event|Tiene lugar cuando se emiten los comandos de objeto CREATE, ALTER y DROP.|  
|119|OLEDB Call Event|Se produce cuando las llamadas de proveedor OLE DB se realizan para consultas distribuidas y procedimientos almacenados remotos.|  
|120|OLEDB QueryInterface Event|Se produce cuando se realizan OLE DB llamadas **QueryInterface** para consultas distribuidas y procedimientos almacenados remotos.|  
|121|OLEDB DataRead Event|Se produce cuando se realiza una llamada de solicitud de datos al proveedor OLE DB.|  
|122|Showplan XML|Se produce cuando se ejecuta una instrucción SQL. Incluya este evento para identificar los operadores de plan de presentación. Cada evento se almacena en un documento XML correcto. Tenga en cuenta que la columna **binaria** de este evento contiene el plan de presentación codificado. Use SQL Server Profiler para abrir el seguimiento y ver el plan de presentación.|  
|123|SQL:FullTextQuery|Se produce al ejecutar una consulta de texto completo.|  
|124|Broker:Conversation|Informa del progreso de una conversación de [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|125|Deprecation Announcement|Se produce cuando el usuario utiliza una característica que se quitará de versiones futuras de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|126|Deprecation Final Support|Se produce cuando el usuario utiliza una característica que se quitará de la próxima versión principal de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|127|Exchange Spill Event|Tiene lugar cuando los búferes de comunicación de un plan de consulta paralelo se han escrito temporalmente en la base de datos **tempdb** .|  
|128|Audit Database Management Event|Se produce al crear, modificar o quitar una base de datos.|  
|129|Audit Database Object Management Event|Se produce al ejecutar una instrucción CREATE, ALTER o DROP en objetos de base de datos como, por ejemplo, esquemas.|  
|130|Audit Database Principal Management Event|Se produce al crear, modificar o quitar entidades de seguridad, como usuarios, en una base de datos.|  
|131|Audit Schema Object Management Event|Se produce al crear, modificar o quitar objetos de servidor.|  
|132|Audit Server Principal Impersonation Event|Se produce cuando hay una suplantación en el ámbito del servidor, como EXECUTE AS LOGIN.|  
|133|Audit Database Principal Impersonation Event|Se produce cuando hay una suplantación en el ámbito de la base de datos, como EXECUTE AS USER o SETUSER.|  
|134|Audit Server Object Take Ownership Event|Se produce cuando se cambia el propietario de objetos en el ámbito del servidor.|  
|135|Audit Database Object Take Ownership Event|Se produce cuando se cambia el propietario de objetos en el ámbito de la base de datos.|  
|136|Broker:Conversation Group|Se produce cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un grupo de conversación o quita un grupo de conversación existente.|  
|137|Blocked Process Report|Se produce cuando un proceso ha estado bloqueado durante más tiempo del especificado. No incluye procesos del sistema ni procesos que esperan en recursos no detectables por interbloqueo. Use **sp_configure** para configurar el umbral y la frecuencia con que se generan los informes.|  
|138|Broker:Connection|Informa del estado de una conexión de transporte administrada por [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|139|Broker:Forwarded Message Sent|Se produce cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] reenvía un mensaje.|  
|140|Broker:Forwarded Message Dropped|Se produce cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] quita un mensaje pensado para reenviarse.|  
|141|Broker:Message Classify|Se produce cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] determina el enrutamiento de un mensaje.|  
|142|Broker:Transmission|Indica que se han producido errores en la capa de transporte de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Los valores del estado y del número de error indican el origen del mismo.|  
|143|Broker:Queue Disabled|Indica que se detectó un mensaje dudoso porque se produjeron cinco reversiones de transacción seguidas en una cola de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. El evento contiene el Id. de base de datos y el Id. de cola de la cola que contiene el mensaje dudoso.|  
|144-145|Reservado||  
|146|Showplan XML Statistics Profile|Se produce cuando se ejecuta una instrucción SQL. Identifica los operadores de plan de presentación y muestra todos los datos de tiempo de compilación. Tenga en cuenta que la columna **binaria** de este evento contiene el plan de presentación codificado. Use SQL Server Profiler para abrir el seguimiento y ver el plan de presentación.|  
|148|Deadlock Graph|Se produce cuando se cancela un intento de obtener un bloqueo porque dicho intento forma parte de un interbloqueo y se ha elegido como sujeto del interbloqueo. Proporciona una descripción en XML de un interbloqueo.|  
|149|Broker:Remote Message Acknowledgement|Se produce cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] envía o recibe un reconocimiento de mensaje.|  
|150|Trace File Close|Se produce cuando se cierra un archivo de seguimiento durante una reversión del mismo.|  
|151|Reservado||  
|152|Audit Change Database Owner|Se produce cuando se utiliza ALTER AUTHORIZATION para cambiar el propietario de una base de datos y se comprueban los permisos para realizar dicha acción.|  
|153|Audit Schema Object Take Ownership Event|Se produce cuando se utiliza ALTER AUTHORIZATION para asignar un propietario a un objeto y se comprueban los permisos para realizar dicha acción.|  
|154|Reservado||  
|155|FT:Crawl Started|Se produce cuando se inicia un rastreo de texto completo (llenado). Utilice esta clase de evento para comprobar si las tareas de trabajo están recogiendo una solicitud de rastreo.|  
|156|FT:Crawl Stopped|Se produce cuando se detiene un rastreo de texto completo (llenado). La detención se debe a un rastreo finalizado correctamente o a un error irrecuperable.|  
|157|FT:Crawl Aborted|Se produce cuando se ha detectado una excepción durante un rastreo de texto completo. Normalmente, el error hará que se detenga el rastreo de texto completo.|  
|158|Audit Broker Conversation|Informa de los mensajes de auditoría relacionados con la seguridad de diálogo de [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|159|Audit Broker Login|Informa de los mensajes de auditoría relacionados con la seguridad de transporte de [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|160|Broker:Message Undeliverable|Se produce cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] no puede retener un mensaje recibido que debería haberse entregado a un servicio.|  
|161|Broker:Corrupted Message|Se produce cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] recibe un mensaje dañado.|  
|162|User Error Message|Muestra los mensajes de error tal y como los ven los usuarios cuando se produce un error o una excepción.|  
|163|Broker:Activation|Se produce cuando un monitor de cola inicia un procedimiento almacenado de activación o envía una notificación QUEUE_ACTIVATION, o cuando un procedimiento almacenado de activación iniciado por un monitor de cola se cierra.|  
|164|Object:Altered|Se produce cuando se modifica un objeto de base de datos.|  
|165|Performance statistics|Se produce cuando un plan de consulta compilado se ha almacenado en caché por primera vez, se ha vuelto a compilar o se ha expulsado de la caché del plan.|  
|166|SQL:StmtRecompile|Se produce al realizar nuevas compilaciones de instrucción.|  
|167|Database Mirroring State Change|Se produce cuando cambia el estado de una base de datos reflejada.|  
|168|Showplan XML For Query Compile|Se produce cuando se compila una instrucción SQL. Muestra todos los datos de tiempo de compilación. Tenga en cuenta que la columna **binaria** de este evento contiene el plan de presentación codificado. Use SQL Server Profiler para abrir el seguimiento y ver el plan de presentación.|  
|169|Showplan All For Query Compile|Se produce cuando se compila una instrucción SQL. Muestra datos completos de tiempo de compilación. Utilice este evento para identificar los operadores de plan de presentación.|  
|170|Audit Server Scope GDR Event|Indica que se ha producido un evento de concesión, denegación o revocación para los permisos en el ámbito del servidor, como la creación de un inicio de sesión.|  
|171|Audit Server Object GDR Event|Indica que se ha producido un evento de concesión, denegación o revocación para un objeto de esquema, como una tabla o función.|  
|172|Audit Database Object GDR Event|Indica que se ha producido un evento de concesión, denegación o revocación para objetos de base de datos, como ensamblados y esquemas.|  
|173|Audit Server Operation Event|Se produce cuando se utilizan operaciones de auditoría de seguridad, como la modificación de la configuración, los recursos, el acceso externo o la autorización.|  
|175|Audit Server Alter Trace Event|Se produce cuando una instrucción busca el permiso ALTER TRACE.|  
|176|Audit Server Object Management Event|Se produce al crear, modificar o quitar objetos de servidor.|  
|177|Audit Server Principal Management Event|Se produce al crear, modificar o quitar entidades de seguridad de servidor.|  
|178|Audit Database Operation Event|Se produce cuando tienen lugar operaciones en una base de datos, como un punto de comprobación o una notificación de consulta de suscripción.|  
|180|Audit Database Object Access Event|Se produce cuando se tiene acceso a objetos de base de datos, como esquemas.|  
|181|TM: Begin Tran starting|Se produce cuando se inicia una solicitud BEGIN TRANSACTION.|  
|182|TM: Begin Tran completed|Se produce cuando se completa una solicitud BEGIN TRANSACTION.|  
|183|TM: Promote Tran starting|Se produce cuando se inicia una solicitud PROMOTE TRANSACTION.|  
|184|TM: Promote Tran completed|Se produce cuando se completa una solicitud PROMOTE TRANSACTION.|  
|185|TM: Commit Tran starting|Se produce cuando se inicia una solicitud COMMIT TRANSACTION.|  
|186|TM: Commit Tran completed|Se produce cuando se completa una solicitud COMMIT TRANSACTION.|  
|187|TM: Rollback Tran starting|Se produce cuando se inicia una solicitud ROLLBACK TRANSACTION.|  
|188|TM: Rollback Tran completed|Se produce cuando se completa una solicitud ROLLBACK TRANSACTION.|  
|189|Lock: timeout (tiempo de espera > 0)|Se produce cuando se agota el tiempo de espera para una solicitud de bloqueo en un recurso, como una página.|  
|190|Progress Report: Online Index Operation|Informa del progreso de una operación de generación de índice en línea mientras está en ejecución.|  
|191|TM: Save Tran starting|Se produce cuando se inicia una solicitud SAVE TRANSACTION.|  
|192|TM: Save Tran completed|Se produce cuando se completa una solicitud SAVE TRANSACTION.|  
|193|Background Job Error|Se produce cuando un trabajo en segundo plano finaliza de forma anómala.|  
|194|OLEDB Provider Information|Se produce cuando una consulta distribuida se ejecuta y recopila información correspondiente a la conexión del proveedor.|  
|195|Mount Tape|Se produce cuando se recibe una solicitud de montaje de cinta.|  
|196|Assembly Load|Se produce cuando se ejecuta una solicitud para cargar un ensamblado CLR.|  
|197|Reservado||  
|198|XQuery Static Type|Se produce cuando se ejecuta una expresión XQuery. Esta clase de eventos proporciona el tipo estático de la expresión XQuery.|  
|199|QN: subscription|Se produce cuando no se puede suscribir un registro de consulta. La columna **TextData** contiene información sobre el evento.|  
|200|QN: parameter table|La información sobre las suscripciones activas se almacena en tablas de parámetros internos. Esta clase de evento se produce al crear o eliminar una tabla de parámetros. Normalmente, estas tablas se crean o eliminan al reiniciar la base de datos. La columna **TextData** contiene información sobre el evento.|  
|201|QN: template|Una plantilla de consulta representa una clase de consultas de suscripción. Normalmente, las consultas de la misma clase son idénticas, excepto por los valores de los parámetros. Esta clase de evento se produce cuando una solicitud de suscripción nueva pertenece a una clase ya existente (Match), a una nueva clase (Create) o a una clase Drop, que indica la limpieza de plantillas de clases de consulta sin suscripciones activas. La columna **TextData** contiene información sobre el evento.|  
|202|QN: dynamics|Hace un seguimiento de las actividades internas de las notificaciones de consulta. La columna **TextData** contiene información sobre el evento.|  
|212|Bitmap Warning|Indica que los filtros de mapas de bits se han deshabilitado en una consulta.|  
|213|Database Suspect Data Page|Indica cuándo se agrega una página a la tabla **suspect_pages** en **msdb**.|  
|214|CPU Threshold Exceeded|Indica cuándo el regulador de recursos detecta que una consulta ha superado el valor de umbral de CPU (REQUEST_MAX_CPU_TIME_SEC).|  
|215|PreConnect:Starting|Indica cuándo se ha iniciado la ejecución de un desencadenador LOGON o de una función clasificadora del regulador de recursos.|  
|216|PreConnect:Completed|Indica cuándo se ha conpletado la ejecución de un desencadenador LOGON o de una función clasificadora del regulador de recursos.|  
|217|Guía de plan correcta|Indica que SQL Server generó correctamente un plan de ejecución para una consulta o lote que contenía una guía de plan.|  
|218|Guía de plan incorrecta|Indica que SQL Server no pudo generar un plan de ejecución para una consulta o lote que contenía una guía de plan. SQL Server intentó generar un plan de ejecución para esta consulta o lote sin aplicar la guía de plan. Una guía de plan no válida puede ser la causa de este problema. Puede utilizar la función del sistema sys.fn_validate_plan_guide para validar la guía de plan.|  
|235|Audit Fulltext||  
  
`[ @columnid = ] column_id` Es el identificador de la columna que se va a agregar para el evento. *column_id* es de **tipo int** y no tiene ningún valor predeterminado.  
  
 En la tabla siguiente se muestra una lista de las columnas que pueden agregarse para un evento.  
  
|Número de columna|Nombre de la columna|Descripción|  
|-------------------|-----------------|-----------------|  
|1|**TextData**|Valor de texto que depende de la clase de eventos que se captura en el seguimiento.|  
|2|**BinaryData**|Valor binario que depende de la clase de eventos que se captura en el seguimiento.|  
|3|**DatabaseID**|IDENTIFICADOR de la base de datos especificada por la instrucción USE *Database* o la base de datos predeterminada si no se emite la instrucción use *Database* para una conexión determinada.<br /><br /> El valor para una base de datos se puede determinar mediante la función DB_ID.|  
|4|**TransactionID**|Id. de la transacción asignado por el sistema.|  
|5|**LineNumber**|Contiene el número de la línea que incluye el error. En eventos que implican instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] , como **SP:StmtStarting**, **LineNumber** contiene el número de línea de la instrucción en el procedimiento almacenado o lote.|  
|6|**NTUserName**|Nombre del usuario de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.|  
|7|**NTDomainName**|Dominio de Windows al que pertenece el usuario.|  
|8|**HostName**|Nombre del equipo cliente que ha originado la solicitud.|  
|9|**ClientProcessID**|Id. asignado por el equipo cliente al proceso en el que se ejecuta la aplicación cliente.|  
|10|**ApplicationName**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|  
|11|**LoginName**|Nombre de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del cliente.|  
|12|**SPID**|Identificador de proceso del servidor que asigna [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al proceso relacionado con el cliente.|  
|13|**Duración**|Cantidad de tiempo transcurrido (en microsegundos) que tarda el evento. Esta columna de datos no se llena con el evento Hash Warning.|  
|14|**StartTime**|Hora a la que se inició el evento, si está disponible.|  
|15|**EndTime**|Hora a la que finalizó el evento. Esta columna no se llena para las clases de eventos de inicio, como **SQL:BatchStarting** o **SP:Starting**. Tampoco se rellena mediante el evento de **ADVERTENCIA hash** .|  
|16|**Reads**|Número de lecturas lógicas de disco que realiza el servidor en nombre del evento. Esta columna no se rellena con el evento **Lock: released** .|  
|17|**Writes**|Número de escrituras físicas de disco que realiza el servidor en nombre del evento.|  
|18|**CPU**|Cantidad de tiempo de CPU (en milisegundos) que utiliza el evento.|  
|19|**Permisos**|Representa el mapa de bits de permisos; utilizado por Auditoría de seguridad.|  
|20|**Gravedad**|Nivel de gravedad de una excepción.|  
|21|**EventSubClass**|Tipo de la subclase de eventos. Esta columna de datos no se llena para todas las clases de evento.|  
|22|**ObjectID**|Identificador del objeto asignado por el sistema.|  
|23|**Success**|Utilización de permisos correcta; se utiliza para la auditoría.<br /><br /> **1** = correcto **0** = error|  
|24|**IndexID**|Id. del índice del objeto afectado por el evento. Para determinar el Id. de índice de un objeto, utilice la columna **indid** de la tabla del sistema **sysindexes** .|  
|25|**IntegerData**|Valor entero que depende de la clase de eventos capturada en el seguimiento.|  
|26|**ServerName**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ya sea *ServerName* o *nombredeservidor\nombredeinstancia*, de la que se realiza un seguimiento.|  
|27|**EventClass**|Tipo de clase de evento que se está registrando.|  
|28|**ObjectType**|Tipo de objeto, por ejemplo: tabla, función o procedimiento almacenado.|  
|29|**NestLevel**|Nivel de anidamiento en el que se ejecuta este procedimiento almacenado. Vea [@ @NESTLEVEL &#40;TRANSACT-SQL&#41;](../../t-sql/functions/nestlevel-transact-sql.md).|  
|30|**Estado**|Estado del servidor, si se produce un error.|  
|31|**Error**|Número de error.|  
|32|**Modo**|Modo de bloqueo del bloqueo adquirido. Esta columna no se rellena con el evento **Lock: released** .|  
|33|**Handle**|Identificador del objeto al que se hace referencia en el evento.|  
|34|**ObjectName**|Nombre del objeto al que se obtiene acceso.|  
|35|**DatabaseName**|Nombre de la base de datos especificada en la instrucción USE *Database* .|  
|36|**FileName**|Nombre lógico del nombre de archivo modificado.|  
|37|**OwnerName**|Nombre del propietario del objeto al que se hace referencia.|  
|38|**RoleName**|Nombre de la base de datos o del rol de todo el servidor que es el destino de una instrucción.|  
|39|**TargetUserName**|Nombre de usuario del destino de alguna acción.|  
|40|**DBUserName**|Nombre de usuario de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del cliente.|  
|41|**LoginSid**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión.|  
|42|**TargetLoginName**|Nombre de inicio de sesión del destino de alguna acción.|  
|43|**TargetLoginSid**|SID del inicio de sesión que es el destino de alguna acción.|  
|44|**ColumnPermissions**|Estado de los permisos de nivel de columna; utilizado por Auditoría de seguridad.|  
|45|**LinkedServerName**|Nombre del servidor vinculado.|  
|46|**ProviderName**|Nombre del proveedor OLE DB.|  
|47|**MethodName**|Nombre del método OLE DB.|  
|48|**RowCounts**|Número de filas del lote.|  
|49|**IdSolicitud**|Identificador de la solicitud que contiene la instrucción.|  
|50|**XactSequence**|Token que describe la transacción actual.|  
|51|**EventSequence**|Número de secuencia de este evento.|  
|52|**BigintData1**|valor **BIGINT** , que depende de la clase de evento capturada en el seguimiento.|  
|53|**BigintData2**|valor **BIGINT** , que depende de la clase de evento capturada en el seguimiento.|  
|54|**GUID**|Valor GUID que depende de la clase de evento capturado en el seguimiento.|  
|55|**IntegerData2**|Valor entero, que depende de la clase de evento capturada en el seguimiento.|  
|56|**ObjectID2**|Id. de la entidad u objeto relacionado si está disponible.|  
|57|**Type**|Valor entero, que depende de la clase de evento capturada en el seguimiento.|  
|58|**OwnerID**|Tipo de objeto propietario de un bloqueo. Solo para eventos de bloqueo.|  
|59|**ParentName**|Nombre del esquema en el que se encuentra el objeto.|  
|60|**IsSystem**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario.<br /><br /> **1** = sistema<br /><br /> **0** = usuario.|  
|61|**Offset**|Desplazamiento inicial de la instrucción en el procedimiento almacenado o lote.|  
|62|**SourceDatabaseID**|Id. de la base de datos en la que se encuentra el origen del objeto.|  
|63|**SqlHandle**|Hash de 64 bits basado en el texto de una consulta ad hoc o en el Id. de base de datos y de objeto de un objeto SQL. Este valor puede pasarse a **sys.dm_exec_sql_text()** para recuperar el texto SQL asociado.|  
|64|**SessionLoginName**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con **inicioDeSesión1** y ejecuta una instrucción como **inicioDeSesión2**, **SessionLoginName** muestra **inicioDeSesión1** y **LoginName** muestra **inicioDeSesión2**. En esta columna de datos se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|  
  
 **[ @on =]** *en*  
 Especifica la activación o desactivación del evento mediante ON (1) u OFF (0). *on es de* **bit** y no tiene ningún valor predeterminado.  
  
 Si *on* está establecido en **1** y *column_id* es null, el evento se establece en on y se borran todas las columnas. Si *column_id* no es null, la columna se establece en ON para ese evento.  
  
 Si *on* está establecido en **0** y *column_id* es null, el evento se desactiva y se borran todas las columnas. Si *column_id* no es null, la columna se desactiva.  
  
 En esta tabla se muestra la interacción entre **\@ on** y **\@ columnid**.  
  
|\@en|\@columnid|Resultado|  
|---------|---------------|------------|  
|ON (**1**)|NULL|El evento está activado.<br /><br /> Se borran todas las columnas.|  
||NOT NULL|La columna está activada para el evento especificado.|  
|OFF (**0**)|NULL|El evento está desactivado.<br /><br /> Se borran todas las columnas.|  
||NOT NULL|La columna está desactivada para el evento especificado.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 En la tabla siguiente se describen los valores del código que los usuarios pueden obtener después de completar el procedimiento almacenado.  
  
|Código devuelto|Descripción|  
|-----------------|-----------------|  
|0|Ningún error.|  
|1|Error desconocido.|  
|2|El seguimiento está actualmente en ejecución. El cambio del seguimiento en este momento generará un error.|  
|3|El evento especificado no es válido. Puede que el evento no exista o que no sea adecuado para el procedimiento almacenado.|  
|4|La columna especificada no es válida.|  
|9|El identificador de seguimiento especificado no es válido.|  
|11|La columna especificada está utilizándose internamente y no puede eliminarse.|  
|13|Memoria insuficiente Se devuelve cuando no hay memoria suficiente para realizar la acción especificada.|  
|16|La función no es válida para este seguimiento.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_trace_setevent** realiza muchas de las acciones ejecutadas previamente por procedimientos almacenados extendidos disponibles en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Use **sp_trace_setevent** en lugar de lo siguiente:  
  
-   **xp_trace_addnewqueue**  
  
-   **xp_trace_eventclassrequired**  
  
-   **xp_trace_seteventclassrequired**  
  
 Los usuarios deben ejecutar **sp_trace_setevent** para cada columna agregada para cada evento. Durante cada ejecución, si **\@ on** está establecido en **1**, **sp_trace_setevent** agrega el evento especificado a la lista de eventos del seguimiento. Si **\@ on** está establecido en **0**, **sp_trace_setevent** quita el evento especificado de la lista.  
  
 Los parámetros de todos los procedimientos almacenados de seguimiento de SQL (**sp_trace_xx**) tienen un tipo estricto. Si no se llama a estos parámetros con los tipos de datos de parámetros de entrada correctos, según se especifica en la descripción del argumento, el procedimiento almacenado devolverá un error.  
  
 Para obtener un ejemplo de cómo usar los procedimientos almacenados de seguimiento, vea [Crear un seguimiento &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 El usuario debe tener permiso ALTER TRACE.  
  
## <a name="see-also"></a>Consulte también  
 [sys.fn_trace_geteventinfo &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [sp_trace_generateevent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [Referencia de las clase de eventos de SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md)   
 [Seguimiento de SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
