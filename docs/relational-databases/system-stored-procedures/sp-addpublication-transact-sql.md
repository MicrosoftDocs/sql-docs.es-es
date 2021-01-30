---
description: sp_addpublication (Transact-SQL)
title: sp_addpublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpublication_TSQL
- sp_addpublication
helpviewer_keywords:
- sp_addpublication
ms.assetid: c7167ed1-2b7e-4824-b82b-65f4667c4407
author: markingmyname
ms.author: maghan
ms.openlocfilehash: de6ba893e44d7bfddda4325cc15c1c4d3f875a0c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206693"
---
# <a name="sp_addpublication-transact-sql"></a>sp_addpublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una publicación de instantáneas o transaccional. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addpublication [ @publication = ] 'publication'  
    [ , [ @taskid = ] tasked ]  
    [ , [ @restricted = ] 'restricted' ]  
    [ , [ @sync_method = ] 'sync_method' ]  
    [ , [ @repl_freq = ] 'repl_freq' ]  
    [ , [ @description = ] 'description' ]  
    [ , [ @status = ] 'status' ]  
    [ , [ @independent_agent = ] 'independent_agent' ]  
    [ , [ @immediate_sync = ] 'immediate_sync' ]  
    [ , [ @enabled_for_internet = ] 'enabled_for_internet' ]  
    [ , [ @allow_push = ] 'allow_push'  
    [ , [ @allow_pull = ] 'allow_pull' ]  
    [ , [ @allow_anonymous = ] 'allow_anonymous' ]  
    [ , [ @allow_sync_tran = ] 'allow_sync_tran' ]  
    [ , [ @autogen_sync_procs = ] 'autogen_sync_procs' ]  
    [ , [ @retention = ] retention ]  
    [ , [ @allow_queued_tran= ] 'allow_queued_updating' ]  
    [ , [ @snapshot_in_defaultfolder= ] 'snapshot_in_default_folder' ]  
    [ , [ @alt_snapshot_folder= ] 'alternate_snapshot_folder' ]  
    [ , [ @pre_snapshot_script= ] 'pre_snapshot_script' ]  
    [ , [ @post_snapshot_script= ] 'post_snapshot_script' ]  
    [ , [ @compress_snapshot= ] 'compress_snapshot' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port= ] ftp_port ]  
    [ , [ @ftp_subdirectory = ] 'ftp_subdirectory' ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @allow_dts = ] 'allow_dts' ]  
    [ , [ @allow_subscription_copy = ] 'allow_subscription_copy' ]  
    [ , [ @conflict_policy = ] 'conflict_policy' ]  
    [ , [ @centralized_conflicts = ] 'centralized_conflicts' ]   
    [ , [ @conflict_retention = ] conflict_retention ]  
    [ , [ @queue_type = ] 'queue_type' ]  
    [ , [ @add_to_active_directory = ] 'add_to_active_directory' ]  
    [ , [ @logreader_job_name = ] 'logreader_agent_name' ]  
    [ , [ @qreader_job_name = ] 'queue_reader_agent_name' ]  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @allow_initialize_from_backup = ] 'allow_initialize_from_backup' ]  
    [ , [ @replicate_ddl = ] replicate_ddl ]  
    [ , [ @enabled_for_p2p = ] 'enabled_for_p2p' ]  
    [ , [ @publish_local_changes_only = ] 'publish_local_changes_only' ]  
    [ , [ @enabled_for_het_sub = ] 'enabled_for_het_sub' ]  
    [ , [ @p2p_conflictdetection = ] 'p2p_conflictdetection' ]  
    [ , [ @p2p_originator_id = ] p2p_originator_id  
    [ , [ @p2p_continue_onconflict = ] 'p2p_continue_onconflict'  
    [ , [ @allow_partition_switch = ] 'allow_partition_switch'  
    [ , [ @replicate_partition_switch = ]'replicate_partition_switch'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ \@publication = ] 'publication'` Es el nombre de la publicación que se va a crear. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado. El nombre debe ser único en la base de datos.  
  
`[ \@taskid = ] taskid` Solo se admite por compatibilidad con versiones anteriores; Use [sp_addpublication_snapshot &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md).  
  
`[ \@restricted = ] 'restricted'` Solo se admite por compatibilidad con versiones anteriores; Use *default_access*.  
  
`[ \@sync_method = ] _'sync_method'` Es el modo de sincronización. *sync_method* es **nvarchar (13)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**native**|Genera la salida de todas las tablas mediante un programa de copia masiva en modo nativo. *No se admite para publicadores de Oracle*.|  
|**carácter**|Genera la salida de todas las tablas mediante un programa de copia masiva en modo de caracteres. _Para un publicador de Oracle,_ el **carácter** _solo es válido para la replicación de instantáneas_.|  
|**simultáneas**|Produce la salida de todas las tablas mediante un programa de copia masiva en modo nativo, pero no bloquea las tablas durante la instantánea. Solo se admite para publicaciones transaccionales. *No se admite para publicadores de Oracle*.|  
|**concurrent_c**|Produce la salida de todas las tablas mediante un programa de copia masiva en modo de carácter, pero no bloquea las tablas durante la instantánea. Solo se admite para publicaciones transaccionales.|  
|**instantánea de base de datos**|Produce la salida de todas las tablas mediante un programa de copia masiva en modo nativo desde una instantánea de base de datos. Las instantáneas de base de datos no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|**database snapshot character**|Produce la salida de todas las tablas mediante un programa de copia masiva en modo de carácter desde una instantánea de base de datos. Las instantáneas de base de datos no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|NULL (predeterminado)|El valor predeterminado es **nativo** para los [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicadores. En el caso de los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el valor predeterminado es **carácter** si el valor de *repl_freq* es **Snapshot** y **concurrent_c** en todos los demás casos.|  
  
`[ \@repl_freq = ] 'repl_freq'` Es el tipo de frecuencia de replicación, *repl_freq* es **nvarchar (10)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Continuous** (valor predeterminado)|El publicador proporciona la salida de todas las transacciones basadas en el registro. En el caso de los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , requiere que *sync_method* establecerse en **concurrent_c**.|  
|**archivos**|El publicador solamente genera eventos de sincronización programados. En el caso de los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , requiere que *sync_method* establecerse en **carácter**.|  
  
`[ \@description = ] 'description'` Es una descripción opcional de la publicación. la *Descripción* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ \@status = ] 'status'` Especifica si los datos de la publicación están disponibles. *status* es **nvarchar (8)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**active**|Los datos de la publicación están inmediatamente disponibles para los suscriptores.|  
|**inactivo** (predeterminado)|Los datos de la publicación no están disponibles para los suscriptores la primera vez que se crea la publicación (pueden suscribirse, pero las suscripciones no se procesan).|  
  
 *No se admite para publicadores de Oracle*.  
  
`[ \@independent_agent = ] 'independent_agent'` Especifica si hay un Agente de distribución independiente para esta publicación. *independent_agent* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si es **true**, hay un agente de distribución independiente para esta publicación. Si **es false**, la publicación utiliza una agente de distribución compartida y cada par de base de datos del publicador/base de datos del suscriptor tiene un único agente compartido.  
  
`[ \@immediate_sync = ] 'immediate_synchronization'` Especifica si los archivos de sincronización de la publicación se crean cada vez que se ejecuta el Agente de instantáneas. *immediate_synchronization* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si **es true**, los archivos de sincronización se crean o se vuelven a crear cada vez que se ejecuta el agente de instantáneas. Los suscriptores pueden disponer inmediatamente de los archivos si el Agente de instantáneas ya ha terminado su tarea antes de crear la suscripción. Las nuevas suscripciones obtienen los últimos archivos de sincronización generados por la ejecución más reciente del Agente de instantáneas. *independent_agent* debe ser **true** para que *immediate_synchronization* sea **true**. Si **es false**, los archivos de sincronización se crean solo si hay nuevas suscripciones. Debe llamar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) para cada suscripción al agregar incrementalmente un nuevo artículo a una publicación existente. Los suscriptores no pueden recibir los archivos de sincronización después de suscribirse hasta que los Agentes de instantáneas hayan empezado y terminado.  
  
`[ \@enabled_for_internet = ] 'enabled_for_internet'` Especifica si la publicación está habilitada para Internet y determina si se puede usar el protocolo de transferencia de archivos (FTP) para transferir los archivos de instantáneas a un suscriptor. *enabled_for_internet* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si **es true**, los archivos de sincronización de la publicación se colocan en el directorio C:\Archivos de Programa\microsoft SQL server\mssql\mssql.x\repldata\ftp. El usuario debe crear el directorio Ftp.  
  
`[ \@allow_push = ] 'allow_push'` Especifica si se pueden crear suscripciones de extracción para la publicación especificada. *allow_push* es de tipo **nvarchar (5)** y su valor predeterminado es true, que permite suscripciones de extracción en la publicación.  
  
`[ \@allow_pull = ] 'allow_pull'` Especifica si se pueden crear suscripciones de extracción para la publicación especificada. *allow_pull* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si **es false**, no se permiten las suscripciones de extracción en la publicación.  
  
`[ \@allow_anonymous = ] 'allow_anonymous'` Especifica si se pueden crear suscripciones anónimas para la publicación especificada. *allow_anonymous* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si **es true**, *immediate_synchronization* también se debe establecer en **true**. Si **es false**, no se permiten suscripciones anónimas en la publicación.  
  
`[ \@allow_sync_tran = ] 'allow_sync_tran'` Especifica si se permiten las suscripciones de actualización inmediata en la publicación. *allow_sync_tran* es de tipo **nvarchar (5)** y su valor predeterminado es false. **true** *no se admite para los publicadores de Oracle*.  
  
`[ \@autogen_sync_procs = ] 'autogen_sync_procs'` Especifica si el procedimiento almacenado de sincronización para las suscripciones de actualización se genera en el publicador. *autogen_sync_procs* es de tipo **nvarchar (5)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**true**|Se define de forma automática cuando se habilita la actualización de suscripciones.|  
|**false**|Se define automáticamente cuando no se ha habilitado la actualización de suscripciones o para publicadores de Oracle.|  
|NULL (predeterminado)|El valor predeterminado es **true** cuando la actualización de suscripciones está habilitada y en **false** cuando no está habilitada la actualización de suscripciones.|  
  
> [!NOTE]  
>  El valor proporcionado por el usuario para *autogen_sync_procs* se invalidará en función de los valores especificados para *allow_queued_tran* y *allow_sync_tran*.  
  
`[ \@retention = ] retention` Es el período de retención en horas para la actividad de suscripción. la *retención* es de **tipo int** y su valor predeterminado es de 336 horas. Si una suscripción no ha estado activa durante el período de retención, expira y se quita. El valor puede ser mayor que el período máximo de retención de la base de datos de distribución utilizada por el publicador. Si es **0**, las suscripciones conocidas a la publicación nunca expirarán y se quitarán del agente de limpieza de suscripciones expiradas.  
  
`[ \@allow_queued_tran = ] 'allow_queued_updating'` Habilita o deshabilita la puesta en cola de los cambios en el suscriptor hasta que se puedan aplicar en el publicador. *allow_queued_updating* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si **es false**, los cambios en el suscriptor no se ponen en cola. **true** *no se admite para los publicadores de Oracle*.  
  
`[ \@snapshot_in_defaultfolder = ] 'snapshot_in_default_folder'` Especifica si los archivos de instantáneas se almacenan en la carpeta predeterminada. *snapshot_in_default_folder* es de tipo **nvarchar (5)** y su valor predeterminado es true. Si **es true**, los archivos de instantáneas se pueden encontrar en la carpeta predeterminada. Si **es false**, los archivos de instantáneas se han almacenado en la ubicación alternativa especificada por *alternate_snapshot_folder*. Las ubicaciones alternativas pueden encontrarse en otro servidor, en una unidad de red o en medios extraíbles (tales como CD-ROM o discos extraíbles). También puede guardar los archivos de instantánea en un sitio FTP, para que el suscriptor los recupere más tarde. Tenga en cuenta que este parámetro puede ser true y seguir teniendo una ubicación en el parámetro **\@ alt_snapshot_folder** . Esta combinación especifica que los archivos de instantáneas se almacenarán tanto en la ubicación predeterminada como en la alternativa.  
  
`[ \@alt_snapshot_folder = ] 'alternate_snapshot_folder'` Especifica la ubicación de la carpeta alternativa de la instantánea. *alternate_snapshot_folder* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ \@pre_snapshot_script = ] 'pre_snapshot_script'` Especifica un puntero a una ubicación de archivos **. SQL** . *pre_snapshot_script* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. El Agente de distribución ejecutará el script previo a la instantánea antes de la ejecución de cualquiera de los scripts de objetos replicados al aplicar la instantánea en un suscriptor. El script se ejecuta en el contexto de seguridad utilizado por el Agente de distribución al conectarse a la base de datos de suscripciones.  
  
`[ \@post_snapshot_script = ] 'post_snapshot_script'` Especifica un puntero a una ubicación de archivos **. SQL** . *post_snapshot_script* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. El Agente de distribución ejecutará el script posterior a la instantánea después de que se apliquen el resto de scripts de objetos replicados y datos durante la sincronización inicial. El script se ejecuta en el contexto de seguridad utilizado por el Agente de distribución al conectarse a la base de datos de suscripciones.  
  
`[ \@compress_snapshot = ] 'compress_snapshot'`Especifica que la instantánea que se escribe en la ubicación **\@ alt_snapshot_folder** se va a comprimir en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] formato CAB. *compress_snapshot* es de tipo **nvarchar (5)** y su valor predeterminado es false. **false** especifica que la instantánea no se comprimirá; **true** especifica que se comprimirá la instantánea. No se pueden comprimir los archivos de instantánea de más de 2 gigabytes (GB). Los archivos de instantánea comprimidos se descomprimen en la ubicación en la que se ejecuta el Agente de distribución; por lo general, se usan suscripciones de extracción con las instantáneas comprimidas para descomprimir los archivos en el suscriptor. La instantánea de la carpeta predeterminada no se puede comprimir.  
  
`[ \@ftp_address = ] 'ftp_address'` Es la dirección de red del servicio FTP para el distribuidor. *ftp_address* es de **tipo sysname y su** valor predeterminado es NULL. Especifica dónde se encuentran los archivos de instantánea de una publicación para que los recoja el Agente de distribución o el Agente de mezcla de un suscriptor. Puesto que esta propiedad se almacena para cada publicación, cada publicación puede tener un *ftp_address* diferente. La publicación debe ser compatible con la propagación de instantáneas mediante FTP.  
  
`[ \@ftp_port = ] ftp_port` Es el número de puerto del servicio FTP para el distribuidor. *ftp_port* es de **tipo int** y su valor predeterminado es 21. Especifica dónde se encuentran los archivos de instantánea de la publicación para que los recoja el Agente de distribución o Agente de mezcla de un suscriptor. Puesto que esta propiedad se almacena para cada publicación, cada publicación puede tener su propio *ftp_port*.  
  
`[ \@ftp_subdirectory = ] 'ftp_subdirectory'` Especifica dónde estarán disponibles los archivos de instantáneas para el Agente de distribución o Agente de mezcla de suscriptor que se recogerán si la publicación admite la propagación de instantáneas mediante FTP. *ftp_subdirectory* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. Puesto que esta propiedad se almacena para cada publicación, cada publicación puede tener su propio *ftp_subdirctory* o elegir no tener ningún subdirectorio, indicado con un valor null.  
  
`[ \@ftp_login = ] 'ftp_login'` Es el nombre de usuario utilizado para conectarse al servicio FTP. *ftp_login* es de **tipo sysname y su** valor predeterminado es Anonymous.  
  
`[ \@ftp_password = ] 'ftp_password'` Es la contraseña de usuario que se utiliza para conectarse al servicio FTP. *FTP_PASSWORD* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ \@allow_dts = ] 'allow_dts'` Especifica que la publicación permite transformaciones de datos. Puede especificar un paquete DTS al crear una suscripción. *allow_transformable_subscriptions* es de tipo **nvarchar (5)** y su valor predeterminado es false, lo que no permite transformaciones DTS. Cuando *allow_dts* es true, *sync_method* debe establecerse en **carácter** o **concurrent_c**.  
  
 **true** *no se admite para los publicadores de Oracle*.  
  
`[ \@allow_subscription_copy = ] 'allow_subscription_copy'` Habilita o deshabilita la capacidad de copiar las bases de datos de suscripciones que se suscriben a esta publicación. *allow_subscription_copy* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
`[ \@conflict_policy = ] 'conflict_policy'` Especifica la Directiva de resolución de conflictos seguida cuando se utiliza la opción de suscriptor de actualización en cola. *conflict_policy* es de tipo **nvarchar (100)** y su valor predeterminado es null, y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**pub wins**|El publicador gana el conflicto|  
|**sub reinit**|Reinicializar la suscripción.|  
|**sub wins**|El suscriptor gana el conflicto|  
|NULL (predeterminado)|Si es NULL y la publicación es una publicación de instantánea, la directiva predeterminada se convierte en **Sub reinit**. Si es NULL y la publicación no es una publicación de instantáneas, el valor predeterminado es **pub WINS**.|  
  
 *No se admite para publicadores de Oracle*.  
  
`[ \@centralized_conflicts = ] 'centralized_conflicts'` Especifica si los registros de conflictos se almacenan en el publicador. *centralized_conflicts* es de tipo **nvarchar (5)** y su valor predeterminado es true. Si **es true**, los registros de conflictos se almacenan en el publicador. Si **es false**, los registros de conflictos se almacenan tanto en el publicador como en el suscriptor que provocó el conflicto. *No se admite para publicadores de Oracle*.  
  
`[ \@conflict_retention = ] conflict_retention` Especifica el período de retención de conflictos, en días. Es el período de tiempo durante el que se almacenan los metadatos de conflictos para la replicación transaccional punto a punto y para las suscripciones de actualización en cola. *conflict_retention* es de **tipo int** y su valor predeterminado es 14. *No se admite para publicadores de Oracle*.  
  
`[ \@queue_type = ] 'queue_type'` Especifica el tipo de cola que se utiliza. *queue_type* es de tipo **nvarchar (10)** y su valor predeterminado es null, y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**sql**|Utiliza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para almacenar las transacciones.|  
|NULL (predeterminado)|Tiene como valor predeterminado **SQL**, que especifica que se va a utilizar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para almacenar transacciones.|  
  
> [!NOTE]  
>  La compatibilidad para utilizar [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queue Server ha dejado de incluirse. Si se especifica un valor de **MSMQ** , se generará una advertencia y la replicación establecerá automáticamente el valor en **SQL**.  
  
 *No se admite para publicadores de Oracle*.  
  
`[ \@add_to_active_directory = ] 'add\to_active_directory'` Este parámetro está en desuso y solo se admite para la compatibilidad con versiones anteriores de los scripts. Ya no se puede agregar información de publicación a [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory.  
  
`[ \@logreader_job_name = ] 'logreader_agent_name'` Es el nombre de un trabajo del agente existente. *logreader_agent_name* es de **tipo sysname y su** valor predeterminado es NULL. Este parámetro se especifica solo cuando el Agente de registro del LOG utiliza un trabajo existente en lugar de otro nuevo que se esté creando.  
  
`[ \@qreader_job_name = ] 'queue_reader_agent_name'` Es el nombre de un trabajo del agente existente. *queue_reader_agent_name* es de **tipo sysname y su** valor predeterminado es NULL. Este parámetro se especifica solo cuando el Agente de lectura de cola utiliza un trabajo existente en lugar de otro nuevo que se esté creando.  
  
`[ \@publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al agregar una publicación a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
`[ \@allow_initialize_from_backup = ] 'allow_initialize_from_backup'` Indica si los suscriptores pueden inicializar una suscripción a esta publicación a partir de una copia de seguridad en lugar de una instantánea inicial. *allow_initialize_from_backup* es de tipo **nvarchar (5)** y puede tener uno de estos valores:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**true**|Habilita la inicialización desde una copia de seguridad.|  
|**false**|Deshabilita la inicialización desde una copia de seguridad.|  
|NULL (predeterminado)|El valor predeterminado **es true** para una publicación en una topología de replicación punto a punto y **false** para todas las demás publicaciones.|  
  
 Para obtener más información, consulte [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
> [!WARNING]  
>  Para evitar perder los datos del suscriptor, al utilizar **sp_addpublication** con `@allow_initialize_from_backup = N'true'`, utilice siempre `@immediate_sync = N'true'`.  
  
`[ \@replicate_ddl = ] replicate_ddl` Indica si se admite la replicación de esquemas para la publicación. *replicate_ddl* es de **tipo int** y su valor predeterminado es **1** para los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicadores y **0** para los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . **1** indica que las instrucciones del lenguaje de definición de datos (DDL) ejecutadas en el publicador se replican y **0** indica que las instrucciones de DDL no se replican. *La replicación de esquema no es compatible con publicadores de Oracle.* Para más información, vea [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
 El parámetro *\@ replicate_ddl* se respeta cuando una instrucción DDL agrega una columna. El parámetro *\@ replicate_ddl* se omite cuando una instrucción DDL modifica o quita una columna por los siguientes motivos.  
  
-   Cuando se quita una columna, sysarticlecolumns debe actualizarse para evitar que las nuevas instrucciones DML incluyan la columna que se quitó, lo que haría que el agente de distribución produjera un error. Se omite el parámetro *\@ replicate_ddl* porque la replicación siempre debe replicar el cambio de esquema.  
  
-   Cuando se modifica una columna, es posible que el tipo de datos de origen o la nulabilidad hayan cambiado, lo que hace que las instrucciones DML contengan un valor que quizás no sea compatible con la tabla en el suscriptor. Estas instrucciones DML pueden hacer que el agente de distribución genere un error. Se omite el parámetro *\@ replicate_ddl* porque la replicación siempre debe replicar el cambio de esquema.  
  
-   Cuando una instrucción DDL agrega una columna nueva, sysarticlecolumns no incluye esta columna nueva. Las instrucciones DML no intentarán replicar datos para la nueva columna. Se respeta el parámetro porque la DDL es aceptable se realice o no la replicación.  
  
`[ \@enabled_for_p2p = ] 'enabled_for_p2p'` Habilita la publicación que se va a usar en una topología de replicación punto a punto. *enabled_for_p2p* es de tipo **nvarchar (5)** y su valor predeterminado es false. **true** indica que la publicación admite la replicación punto a punto. Al establecer *enabled_for_p2p* en **true**, se aplican las restricciones siguientes:  
  
-   *allow_anonymous* debe ser **false**.  
  
-   *allow_dts* debe ser **false**.  
  
-   *allow_initialize_from_backup* debe ser **true**.  
  
-   *allow_queued_tran* debe ser **false**.  
  
-   *allow_sync_tran* debe ser **false**.  
  
-   *conflict_policy* debe ser **false**.  
  
-   *independent_agent* debe ser **true**.  
  
-   *repl_freq* debe ser **continua**.  
  
-   *replicate_ddl* debe ser **1**.  
  
 Para obtener más información, consulte [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
`[ \@publish_local_changes_only = ] 'publish_local_changes_only'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ \@enabled_for_het_sub = ] 'enabled_for_het_sub'` Habilita la publicación para que admita suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *enabled_for_het_sub* es de tipo **nvarchar (5)** y su valor predeterminado es false. Un valor de **true** significa que la publicación admite suscriptores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cuando *enabled_for_het_sub* es **true**, se aplican las restricciones siguientes:  
  
-   *allow_initialize_from_backup* debe ser **false**.  
  
-   *allow_push* debe ser **true**.  
  
-   *allow_queued_tran* debe ser **false**.  
  
-   *allow_subscription_copy* debe ser **false**.  
  
-   *allow_sync_tran* debe ser **false**.  
  
-   *autogen_sync_procs* debe ser **false**.  
  
-   *conflict_policy* debe ser null.  
  
-   *enabled_for_internet* debe ser **false**.  
  
-   *enabled_for_p2p* debe ser **false**.  
  
-   *ftp_address* debe ser null.  
  
-   *ftp_subdirectory* debe ser null.  
  
-   *FTP_PASSWORD* debe ser null.  
  
-   *pre_snapshot_script* debe ser null.  
  
-   *post_snapshot_script* debe ser null.  
  
-   *replicate_ddl* debe ser 0.  
  
-   *qreader_job_name* debe ser null.  
  
-   *queue_type* debe ser null.  
  
-   *sync_method* no puede ser **nativo** ni **simultáneo**.  
  
 Para más información, consulte [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
`[ \@p2p_conflictdetection = ] 'p2p_conflictdetection'` Habilita el Agente de distribución para detectar conflictos si la publicación está habilitada para la replicación punto a punto. *p2p_conflictdetection* es de tipo **nvarchar (5)** y su valor predeterminado es true. Para más información, consulte [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
`[ \@p2p_originator_id = ] p2p_originator_id` Especifica un identificador para un nodo en una topología punto a punto. *p2p_originator_id* es de **tipo int** y su valor predeterminado es NULL. Este identificador se usa para la detección de conflictos si *p2p_conflictdetection* está establecido en true. Especifique un identificador positivo distinto de cero que no se haya utilizado jamás en la topología. Para obtener una lista de identificadores que ya se han utilizado, ejecute [sp_help_peerconflictdetection](../../relational-databases/system-stored-procedures/sp-help-peerconflictdetection-transact-sql.md).  
  
`[ \@p2p_continue_onconflict = ] 'p2p_continue_onconflict'` Determina si el Agente de distribución continúa procesando los cambios después de que se detecte un conflicto. *p2p_continue_onconflict* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
> [!CAUTION]  
>  Recomendamos que utilice el valor predeterminado de FALSE. Cuando esta opción está establecida en TRUE, el Agente de distribución intenta converger los datos en la topología aplicando la fila en conflicto del nodo que tiene el Id. de originador más alto. Este método no garantiza la convergencia. Debe asegurarse de que la topología sea coherente una vez detectado un conflicto. Para obtener más información, vea "Controlar los conflictos" en [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
  
`[ \@allow_partition_switch = ] 'allow_partition_switch'` Especifica si se va a modificar la tabla... Las instrucciones SWITCH se pueden ejecutar en la base de datos publicada. *allow_partition_switch* es de tipo **nvarchar (5)** y su valor predeterminado es false. Para obtener más información, vea [Replicar tablas e índices con particiones](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).  
  
`[ \@replicate_partition_switch = ] 'replicate_partition_switch'` Especifica si se va a modificar la tabla... Las instrucciones SWITCH que se ejecutan en la base de datos publicada se deben replicar en los suscriptores. *replicate_partition_switch* es de tipo **nvarchar (5)** y su valor predeterminado es false. Esta opción solo es válida si *allow_partition_switch* está establecido en true.  

## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addpublication** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
 Si existen varias publicaciones que publican el mismo objeto de base de datos, solo las publicaciones con un valor *replicate_ddl* de **1** REPLICARÁN las instrucciones ALTER TABLE, Alter View, ALTER PROCEDURE, ALTER FUNCTION y Alter Trigger DDL. Sin embargo, todas las publicaciones que publiquen la columna quitada replicarán una instrucción ALTER TABLE DROP COLUMN de DDL.  
  
 Con la replicación DDL habilitada (*replicate_ddl*  =  **1**) para una publicación, para realizar cambios de DDL que no se replican en la publicación, [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) se debe ejecutar primero para establecer *replicate_ddl* en **0**. Una vez emitidas las instrucciones DDL que no son de replicación, [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) se pueden ejecutar de nuevo para volver a activar la replicación DDL.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addpublication-transa_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addpublication**. Los inicios de sesión que usan la autenticación de Windows deben tener una cuenta de usuario en la base de datos que represente su cuenta de usuario de Windows. No es suficiente con una cuenta de usuario que represente un grupo de Windows.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addlogreader_agent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)   
 [sp_addpublication_snapshot &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_droppublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [sp_replicationdboption &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)   
 [Publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
