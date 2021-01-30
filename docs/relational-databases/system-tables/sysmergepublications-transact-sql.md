---
description: sysmergepublications (Transact-SQL)
title: sysmergepublications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergepublications
- sysmergepublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepublications system table
ms.assetid: 7f82c6c3-22d1-47c0-a92b-4d64b98cc455
author: cawrites
ms.author: chadam
ms.openlocfilehash: 70cfe557241aa78c60726cc4e094b5945e876c61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99102560"
---
# <a name="sysmergepublications-transact-sql"></a>sysmergepublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada publicación de combinación definida en la base de datos. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Nombre del servidor predeterminado.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador predeterminado.|  
|**name**|**sysname**|Nombre de la publicación.|  
|**description**|**nvarchar(255)**|Descripción breve de la publicación.|  
|**políticas**|**int**|El período de retención para todo el conjunto de publicaciones, donde la unidad se indica mediante el valor de la columna **retention_period_unit** .|  
|**publication_type**|**tinyint**|Indica si la publicación se filtra:<br /><br /> **0** = no filtrado.<br /><br /> **1** = filtrado.|  
|**pubid**|**uniqueidentifier**|Número de identificación único para esta publicación. Se genera cuando se agrega la publicación.|  
|**designmasterid**|**uniqueidentifier**|Reservado para un uso futuro.|  
|**parentid**|**uniqueidentifier**|Indica la publicación primaria a partir de la cual se creó la publicación del mismo nivel o subconjunto actual (utilizado en las topologías jerárquicas de publicación).|  
|**sync_mode**|**tinyint**|Modo de sincronización de esta publicación:<br /><br /> **0** = nativo.<br /><br /> **1** = carácter.|  
|**allow_push**|**int**|Indica si la publicación admite suscripciones de inserción.<br /><br /> **0** = no se permiten suscripciones de extracción.<br /><br /> **1** = se permiten suscripciones de extracción.|  
|**allow_pull**|**int**|Indica si la publicación admite suscripciones extracción.<br /><br /> **0** = no se permiten suscripciones de extracción.<br /><br /> **1** = se permiten suscripciones de extracción.|  
|**allow_anonymous**|**int**|Indica si la publicación admite suscripciones anónimas.<br /><br /> **0** = no se permiten suscripciones anónimas.<br /><br /> **1** = se permiten suscripciones anónimas.|  
|**centralized_conflicts**|**int**|Indica si los registros de conflictos se almacenan en el publicador:<br /><br /> **0** = los registros de conflictos no se almacenan en el publicador.<br /><br /> **1** = los registros de conflictos se almacenan en el publicador.|  
|**status**|**tinyint**|Reservado para un uso futuro.|  
|**snapshot_ready**|**tinyint**|Indica el estado de la instantánea de la publicación:<br /><br /> **0** = la instantánea no está lista para su uso.<br /><br /> **1** = la instantánea está lista para su uso.<br /><br /> **2** = se debe crear una nueva instantánea para esta publicación.|  
|**enabled_for_internet**|**bit**|Indica si los archivos de sincronización de la publicación se exponen en Internet a través de FTP u otros servicios.<br /><br /> **0** = se puede tener acceso a los archivos de sincronización desde Internet.<br /><br /> **1** = no se puede tener acceso a los archivos de sincronización desde Internet.|  
|**dynamic_filters**|**bit**|Indica si la publicación se filtra utilizando un filtro de fila con parámetros.<br /><br /> **0** = la publicación no está filtrada por filas.<br /><br /> **1** = la publicación está filtrada por filas.|  
|**snapshot_in_defaultfolder**|**bit**|Especifica si los archivos de instantánea se almacenan en la carpeta predeterminada:<br /><br /> **0** = los archivos de instantáneas se encuentran en la carpeta predeterminada.<br /><br /> **1** = los archivos de instantáneas se almacenan en la ubicación especificada por **alt_snapshot_folder**.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Ubicación de la carpeta alternativa de la instantánea.|  
|**pre_snapshot_script**|**nvarchar(255)**|Puntero a. archivo **SQL** que el agente de mezcla ejecuta antes que cualquiera de los scripts de objetos de replicación al aplicar la instantánea en el suscriptor.|  
|**post_snapshot_script**|**nvarchar(255)**|Puntero a. archivo **SQL** que el agente de mezcla ejecuta después de que se hayan aplicado todos los demás datos y scripts de objetos de replicación durante una sincronización inicial.|  
|**compress_snapshot**|**bit**|Especifica si la instantánea escrita en la ubicación **alt_snapshot_folder** se comprime en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] formato CAB. **0** especifica que el archivo no está comprimido.|  
|**ftp_address**|**sysname**|Dirección de red del servicio de protocolo de transferencia de archivos (FTP) para el distribuidor. Especifica dónde se encuentran los archivos de instantánea de la publicación para que los recoja el Agente de mezcla, si se ha habilitado FTP.|  
|**ftp_port**|**int**|El número de puerto del servicio FTP para el distribuidor.|  
|**ftp_subdirectory**|**nvarchar(255)**|Subdirectorio en el que están disponibles los archivos de instantánea para que los recoja el Agente de mezcla.|  
|**ftp_login**|**sysname**|Nombre de usuario que se utiliza para conectarse al servicio FTP.|  
|**ftp_password**|**nvarchar (524)**|Contraseña de usuario utilizada para conectarse al servicio FTP.|  
|**conflict_retention**|**int**|Especifica el período de retención, expresado en días, durante el que se conservan los conflictos. Transcurrido ese tiempo, la fila del conflicto se limpia de la tabla de conflictos.|  
|**keep_before_values**|**int**|Especifica si se está optimizando la sincronización para esta publicación:<br /><br /> **0** = la sincronización no está optimizada y las particiones enviadas a todos los suscriptores se comprobarán cuando los datos cambien en una partición.<br /><br /> **1** = la sincronización se optimiza y solo se ven afectados los suscriptores que tienen filas en la partición modificada.|  
|**allow_subscription_copy**|**bit**|Especifica si se ha habilitado la capacidad para copiar la base de datos de suscripciones. **0** significa que no se permite la copia.|  
|**allow_synctoalternate**|**bit**|Especifica si se permite un asociado de sincronización alternativo para sincronizar con este publicador. **0** significa que no se permite un asociado de sincronización.|  
|**validate_subscriber_info**|**nvarchar (500)**|Enumera las funciones que se están utilizando para recuperar información del suscriptor y validar los criterios de filtrado de filas con parámetros del suscriptor.|  
|**ad_guidname**|**sysname**|Especifica si la información de publicación se publica en [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un GUID válido especifica que la publicación se publica en el Active Directory y el GUID es el **objectGUID** del objeto de publicación Active Directory correspondiente. Si es NULL, la publicación no se publica en Active Directory.|  
|**backward_comp_level**|**int**|Nivel de compatibilidad de la base de datos. Puede ser uno de los siguientes valores:<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .|  
|**max_concurrent_merge**|**int**|Número máximo de procesos de mezcla simultáneos permitido. Un valor de **0** para esta propiedad significa que no hay ningún límite en el número de procesos de mezcla simultáneos que se ejecutan en un momento dado. Esta propiedad establece un límite en el número de procesos de mezcla simultáneos que se pueden ejecutar con una publicación de combinación en un momento determinado. Si hay más procesos de instantánea programados al mismo tiempo que los que permite ejecutar el valor, los trabajos sobrantes se colocarán en una cola y esperarán hasta que finalice el proceso de mezcla que se está ejecutando actualmente.|  
|**max_concurrent_dynamic_snapshots**|**int**|Número máximo de sesiones de instantánea de datos filtrados simultáneas que se pueden ejecutar con la publicación de combinación. Si es **0**, no hay ningún límite en el número máximo de sesiones de instantáneas de datos filtrados simultáneas que se pueden ejecutar simultáneamente en la publicación en un momento dado. Esta propiedad establece un límite en el número de procesos de instantánea simultáneos que se pueden ejecutar con una publicación de combinación en un momento determinado. Si hay más procesos de instantánea programados al mismo tiempo que los que permite ejecutar el valor, los trabajos sobrantes se colocarán en una cola y esperarán hasta que finalice el proceso de mezcla que se está ejecutando actualmente.|  
|**use_partition_groups**|**smallint**|Especifica si la publicación utiliza particiones precalculadas.|  
|**dynamic_filters_function_list**|**nvarchar (500)**|Lista de funciones separadas por punto y coma que se utiliza en los filtros de fila con parámetros de la publicación.|  
|**partition_id_eval_proc**|**sysname**|Especifica el nombre del procedimiento ejecutado por el Agente de mezcla de un suscriptor para determinar su Id. de partición asignada.|  
|**publication_number**|**smallint**|Especifica la columna de identidad que proporciona una asignación de 2 bytes a **pubid**. **pubid** es un identificador único global para una publicación, mientras que el número de publicación solo es único en una base de datos especificada.|  
|**replicate_ddl**|**int**|Indica si se admite la replicación de esquemas para la publicación.<br /><br /> **0** = las instrucciones de DDL no se replican.<br /><br /> **1** = las instrucciones de DDL ejecutadas en el publicador se replican.<br /><br /> Para más información, vea [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).|  
|**allow_subscriber_initiated_snapshot**|**bit**|Indica que los suscriptores pueden iniciar el proceso que genera la instantánea para una publicación utilizando filtros con parámetros. **1** indica que los suscriptores pueden iniciar el proceso de instantánea.|  
|**dynamic_snapshot_queue_timeout**|**int**|Especifica cuántos minutos debe esperar en la cola un suscriptor para que empiece el proceso de generación de instantáneas al utilizar filtros con parámetros.|  
|**dynamic_snapshot_ready_timeout**|**int**|Especifica cuántos minutos debe esperar un suscriptor para que empiece el proceso de generación de instantáneas al utilizar filtros con parámetros.|  
|**distribuidor**|**sysname**|Nombre del distribuidor de la publicación.|  
|**snapshot_jobid**|**binario (16)**|Identifica el trabajo de agente que genera la instantánea cuando el suscriptor puede iniciar el proceso de generación correspondiente.|  
|**allow_web_synchronization**|**bit**|Especifica si la publicación está habilitada para la sincronización Web, donde **1** significa que la sincronización web está habilitada para la publicación.|  
|**web_synchronization_url**|**nvarchar (500)**|Especifica el valor predeterminado de la dirección URL de Internet utilizada para la sincronización web.|  
|**allow_partition_realignment**|**bit**|Indica si se envían eliminaciones al suscriptor cuando una modificación de la fila del publicador provoca que se modifique su partición.<br /><br /> **0** = los datos de una partición antigua se dejarán en el suscriptor, donde los cambios realizados en estos datos en el publicador no se replicarán en este suscriptor, pero los cambios realizados en el suscriptor se replicarán en el publicador.<br /><br /> **1** = elimina en el suscriptor para reflejar los resultados de un cambio de partición quitando los datos que ya no forman parte de la partición del suscriptor.<br /><br /> Para obtener más información, vea [sp_addmergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).<br /><br /> Nota: los datos que permanecen en el suscriptor cuando este valor es **0** deberían tratarse como si fueran de solo lectura; sin embargo, el sistema de replicación no aplica estrictamente esto.|  
|**retention_period_unit**|**tinyint**|Define la unidad utilizada al definir la *retención*, que puede ser uno de estos valores:<br /><br /> **0** = día.<br /><br /> **1** = semana.<br /><br /> **2** = mes.<br /><br /> **3** = año.|  
|**decentralized_conflicts**|**int**|Indica si los registros de conflictos se almacenan en el suscriptor que provocó el conflicto:<br /><br /> **0** = los registros de conflictos no se almacenan en el suscriptor.<br /><br /> **1** = los registros de conflictos se almacenan en el suscriptor.|  
|**generation_leveling_threshold**|**int**|Especifica el número de cambios contenidos en una generación. Una generación es una colección de cambios que se entregan a un publicador o a un suscriptor.|  
|**automatic_reinitialization_policy**|**bit**|Indica si se cargan los cambios desde el suscriptor antes de que se produzca una reinicialización automática.<br /><br /> **1** = los cambios se cargan desde el suscriptor antes de que se produzca una reinicialización automática.<br /><br /> **0** = los cambios no se cargan antes de una reinicialización automática.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)  
  
  
