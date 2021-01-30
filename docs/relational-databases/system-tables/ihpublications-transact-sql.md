---
description: IHpublications (Transact-SQL)
title: IHpublications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHpublications_TSQL
- IHpublications
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublications system table
ms.assetid: b519a101-fa53-44be-bd55-6ea79245b5d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: cb4402e34f11463761dda8ce872568dffb029906
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201686"
---
# <a name="ihpublications-transact-sql"></a>IHpublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla del sistema **IHpublications** contiene una fila por cada publicación que no sea de SQL Server mediante el distribuidor actual. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**pubid**|**int**|Columna de identidad que proporciona un id. único para la publicación.|  
|**name**|**sysname**|Nombre único asociado a la publicación.|  
|**repl_freq**|**tinyint**|Frecuencia de replicación:<br /><br /> **0** = basada en transacciones.<br /><br /> **1** = actualización programada de la tabla.|  
|**status**|**tinyint**|El estado de la publicación, que puede ser uno de los siguientes.<br /><br /> **0** = inactivo.<br /><br /> **1** = activo.|  
|**sync_method**|**tinyint**|Método de sincronización:<br /><br /> **1** = copia masiva de caracteres.<br /><br /> **4** = Concurrent_c, lo que significa que se utiliza la copia masiva de caracteres pero no se bloquean las tablas durante la instantánea.|  
|**snapshot_jobid**|**binary**|Id. de la tarea programada.|  
|**enabled_for_internet**|**bit**|Indica si los archivos de sincronización de la publicación se exponen a Internet a través de FTP y otros servicios, donde **1** significa que se puede tener acceso a ellos desde Internet.|  
|**immediate_sync_ready**|**bit**|Indica si los archivos de sincronización están disponibles, donde **1** significa que están disponibles. *No se admite en los publicadores que no son de SQL.*|  
|**allow_queued_tran**|**bit**|Especifica si se ha habilitado la colocación en cola de los cambios del suscriptor hasta que se puedan aplicar en el publicador. Si es **1**, los cambios en el suscriptor se ponen en cola. *No se admite en los publicadores que no son de SQL.*|  
|**allow_sync_tran**|**bit**|Especifica si se permiten las suscripciones de actualización inmediata en la publicación. **1** significa que se permiten las suscripciones de actualización inmediata. *No se admite en los publicadores que no son de SQL.*|  
|**autogen_sync_procs**|**bit**|Especifica si se ha generado en el publicador un procedimiento almacenado de sincronización para suscripciones de actualización inmediata. **1** significa que se genera en el publicador. *No se admite en los publicadores que no son de SQL.*|  
|**snapshot_in_defaultfolder**|**bit**|Especifica si los archivos de instantáneas se almacenan en la carpeta predeterminada. Si es **0**, los archivos de instantáneas se han almacenado en la ubicación alternativa especificada por *alternate_snapshot_folder*. Si es **1**, los archivos de instantáneas se pueden encontrar en la carpeta predeterminada.|  
|**alt_snapshot_folder**|**nvarchar (510)**|Especifica la ubicación de la carpeta alternativa de la instantánea.|  
|**pre_snapshot_script**|**nvarchar (510)**|Especifica un puntero a la ubicación de un archivo **. SQL** . El Agente de distribución ejecuta el script previo a la instantánea antes de la ejecución de cualquiera de los scripts de los objetos replicados al aplicar la instantánea en un suscriptor.|  
|**post_snapshot_script**|**nvarchar (510)**|Especifica un puntero a la ubicación de un archivo **. SQL** . El Agente de distribución ejecuta el script posterior a la instantánea después de que se aplique el resto de scripts de objetos replicados y datos durante la sincronización inicial.|  
|**compress_snapshot**|**bit**|Especifica que la instantánea que se escribe en la ubicación *alt_snapshot_folder* se va a comprimir en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] formato CAB. **0** especifica que no se comprimirá la instantánea.|  
|**ftp_address**|**sysname**|Dirección de red del servicio FTP para el distribuidor. Especifica dónde se encuentran los archivos de instantánea de la publicación para que los recoja el Agente de distribución.|  
|**ftp_port**|**int**|El número de puerto del servicio FTP para el distribuidor. Especifica dónde se encuentran los archivos de instantáneas de la publicación a fin de que los recopile el Agente de distribución.|  
|**ftp_subdirectory**|**nvarchar (510)**|Especifica dónde están disponibles los archivos de instantánea para que los recoja el Agente de distribución si la publicación admite la propagación de instantáneas mediante FTP.|  
|**ftp_login**|**nvarchar(256)**|El nombre de usuario utilizado para conectarse al servicio FTP.|  
|**ftp_password**|**nvarchar (1048)**|Contraseña de usuario utilizada para conectarse al servicio FTP.|  
|**allow_dts**|**bit**|Especifica que la publicación permite transformaciones de datos. **1** especifica que se permiten las transformaciones DTS. *No se admite en los publicadores que no son de SQL.*|  
|**allow_anonymous**|**bit**|Indica si se permiten suscripciones anónimas en la publicación, donde **1** significa que están permitidas.|  
|**centralized_conflicts**|**bit**|Especifica si los registros de conflicto se almacenan en el publicador.<br /><br /> **0** = los registros de conflictos se almacenan tanto en el publicador como en el suscriptor que provocó el conflicto.<br /><br /> **1** = los registros de conflictos se almacenan en el publicador.<br /><br /> *No se admite en los publicadores que no son de SQL.*|  
|**conflict_retention**|**int**|Especifica el período de retención de conflictos, en días. *No se admite en los publicadores que no son de SQL.*|  
|**conflict_policy**|**int**|Especifica la directiva de resolución de conflictos seguida cuando se utiliza la opción de suscriptor de actualización en cola. Puede ser uno de estos valores:<br /><br /> **1** = el publicador gana el conflicto.<br /><br /> **2** = el suscriptor gana el conflicto.<br /><br /> **3** = la suscripción se ha reinicializado.<br /><br /> *No se admite en los publicadores que no son de SQL.*|  
|**queue_type**|**int**|Especifica el tipo de cola utilizado. Puede ser uno de estos valores:<br /><br /> **1** = MSMQ, que utiliza [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queue Server para almacenar las transacciones.<br /><br /> **2** = SQL, que utiliza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para almacenar transacciones.<br /><br /> Esta columna no la usan los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Nota: el uso [!INCLUDE[msCoName](../../includes/msconame-md.md)] de Message Queue Server ha quedado desusado y ya no se admite.<br /><br /> *Esta columna no se admite para publicadores que no sean de SQL.*|  
|**ad_guidname**|**sysname**|Especifica si la información de publicación se publica en [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un identificador único global (GUID) válido especifica que la publicación se publica en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory y el GUID es el **objectGUID** del objeto de publicación Active Directory correspondiente. Si es NULL, la publicación no se publica en [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. *No se admite en los publicadores que no son de SQL.*|  
|**backward_comp_level**|**int**|Nivel de compatibilidad de la base de datos, que puede ser uno de los valores siguientes:<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .<br /><br /> *No se admite en los publicadores que no son de SQL.*|  
|**description**|**nvarchar(255)**|Entrada descriptiva de la publicación.|  
|**independent_agent**|**bit**|Especifica si hay un Agente de distribución independiente para esta publicación.<br /><br /> **0** = la publicación utiliza una agente de distribución compartida y cada par de base de datos del publicador/base de datos de suscriptor tiene un único agente compartido.<br /><br /> **1** = hay un agente de distribución independiente para esta publicación.|  
|**immediate_sync**|**bit**|Indica si los archivos de sincronización se crean o se recrean cada vez que se ejecuta el Agente de instantáneas, donde **1** significa que se crean cada vez que se ejecuta el agente.|  
|**allow_push**|**bit**|Indica si se permiten suscripciones de extracción en la publicación, donde **1** significa que están permitidas.|  
|**allow_pull**|**bit**|Indica si se permiten suscripciones de extracción en la publicación, donde **1** significa que están permitidas.|  
|**políticas**|**int**|El volumen de cambios, en horas, que se deben guardar para la publicación indicada.|  
|**allow_subscription_copy**|**bit**|Especifica si se ha habilitado la capacidad de copiar las bases de datos de suscripciones que se suscriben a esta publicación. **1** significa que se permite la copia.|  
|**allow_initialize_from_backup**|**bit**|Indica si los suscriptores pueden inicializar una suscripción a esta publicación a partir de una copia de seguridad en lugar de una instantánea inicial. **1** significa que las suscripciones se pueden inicializar a partir de una copia de seguridad y **0** significa que no pueden hacerlo. Para obtener más información, consulte [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md). *No se admite en los publicadores que no son de SQL.*|  
|**min_autonosync_lsn**|**binario (1)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**replicate_ddl**|**int**|Indica si la publicación admite replicación de esquema. **1** indica que las instrucciones de DDL ejecutadas en el publicador se replican y **0** indica que las instrucciones de DDL no se replican. Para más información, vea [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md). *No se admite en los publicadores que no son de SQL.*|  
|**options**|**int**|Mapa de bits que especifica las opciones de publicación adicionales, donde los valores de la opción bit a bit son:<br /><br /> **0x1** : habilitada para la replicación punto a punto.<br /><br /> **0X2** -publicar solo cambios locales.<br /><br /> **0x4** : habilitado para suscriptores que no son de SQL Server.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addpublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [syspublications &#40;vista del sistema&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/syspublications-system-view-transact-sql.md)   
 [syspublications &#40;Transact-SQL&#41;](../../relational-databases/system-tables/syspublications-transact-sql.md)  
  
  
