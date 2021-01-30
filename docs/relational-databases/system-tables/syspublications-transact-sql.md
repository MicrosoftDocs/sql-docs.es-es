---
description: syspublications (Transact-SQL)
title: syspublications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- syspublications
- syspublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspublications system table
ms.assetid: a86eb4f5-1f7b-493e-af55-3d15cf878228
author: cawrites
ms.author: chadam
ms.openlocfilehash: afd2ba410a18aab5acf7bdde290aa498659b8cfb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180399"
---
# <a name="syspublications-transact-sql"></a>syspublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada publicación definida en la base de datos. Esta tabla se almacena en la base de datos de publicación.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**description**|**nvarchar(255)**|Entrada descriptiva para la publicación.|  
|**name**|**sysname**|Nombre único asociado a la publicación.|  
|**pubid**|**int**|Columna de identidad que proporciona un id. único para la publicación.|  
|**repl_freq**|**tinyint**|Frecuencia de replicación:<br /><br /> **0** = basada en transacciones.<br /><br /> **1** = actualización programada de la tabla.|  
|**status**|**tinyint**|El estado:<br /><br /> **0** = inactivo.<br /><br /> **1** = activo.|  
|**sync_method**|**tinyint**|Método de sincronización:<br /><br /> **0** = utilidad de programa de copia masiva en modo nativo (**BCP**).<br /><br /> **1** = BCP de modo de carácter.<br /><br /> **3** = simultáneo, lo que significa que se utiliza BCP en modo nativo, pero las tablas no se bloquean durante la instantánea.<br /><br /> **4** = Concurrent_c, lo que significa que se utiliza BCP de modo de carácter, pero las tablas no se bloquean durante la instantánea.|  
|**snapshot_jobid**|**binario (16)**|Id. de la tarea programada.|  
|**independent_agent**|**bit**|Especifica si hay un Agente de distribución independiente para esta publicación.<br /><br /> **0** = la publicación utiliza una agente de distribución compartida y cada par de base de datos del publicador/base de datos de suscriptor tiene un único agente compartido.<br /><br /> **1** = hay un agente de distribución independiente para esta publicación.|  
|**immediate_sync**|**bit**|Indica si los archivos de sincronización se crean o se recrean cada vez que se ejecuta el Agente de instantáneas, donde **1** significa que se crean cada vez que se ejecuta el agente.|  
|**enabled_for_internet**|**bit**|Indica si los archivos de sincronización de la publicación se exponen a Internet a través del Protocolo de transferencia de archivos (FTP) y otros servicios, donde **1** significa que se puede tener acceso a ellos desde Internet.|  
|**allow_push**|**bit**|Indica si se permiten suscripciones de extracción en la publicación, donde **1** significa que están permitidas.|  
|**allow_pull**|**bit**|Indica si se permiten suscripciones de extracción en la publicación, donde **1** significa que están permitidas.|  
|**allow_anonymous**|**bit**|Indica si se permiten suscripciones anónimas en la publicación, donde **1** significa que están permitidas.|  
|**immediate_sync_ready**|**bit**|Indica si el Agente de instantáneas ha generado la instantánea y está lista para que la utilicen las nuevas suscripciones. Es significativo únicamente para publicaciones de actualización inmediata. **1** indica que la instantánea está lista.|  
|**allow_sync_tran**|**bit**|Especifica si se permiten las suscripciones de actualización inmediata en la publicación. **1** significa que se permiten las suscripciones de actualización inmediata.|  
|**autogen_sync_procs**|**bit**|Especifica si se ha generado en el publicador un procedimiento almacenado de sincronización para suscripciones de actualización inmediata. **1** significa que se genera en el publicador.|  
|**políticas**|**int**|El volumen de cambios, en horas, que se deben guardar para la publicación indicada.|  
|**allowed_queued_tran**|**bit**|Especifica si se ha habilitado la colocación en cola de los cambios del suscriptor hasta que se puedan aplicar en el publicador. Si es **1**, los cambios en el suscriptor se ponen en cola.|  
|**snapshot_in_defaultfolder**|**bit**|Especifica si los archivos de instantáneas se almacenan en la carpeta predeterminada.<br /><br /> **0** = los archivos de instantánea se han almacenado en la ubicación alternativa especificada por *alternate_snapshot_folder*.<br /><br /> **1** = los archivos de instantáneas se pueden encontrar en la carpeta predeterminada.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Especifica la ubicación de la carpeta alternativa de la instantánea.|  
|**pre_snapshot_script**|**nvarchar(255)**|Especifica un puntero a la ubicación de un archivo **. SQL** . El Agente de distribución ejecuta el script previo a la instantánea antes de la ejecución de cualquiera de los scripts de los objetos replicados al aplicar la instantánea en un suscriptor.|  
|**post_snapshot_script**|**nvarchar(255)**|Especifica un puntero a la ubicación de un archivo **. SQL** . El Agente de distribución ejecuta el script posterior a la instantánea después de que se aplique el resto de scripts de objetos replicados y datos durante la sincronización inicial.|  
|**compress_snapshot**|**bit**|Especifica que la instantánea que se escribe en la ubicación *alt_snapshot_folder* se va a comprimir en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] formato CAB.**1** significa que se comprimirá la instantánea.|  
|**ftp_address**|**sysname**|Dirección de red del servicio FTP para el distribuidor. Especifica dónde se encuentran los archivos de instantánea de la publicación para que los recoja el Agente de distribución.|  
|**ftp_port**|**int**|El número de puerto del servicio FTP para el distribuidor. Especifica dónde se encuentran los archivos de instantáneas de la publicación a fin de que los recopile el Agente de distribución.|  
|**ftp_subdirectory**|**nvarchar(255)**|Especifica dónde estarán disponibles los archivos de instantáneas para que los recopile el Agente de distribución si la publicación admite la propagación de instantáneas mediante FTP.|  
|**ftp_login**|**sysname**|El nombre de usuario utilizado para conectarse al servicio FTP.|  
|**ftp_password**|**nvarchar (524)**|Contraseña de usuario utilizada para conectarse al servicio FTP.|  
|**allow_dts**|**bit**|Especifica si la publicación permite transformaciones de datos. **1** especifica que se permiten las transformaciones DTS.|  
|**allow_subscription_copy**|**bit**|Especifica si se ha habilitado la capacidad de copiar las bases de datos de suscripciones que se suscriben a esta publicación. **1** significa que se permite la copia.|  
|**centralized_conflicts**|**bit**|Especifica si los registros de conflicto se almacenan en el publicador.<br /><br /> **0** = los registros de conflictos se almacenan tanto en el publicador como en el suscriptor que provocó el conflicto.<br /><br /> **1** = los registros de conflictos se almacenan en el publicador.|  
|**conflict_retention**|**int**|Especifica el período de retención de conflictos, en días.|  
|**conflict_policy**|**int**|Especifica la directiva de resolución de conflictos seguida cuando se utiliza la opción de suscriptor de actualización en cola. Puede ser uno de estos valores:<br /><br /> **1** = el publicador gana el conflicto.<br /><br /> **2** = el suscriptor gana el conflicto.<br /><br /> **3** = la suscripción se ha reinicializado.|  
|**queue_type**|**int**|Especifica el tipo de cola utilizado. Puede ser uno de estos valores:<br /><br /> **1** = MSMQ, que utiliza [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queue Server para almacenar las transacciones.<br /><br /> **2** = SQL, que utiliza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para almacenar transacciones.<br /><br /> Nota: el uso [!INCLUDE[msCoName](../../includes/msconame-md.md)] de Message Queue Server ha quedado desusado y ya no está disponible.|  
|**ad_guidname**|**sysname**|Especifica si la información de publicación se publica en [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un identificador único global (GUID) válido especifica que la publicación se publica en el Active Directory y el GUID es el **objectGUID** del objeto de publicación Active Directory correspondiente. Si es NULL, la publicación no se publica en Active Directory.|  
|**backward_comp_level**|**int**|Nivel de compatibilidad de la base de datos, que puede ser uno de los valores siguientes:<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .<br /><br /> **110**  =  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] .<br /><br /> **120**  =  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] .|  
|**allow_initialize_from_backup**|**bit**|Indica si los suscriptores pueden inicializar una suscripción a esta publicación a partir de una copia de seguridad en lugar de una instantánea inicial. **1** significa que las suscripciones se pueden inicializar a partir de una copia de seguridad y **0** significa que no pueden hacerlo. Para obtener más información, consulte [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).|  
|**min_autonosync_lsn**|**binary**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**replicate_ddl**|**int**|Indica si se admite la replicación de esquemas para la publicación. **1** indica que las instrucciones del lenguaje de definición de datos (DDL) ejecutadas en el publicador se replican y **0** indica que las instrucciones de DDL no se replican. Para más información, vea [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).|  
|**options**|**int**|Mapa de bits que especifica opciones de publicación adicionales. Los valores de opciones binarias son los siguientes:<br /><br /> **0x1** : habilitada para la replicación punto a punto.<br /><br /> **0X2** -publicar solo cambios locales para la replicación punto a punto.<br /><br /> **0x4** : habilitado para suscriptores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **0x8** : habilitado para la detección de conflictos punto a punto.|  
|**originator_id**|**smallint**|Identifica cada nodo en una topología de replicación punto a punto para detectar conflictos. Para más información, consulte [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addpublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)  
  
  
