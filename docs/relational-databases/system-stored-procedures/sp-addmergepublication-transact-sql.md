---
description: sp_addmergepublication (Transact-SQL)
title: sp_addmergepublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepublication
- sp_addmergepublication_TSQL
helpviewer_keywords:
- sp_addmergepublication
ms.assetid: 28a629a1-7374-4614-9b04-279d290a942a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0c132ffc9315bafc28530afeef83f779c2975e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209442"
---
# <a name="sp_addmergepublication-transact-sql"></a>sp_addmergepublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una publicación de combinación nueva. Este procedimiento almacenado se ejecuta en el publicador de la base de datos que se publica.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addmergepublication [ @publication = ] 'publication'   
    [ , [ @description = ] 'description'   
    [ , [ @retention = ] retention ]   
    [ , [ @sync_mode = ] 'sync_mode' ]   
    [ , [ @allow_push = ] 'allow_push' ]   
    [ , [ @allow_pull = ] 'allow_pull' ]   
    [ , [ @allow_anonymous = ] 'allow_anonymous' ]   
    [ , [ @enabled_for_internet = ] 'enabled_for_internet' ]   
    [ , [ @centralized_conflicts = ] 'centralized_conflicts' ]   
    [ , [ @dynamic_filters = ] 'dynamic_filters' ]   
    [ , [ @snapshot_in_defaultfolder = ] 'snapshot_in_default_folder' ]   
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]   
    [ , [ @pre_snapshot_script = ] 'pre_snapshot_script' ]   
    [ , [ @post_snapshot_script = ] 'post_snapshot_script' ]   
    [ , [ @compress_snapshot = ] 'compress_snapshot' ]   
    [ , [ @ftp_address = ] 'ftp_address' ]   
    [ , [ @ftp_port = ] ftp_port ]   
    [ , [ @ftp_subdirectory = ] 'ftp_subdirectory' ]   
    [ , [ @ftp_login = ] 'ftp_login' ]   
    [ , [ @ftp_password = ] 'ftp_password' ]   
    [ , [ @conflict_retention = ] conflict_retention ]   
    [ , [ @keep_partition_changes = ] 'keep_partition_changes' ]   
    [ , [ @allow_subscription_copy = ] 'allow_subscription_copy' ]   
    [ , [ @allow_synctoalternate = ] 'allow_synctoalternate' ]   
    [ , [ @validate_subscriber_info = ] 'validate_subscriber_info' ]   
    [ , [ @add_to_active_directory = ] 'add_to_active_directory' ]   
    [ , [ @max_concurrent_merge = ] maximum_concurrent_merge ]   
    [ , [ @max_concurrent_dynamic_snapshots = ] max_concurrent_dynamic_snapshots ]  
    [ , [ @use_partition_groups = ] 'use_partition_groups' ]  
    [ , [ @publication_compatibility_level = ] 'backward_comp_level' ]  
    [ , [ @replicate_ddl = ] replicate_ddl ]  
    [ , [ @allow_subscriber_initiated_snapshot = ] 'allow_subscriber_initiated_snapshot' ]   
    [ , [ @allow_web_synchronization = ] 'allow_web_synchronization' ]   
    [ , [ @web_synchronization_url = ] 'web_synchronization_url' ]  
    [ , [ @allow_partition_realignment = ] 'allow_partition_realignment' ]  
    [ , [ @retention_period_unit = ] 'retention_period_unit' ]  
    [ , [ @generation_leveling_threshold = ] generation_leveling_threshold ]  
    [ , [ @automatic_reinitialization_policy = ] automatic_reinitialization_policy ]  
    [ , [ @conflict_logging = ] 'conflict_logging' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación de combinación que se va a crear. *Publication* es de **tipo sysname**, no tiene ningún valor predeterminado y no debe ser la palabra clave ALL. El nombre de la publicación debe ser único en la base de datos.  
  
`[ @description = ] 'description'` Es la descripción de la publicación. la *Descripción* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @retention = ] retention` Es el período de retención, en unidades de período de retención, para el que se van a guardar los cambios de la *publicación* indicada. la *retención* es de **tipo int** y su valor predeterminado es 14 unidades. Las unidades del período de retención se definen mediante *retention_period_unit*. Si la suscripción no está sincronizada en el período de retención y se han quitado, por medio de una operación de limpieza en el distribuidor, los cambios pendientes que podía haber recibido, la suscripción expira y es necesario reinicializarla. El período de retención máximo permitido es el número de días comprendido entre el 31 de diciembre de 9999 y la fecha actual.  
  
> [!NOTE]  
>  El período de retención para las publicaciones de combinación tiene un plazo de gracia de 24 horas para adaptarse a los suscriptores de las diferentes zonas horarias. Si, por ejemplo, se establece un período de retención de un día, el período de retención real será de 48 horas.  
  
`[ @sync_mode = ] 'sync_mode'` Es el modo de la sincronización inicial de los suscriptores a la publicación. *sync_mode* es **nvarchar (10)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**nativo** (valor predeterminado)|Genera la salida de todas las tablas mediante un programa de copia masiva en modo nativo.|  
|**carácter**|Genera la salida de todas las tablas mediante un programa de copia masiva en modo de caracteres. Necesario para admitir [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores de y que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
  
`[ @allow_push = ] 'allow_push'` Especifica si se pueden crear suscripciones de extracción para la publicación especificada. *allow_push* es de tipo **nvarchar (5)** y su valor predeterminado es true, que permite suscripciones de extracción en la publicación.  
  
`[ @allow_pull = ] 'allow_pull'` Especifica si se pueden crear suscripciones de extracción para la publicación especificada. *allow_pull* es de tipo **nvarchar (5)** y su valor predeterminado es true, que permite suscripciones de extracción en la publicación. Debe especificar True para admitir los [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores.  
  
`[ @allow_anonymous = ] 'allow_anonymous'` Especifica si se pueden crear suscripciones anónimas para la publicación especificada. *allow_anonymous* es de tipo **nvarchar (5)** y su valor predeterminado es true, que permite suscripciones anónimas en la publicación. Para admitir [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores, debe especificar **true**.  
  
`[ @enabled_for_internet = ] 'enabled_for_internet'` Especifica si la publicación está habilitada para Internet y determina si se puede usar el protocolo de transferencia de archivos (FTP) para transferir los archivos de instantáneas a un suscriptor. *enabled_for_internet* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si **es true**, los archivos de sincronización de la publicación se colocan en el directorio C:\Archivos de Programa\microsoft SQL server\mssql\mssql.x\repldata\ftp. El usuario debe crear el directorio Ftp. Si es **false**, la publicación no está habilitada para el acceso a Internet.  
  
`[ @centralized_conflicts = ] 'centralized_conflicts'` Este parámetro está en desuso y solo se admite para la compatibilidad con versiones anteriores de los scripts. Utilice *conflict_logging* para especificar la ubicación donde se almacenan los registros de conflictos.  
  
`[ @dynamic_filters = ] 'dynamic_filters'` Permite que la publicación de combinación utilice filtros de fila con parámetros. *dynamic_filters* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
> [!NOTE]  
>  No se recomienda especificar este parámetro; es mejor que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] determine de forma automática si se están utilizando filtros de fila con parámetros. Si especifica un valor de **true** para *dynamic_filters*, debe definir un filtro de fila con parámetros para el artículo. Para más información, consulte [Definir y modificar un filtro de fila con parámetros para un artículo de mezcla](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
`[ @snapshot_in_defaultfolder = ] 'snapshot_in_default_folder'` Especifica si los archivos de instantáneas se almacenan en la carpeta predeterminada. *snapshot_in_default_folder* es de tipo **nvarchar (5)** y su valor predeterminado es true. Si **es true**, los archivos de instantáneas se pueden encontrar en la carpeta predeterminada. Si **es false**, los archivos de instantáneas se almacenarán en la ubicación alternativa especificada por *alternate_snapshot_folder*. Las ubicaciones alternativas pueden encontrarse en otro servidor, en una unidad de red o en medios extraíbles (como CD-ROM o discos extraíbles). También puede guardar los archivos de instantáneas en un sitio FTP (Protocolo de transferencia de archivos), para que el suscriptor los recupere más tarde. Tenga en cuenta que este parámetro puede ser true y seguir teniendo una ubicación especificada por *alt_snapshot_folder*. Esta combinación especifica que los archivos de instantáneas se almacenarán tanto en la ubicación predeterminada como en la alternativa.  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'` Especifica la ubicación de la carpeta alternativa de la instantánea. *alternate_snapshot_folder* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @pre_snapshot_script = ] 'pre_snapshot_script'` Especifica un puntero a una ubicación de archivos **. SQL** . *pre_snapshot_script* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. El Agente de mezcla ejecutará el script previo a la instantánea antes que cualquiera de los scripts de objetos replicados al aplicar la instantánea en un suscriptor. El script se ejecuta en el contexto de seguridad utilizado por el Agente de mezcla durante la conexión con la base de datos de suscripciones. Los scripts anteriores a la instantánea no se ejecutan en los [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores.  
  
`[ @post_snapshot_script = ] 'post_snapshot_script'` Especifica un puntero a una ubicación de archivos **. SQL** . *post_snapshot_script* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. El Agente de mezcla ejecutará el script posterior a la instantánea después de que se apliquen los demás scripts y datos de objetos replicados durante una sincronización inicial. El script se ejecuta en el contexto de seguridad utilizado por el Agente de mezcla durante la conexión con la base de datos de suscripciones. Los scripts posteriores a la instantánea no se ejecutan en los [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores.  
  
`[ @compress_snapshot = ] 'compress_snapshot'`Especifica que la instantánea escrita en la ubicación **\@ alt_snapshot_folder** se va a comprimir en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] formato CAB. *compress_snapshot* es de tipo **nvarchar (5)** y su valor predeterminado es false. **false** especifica que la instantánea no se comprimirá; **true** especifica que se va a comprimir la instantánea. No se pueden comprimir archivos de instantáneas superiores a 2 GB. Los archivos de instantáneas comprimidos se descomprimen en la ubicación en la que se ejecuta el Agente de mezcla; normalmente, se utilizan suscripciones de extracción con las instantáneas comprimidas para descomprimir los archivos en el suscriptor. La instantánea de la carpeta predeterminada no se puede comprimir. Para admitir [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores, debe especificar **false**.  
  
`[ @ftp_address = ] 'ftp_address'` Es la dirección de red del servicio FTP para el distribuidor. *ftp_address* es de **tipo sysname y su** valor predeterminado es NULL. Especifica dónde se encuentran los archivos de instantánea de la publicación para que los recoja el Agente de mezcla de un suscriptor. Puesto que esta propiedad se almacena para cada publicación, cada publicación puede tener un *ftp_address* diferente. La publicación debe ser compatible con la propagación de instantáneas mediante FTP.  
  
`[ @ftp_port = ] ftp_port` Es el número de puerto del servicio FTP para el distribuidor. *ftp_port* es de **tipo int** y su valor predeterminado es 21. Especifica dónde se encuentran los archivos de instantánea de la publicación para que los recoja el Agente de mezcla de un suscriptor. Puesto que esta propiedad se almacena para cada publicación, cada publicación puede tener su propio *ftp_port*.  
  
`[ @ftp_subdirectory = ] 'ftp_subdirectory'` Especifica dónde estarán disponibles los archivos de instantáneas para que los recoja el Agente de mezcla del suscriptor si la publicación admite la propagación de instantáneas mediante FTP. *ftp_subdirectory* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. Puesto que esta propiedad se almacena para cada publicación, cada publicación puede tener su propio *ftp_subdirctory* o elegir no tener ningún subdirectorio, indicado con un valor null.  
  
 En la generación previa de instantáneas para publicaciones con filtros con parámetros, la instantánea de datos de cada partición del suscriptor debe estar en su propia carpeta. La estructura de directorios para las instantáneas generadas previamente mediante FTP debe ser la siguiente:  
  
 *alternate_snapshot_folder*\ftp \\ *publisher_publicationDB_publication* \\ *partitionID*.  
  
> [!NOTE]  
>  Los valores indicados en cursiva dependen de la configuración específica de la publicación y partición del suscriptor.  
  
`[ @ftp_login = ] 'ftp_login'` Es el nombre de usuario utilizado para conectarse al servicio FTP. *ftp_login* es de **tipo sysname y su** valor predeterminado es ' Anonymous '.  
  
`[ @ftp_password = ] 'ftp_password'` Es la contraseña de usuario que se utiliza para conectarse al servicio FTP. *FTP_PASSWORD* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  No utilice una contraseña en blanco. Utilice una contraseña segura.  
  
`[ @conflict_retention = ] conflict_retention` Especifica el período de retención, en días, durante el que se conservan los conflictos. *conflict_retention* es de **tipo int** y su valor predeterminado es 14 días antes de que se purgue la fila de conflictos de la tabla de conflictos.  
  
`[ @keep_partition_changes = ] 'keep_partition_changes'` Especifica si se van a habilitar las optimizaciones de cambio de particiones cuando no se pueden usar particiones precalculadas. *keep_partition_changes* es de tipo **nvarchar (5)** y su valor predeterminado es true. **false** significa que los cambios en las particiones no están optimizados y, cuando no se utilizan las particiones precalculadas, se comprueban las particiones enviadas a todos los suscriptores cuando los datos cambian en una partición. **true** significa que los cambios en las particiones están optimizados y solo se ven afectados los suscriptores que tienen filas en las particiones modificadas. Al usar particiones precalculadas, establezca *use_partition_groups* en **true** y establezca *keep_partition_changes* en **false**. Para obtener más información, vea [Optimización del rendimiento de los filtros con parámetros con particiones calculadas previamente](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
> [!NOTE]  
>  Si especifica un valor de **true** para *keep_partition_changes*, especifique un valor de **1** para el parámetro agente de instantáneas **-MaxNetworkOptimization**. Para obtener más información acerca de este parámetro, vea [replication agente de instantáneas](../../relational-databases/replication/agents/replication-snapshot-agent.md). Para obtener información acerca de cómo especificar parámetros de agente, consulte [Administración del agente de replicación](../../relational-databases/replication/agents/replication-agent-administration.md).  
  
 Con los [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores, *keep_partition_changes* debe establecerse en true para asegurarse de que las eliminaciones se propagan correctamente. Si se establece en false, el suscriptor puede tener más filas de las esperadas.  
  
`[ @allow_subscription_copy = ] 'allow_subscription_copy'` Habilita o deshabilita la capacidad de copiar las bases de datos de suscripciones que se suscriben a esta publicación. *allow_subscription_copy* es de tipo **nvarchar (5)** y su valor predeterminado es false. El tamaño de la base de datos de suscripciones que se va a copiar debe ser inferior a 2 gigabytes (GB).  
  
`[ @allow_synctoalternate = ] 'allow_synctoalternate'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @validate_subscriber_info = ] 'validate_subscriber_info'` Enumera las funciones que se usan para definir una partición de suscriptor de los datos publicados cuando se usan filtros de fila con parámetros. *validate_subscriber_info* es de tipo **nvarchar (500)** y su valor predeterminado es NULL. El Agente de mezcla utiliza esta información para validar la partición del suscriptor. Por ejemplo, si se utiliza [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) en el filtro de fila con parámetros, el parámetro debe ser `@validate_subscriber_info=N'SUSER_SNAME()'` .  
  
> [!NOTE]  
>  No se recomienda especificar este parámetro; es mejor que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] determine de forma automática el criterio de filtrado.  
  
`[ @add_to_active_directory = ] 'add_to_active_directory'` Este parámetro está en desuso y solo se admite para la compatibilidad con versiones anteriores de los scripts. Ya no se puede agregar información de publicación a [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory.  
  
`[ @max_concurrent_merge = ] maximum_concurrent_merge` Número máximo de procesos de mezcla simultáneos. *maximum_concurrent_merge* es de **tipo int** y su valor predeterminado es 0. Un valor de **0** para esta propiedad significa que no hay ningún límite en el número de procesos de mezcla simultáneos que se ejecutan en un momento dado. Esta propiedad establece un límite para el número de procesos de mezcla simultáneos que se pueden ejecutar con una publicación de combinación en un momento determinado. Si hay más procesos de mezcla programados para ejecutarse simultáneamente de los que permite el valor de esta propiedad, los trabajos restantes se colocan en una cola hasta que finalice el proceso de mezcla que se está ejecutando en ese momento.  
  
`[ @max_concurrent_dynamic_snapshots = ] max_concurrent_dynamic_snapshots` El número máximo de sesiones Agente de instantáneas que se pueden ejecutar simultáneamente para generar instantáneas de datos filtrados para las particiones del suscriptor. *maximum_concurrent_dynamic_snapshots* es de **tipo int** y su valor predeterminado es 0. Si es **0**, no hay ningún límite en el número de sesiones de instantáneas. Si hay más procesos de instantánea programados al mismo tiempo que los que permite ejecutar el valor, los trabajos sobrantes se colocarán en una cola y esperarán hasta que finalice el proceso de instantánea que se está ejecutando actualmente.  
  
`[ @use_partition_groups = ] 'use_partition_groups'` Especifica que se deben usar particiones precalculadas para optimizar el proceso de sincronización. *use_partition_groups* es de tipo **nvarchar (5)** y puede tener uno de estos valores:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**true**|La publicación utiliza particiones previamente calculadas.|  
|**false**|La publicación no utiliza particiones previamente calculadas.|  
|NULL (valor predeterminado)|El sistema decide la estrategia de partición.|  
  
 Las particiones precalculadas se utilizan de manera predeterminada. Para evitar el uso de particiones precalculadas, *use_partition_groups* debe establecerse en **false**. Si es NULL, el sistema decidirá si se pueden utilizar. Si no se pueden usar particiones precalculadas, este valor se convierte en **false** sin generar ningún error. En tales casos, *keep_partition_changes* se puede establecer en **true** para proporcionar una optimización. Para obtener más información, vea [filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md) y optimizar el [rendimiento de los filtros con parámetros con particiones precalculadas](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
`[ @publication_compatibility_level = ] backward_comp_level` Indica la compatibilidad con versiones anteriores de la publicación. *backward_comp_level* es **nvarchar (6)** y puede tener uno de estos valores:  
  
|Value|Versión|  
|-----------|-------------|  
|**90RTM**|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|  
|**100RTM**|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
  
`[ @replicate_ddl = ] replicate_ddl` Indica si se admite la replicación de esquemas para la publicación. *replicate_ddl* es de **tipo int** y su valor predeterminado es 1. **1** indica que las instrucciones del lenguaje de definición de datos (DDL) ejecutadas en el publicador se replican y **0** indica que las instrucciones de DDL no se replican. Para más información, vea [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
 El parámetro *\@ replicate_ddl* se respeta cuando una instrucción DDL agrega una columna. El parámetro *\@ replicate_ddl* se omite cuando una instrucción DDL modifica o quita una columna por los siguientes motivos.  
  
-   Cuando se quita una columna, sysarticlecolumns debe actualizarse para evitar que las nuevas instrucciones DML incluyan la columna que se quitó, lo que haría que el agente de distribución produjera un error. Se omite el parámetro *\@ replicate_ddl* porque la replicación siempre debe replicar el cambio de esquema.  
  
-   Cuando se modifica una columna, es posible que el tipo de datos de origen o la nulabilidad hayan cambiado, lo que hace que las instrucciones DML contengan un valor que quizás no sea compatible con la tabla en el suscriptor. Estas instrucciones DML pueden hacer que el agente de distribución genere un error. Se omite el parámetro *\@ replicate_ddl* porque la replicación siempre debe replicar el cambio de esquema.  
  
-   Cuando una instrucción DDL agrega una columna nueva, sysarticlecolumns no incluye esta columna nueva. Las instrucciones DML no intentarán replicar datos para la nueva columna. Se respeta el parámetro porque la DDL es aceptable se realice o no la replicación.  
  
`[ @allow_subscriber_initiated_snapshot = ] 'allow_subscriber_initiated_snapshot'` Indica si los suscriptores de esta publicación pueden iniciar el proceso de instantánea para generar la instantánea filtrada para su partición de datos. *allow_subscriber_initiated_snapshot* es de tipo **nvarchar (5)** y su valor predeterminado es false. **true** indica que los suscriptores pueden iniciar el proceso de instantánea.  
  
`[ @allow_web_synchronization = ] 'allow_web_synchronization'` Especifica si la publicación está habilitada para la sincronización Web. *allow_web_synchronization* es de tipo **nvarchar (5)** y su valor predeterminado es false. **true** especifica que las suscripciones a esta publicación se pueden sincronizar a través de HTTPS. Para más información, consulte [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md). Para admitir [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores, debe especificar **true**.  
  
`[ @web_synchronization_url = ] 'web_synchronization_url'` Especifica el valor predeterminado de la dirección URL de Internet que se usa para la sincronización Web. *web_synchronization_url* es de tipo **nvarchar (500)** y su valor predeterminado es NULL. Define la dirección URL de Internet predeterminada si no se establece una explícitamente cuando se ejecuta [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) .  
  
`[ @allow_partition_realignment = ] 'allow_partition_realignment'` Determina si las eliminaciones se envían al suscriptor cuando la modificación de la fila en el publicador hace que cambie su partición. *allow_partition_realignment* es de tipo **nvarchar (5)** y su valor predeterminado es true. **true** envía las eliminaciones al suscriptor para reflejar los resultados de un cambio en la partición mediante la eliminación de los datos que ya no forman parte de la partición del suscriptor. **false** deja los datos de una partición antigua en el suscriptor, donde los cambios realizados en estos datos en el publicador no se replicarán en este suscriptor, pero los cambios realizados en el suscriptor se replicarán en el publicador. La configuración de *allow_partition_realignment* en **false** se usa para conservar los datos en una suscripción de una partición antigua cuando es necesario tener acceso a los datos con fines históricos.  
  
> [!NOTE]  
>  Los datos que permanecen en el suscriptor como resultado de la configuración de *allow_partition_realignment* en **false** deben tratarse como si fueran de solo lectura; sin embargo, el sistema de replicación no aplica esto.  
  
`[ @retention_period_unit = ] 'retention_period_unit'` Especifica las unidades para el período de retención establecido por *retención*. *retention_period_unit* es **nvarchar (10)** y puede tener uno de los valores siguientes.  
  
|Value|Versión|  
|-----------|-------------|  
|**Day** (valor predeterminado)|El período de retención se especifica en días.|  
|**week**|El período de retención se especifica en semanas.|  
|**month**|El período de retención se especifica en meses.|  
|**year**|El período de retención se especifica en años.|  
  
`[ @generation_leveling_threshold = ] generation_leveling_threshold` Especifica el número de cambios contenidos en una generación. Una generación es una colección de cambios que se entregan a un publicador o a un suscriptor. *generation_leveling_threshold* es de **tipo int** y su valor predeterminado es 1000.  
  
`[ @automatic_reinitialization_policy = ] automatic_reinitialization_policy`Especifica si los cambios se cargan desde el suscriptor antes de una reinicialización automática requerida por un cambio en la publicación, donde se especificó un valor de **1** para **\@ force_reinit_subscription**. *automatic_reinitialization_policy* es de bit y su valor predeterminado es 0. **1** significa que los cambios se cargan desde el suscriptor antes de que se produzca una reinicialización automática.  
  
> [!IMPORTANT]  
>  Si se agrega, quita o modifica un filtro con parámetros, los cambios pendientes en el suscriptor no se pueden cargar en el publicador durante la reinicialización. Si desea cargar los cambios pendientes, sincronice todas las suscripciones antes de cambiar el filtro.  
  
`[ @conflict_logging = ] 'conflict_logging'` Especifica dónde se almacenan los registros de conflictos. *conflict_logging* es **nvarchar (15)** y puede tener uno de los valores siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**publisher**|Los registros de conflictos se almacenan en el publicador.|  
|**suscriptor**|Los registros de conflictos se almacenan en el suscriptor que causó el conflicto. No se admite para los [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores de.|  
|**ambos**|Los registros de conflictos se almacenan tanto en el publicador como en el suscriptor.|  
|NULL (predeterminado)|La replicación establece  automáticamente conflict_logging **en cuando el** valor *backward_comp_level* es **90rtm** y en el **publicador** en todos los demás casos.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addmergepublication** se utiliza en la replicación de mezcla.  
  
 Para enumerar los objetos de publicación en el Active Directory mediante el parámetro **\@ add_to_active_directory** , el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objeto ya debe estar creado en el Active Directory.  
  
 Si existen varias publicaciones que publican el mismo objeto de base de datos, solo las publicaciones con un valor *replicate_ddl* de **1** REPLICARÁN las instrucciones ALTER TABLE, Alter View, ALTER PROCEDURE, ALTER FUNCTION y Alter Trigger DDL. Sin embargo, todas las publicaciones que publiquen la columna quitada replicarán una instrucción ALTER TABLE DROP COLUMN de DDL.  
  
 En el caso de los [!INCLUDE[ssEW](../../includes/ssew-md.md)] suscriptores, el valor de *alternate_snapshot_folder* solo se usa cuando el valor de *snapshot_in_default_folder* es **false**.  
  
 Con la replicación DDL habilitada (_replicate_ddl_**= 1**) para una publicación, para realizar cambios de DDL que no se replican en la publicación, se debe ejecutar primero [sp_changemergepublication &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) para establecer *replicate_ddl* en **0**. Una vez emitidas las instrucciones DDL que no son de replicación, **sp_changemergepublication** se pueden ejecutar de nuevo para volver a activar la replicación DDL.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_AddMergePub](../../relational-databases/replication/codesnippet/tsql/sp-addmergepublication-t_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addmergepublication**.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
