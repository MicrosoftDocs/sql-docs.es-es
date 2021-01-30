---
description: sp_helpmergepublication (Transact-SQL)
title: sp_helpmergepublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepublication
- sp_helpmergepublication_TSQL
helpviewer_keywords:
- sp_helpmergepublication
ms.assetid: dfe1e1e1-9a65-406a-aced-6385a078e135
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8001e98b207ca9493ff71b1f874b12b0d5227306
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160605"
---
# <a name="sp_helpmergepublication-transact-sql"></a>sp_helpmergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información sobre una publicación de combinación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpmergepublication [ [ @publication = ] 'publication' ]  
    [ , [ @found = ] 'found' OUTPUT ]  
    [ , [ @publication_id = ] 'publication_id' OUTPUT ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @publication **=** ] **'**_publicación_**'**  
 Nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve información acerca de todas las publicaciones de combinación en la base de datos actual.  
  
 [ @found **=** ] **'**_se encontró_ el resultado *_'_*  
 Marca que indica las filas devueltas. *found* es de **tipo int** y un parámetro output, con un valor predeterminado de NULL. **1** indica que se ha encontrado la publicación. **0** indica que no se encuentra la publicación.  
  
 salida de [ @publication_id **=** ] **'**_publication_id_*_'_*  
 El número de identificación de la publicación. *publication_id* es de tipo **uniqueidentifier** y un parámetro output, con un valor predeterminado de NULL.  
  
 [ @reserved **=** ] **'**_reservado_*_'_*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]*Reserved* es de tipo **nvarchar (20)** y su valor predeterminado es NULL.  
  
 [ @publisher **=** ] **'**_publicador_*_'_*  
 El nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
 [ @publisher_db **=** ] **'**_publisher_db_*_'_*  
 Nombre de la base de datos de publicación. *publisher_db* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|id|**int**|Orden secuencial de la publicación en la lista del conjunto de resultados.|  
