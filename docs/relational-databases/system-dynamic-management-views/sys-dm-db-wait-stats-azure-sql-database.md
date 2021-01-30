---
description: sys.dm_db_wait_stats (Azure SQL Database)
title: sys.dm_db_wait_stats (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- dm_db_wait_stats_TSQL
- dm_db_wait_stats
- sys.dm_db_wait_stats
- sys.dm_db_wait_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_wait_stats dynamic management view
- dm_db_wait_stats
ms.assetid: 00abd0a5-bae0-4d71-b173-f7a14cddf795
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: 9787c91301678463ee324b6c6fa473b03af61a80
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99109142"
---
# <a name="sysdm_db_wait_stats-azure-sql-database"></a>sys.dm_db_wait_stats (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Devuelve información acerca de todas las esperas encontradas por los subprocesos ejecutados durante la operación. Puede utilizar esta vista agregada para diagnosticar problemas de rendimiento con [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y también con lotes y consultas específicos.  
  
 Determinados tipos de tiempos de espera durante la ejecución de consultas pueden indicar cuellos de botella o puntos de pausa en la consulta. De forma similar, tiempos de espera altos, o contadores de espera en todo el servidor pueden indicar cuellos de botella o puntos de actividad en interacciones de consultas de interacción en la instancia del servidor. Por ejemplo, las esperas de bloqueos indican la contención de datos por las consultas; las esperas de bloqueos temporales de E/S de páginas indican tiempos de respuesta de E/S bajos; las esperas de actualizaciones de bloqueos temporales de páginas indican un diseño de archivo incorrecto.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|wait_type|**nvarchar(60)**|Nombre del tipo de espera. Para obtener más información, vea [Tipos de esperas](#WaitTypes), más adelante en este tema.|  
|waiting_tasks_count|**bigint**|Número de esperas de este tipo de espera. Este recuento se incrementa al inicio de cada espera.|  
|wait_time_ms|**bigint**|Tiempo total de espera de este tipo en milisegundos. Este tiempo incluye el tiempo de signal_wait_time_ms.|  
|max_wait_time_ms|**bigint**|Tiempo de espera máximo de este tipo de espera.|  
|signal_wait_time_ms|**bigint**|Diferencia entre el momento en que se indicó el subproceso en espera y el momento en que empezó a ejecutarse.|  
  
## <a name="remarks"></a>Observaciones  
  
-   Esta vista de administración dinámica solo muestra datos para la base de datos actual.  
  
-   Esta vista de administración dinámica muestra el tiempo de las esperas que se han completado. No muestra las esperas actuales.  
  
-   Se restablecen los contadores a cero siempre que la base de datos se mueve o se pone sin conexión.  
  
-   Un subproceso de trabajo de SQL Server no se considera en espera si se produce alguna de las situaciones siguientes:  
  
    -   Un recurso pasa a estar disponible.  
  
    -   Una cola no está vacía.  
  
    -   Un proceso externo finaliza.  
  
-   Estas estadísticas no permanecen en los eventos de conmutación por error de SQL Database, y todos los datos se acumulan desde la última vez que se restablecieron las estadísticas.  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW DATABASE STATE en el servidor.  
  
##  <a name="types-of-waits"></a><a name="WaitTypes"></a> Tipos de esperas  
 Esperas de recursos  
 Las esperas de recursos tienen lugar cuando un trabajador solicita acceso a un recurso que no está disponible porque otro trabajador lo está utilizando o aún no está disponible. Algunos ejemplos de esperas de recursos son los bloqueos, bloqueos temporales y esperas de red y E/S de disco. Las esperas de bloqueos y bloqueos temporales son esperas en objetos de sincronización.  
  
 Esperas de colas  
 Las esperas de colas tienen lugar cuando un trabajador está inactivo, esperando que se le asigne un trabajo. Las esperas de colas se ven normalmente con tareas en segundo plano del sistema como las tareas de supervisión de interbloqueos y de limpieza de registros eliminados. Estas tareas esperarán a que las solicitudes de trabajo se pongan en una cola de trabajo. Las esperas de colas también pueden activarse periódicamente incluso si no se han puesto en cola paquetes nuevos.  
  
 Esperas externas  
 Las esperas externas tienen lugar cuando un trabajador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espera que se termine un evento externo, como una llamada a un procedimiento almacenado extendido o una consulta de servidor vinculado. Cuando diagnostique problemas de bloqueo, recuerde que las esperas externas no siempre implican que el trabajador esté inactivo, porque se puede ejecutar activamente algún código externo.  
  
 Aunque el subproceso ya no esté en espera, no tiene que empezar a ejecutarse inmediatamente. Esto se debe a que un subproceso primero se pone en la cola de trabajadores ejecutables y debe esperar a que se ejecute un cuanto en el programador.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , los contadores de tiempo de espera son valores **BIGINT** y, por tanto, no son tan propensos a la sustitución de contadores que los contadores equivalentes en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 En la tabla siguiente se muestran los tipos de espera encontrados por las tareas.  
  
|Tipo de espera|Descripción|  
|---------------|-----------------|  
|ABR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|ASSEMBLY_LOAD|Tiene lugar durante el acceso exclusivo a la carga de ensamblados.|  
|ASYNC_DISKPOOL_LOCK|Tiene lugar cuando existe un intento de sincronizar subprocesos paralelos que ejecutan tareas como la creación o la inicialización de un archivo.|  
|ASYNC_IO_COMPLETION|Tiene lugar cuando una tarea está esperando que finalice una operación de E/S.|  
|ASYNC_NETWORK_IO|Tiene lugar en escrituras en la red cuando la tarea se bloquea tras la red. Comprueba que el cliente procesa datos en el servidor.|  
|AUDIT_GROUPCACHE_LOCK|Tiene lugar cuando se produce una espera en un bloqueo que controla el acceso a una caché especial. La memoria caché contiene información acerca de las auditorías que se usan para auditar cada grupo de acciones de auditoría.|  
|AUDIT_LOGINCACHE_LOCK|Tiene lugar cuando se produce una espera en un bloqueo que controla el acceso a una caché especial. La memoria caché contiene información acerca de las auditorías que se usan para auditar los grupo de acciones de auditoría de inicio de sesión.|  
|AUDIT_ON_DEMAND_TARGET_LOCK|Tiene lugar cuando se produce una espera en un bloqueo que se usa para asegurar la inicialización única de destinos de Extended Events relacionados con las auditorías.|  
|AUDIT_XE_SESSION_MGR|Tiene lugar cuando se produce una espera en un bloqueo que se usa para sincronizar el inicio y la detención de sesiones de Extended Events relacionados con las auditorías.|  
|BACKUP|Tiene lugar cuando una tarea se bloquea como parte de un proceso de copia de seguridad.|  
|BACKUP_OPERATOR|Tiene lugar cuando una tarea está esperando a que se monte una cinta. Para ver el estado de la cinta, consulta [sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md). Si no hay pendiente ninguna operación de montaje, este tiempo de espera puede indicar un problema de hardware con la unidad de cinta.|  
|BACKUPBUFFER|Tiene lugar cuando una tarea de copia de seguridad espera por datos, o espera a un búfer donde se almacenarán datos. Este tipo no es normal, excepto cuando una tarea está esperando a que se monte una cinta.|  
|BACKUPIO|Tiene lugar cuando una tarea de copia de seguridad espera por datos, o espera a un búfer donde se almacenarán datos. Este tipo no es normal, excepto cuando una tarea está esperando a que se monte una cinta.|  
|BACKUPTHREAD|Tiene lugar cuando una tarea está esperando que finalice una tarea de copia de seguridad. Los tiempos de espera pueden ser largos, desde unos minutos a varias horas. Si la tarea que se está esperando está en un proceso de E/S, este tipo no indica ningún problema.|  
|BAD_PAGE_PROCESS|Tiene lugar cuando el registrador de páginas sospechosas en segundo plano intenta impedir la ejecución más veces que cada cinco segundos. Un exceso de páginas sospechosas provoca la ejecución frecuente del registrador.|  
|BROKER_CONNECTION_RECEIVE_TASK|Tiene lugar cuando se espera el acceso para recibir un mensaje en el extremo de una conexión. El acceso de recepción al extremo está serializado.|  
|BROKER_ENDPOINT_STATE_MUTEX|Tiene lugar cuando se produce una contención en el acceso al estado de un extremo de conexión de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. El acceso al estado de los cambios está serializado.|  
|BROKER_EVENTHANDLER|Tiene lugar cuando una tarea está esperando en el controlador de eventos principal del [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Debería ocurrir brevemente.|  
|BROKER_INIT|Tiene lugar al inicializar [!INCLUDE[ssSB](../../includes/sssb-md.md)] en cada base de datos activa. No debería ocurrir con frecuencia.|  
|BROKER_MASTERSTART|Tiene lugar cuando una tarea está esperando que se inicie el controlador de eventos principal del [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Debería ocurrir brevemente.|  
|BROKER_RECEIVE_WAITFOR|Tiene lugar cuando RECEIVE WAITFOR está en espera. Es normal si no hay mensajes preparados para recibirse.|  
|BROKER_REGISTERALLENDPOINTS|Tiene lugar durante la inicialización de un extremo de conexión de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Debería ocurrir brevemente.|  
|BROKER_SERVICE|Tiene lugar cuando se actualiza o se vuelve a establecer la prioridad de la lista de destinos de [!INCLUDE[ssSB](../../includes/sssb-md.md)] asociada con un servicio de destino.|  
|BROKER_SHUTDOWN|Tiene lugar cuando existe un apagado planeado de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Debería ocurrir brevemente, si ocurre.|  
|BROKER_TASK_STOP|Tiene lugar cuando el controlador de tareas de cola de [!INCLUDE[ssSB](../../includes/sssb-md.md)] intenta cerrar la tarea. La comprobación de estado se serializa y debe estar en estado de ejecución de antemano.|  
|BROKER_TO_FLUSH|Tiene lugar cuando el dispositivo de vaciado lento de [!INCLUDE[ssSB](../../includes/sssb-md.md)] vacía los objetos de transmisión en memoria a una tabla de trabajo.|  
|BROKER_TRANSMITTER|Tiene lugar cuando el transmisor de [!INCLUDE[ssSB](../../includes/sssb-md.md)] está esperando trabajo.|  
|BUILTIN_HASHKEY_MUTEX|Puede producirse después del inicio de una instancia, mientras se inicializan las estructuras de datos internas. No se producirá después de la inicialización de las estructuras de datos.|  
|CHECK_PRINT_RECORD|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|CHECKPOINT_QUEUE|Tiene lugar mientras la tarea del punto de comprobación está esperando la siguiente solicitud de punto de comprobación.|  
|CHKPT|Tiene lugar en el inicio del servidor para indicar al subproceso de punto de comprobación que puede iniciarse.|  
|CLEAR_DB|Tiene lugar durante las operaciones que cambian el estado de una base de datos, como la apertura o el cierre de una base de datos.|  
|CLR_AUTO_EVENT|Tiene lugar cuando una tarea está realizando actualmente la ejecución de Common Language Runtime (CLR) y espera que se inicie un evento automático determinado. Son habituales las esperas largas y no indican ningún problema.|  
|CLR_CRST|Tiene lugar cuando una tarea está realizando actualmente una ejecución CLR y espera entrar en una sección crítica de la tarea que utiliza actualmente otra tarea.|  
|CLR_JOIN|Tiene lugar cuando una tarea está realizando actualmente la ejecución de CLR y espera a que finalice otra tarea. Este estado de espera tiene lugar cuando existe una combinación entre tareas.|  
|CLR_MANUAL_EVENT|Tiene lugar cuando una tarea está realizando actualmente la ejecución de CLR y espera que se inicie un evento manual específico.|  
|CLR_MEMORY_SPY|Tiene lugar durante la espera de una adquisición de bloqueo para una estructura de datos que se usa para registrar todas las asignaciones de memoria virtual que proceden de CLR. La estructura de datos se bloquea para mantener su integridad si existe un acceso paralelo.|  
|CLR_MONITOR|Tiene lugar cuando una tarea está realizando actualmente la ejecución de CLR y espera para obtener un bloqueo en la supervisión.|  
|CLR_RWLOCK_READER|Tiene lugar cuando una tarea está realizando actualmente la ejecución de CLR y espera un bloqueo del lector.|  
|CLR_RWLOCK_WRITER|Tiene lugar cuando una tarea está realizando actualmente la ejecución de CLR y espera un bloqueo del escritor.|  
|CLR_SEMAPHORE|Tiene lugar cuando una tarea está realizando actualmente la ejecución de CLR y espera un semáforo.|  
|CLR_TASK_START|Tiene lugar mientras se espera que una tarea CLR complete el inicio.|  
|CLRHOST_STATE_ACCESS|Tiene lugar cuando se produce una espera para obtener acceso exclusivo a las estructuras de datos que hospedan el CLR. Este tipo de espera se produce al configurar o anular el motor en tiempo de ejecución de CRL.|  
|CMEMTHREAD|Tiene lugar cuando una tarea está esperando en un objeto de memoria seguro para subprocesos. El tiempo de espera puede aumentar cuando existe una contención porque muchas tareas intentan asignar memoria desde el mismo objeto de memoria.|  
|CXPACKET|Tiene lugar cuando se intenta sincronizar el iterador de intercambios del procesador de consultas. Debe pensar en reducir el grado de paralelismo si la contención en este tipo de espera llega a ser un problema.|  
|CXROWSET_SYNC|Tiene lugar durante un examen de intervalo en paralelo.|  
|DAC_INIT|Tiene lugar mientras se inicializa la conexión de administrador dedicada.|  
|DBMIRROR_DBM_EVENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|DBMIRROR_DBM_MUTEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|DBMIRROR_EVENTS_QUEUE|Tiene lugar cuando la creación de reflejo de bases de datos espera que se procesen eventos.|  
|DBMIRROR_SEND|Tiene lugar cuando una tarea está esperando un trabajo acumulado de comunicaciones en el nivel de red para limpiar y poder enviar mensajes. Indica que el nivel de comunicaciones empieza a sobrecargarse y afecta al rendimiento de la creación del reflejo de los datos de la base de datos.|  
|DBMIRROR_WORKER_QUEUE|Indica que la tarea de trabajo de creación de reflejo de bases de datos está esperando más trabajo.|  
|DBMIRRORING_CMD|Tiene lugar cuando una tarea está esperando que los registros se guarden en el disco. Este estado de espera está previsto que dure largos periodos de tiempo.|  
|DEADLOCK_ENUM_MUTEX|Tiene lugar cuando la supervisión de interbloqueos y sys.dm_os_waiting_tasks intentan asegurarse de que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no ejecuta varias búsquedas de interbloqueos simultáneamente.|  
|DEADLOCK_TASK_SEARCH|Un tiempo de espera largo en este recurso indica que el servidor está ejecutando consultas por encima de sys.dm_os_waiting_tasks y que estas consultas impiden que la supervisión de interbloqueos ejecute la búsqueda de interbloqueos. Este tipo de espera solo lo utiliza el supervisor de interbloqueos. Las consultas por encima de sys.dm_os_waiting_tasks utilizan DEADLOCK_ENUM_MUTEX.|  
|DEBUG|Tiene lugar durante la depuración de [!INCLUDE[tsql](../../includes/tsql-md.md)] y CLR para la sincronización interna.|  
|DISABLE_VERSIONING|Tiene lugar cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sondea al administrador de transacciones de versiones para comprobar si la marca de tiempo de la primera transacción activa es posterior a la marca de tiempo de cuando el estado empezó a cambiar. Si es así, todas las transacciones de instantáneas que se iniciaron antes de ejecutar la instrucción ALTER DATABASE han finalizado. Este estado de espera se utiliza cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deshabilita las versiones utilizando la instrucción ALTER DATABASE.|  
|DISKIO_SUSPEND|Tiene lugar cuando una tarea está esperando tener acceso a un archivo cuando está activa una copia de seguridad externa. Se notifica para cada proceso de usuario en espera. Un recuento mayor de cinco procesos por usuario puede indicar que la copia de seguridad externa tarda mucho en finalizarse.|  
|DISPATCHER_QUEUE_SEMAPHORE|Tiene lugar cuando un subproceso del grupo de distribuidores espera más trabajo para su procesamiento. Se prevé que el tiempo de espera de este tipo de espera aumente cuando el distribuidor esté inactivo.|  
|DLL_LOADING_MUTEX|Tiene lugar una vez mientras se espera que se cargue la DLL del analizador XML.|  
|DROPTEMP|Tiene lugar entre intentos para quitar un objeto temporal si se produjo un error en el intento anterior. La duración de la espera crece de forma exponencial en cada intento de eliminación con error.|  
|DTC|Tiene lugar cuando una tarea está esperando en un evento que se utiliza para administrar la transición de estado. Este estado controla cuándo tiene lugar la recepción de transacciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)] DTC (Coordinador de transacciones distribuidas) después de que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recibe la notificación de que el servicio MS DTC ha dejado de estar disponible.<br /><br /> Este estado también describe una tarea que está en espera cuando una confirmación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inicia una transacción de MS DTC y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espera que finalice la confirmación de MS DTC.|  
|DTC_ABORT_REQUEST|Tiene lugar en una sesión del trabajador de MS DTC cuando la sesión espera a tener la propiedad de una transacción de MS DTC. Después de que MS DTC es propietario de la transacción, la sesión puede revertir la transacción. Generalmente, la sesión esperará a otra sesión que esté utilizando la transacción.|  
|DTC_RESOLVE|Tiene lugar cuando una tarea de recepción espera a la base de datos maestra en una transacción entre bases de datos por lo que la tarea puede consultar el resultado de la transacción.|  
|DTC_STATE|Tiene lugar cuando una tarea está esperando en un evento que protege los cambios al objeto de estado global de MS DTC. Este estado debe mantenerse durante periodos muy cortos.|  
|DTC_TMDOWN_REQUEST|Tiene lugar en una sesión del trabajador de MS DTC cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recibe notificación de que el servicio MS DTC no está disponible. Primero, el trabajador esperará a que se inicie el proceso de recepción de MS DTC. A continuación, el trabajador espera a obtener el resultado de la transacción distribuida en la que está trabajando. Esto puede continuar hasta que se restablezca la conexión con el servicio de MS DTC.|  
|DTC_WAITFOR_OUTCOME|Tiene lugar cuando las tareas de recepción esperan a que MS DTC se vuelva a activar para habilitar la resolución de las transacciones preparadas.|  
|DUMP_LOG_COORDINATOR|Tiene lugar cuando una tarea principal espera que una subtarea genere datos. Normalmente, este estado no tiene lugar. Una espera larga indica un bloqueo inesperado. La subtarea debe investigarse.|  
|DUMPTRIGGER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|EC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|EE_PMOLOCK|Tiene lugar durante la sincronización de determinados tipos de asignaciones de memoria durante la ejecución de instrucciones.|  
|EE_SPECPROC_MAP_INIT|Tiene lugar durante la sincronización de la creación de una tabla hash de procedimiento interna. Esta espera solo puede producirse durante el acceso inicial de la tabla hash después de que se inicie la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|ENABLE_VERSIONING|Tiene lugar cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espera a que finalicen todas las transacciones de actualización de esta base de datos antes de declarar la base de datos preparada para pasar al estado permitido de aislamiento de instantáneas. Este estado se utiliza cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] habilita el aislamiento de instantáneas mediante la instrucción ALTER DATABASE.|  
|ERROR_REPORTING_MANAGER|Tiene lugar durante la sincronización de múltiples inicializaciones simultáneas de registros de errores.|  
|EXCHANGE|Tiene lugar en la sincronización en el iterador de intercambios del procesador de consultas, durante consultas en paralelo.|  
|EXECSYNC|Tiene lugar durante consultas en paralelo mientras se sincroniza en el procesador de consultas en áreas no relacionadas con el iterador de intercambios. Algunos ejemplos de estas áreas son mapas de bits, objetos binarios grandes (LOB) y el iterador de spool. Los LOB pueden utilizar con frecuencia este estado de espera.|  
|EXECUTION_PIPE_EVENT_INTERNAL|Tiene lugar durante la sincronización entre las partes productor y consumidor de la ejecución por lotes que se envían a través del contexto de conexión.|  
|FAILPOINT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|FCB_REPLICA_READ|Tiene lugar cuando se sincronizan las lecturas de un archivo disperso de instantáneas (o una instantánea temporal creada por DBCC).|  
|FCB_REPLICA_WRITE|Tiene lugar cuando se sincroniza la inserción o extracción de una página en un archivo disperso de instantáneas (o en una instantánea temporal creada por DBCC).|  
|FS_FC_RWLOCK|Tiene lugar cuando se produce una espera en el recolector de elementos no utilizados FILESTREAM para realizar una de las acciones siguientes:<br /><br /> Deshabilitar la recopilación de elementos no utilizados (se usa en copias de seguridad y restauración).<br /><br /> Ejecutar un ciclo del recolector de elementos no utilizados FILESTREAM.|  
|FS_GARBAGE_COLLECTOR_SHUTDOWN|Tiene lugar cuando el recolector de elementos no utilizados FILESTREAM espera hasta que se completan las tareas de limpieza.|  
|FS_HEADER_RWLOCK|Tiene lugar cuando se produce una espera para adquirir acceso al encabezado FILESTREAM de un contenedor de datos FILESTREAM al contenido de lectura o de actualización en el archivo de encabezado de FILESTREAM (Filestream.hdr).|  
|FS_LOGTRUNC_RWLOCK|Tiene lugar cuando se produce una espera para adquirir acceso al truncamiento del registro de FILESTREAM para realizar una de las acciones siguientes:<br /><br /> Deshabilitar temporalmente el truncamiento del registro de FILESTREAM (FSLOG) (se usa en copias de seguridad y restauración).<br /><br /> Ejecutar un ciclo de truncamiento de FSLOG.|  
|FSA_FORCE_OWN_XACT|Tiene lugar cuando una operación de E/S del archivo de FILESTREAM debe enlazarse con la transacción asociada pero ésta pertenece actualmente a otra sesión.|  
|FSAGENT|Tiene lugar cuando una operación de E/S del archivo de FILESTREAM espera un recurso del agente de FILESTREAM que está utilizando otra operación de E/S de archivo.|  
|FSTR_CONFIG_MUTEX|Tiene lugar cuando se produce una espera hasta que se complete la configuración de otra característica de FILESTREAM.|  
|FSTR_CONFIG_RWLOCK|Tiene lugar cuando se produce una espera para serializar el acceso a los parámetros de configuración de FILESTREAM.|  
|FT_METADATA_MUTEX|Solamente se documenta con fines informativos. No se admite. La compatibilidad con versiones posteriores no está garantizada.|  
|FT_RESTART_CRAWL|Tiene lugar cuando un rastreo de texto completo debe reiniciarse desde el último punto correcto conocido para recuperarse de un error transitorio. La espera permite que completen o abandonen el paso actual las tareas del trabajador que se están ejecutando en dicho rellenado.|  
|FULLTEXT GATHERER|Tiene lugar durante la sincronización de operaciones de texto completo.|  
|GUARDIAN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|HTTP_ENUMERATION|Tiene lugar en el inicio para enumerar los extremos HTTP al iniciar HTTP.|  
|HTTP_START|Tiene lugar cuando una conexión espera hasta que HTTP complete la inicialización.|  
|IMPPROV_IOWAIT|Tiene lugar cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espera que finalice una E/S de carga masiva.|  
|INTERNAL_TESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|IO_AUDIT_MUTEX|Tiene lugar durante la sincronización de búferes de eventos de seguimiento.|  
|IO_COMPLETION|Tiene lugar mientras se espera la finalización de operaciones de E/S. Generalmente, este tipo de espera representa operaciones de E/S de páginas que no son de datos. Las esperas de finalización de operaciones de E/S de páginas de datos aparecen como esperas PAGEIOLATCH_*.|  
|IO_QUEUE_LIMIT|Se produce cuando la cola de E/S asincrónica de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]tiene demasiadas E/S pendientes. Las tareas que intentan emitir otra E/S están bloqueadas en este tipo de espera hasta que el número de E/S pendientes caen por debajo del umbral. El umbral es proporcional a las DTU asignadas a la base de datos.|  
|IO_RETRY|Tiene lugar cuando una operación de E/S como una lectura o una escritura de disco no se realiza correctamente debido a un número insuficiente de recursos y, posteriormente, se vuelve a intentar.|  
|IOAFF_RANGE_QUEUE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KSOURCE_WAKEUP|Se utiliza en la tarea de control de servicios mientras se esperan solicitudes del Administrador de control de servicios. Se prevén esperas largas que no indican ningún problema.|  
|KTM_ENLISTMENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KTM_RECOVERY_MANAGER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KTM_RECOVERY_RESOLUTION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LATCH_DT|Tiene lugar cuando se espera un bloqueo temporal de destrucción (DT). No incluye bloqueos temporales de búfer ni de marca de transacción. En sys.dm_os_latch_stats está disponible una lista de esperas LATCH_*. Tenga en cuenta que los grupos LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX y LATCH_DT de sys.dm_os_latch_stats esperan juntos.|  
|LATCH_EX|Tiene lugar cuando se espera un bloqueo temporal exclusivo (EX). No incluye bloqueos temporales de búfer ni de marca de transacción. En sys.dm_os_latch_stats está disponible una lista de esperas LATCH_*. Tenga en cuenta que los grupos LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX y LATCH_DT de sys.dm_os_latch_stats esperan juntos.|  
|LATCH_KP|Tiene lugar cuando se espera un bloqueo temporal de mantenimiento (KP). No incluye bloqueos temporales de búfer ni de marca de transacción. En sys.dm_os_latch_stats está disponible una lista de esperas LATCH_*. Tenga en cuenta que los grupos LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX y LATCH_DT de sys.dm_os_latch_stats esperan juntos.|  
|LATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LATCH_SH|Tiene lugar cuando se espera un bloqueo temporal de uso compartido (SH). No incluye bloqueos temporales de búfer ni de marca de transacción. En sys.dm_os_latch_stats está disponible una lista de esperas LATCH_*. Tenga en cuenta que los grupos LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX y LATCH_DT de sys.dm_os_latch_stats esperan juntos.|  
|LATCH_UP|Tiene lugar cuando se espera un bloqueo temporal de actualización (UP). No incluye bloqueos temporales de búfer ni de marca de transacción. En sys.dm_os_latch_stats está disponible una lista de esperas LATCH_*. Tenga en cuenta que los grupos LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX y LATCH_DT de sys.dm_os_latch_stats esperan juntos.|  
|LAZYWRITER_SLEEP|Tiene lugar cuando se suspenden tareas de escritura diferida. Ésta es una medida del tiempo invertido por las tareas en segundo plano que esperan. No tenga en cuenta este estado cuando busque pausas del usuario.|  
|LCK_M_BU|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de actualización masiva (BU). Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_IS|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de intención compartida (IS). Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_IU|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de intención de actualización (IU). Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_IX|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de intención exclusiva (IX). Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_NL|Tiene lugar cuando una tarea está esperando adquirir un bloqueo NULL en el valor de clave actual y un bloqueo de intervalo de inserción entre la clave anterior y la actual. Un bloqueo NULL en la clave es un bloqueo de liberación instantánea. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_S|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de uso compartido en el valor de clave actual y un bloqueo de intervalo de inserción entre la clave anterior y la actual. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_U|La tarea espera adquirir un bloqueo de actualización en el valor de clave actual y un bloqueo de intervalo de inserción entre la clave anterior y la actual. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_X|Tiene lugar cuando una tarea está esperando adquirir un bloqueo exclusivo en el valor de clave actual y un bloqueo de intervalo de inserción entre la clave anterior y la actual. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RS_S|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de uso compartido en el valor de clave actual y un bloqueo de intervalo compartido entre la clave anterior y la actual. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RS_U|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de actualización en el valor de clave actual y un bloqueo de intervalo de actualización entre la clave anterior y la actual. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RX_S|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de uso compartido en el valor de clave actual y un bloqueo de intervalo exclusivo entre la clave anterior y la actual. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RX_U|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de actualización en el valor de clave actual y un bloqueo de intervalo exclusivo entre la clave anterior y la actual. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RX_X|Tiene lugar cuando una tarea está esperando adquirir un bloqueo exclusivo en el valor de clave actual y un bloqueo de intervalo exclusivo entre la clave anterior y la actual. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_S|Tiene lugar cuando una tarea está esperando a adquirir un bloqueo compartido. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SCH_M|Tiene lugar cuando una tarea está esperando a adquirir un bloqueo de modificación de esquema. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SCH_S|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de uso compartido de esquema. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SIU|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de uso compartido con intento de actualización. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SIX|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de uso compartido con intención exclusiva. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_U|Tiene lugar cuando una tarea está esperando a adquirir un bloqueo de actualización. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_UIX|Tiene lugar cuando una tarea está esperando adquirir un bloqueo de actualización con intención exclusiva. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_X|Tiene lugar cuando una tarea está esperando a adquirir un bloqueo exclusivo. Para obtener una matriz de compatibilidad de bloqueos, vea [sys.dm_tran_locks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LOG_RATE_GOVERNOR|Se produce cuando la base de datos está esperando la cuota para escribir en el registro.|  
|LOGBUFFER|Tiene lugar cuando una tarea está esperando tener espacio en el búfer del registro para almacenar un registro. Valores coherentemente altos pueden indicar que los dispositivos de registro no pueden hacer frente a la cantidad de registros que va a generar el servidor.|  
|LOGGENERATION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LOGMGR|Tiene lugar cuando una tarea está esperando que finalicen operaciones de E/S pendientes para cerrar el registro mientras se cierra la base de datos.|  
|LOGMGR_FLUSH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LOGMGR_QUEUE|Tiene lugar mientras la tarea de escritura en registro espera solicitudes de trabajo.|  
|LOGMGR_RESERVE_APPEND|Tiene lugar cuando una tarea está esperando comprobar si el truncamiento del registro libera espacio del registro para permitir que la tarea escriba un nuevo registro. Para reducir esta espera, puede aumentar el tamaño de los archivos de registro de la base de datos correspondiente.|  
|LOWFAIL_MEMMGR_QUEUE|Tiene lugar mientras se espera que haya memoria disponible para su uso.|  
|MSQL_DQ|Tiene lugar cuando una tarea está esperando que finalice una operación de consulta distribuida. Se utiliza para detectar potenciales interbloqueos de aplicación MARS (Conjuntos de resultados activos múltiples). La espera termina cuando finaliza la llamada a la consulta distribuida.|  
|MSQL_XACT_MGR_MUTEX|Tiene lugar cuando una tarea está esperando obtener la propiedad del administrador de transacciones de la sesión para realizar una operación de transacción en el nivel de sesión.|  
|MSQL_XACT_MUTEX|Tiene lugar durante la sincronización del uso de transacciones. Una solicitud debe adquirir la exclusión mutua para poder utilizar la transacción.|  
|MSQL_XP|Tiene lugar cuando una tarea está esperando que finalice un procedimiento almacenado extendido. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza este estado de espera para detectar interbloqueos potenciales de la aplicación MARS. La espera se detiene cuando finaliza la llamada al procedimiento almacenado extendido.|  
|MSSEARCH|Tiene lugar durante las llamadas a la búsqueda de texto completo. Esta espera termina cuando finaliza la operación de texto completo. No indica contención, sino la duración de las operaciones de texto completo.|  
|NET_WAITFOR_PACKET|Tiene lugar cuando una conexión está esperando un paquete de red durante una lectura de red.|  
|OLEDB|Tiene lugar cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] llama al proveedor OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Este estado de espera no se usa para la sincronización. Se usa para indicar la duración de las llamadas al proveedor OLE DB.|  
|ONDEMAND_TASK_QUEUE|Tiene lugar mientras una tarea en segundo plano espera solicitudes de tarea del sistema de alta prioridad. Los tiempos de espera largos indican que no ha habido que procesar solicitudes de alta prioridad; no deben suponer un problema.|  
|PAGEIOLATCH_DT|Tiene lugar cuando una tarea está esperando en un bloqueo temporal por un búfer que está en una solicitud de E/S. La solicitud de bloqueo temporal está en modo de destrucción. Las esperas largas pueden indicar problemas en el subsistema del disco.|  
|PAGEIOLATCH_EX|Tiene lugar cuando una tarea está esperando en un bloqueo temporal por un búfer que está en una solicitud de E/S. La solicitud de bloqueo temporal está en modo exclusivo. Las esperas largas pueden indicar problemas en el subsistema del disco.|  
|PAGEIOLATCH_KP|Tiene lugar cuando una tarea está esperando en un bloqueo temporal por un búfer que está en una solicitud de E/S. La solicitud de bloqueo temporal está en modo de conservación. Las esperas largas pueden indicar problemas en el subsistema del disco.|  
|PAGEIOLATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PAGEIOLATCH_SH|Tiene lugar cuando una tarea está esperando en un bloqueo temporal por un búfer que está en una solicitud de E/S. La solicitud de bloqueo temporal está en modo compartido. Las esperas largas pueden indicar problemas en el subsistema del disco.|  
|PAGEIOLATCH_UP|Tiene lugar cuando una tarea está esperando en un bloqueo temporal por un búfer que está en una solicitud de E/S. La solicitud de bloqueo temporal está en modo de actualización. Las esperas largas pueden indicar problemas en el subsistema del disco.|  
|PAGELATCH_DT|Tiene lugar cuando una tarea está esperando en un bloqueo temporal por un búfer que no está en una solicitud de E/S. La solicitud de bloqueo temporal está en modo de destrucción.|  
|PAGELATCH_EX|Tiene lugar cuando una tarea está esperando en un bloqueo temporal por un búfer que no está en una solicitud de E/S. La solicitud de bloqueo temporal está en modo exclusivo.|  
|PAGELATCH_KP|Tiene lugar cuando una tarea está esperando en un bloqueo temporal por un búfer que no está en una solicitud de E/S. La solicitud de bloqueo temporal está en modo de conservación.|  
|PAGELATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PAGELATCH_SH|Tiene lugar cuando una tarea está esperando en un bloqueo temporal por un búfer que no está en una solicitud de E/S. La solicitud de bloqueo temporal está en modo compartido.|  
|PAGELATCH_UP|Tiene lugar cuando una tarea está esperando en un bloqueo temporal por un búfer que no está en una solicitud de E/S. La solicitud de bloqueo temporal está en modo de actualización.|  
|PARALLEL_BACKUP_QUEUE|Tiene lugar cuando se serializa la salida generada por RESTORE HEADERONLY, RESTORE FILELISTONLY o RESTORE LABELONLY.|  
|PREEMPTIVE_ABR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_AUDIT_ACCESS_EVENTLOG|Tiene lugar cuando el programador del sistema operativo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQLOS) cambia a modo preferente para escribir un evento de auditoría en el registro de eventos de Windows.|  
|PREEMPTIVE_AUDIT_ACCESS_SECLOG|Tiene lugar cuando el programador de SQLOS cambia a modo preferente para escribir un evento de auditoría en el registro de seguridad de Windows.|  
|PREEMPTIVE_CLOSEBACKUPMEDIA|Tiene lugar cuando el programador de SQLOS cambia a modo preferente para cerrar el medio de copia de seguridad.|  
|PREEMPTIVE_CLOSEBACKUPTAPE|Tiene lugar cuando el programador de SQLOS cambia a modo preferente para cerrar un dispositivo de copia de seguridad de cinta.|  
|PREEMPTIVE_CLOSEBACKUPVDIDEVICE|Tiene lugar cuando el programador de SQLOS cambia a modo preferente para cerrar un dispositivo de copia de seguridad virtual.|  
|PREEMPTIVE_CLUSAPI_CLUSTERRESOURCECONTROL|Tiene lugar cuando el programador de SQLOS cambia a modo preferente para realizar las operaciones de clúster de conmutación por error de Windows.|  
|PREEMPTIVE_COM_COCREATEINSTANCE|Tiene lugar cuando el programador de SQLOS cambia a modo preferente para crear un objeto COM.|  
|PREEMPTIVE_HADR_LEASE_MECHANISM|Always On la programación del administrador de concesiones de grupos de disponibilidad para el diagnóstico de CSS.|  
|PREEMPTIVE_SOSTESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_STRESSDRIVER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_TESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_XETESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PRINT_ROLLBACK_PROGRESS|Se utiliza para esperar mientras los procesos del usuario finalizan en una base de datos que se ha pasado utilizando la cláusula de terminación ALTER DATABASE. Para obtener más información, vea [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).|  
|PWAIT_HADR_CHANGE_NOTIFIER_TERMINATION_SYNC|Se produce cuando una tarea en segundo plano está esperando a que se termine la tarea en segundo plano que recibe (a través de sondeo) las notificaciones de Clústeres de conmutación por error de Windows Server.  Solo para uso interno.|  
|PWAIT_HADR_CLUSTER_INTEGRATION|Una operación de anexar, reemplazar o quitar está esperando para obtener un bloqueo de escritura en una lista interna de Always On (como una lista de redes, direcciones de red o agentes de escucha del grupo de disponibilidad).  Solo para uso interno.|  
|PWAIT_HADR_OFFLINE_COMPLETED|Un Always On operación de eliminación de grupo de disponibilidad está esperando a que el grupo de disponibilidad de destino quede sin conexión antes de destruir los objetos de clústeres de conmutación por error de Windows Server.|  
|PWAIT_HADR_ONLINE_COMPLETED|Un Always On operación de creación o conmutación por error del grupo de disponibilidad está esperando a que el grupo de disponibilidad de destino se ponga en línea.|  
|PWAIT_HADR_POST_ONLINE_COMPLETED|Una operación de Always On Drop Availability Group está esperando la finalización de cualquier tarea en segundo plano programada como parte de un comando anterior. Por ejemplo, el puede haber una tarea en segundo plano que esté pasando las bases de datos de disponibilidad al rol principal. La DDL DROP AVAILABILITY GROUP siempre debe esperar a que esta tarea en segundo plano termine para evitar las condiciones de carrera.|  
|PWAIT_HADR_WORKITEM_COMPLETED|Espera interna de un subproceso que espera a que una tarea de trabajo asincrónico se complete. Se trata de una espera prevista y es para uso de CSS.|  
|PWAIT_MD_LOGIN_STATS|Se produce durante la sincronización interna en las estadísticas de inicio de sesión de los metadatos.|  
|PWAIT_MD_RELATION_CACHE|Se produce durante la sincronización interna en los metadatos de la tabla o el índice.|  
|PWAIT_MD_SERVER_CACHE|Se produce durante la sincronización interna en los metadatos de servidores vinculados.|  
|PWAIT_MD_UPGRADE_CONFIG|Se produce durante la sincronización interna al actualizar las configuraciones generales de servidor.|  
|PWAIT_METADATA_LAZYCACHE_RWLOCk|Se produce durante la sincronización interna de la memoria caché de metadatos junto con la iteración del índice o las estadísticas en una tabla.|  
|QPJOB_KILL|Indica que una llamada a KILL ha cancelado una actualización de estadísticas automáticas asincrónicas cuando la actualización se empezaba a ejecutar. El subproceso de terminación está suspendido, en espera de que empiece a escuchar comandos KILL. Un buen valor es menor que un segundo.|  
|QPJOB_WAITFOR_ABORT|Indica que una llamada a KILL ha cancelado una actualización de estadísticas automáticas asincrónicas cuando se estaba ejecutando. La actualización no se ha completado, sino que está suspendida hasta que finalice la coordinación del mensaje del subproceso de terminación. Es un estado normal, pero excepcional, y debe ser muy corto. Un buen valor es menor que un segundo.|  
|QRY_MEM_GRANT_INFO_MUTEX|Tiene lugar cuando la administración de memoria de ejecución de la consulta intenta controlar el acceso a la lista estática de información de concesiones. Este estado muestra información acerca de las solicitudes de memoria en espera y concedidas actualmente. Este estado es un sencillo estado de control de acceso. En este estado nunca debe esperarse mucho. Si esta exclusión mutua no se libera, todas las nuevas consultas que utilizan memoria dejarán de responder.|  
|QUERY_ERRHDL_SERVICE_DONE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_EXECUTION_INDEX_SORT_EVENT_OPEN|Tiene lugar en determinados casos, cuando la generación de índices sin conexión se ejecuta en paralelo y los diferentes subprocesos de trabajo que realizan la ordenación sincronizan el acceso a los archivos de ordenación.|  
|QUERY_NOTIFICATION_MGR_MUTEX|Tiene lugar durante la sincronización de la recopilación de elementos no utilizados en el administrador de notificaciones de consulta.|  
|QUERY_NOTIFICATION_SUBSCRIPTION_MUTEX|Tiene lugar durante la sincronización del estado en las transacciones de notificaciones de consulta.|  
|QUERY_NOTIFICATION_TABLE_MGR_MUTEX|Tiene lugar durante la sincronización interna en el administrador de notificaciones de consulta.|  
|QUERY_NOTIFICATION_UNITTEST_MUTEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_OPTIMIZER_PRINT_MUTEX|Tiene lugar durante la sincronización de la producción de salida de diagnóstico del optimizador de consultas. Este tipo de espera solo se produce si la configuración de diagnóstico se ha habilitado bajo la dirección del Servicio de soporte técnico de [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|QUERY_TRACEOUT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_WAIT_ERRHDL_SERVICE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|RECOVER_CHANGEDB|Tiene lugar durante la sincronización del estado de base de datos en una base de datos en estado de espera activa.|  
|REPL_CACHE_ACCESS|Tiene lugar durante la sincronización en caché de artículos de una replicación. Durante estas esperas, el registro del LOG de replicación se detiene temporalmente y se bloquean las instrucciones de lenguaje de definición de datos (DLL) en una tabla publicada.|  
|REPL_SCHEMA_ACCESS|Tiene lugar durante la sincronización de la información de versión del esquema de replicación. Este estado se produce cuando las instrucciones de DDL se ejecutan en el objeto replicado y cuando el registro del LOG genera o consume un esquema con versiones basado en las repeticiones de DDL.|  
|REPLICA_WRITES|Tiene lugar mientras una tarea espera que finalicen las escrituras de página en instantáneas de la base de datos o en réplicas DBCC.|  
|REQUEST_DISPENSER_PAUSE|Tiene lugar cuando una tarea espera que finalicen todas las operaciones de E/S pendientes para poder inmovilizar la E/S en un archivo y realizar una copia de seguridad de instantánea.|  
|REQUEST_FOR_DEADLOCK_SEARCH|Tiene lugar mientras la supervisión de interbloqueos espera que comience la siguiente búsqueda de interbloqueos. Esta espera está prevista entre detecciones de interbloqueos; un tiempo de espera total largo en este recurso no indica un problema.|  
|RESMGR_THROTTLED|Tiene lugar cuando entra una nueva solicitud y se acelera basándose en GROUP_MAX_REQUESTS.|  
|RESOURCE_QUEUE|Tiene lugar durante la sincronización de diferentes colas internas de recursos.|  
|RESOURCE_SEMAPHORE|Tiene lugar cuando una solicitud de memoria de consulta no se puede conceder de forma inmediata debido a otras consultas simultáneas. Un número alto de esperas y tiempos de espera largos pueden indicar un número excesivo de consultas simultáneas o cantidades excesivas de solicitud de memoria.|  
|RESOURCE_SEMAPHORE_MUTEX|Tiene lugar mientras una consulta espera que se satisfaga su solicitud de reserva de subproceso. También se produce durante la sincronización de solicitudes de compilación de consultas y de concesión de memoria.|  
|RESOURCE_SEMAPHORE_QUERY_COMPILE|Tiene lugar cuando el número de compilaciones de consultas simultáneas alcanza un límite de aceleración. Un número alto de esperas y tiempos de espera largos pueden indicar compilaciones excesivas, recompilaciones o planes que no se pueden almacenar en caché.|  
|RESOURCE_SEMAPHORE_SMALL_QUERY|Tiene lugar cuando no se puede conceder de forma inmediata una solicitud de memoria de una consulta pequeña debido a otras consultas simultáneas. El tiempo de espera no debe superar unos segundos, ya que el servidor transfiere la solicitud al grupo principal de memoria de consulta si no puede conceder la memoria solicitada en este tiempo. Esperas altas pueden indicar un número excesivo de consultas pequeñas simultáneas y el bloqueo del grupo principal de memoria debido a consultas en espera.|  
|SE_REPL_CATCHUP_THROTTLE|Se produce cuando la transacción espera uno de los secundarios de la base de datos para avanzar.|  
|SE_REPL_COMMIT_ACK|Se produce cuando la transacción espera el reconocimiento de confirmación de quórum de las réplicas secundarias.|  
|SE_REPL_COMMIT_TURN|Se produce cuando la transacción espera la confirmación después de recibir reconocimientos de confirmación de quórum.|  
|SE_REPL_ROLLBACK_ACK|Se produce cuando la transacción espera el reconocimiento de reversión de quórum de las réplicas secundarias.|  
|SE_REPL_SLOW_SECONDARY_THROTTLE|Se produce cuando el subproceso espera una de las réplicas secundarias de la base de datos.|  
|SEC_DROP_TEMP_KEY|Tiene lugar después de un error en el intento de quitar una clave de seguridad temporal y antes de volver a intentarlo.|  
|SECURITY_MUTEX|Tiene lugar cuando se esperan exclusiones mutuas que controlen el acceso a la lista global de proveedores de servicios criptográficos de Administración extensible de claves (EKM) y la lista de ámbito de sesión de sesiones de EKM.|  
|SEQUENTIAL_GUID|Tiene lugar mientras se obtiene un nuevo GUID secuencial.|  
|SERVER_IDLE_CHECK|Tiene lugar durante la sincronización del estado inactivo de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando un monitor de recursos intenta declarar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como inactiva o intentando activarse.|  
|SHUTDOWN|Tiene lugar mientras una instrucción de cierre del sistema espera que las conexiones activas salgan.|  
|SLEEP_BPOOL_FLUSH|Tiene lugar cuando un punto de comprobación acelera la emisión de nuevas operaciones de E/S para evitar sobrecargar el subsistema del disco.|  
|SLEEP_DBSTARTUP|Tiene lugar durante el inicio de la base de datos mientras se espera la recuperación de todas las bases de datos.|  
|SLEEP_DCOMSTARTUP|Tiene lugar una vez como máximo durante el inicio de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mientras se espera que finalice la inicialización de DCOM.|  
|SLEEP_MSDBSTARTUP|Tiene lugar cuando Seguimiento de SQL espera a que la base de datos msdb complete su inicio.|  
|SLEEP_SYSTEMTASK|Tiene lugar durante el inicio de una tarea en segundo plano mientras se espera que tempdb finalice el inicio.|  
|SLEEP_TASK|Tiene lugar cuando una tarea se mantiene inactiva mientras espera que se produzca un evento genérico.|  
|SLEEP_TEMPDBSTARTUP|Tiene lugar mientras una tarea espera que tempdb finalice el inicio.|  
|SNI_CRITICAL_SECTION|Tiene lugar durante la sincronización interna en los componentes de red de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SNI_HTTP_WAITFOR_0_DISCON|Tiene lugar durante el cierre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mientras se espera que salgan todas las conexiones HTTP pendientes.|  
|SNI_LISTENER_ACCESS|Tiene lugar mientras se espera que los nodos de acceso a memoria no uniforme (NUMA) actualicen el cambio de estado. El acceso al cambio de estado está serializado.|  
|SNI_TASK_COMPLETION|Tiene lugar cuando se espera que finalicen todas las tareas durante un cambio de estado del nodo NUMA.|  
|SOAP_READ|Tiene lugar mientras se espera que se complete una lectura de red HTTP.|  
|SOAP_WRITE|Tiene lugar mientras se espera que finalice una escritura de red HTTP.|  
|SOS_CALLBACK_REMOVAL|Tiene lugar mientras se lleva a cabo la sincronización en una lista de devoluciones de llamada para quitar una devolución de llamada. No se espera que este contador cambie una vez completada la inicialización del servidor.|  
|SOS_DISPATCHER_MUTEX|Tiene lugar durante la sincronización interna del grupo de distribuidores. Esto incluye el ajuste del grupo.|  
|SOS_LOCALALLOCATORLIST|Tiene lugar durante la sincronización interna en el administrador de memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOS_MEMORY_USAGE_ADJUSTMENT|Tiene lugar cuando se ajusta el uso de memoria entre los fondos.|  
|SOS_OBJECT_STORE_DESTROY_MUTEX|Tiene lugar durante la sincronización interna en grupos de memoria cuando se destruyen objetos del grupo.|  
|SOS_PROCESS_AFFINITY_MUTEX|Tiene lugar durante la sincronización del acceso a la configuración de afinidad de procesos.|  
|SOS_RESERVEDMEMBLOCKLIST|Tiene lugar durante la sincronización interna en el administrador de memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOS_SCHEDULER_YIELD|Tiene lugar cuando una tarea genera de forma voluntaria el programador para que se ejecuten otras tareas. Mientras, la tarea espera la renovación de su cuanto.|  
|SOS_SMALL_PAGE_ALLOC|Tiene lugar durante la asignación y la liberación de la memoria que administran algunos objetos de memoria.|  
|SOS_STACKSTORE_INIT_MUTEX|Tiene lugar durante la sincronización de la inicialización de almacenamiento interno.|  
|SOS_SYNC_TASK_ENQUEUE_EVENT|Tiene lugar cuando una tarea se inicia de forma sincrónica. La mayor parte de las tareas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inician de modo asincrónico, en el que el control vuelve al iniciador inmediatamente después de incluir la solicitud de tarea en la cola de trabajo.|  
|SOS_VIRTUALMEMORY_LOW|Tiene lugar cuando una asignación de memoria espera que un administrador de recursos libere memoria virtual.|  
|SOSHOST_EVENT|Tiene lugar cuando un componente hospedado, como CLR, espera un objeto de sincronización de eventos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_INTERNAL|Tiene lugar durante la sincronización de devoluciones de llamada del administrador de memoria que utilizan los componentes hospedados, como CLR.|  
|SOSHOST_MUTEX|Tiene lugar cuando un componente hospedado, como CLR, espera un objeto de sincronización de exclusión mutua de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_RWLOCK|Tiene lugar cuando un componente hospedado, como CLR, espera un objeto de sincronización de lectura-escritura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_SEMAPHORE|Tiene lugar cuando un componente hospedado, como CLR, espera un objeto de sincronización de semáforo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_SLEEP|Tiene lugar cuando una tarea hospedada se mantiene inactiva mientras espera que se produzca un evento genérico. Las tareas hospedadas son las que utilizan los componentes hospedados, como CLR.|  
|SOSHOST_TRACELOCK|Tiene lugar durante la sincronización del acceso a flujos de seguimiento.|  
|SOSHOST_WAITFORDONE|Tiene lugar cuando un componente hospedado, como CLR, espera la finalización de una tarea.|  
|SQLCLR_APPDOMAIN|Tiene lugar mientras CLR espera que complete el inicio de un dominio de aplicación.|  
|SQLCLR_ASSEMBLY|Tiene lugar mientras se espera el acceso a la lista de ensamblados cargada en el dominio de aplicación.|  
|SQLCLR_DEADLOCK_DETECTION|Tiene lugar mientras CLR espera la finalización de la detección de interbloqueos.|  
|SQLCLR_QUANTUM_PUNISHMENT|Tiene lugar cuando una tarea de CLR se acelera porque ha sobrepasado su cuanto de ejecución. Esta aceleración se lleva a cabo para reducir el efecto de esta tarea que consume muchos recursos en otras tareas.|  
|SQLSORT_NORMMUTEX|Tiene lugar durante la sincronización interna, mientras se inicializan estructuras de ordenación internas.|  
|SQLSORT_SORTMUTEX|Tiene lugar durante la sincronización interna, mientras se inicializan estructuras de ordenación internas.|  
|SQLTRACE_BUFFER_FLUSH|Tiene lugar cuando una tarea está esperando a que una tarea en segundo plano vuelque los búferes de seguimiento al disco cada cuatro segundos.|  
|SQLTRACE_LOCK|Tiene lugar mientras se lleva a cabo la sincronización en búferes de seguimiento durante un seguimiento de archivos.|  
|SQLTRACE_SHUTDOWN|Tiene lugar mientras el cierre del sistema de seguimiento espera la finalización de los eventos de seguimiento pendientes.|  
|SQLTRACE_WAIT_ENTRIES|Tiene lugar cuando una cola de eventos de Seguimiento de SQL espera que lleguen paquetes a la cola.|  
|SRVPROC_SHUTDOWN|Tiene lugar mientras el proceso de cierre del sistema espera la liberación de los recursos internos para cerrar sin problemas.|  
|TEMPOBJ|Tiene lugar cuando se sincronizan eliminaciones de objetos temporales. Esta espera no es muy común y solo se produce si una tarea ha solicitado el acceso exclusivo para eliminaciones de tablas temp.|  
|THREADPOOL|Tiene lugar cuando una tarea está esperando un trabajador en el que ejecutarse. Puede indicar que la configuración de número máximo de trabajadores es demasiado baja o que se tarda un tiempo inusualmente largo en las ejecuciones por lotes, lo que reduce el número de trabajadores disponibles para satisfacer otros lotes.|  
|TIMEPRIV_TIMEPERIOD|Tiene lugar durante la sincronización interna del temporizador de Extended Events.|  
|TRACEWRITE|Tiene lugar cuando el proveedor de seguimiento de conjuntos de filas de Seguimiento de SQL espera un búfer libre o el procesamiento de un búfer con eventos.|  
|TRAN_MARKLATCH_DT|Tiene lugar cuando se espera un bloqueo temporal en modo de destrucción en un bloqueo temporal de marca de transacción. Los bloqueos temporales de marca de transacción se utilizan para la sincronización de confirmaciones con transacciones marcadas.|  
|TRAN_MARKLATCH_EX|Tiene lugar cuando se espera un bloqueo temporal en modo exclusivo en una transacción marcada. Los bloqueos temporales de marca de transacción se utilizan para la sincronización de confirmaciones con transacciones marcadas.|  
|TRAN_MARKLATCH_KP|Tiene lugar cuando se espera un bloqueo temporal en modo de mantenimiento en una transacción marcada. Los bloqueos temporales de marca de transacción se utilizan para la sincronización de confirmaciones con transacciones marcadas.|  
|TRAN_MARKLATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|TRAN_MARKLATCH_SH|Tiene lugar cuando se espera un bloqueo temporal en modo compartido en una transacción marcada. Los bloqueos temporales de marca de transacción se utilizan para la sincronización de confirmaciones con transacciones marcadas.|  
|TRAN_MARKLATCH_UP|Tiene lugar cuando se espera un bloqueo temporal en modo de actualización en una transacción marcada. Los bloqueos temporales de marca de transacción se utilizan para la sincronización de confirmaciones con transacciones marcadas.|  
|TRANSACTION_MUTEX|Tiene lugar durante la sincronización del acceso a una transacción por parte de varios lotes.|  
|UTIL_PAGE_ALLOC|Tiene lugar cuando los exámenes del registro de transacciones esperan que haya memoria disponible durante la presión de memoria.|  
|VIA_ACCEPT|Tiene lugar cuando se completa la conexión del proveedor del Adaptador de interfaz virtual (VIA) durante el inicio.|  
|VIEW_DEFINITION_MUTEX|Tiene lugar durante la sincronización del acceso a definiciones de vista almacenadas en memoria caché.|  
|WAIT_FOR_RESULTS|Tiene lugar cuando se espera el inicio de una notificación de consulta.|  
|WAITFOR|Tiene lugar como resultado de una instrucción WAITFOR de [!INCLUDE[tsql](../../includes/tsql-md.md)]. La duración de la espera viene determinada por los parámetros de la instrucción. Se trata de una espera iniciada por el usuario.|  
|WAITFOR_TASKSHUTDOWN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|WAITSTAT_MUTEX|Tiene lugar durante la sincronización del acceso a la colección de estadísticas utilizadas para rellenar sys.dm_os_wait_stats.|  
|WCC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|WORKTBL_DROP|Tiene lugar mientras se establece una pausa antes de volver a intentar una eliminación incorrecta de tablas de trabajo.|  
|WRITE_COMPLETION|Tiene lugar mientras está en curso una operación de escritura.|  
|WRITELOG|Tiene lugar mientras se espera que se complete un vaciado del registro. Las operaciones habituales que provocan vaciados del registro son los puntos de comprobación y las confirmaciones de transacciones.|  
|XACT_OWN_TRANSACTION|Tiene lugar mientras se espera adquirir la propiedad de una transacción.|  
|XACT_RECLAIM_SESSION|Tiene lugar mientras se espera que el propietario actual de una sesión libere la propiedad de la sesión.|  
|XACTLOCKINFO|Tiene lugar durante la sincronización del acceso a la lista de bloqueos de una transacción. Además de la propia transacción, a la lista de bloqueos tienen acceso operaciones como la detección de interbloqueos y la migración de bloqueos durante divisiones de página.|  
|XACTWORKSPACE_MUTEX|Tiene lugar durante la sincronización de bajas de una transacción, así como del número de bloqueos de base de datos entre los miembros dados de alta de una transacción.|  
|XE_BUFFERMGR_ALLPROCESSED_EVENT|Tiene lugar cuando los búferes de sesión de Extended Events se vacían en los destinos. Esta espera se produce en un subproceso en segundo plano.|  
|XE_BUFFERMGR_FREEBUF_EVENT|Tiene lugar cuando se presenta alguna de las siguientes condiciones:<br /><br /> Una sesión de Extended Events se configura para que no haya pérdida de eventos y todos los búferes de la sesión están actualmente llenos. Esto puede indicar que los búferes para una sesión de Extended Events son demasiado pequeños o se deben dividir.<br /><br /> Las auditorías experimentan un retraso. Esto puede indicar un cuello de botella en disco en la unidad donde se escriben las auditorías.|  
|XE_DISPATCHER_CONFIG_SESSION_LIST|Tiene lugar cuando una sesión de Extended Events que está utilizando destinos asincrónicos se inicia o se detiene. Esta espera indica alguna de las siguientes situaciones:<br /><br /> Una sesión de Extended Events se está registrando con un grupo de subprocesos en segundo plano.<br /><br /> El grupo de subprocesos en segundo plano está calculando el número requerido de subprocesos basándose en la carga actual.|  
|XE_DISPATCHER_JOIN|Tiene lugar cuando un subproceso en segundo plano que se utiliza para sesiones de Extended Events está finalizando.|  
|XE_DISPATCHER_WAIT|Tiene lugar cuando un subproceso en segundo plano que se utiliza para sesiones de Extended Events está esperando a que se procesen los búferes de eventos.|  
|XE_MODULEMGR_SYNC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|XE_OLS_LOCK|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|XE_PACKAGE_LOCK_BACKOFF|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|FT_COMPROWSET_RWLOCK|El texto completo espera en la operación de metadatos de fragmento. Solamente se documenta con fines informativos. No se admite. La compatibilidad con versiones posteriores no está garantizada.|  
|FT_IFTS_RWLOCK|El texto completo está esperando la sincronización interna. Solamente se documenta con fines informativos. No se admite. La compatibilidad con versiones posteriores no está garantizada.|  
|TDPFT_IFTS_SCHEDULER_IDLE_WAIT|Tipo de espera de la suspensión del programador de texto completo. El programador está inactivo.|  
|FT_IFTSHC_MUTEX|El texto completo está esperando una operación de control de fdhost. Solamente se documenta con fines informativos. No se admite. La compatibilidad con versiones posteriores no está garantizada.|  
|FT_IFTSISM_MUTEX|El texto completo está esperando la operación de comunicación. Solamente se documenta con fines informativos. No se admite. La compatibilidad con versiones posteriores no está garantizada.|  
|FT_MASTER_MERGE|El texto completo está esperando la operación de combinación maestra. Solamente se documenta con fines informativos. No se admite. La compatibilidad con versiones posteriores no está garantizada.|  
  
  
