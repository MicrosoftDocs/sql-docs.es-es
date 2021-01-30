---
description: sp_helppublication (Transact-SQL)
title: sp_helppublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/18/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helppublication_TSQL
- sp_helppublication
helpviewer_keywords:
- sp_helppublication
ms.assetid: e801c3f0-dcbd-4b4a-b254-949a05f63518
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e45091866308b3c674217e0582bb64da59d51f75
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210242"
---
# <a name="sp_helppublication-transact-sql"></a>sp_helppublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve información acerca de una publicación. En el caso de una [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicación, este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación. En el caso de una publicación Oracle, se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helppublication [ [ @publication = ] 'publication' ]  
    [ , [ @found=] found OUTPUT]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación que se va a ver. *Publication* es de tipo sysname y su valor predeterminado es **%** , que devuelve información acerca de todas las publicaciones.  
  
`[ @found = ] 'found' OUTPUT` Es una marca que indica que se devuelven filas. *found* es de **tipo int** y un parámetro output, con un valor predeterminado de **23456**. **1** indica que se ha encontrado la publicación. **0** indica que no se encuentra la publicación.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de tipo sysname y su valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe especificar el *publicador* al solicitar información de publicación de un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|pubid|**int**|IDENTIFICADOR de la publicación.|  
|name|**sysname**|Nombre de la publicación.|  
|restricted|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|status|**tinyint**|Estado actual de la publicación.<br /><br /> **0** = inactivo.<br /><br /> **1** = activo.|  
|task||Se utiliza para mantener la compatibilidad con versiones anteriores.|  
|replication frequency|**tinyint**|Tipo de frecuencia de replicación:<br /><br /> **0** = transaccional<br /><br /> **1** = instantánea|  
|synchronization method|**tinyint**|Modo de sincronización:<br /><br /> **0** = programa nativo de copia masiva (utilidad **BCP** )<br /><br /> **1** = copia masiva de caracteres<br /><br /> **3** = simultánea, lo que significa que se usa la copia masiva nativa (utilidad **BCP**), pero no se bloquean las tablas durante la instantánea.<br /><br /> **4** = Concurrent_c, lo que significa que se utiliza la copia masiva de caracteres pero no se bloquean las tablas durante la instantánea.|  
|description|**nvarchar(255)**|Descripción opcional de la publicación.|  
|immediate_sync|**bit**|Indica si los archivos de sincronización se crean, o se vuelven a crear, cada vez que se ejecuta el Agente de instantáneas.|  
|enabled_for_internet|**bit**|Indica si los archivos de sincronización de la publicación se exponen en Internet a través del protocolo de transferencias de archivos (FTP) u otros servicios.|  
|allow_push|**bit**|Indica si se admiten suscripciones de inserción a la publicación.|  
|allow_pull|**bit**|Indica si se admiten suscripciones de extracción a la publicación.|  
|allow_anonymous|**bit**|Indica si se admiten suscripciones anónimas a la publicación.|  
|independent_agent|**bit**|Indica si hay un Agente de distribución independiente para esta publicación.|  
|immediate_sync_ready|**bit**|Indica si el Agente de instantáneas generó o no una instantánea que está lista para que la utilicen las nuevas suscripciones. Este parámetro se define únicamente si la publicación se define para tener siempre una instantánea disponible para las suscripciones nuevas o reinicializadas.|  
|allow_sync_tran|**bit**|Indica si se permiten suscripciones de actualización inmediata a la publicación.|  
|autogen_sync_procs|**bit**|Indica si se generan automáticamente procedimientos almacenados para admitir las suscripciones de actualización inmediata.|  
|snapshot_jobid|**binario (16)**|Id. de tarea programada.|  
|retention|**int**|Volumen de cambio, en horas, que se debe guardar para la publicación indicada.|  
|has subscription|**bit**|Indica si la publicación tiene suscripciones activas. **1** significa que la publicación tiene suscripciones activas y **0** significa que la publicación no tiene suscripciones.|  
|allow_queued_tran|**bit**|Especifica si se han habilitado las deshabilitaciones de colocación en cola de los cambios del suscriptor hasta que se puedan aplicar en el publicador. Si es **0**, los cambios en el suscriptor no se ponen en cola.|  
|snapshot_in_defaultfolder|**bit**|Especifica si los archivos de instantáneas se almacenan en la carpeta predeterminada. Si es **0**, los archivos de instantáneas se han almacenado en la ubicación alternativa especificada por *alternate_snapshot_folder*. Si es **1**, los archivos de instantáneas se pueden encontrar en la carpeta predeterminada.|  
|alt_snapshot_folder|**nvarchar(255)**|Especifica la ubicación de la carpeta alternativa de la instantánea.|  
|pre_snapshot_script|**nvarchar(255)**|Especifica un puntero a una ubicación de archivos **. SQL** . El Agente de distribución ejecutará el script previo a la instantánea antes de la ejecución de cualquiera de los scripts de objetos replicados al aplicar la instantánea en un suscriptor.|  
|post_snapshot_script|**nvarchar(255)**|Especifica un puntero a una ubicación de archivos **. SQL** . El Agente de distribución ejecutará el script posterior a la instantánea después de que se apliquen el resto de scripts de objetos replicados y datos durante la sincronización inicial.|  
|compress_snapshot|**bit**|Especifica que la instantánea que se escribe en la ubicación *alt_snapshot_folder* se va a comprimir en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] formato CAB. **0** especifica que no se comprimirá la instantánea.|  
|ftp_address|**sysname**|Dirección de red del servicio FTP para el distribuidor. Especifica dónde se encuentran los archivos de instantánea de una publicación para que los recoja el Agente de distribución o el Agente de mezcla de un suscriptor.|  
|ftp_port|**int**|El número de puerto del servicio FTP para el distribuidor.|  
|ftp_subdirectory|**nvarchar(255)**|Especifica dónde estarán disponibles los archivos de instantánea para que los recoja el Agente de distribución o el Agente de mezcla del suscriptor si la publicación admite la propagación de instantáneas mediante FTP.|  
|ftp_login|**sysname**|El nombre de usuario utilizado para conectarse al servicio FTP.|  
|allow_dts|**bit**|Especifica que la publicación permite transformaciones de datos. **0** especifica que no se permiten las transformaciones DTS.|  
|allow_subscription_copy|**bit**|Especifica si se ha habilitado la capacidad de copiar las bases de datos de suscripciones que se suscriben a esta publicación. **0** significa que no se permite la copia.|  
|centralized_conflicts|**bit**|Especifica si los registros de conflicto se almacenan en el publicador.<br /><br /> **0** = los registros de conflictos se almacenan tanto en el publicador como en el suscriptor que provocó el conflicto.<br /><br /> **1** = los registros de conflictos se almacenan en el publicador.|  
|conflict_retention|**int**|Especifica el período de retención de conflictos, en días.|  
|conflict_policy|**int**|Especifica la directiva de resolución de conflictos seguida cuando se utiliza la opción de suscriptor de actualización en cola. Puede ser uno de estos valores:<br /><br /> **1** = el publicador gana el conflicto.<br /><br /> **2** = el suscriptor gana el conflicto.<br /><br /> **3** = la suscripción se ha reinicializado.|  
|queue_type||Especifica el tipo de cola utilizado. Puede ser uno de estos valores:<br /><br /> **MSMQ** = usar [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queue Server para almacenar las transacciones.<br /><br /> **SQL** = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se usa para almacenar transacciones.<br /><br /> Nota: se ha interrumpido la compatibilidad con Message Queue Server.|  
|backward_comp_level||Nivel de compatibilidad de la base de datos, que puede ser uno de los que se especifican a continuación:<br /><br />   =  90 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br />   =  100 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|publish_to_AD|**bit**|Especifica si la información de publicación se publica en [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un valor de **1** indica que se ha publicado y un valor de **0** indica que no se ha publicado.|  
|allow_initialize_from_backup|**bit**|Indica si los suscriptores pueden inicializar una suscripción a esta publicación a partir de una copia de seguridad en lugar de una instantánea inicial. **1** significa que las suscripciones se pueden inicializar a partir de una copia de seguridad y **0** significa que no pueden hacerlo. Para obtener más información, vea [inicializar una suscripción transaccional sin una instantánea](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md) de un suscriptor transaccional sin una instantánea.|  
|replicate_ddl|**int**|Indica si la publicación admite replicación de esquema. **1** indica que las instrucciones del lenguaje de definición de datos (DDL) ejecutadas en el publicador se replican y **0** indica que las instrucciones de DDL no se replican. Para más información, vea [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).|  
|enabled_for_p2p|**int**|Indica si la publicación se puede utilizar en una topología de replicación punto a punto. **1** indica que la publicación admite la replicación punto a punto. Para obtener más información, consulte [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).|  
|publish_local_changes_only|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|enabled_for_het_sub|**int**|Especifica si la publicación admite suscriptores que no son [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un valor de **1** significa que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se admiten suscriptores que no son de. Un valor de **0** significa que solo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se admiten los suscriptores. Para más información, consulte [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).|  
|enabled_for_p2p_conflictdetection|**int**|Especifica si el Agente de distribución detecta los conflictos para una publicación que está habilitada para la replicación punto a punto. Un valor de **1** significa que se detectan conflictos. Para más información, consulte [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
|originator_id|**int**|Especifica un Id. para un nodo en una topología punto a punto. Este identificador se usa para la detección de conflictos si **enabled_for_p2p_conflictdetection** está establecido en **1**. Para obtener una lista de identificadores que ya se hayan utilizado, consulte la tabla del sistema [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md) .|  
|p2p_continue_onconflict|**int**|Especifica si el Agente de distribución continúa procesando los cambios cuando se detecta un conflicto. Un valor de **1** significa que el agente continúa procesando los cambios.<br /><br /> **\* \* PRECAUCIÓN \* se \*** recomienda usar el valor predeterminado de **0**. Cuando esta opción está establecida en **1**, la agente de distribución intenta converger los datos en la topología aplicando la fila en conflicto del nodo que tiene el identificador de originador más alto. Este método no garantiza la convergencia. Debe asegurarse de que la topología sea coherente una vez detectado un conflicto. Para obtener más información, vea "Controlar los conflictos" en [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
|allow_partition_switch|**int**|Especifica si se va a modificar la tabla... Las instrucciones SWITCH se pueden ejecutar en la base de datos publicada. Para obtener más información, vea [Replicar tablas e índices con particiones](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).|  
|replicate_partition_switch|**int**|Especifica si se va a modificar la tabla... Las instrucciones SWITCH que se ejecutan en la base de datos publicada se deben replicar en los suscriptores. Esta opción solo es válida si *allow_partition_switch* está establecido en **1**.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 El procedimiento sp_helppublication se utiliza en la replicación de instantáneas y transaccional.  
  
 sp_helppublication devolverá información sobre todas las publicaciones que son propiedad del usuario que ejecuta este procedimiento.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_helppublication](../../relational-databases/replication/codesnippet/tsql/sp-helppublication-trans_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor sysadmin en el publicador o los miembros del rol fijo de base de datos db_owner en la base de datos de publicación o los usuarios de la lista de acceso a la publicación (PAL) pueden ejecutar sp_helppublication.  
  
 Para un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , solo los miembros del rol fijo de servidor sysadmin en el distribuidor o los miembros del rol fijo de base de datos db_owner en la base de datos de distribución o los usuarios de PAL pueden ejecutar sp_helppublication.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar propiedades de publicación](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addpublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_droppublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