|name|**sysname**|Nombre de la publicación.|  
|description|**nvarchar(255)**|Descripción de la publicación.|  
|status|**tinyint**|Indica cuándo están disponibles los datos de la publicación.|  
|retention|**int**|Tiempo durante el que guardar los metadatos sobre los cambios de los artículos de la publicación. Las unidades de este período pueden ser días, semanas, meses o años. Para obtener información sobre las unidades, vea la columna retention_period_unit.|  
|sync_mode|**tinyint**|Modo de sincronización de esta publicación:<br /><br /> **0** = programa nativo de copia masiva (utilidad **BCP** )<br /><br /> **1** = copia masiva de caracteres|  
|allow_push|**int**|Determina si es posible crear suscripciones de inserción para la publicación indicada. **0** significa que no se permite una suscripción de extracción.|  
|allow_pull|**int**|Determina si es posible crear suscripciones de extracción para la publicación indicada. **0** significa que no se permite una suscripción de extracción.|  
|allow_anonymous|**int**|Determina si es posible crear suscripciones anónimas para la publicación indicada. **0** significa que no se permite una suscripción anónima.|  
|centralized_conflicts|**int**|Determina si los registros de los conflictos se almacenan en el publicador dado:<br /><br /> **0** = los registros de conflictos se almacenan tanto en el publicador como en el suscriptor que provocó el conflicto.<br /><br /> **1** = todos los registros de conflictos se almacenan en el publicador.|  
|priority|**Float (8)**|Prioridad de la suscripción en bucle invertido.|  
|snapshot_ready|**tinyint**|Indica si la instantánea de esta publicación está lista:<br /><br /> **0** = la instantánea está lista para su uso.<br /><br /> **1** = la instantánea no está lista para su uso.|  
|publication_type|**int**|Tipo de publicación:<br /><br /> **0** = instantánea.<br /><br /> **1** = transaccional.<br /><br /> **2** = fusionar mediante combinación.|  
|pubid|**uniqueidentifier**|Identificador único de esta publicación.|  
|snapshot_jobid|**binario (16)**|Identificador de trabajo del Agente de instantáneas. Para obtener la entrada del trabajo de instantáneas en la tabla del sistema [sysjobs](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md) , debe convertir este valor hexadecimal en **uniqueidentifier**.|  
|enabled_for_internet|**int**|Determina si la publicación está habilitada para Internet. Si es **1**, los archivos de sincronización de la publicación se colocan en el `C:\Program Files\Microsoft SQL Server\MSSQL\Repldata\Ftp` directorio. El usuario debe crear el directorio FTP (Protocolo de transferencia de archivos). Si es **0**, la publicación no está habilitada para el acceso a Internet.|  
|dynamic_filter|**int**|Indica si se debe usar el filtro de filas con parámetros. **0** significa que no se utiliza un filtro de fila con parámetros.|  
|has_subscription|**bit**|Indica si la publicación tiene alguna suscripción. **0** significa que actualmente no hay suscripciones a esta publicación.|  
|snapshot_in_default_folder|**bit**|Especifica si los archivos de instantáneas se almacenan en la carpeta predeterminada.<br /><br /> Si es **1**, los archivos de instantáneas se pueden encontrar en la carpeta predeterminada.<br /><br /> Si es **0**, los archivos de instantáneas se almacenan en la ubicación alternativa especificada por **alt_snapshot_folder**. Las ubicaciones alternativas pueden encontrarse en otro servidor, en una unidad de red o en medios extraíbles (como CD-ROM o discos extraíbles). También puede guardar los archivos de instantáneas en un sitio FTP para que el suscriptor los recupere más adelante.<br /><br /> Nota: este parámetro puede ser true y seguir teniendo una ubicación en el parámetro **alt_snapshot_folder** . Esta combinación especifica que los archivos de instantánea se almacenan tanto en la ubicación predeterminada como en la alternativa.|  
|alt_snapshot_folder|**nvarchar(255)**|Especifica la ubicación de la carpeta alternativa de la instantánea.|  
|pre_snapshot_script|**nvarchar(255)**|Especifica un puntero a un archivo **. SQL** que el agente de mezcla ejecuta antes que cualquiera de los scripts de objetos replicados al aplicar la instantánea en un suscriptor.|  
|post_snapshot_script|**nvarchar(255)**|Especifica un puntero a un archivo **. SQL** que el agente de mezcla ejecuta después de que se hayan aplicado todos los demás datos y scripts de objetos replicados durante una sincronización inicial.|  
|compress_snapshot|**bit**|Especifica que la instantánea que se escribe en la ubicación **alt_snapshot_folder** se comprime en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] formato CAB.|  
|ftp_address|**sysname**|Es la dirección de red del servicio FTP para el distribuidor. Especifica dónde se encuentran los archivos de instantánea de la publicación para el Agente de mezcla que se va a recoger.|  
|ftp_port|**int**|Es el número de puerto del servicio FTP para el distribuidor. **ftp_port** tiene un valor predeterminado de **21**. Especifica dónde se encuentran los archivos de instantánea de la publicación para que los recoja el Agente de mezcla.|  
|ftp_subdirectory|**nvarchar(255)**|Especifica dónde se encuentran los archivos de instantánea para que los recoja el Agente de mezcla cuando se envía la instantánea mediante FTP.|  
|ftp_login|**sysname**|Es el nombre de usuario utilizado para conectarse al servicio FTP.|  
|conflict_retention|**int**|Especifica el período de retención, expresado en días, durante el que se conservan los conflictos. Transcurrido el número de días especificado, se purga la fila del conflicto de la tabla de conflictos.|  
|keep_partition_changes|**int**|Especifica si se está optimizando la sincronización para esta publicación. **keep_partition_changes** tiene un valor predeterminado de **0**. Un valor de **0** significa que la sincronización no se optimiza y las particiones enviadas a todos los suscriptores se comprueban cuando los datos cambian en una partición.<br /><br /> **1** significa que la sincronización se optimiza y solo se ven afectados los suscriptores que tienen filas en la partición modificada.<br /><br /> Nota: de forma predeterminada, las publicaciones de combinación utilizan particiones precalculadas, lo que proporciona un mayor grado de optimización que esta opción. Para obtener más información, vea [filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md) y optimizar el [rendimiento de los filtros con parámetros con particiones precalculadas](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).|  
|allow_subscription_copy|**int**|Especifica si se ha habilitado la capacidad de copiar las bases de datos de suscripciones que se suscriben a esta publicación. Un valor de **0** significa que no se permite la copia.|  
|allow_synctoalternate|**int**|Especifica si se permite un asociado de sincronización alternativo para sincronizar con este publicador. Un valor de **0** significa que no se permite un asociado de sincronización.|  
|validate_subscriber_info|**nvarchar (500)**|Enumera las funciones que se están utilizando para recuperar información del suscriptor y validar los criterios de filtrado de filas con parámetros del suscriptor. Ayuda a comprobar que se hayan creado particiones de la información de manera coherente con cada combinación.|  
|backward_comp_level|**int**|Nivel de compatibilidad de la base de datos, que puede ser uno de los que se especifican a continuación:<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> SP1 de **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP2<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|publish_to_activedirectory|**bit**|Especifica si la información de publicación se publica en Active Directory. Un valor de **0** significa que la información de la publicación no está disponible en Active Directory.<br /><br /> Este parámetro ha quedado desusado y solo se admite para la compatibilidad de scripts con versiones anteriores. Ya no es posible agregar información de publicación a Active Directory.|  
|max_concurrent_merge|**int**|Número de procesos de combinación simultáneos. Si es **0**, no hay ningún límite en el número de procesos de mezcla simultáneos que se ejecutan en un momento dado.|  
|max_concurrent_dynamic_snapshots|**int**|Número máximo de sesiones de instantáneas de datos filtrados simultáneas que se pueden ejecutar con la publicación de combinación. Si es **0**, no hay ningún límite en el número máximo de sesiones de instantáneas de datos filtrados simultáneas que se pueden ejecutar simultáneamente en la publicación en un momento dado.|  
|use_partition_groups|**int**|Determina si se utilizan particiones precalculadas. Un valor de **1** significa que se utilizan las particiones precalculadas.|  
|num_of_articles|**int**|Número de artículos de la publicación.|  
|replicate_ddl|**int**|Indica si se replican los cambios de esquema realizados en tablas publicadas. Un valor de **1** significa que los cambios de esquema se replican.|  
|publication_number|**smallint**|Número asignado a esta publicación.|  
|allow_subscriber_initiated_snapshot|**bit**|Determina si los suscriptores pueden iniciar el proceso de generación de instantáneas de datos filtrados. Un valor de **1** significa que los suscriptores pueden iniciar el proceso de instantánea.|  
|allow_web_synchronization|**bit**|Determina si se habilita la publicación para sincronización web. Un valor de **1** significa que la sincronización web está habilitada.|  
|web_synchronization_url|**nvarchar (500)**|Dirección URL de Internet que se usa para la sincronización web.|  
|allow_partition_realignment|**bit**|Determina si las eliminaciones se envían al suscriptor cuando la modificación de la fila en el publicador hace que se cambie su partición. Un valor de **1** significa que las eliminaciones se envían al suscriptor.  Para obtener más información, vea [sp_addmergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).|  
|retention_period_unit|**tinyint**|Define la unidad que se utiliza al definir la retención. Puede ser uno de los siguientes valores:<br /><br /> **0** = día<br /><br /> **1** = semana<br /><br /> **2** = mes<br /><br /> **3** = año|  
|has_downloadonly_articles|**bit**|Indica si alguno de los artículos pertenecientes a la publicación son artículos de solo descarga. Un valor de **1** indica que hay artículos de solo descarga.|  
|decentralized_conflicts|**int**|Indica si los registros de los conflictos se almacenan en el suscriptor que provocó el conflicto. Un valor de **0** indica que los registros de conflictos no se almacenan en el suscriptor. El valor 1 indica que los registros de los conflictos se almacenan en el suscriptor.|  
|generation_leveling_threshold|**int**|Especifica el número de cambios contenidos en una generación. Una generación es un conjunto de cambios que se entregan a un publicador o a un suscriptor|  
|automatic_reinitialization_policy|**bit**|Indica si se cargan los cambios desde el suscriptor antes de que se produzca una reinicialización automática. Un valor de **1** indica que los cambios se cargan desde el suscriptor antes de que se produzca una reinicialización automática. El valor 0 indica que los cambios no se cargan antes de una reinicialización automática.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 sp_helpmergepublication se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Los miembros de la lista de acceso a la publicación pueden ejecutar sp_helpmergepublication para esa publicación. Los miembros del rol fijo de base de datos db_owner en la base de datos de publicaciones pueden ejecutar sp_helpmergepublication para obtener información de todas las publicaciones.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_helpmergepublication](../../relational-databases/replication/codesnippet/tsql/sp-helpmergepublication-_1.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar propiedades de publicación](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addmergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
