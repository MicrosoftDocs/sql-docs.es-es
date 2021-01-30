---
description: sp_addmergesubscription (Transact-SQL)
title: sp_addmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergesubscription_TSQL
- sp_addmergesubscription
helpviewer_keywords:
- sp_addmergesubscription
ms.assetid: a191d817-0132-49ff-93ca-76f13e609b38
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d6689fed03126a8e5bc65d8d9e7e556a070ec483
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209387"
---
# <a name="sp_addmergesubscription-transact-sql"></a>sp_addmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una suscripción de mezcla de inserción o extracción. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addmergesubscription [ @publication= ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db= ] 'subscriber_db' ]  
    [ , [ @subscription_type= ] 'subscription_type' ]  
    [ , [ @subscriber_type= ] 'subscriber_type' ]  
    [ , [ @subscription_priority= ] subscription_priority ]  
    [ , [ @sync_type= ] 'sync_type' ]  
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @optional_command_line= ] 'optional_command_line' ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @enabled_for_syncmgr= ] 'enabled_for_syncmgr' ]  
    [ , [ @offloadagent= ] remote_agent_activation]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @use_interactive_resolver= ] 'use_interactive_resolver' ]  
    [ , [ @merge_job_name= ] 'merge_job_name' ]  
    [ , [ @hostname = ] 'hostname'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado. La publicación debe existir.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscription_type = ] 'subscription_type'` Es el tipo de suscripción. *subscription_type* es de tipo **nvarchar (15)** y su valor predeterminado es inserciones. Si se **envía**, se agrega una suscripción de extracción y se agrega el agente de mezcla al distribuidor. Si **extrae**, se agrega una suscripción de extracción sin agregar un agente de mezcla en el distribuidor.  
  
> [!NOTE]  
>  Las suscripciones anónimas no necesitan utilizar este procedimiento almacenado.  
  
`[ @subscriber_type = ] 'subscriber_type'` Es el tipo de suscriptor. *subscriber_type* es **nvarchar (15)** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**local** (valor predeterminado)|Suscriptor solo conocido para el publicador.|  
|**global**|Suscriptor conocido para todos los servidores.|  
  
 En [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores, se hace referencia a las suscripciones locales como suscripciones de cliente y a las suscripciones globales como suscripciones de servidor.  
  
`[ @subscription_priority = ] subscription_priority` Es un número que indica la prioridad de la suscripción. *subscription_priority* es **real** y su valor predeterminado es NULL. En las suscripciones locales y anónimas, la prioridad es 0.0. En el caso de las suscripciones globales, la prioridad debe ser inferior a 100.0.  
  
`[ @sync_type = ] 'sync_type'` Es el tipo de sincronización de la suscripción. *sync_type* es de tipo **nvarchar (15)** y su valor predeterminado es **Automatic**. Puede ser **automático** o **ninguno**. Si es **automático**, el esquema y los datos iniciales de las tablas publicadas se transfieren primero al suscriptor. Si no hay **ninguno**, se supone que el suscriptor ya tiene el esquema y los datos iniciales de las tablas publicadas. Las tablas y los datos del sistema se transfieren siempre.  
  
> [!NOTE]  
>  Se recomienda no especificar un valor de **None**.  
  
`[ @frequency_type = ] frequency_type` Es un valor que indica cuándo se ejecutará el Agente de mezcla. *frequency_type* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**4**|Diario|  
|**8**|Cada semana|  
|**10**|Mensual|  
|**20**|Mensualmente, dependiendo del intervalo de frecuencia|  
|**40**|Cuando se inicia el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|NULL (predeterminado)||  
  
`[ @frequency_interval = ] frequency_interval` El día o los días en los que se ejecuta el Agente de mezcla. *frequency_interval* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Domingo|  
|**2**|Lunes|  
|**3**|Martes|  
|**4**|Miércoles|  
|**5**|Jueves|  
|**6**|Viernes|  
|**7**|Sábado|  
|**8**|Día|  
|**9**|Días de la semana|  
|**10**|Días del fin de semana|  
|NULL (predeterminado)||  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la repetición de combinación programada del intervalo de frecuencia de cada mes. *frequency_relative_interval* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|First|  
|**2**|Segundo|  
|**4**|Tercero|  
|**8**|Cuarto|  
|**16**|Último|  
|NULL (predeterminado)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @frequency_subday = ] frequency_subday` Es la unidad de *frequency_subday_interval*. *frequency_subday* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**2**|Segundo|  
|**4**|Minute|  
|**8**|Hora|  
|NULL (predeterminado)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es la frecuencia con la que se producen *frequency_subday* entre cada combinación. *frequency_subday_interval* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día a la que se programa el Agente de mezcla por primera vez, con el formato HHMMSS. *active_start_time_of_day* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día en que el Agente de mezcla deja de estar programado, con el formato HHMMSS. *active_end_time_of_day* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en la que se programa el Agente de mezcla por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que el Agente de mezcla deja de estar programado, con el formato AAAAMMDD. *active_end_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @optional_command_line = ] 'optional_command_line'` Es el símbolo del sistema opcional que se va a ejecutar. *optional_command_line* es de tipo **nvarchar (4000)** y su valor predeterminado es NULL. Este parámetro se utiliza para agregar un comando que captura el resultado y lo guarda en un archivo o para especificar un atributo o archivo de configuración.  
  
`[ @description = ] 'description'` Es una breve descripción de esta suscripción de mezcla. la *Descripción* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. El monitor de replicación muestra este valor en la columna **nombre descriptivo** , que se puede utilizar para ordenar las suscripciones de una publicación supervisada.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Especifica si la suscripción se puede sincronizar mediante el [!INCLUDE[msCoName](../../includes/msconame-md.md)] Administrador de sincronización de Windows. *enabled_for_syncmgr* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si es **false**, la suscripción no está registrada en el administrador de sincronización. Si es **true**, la suscripción se registra con el administrador de sincronización y se puede sincronizar sin iniciar [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @offloadagent = ] remote_agent_activation` Especifica que el agente puede activarse de forma remota. *remote_agent_activation* es de **bit** y su valor predeterminado es **0**.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solamente se mantiene por compatibilidad con versiones anteriores de scripts.  
  
`[ @offloadserver = ] 'remote_agent_server_name'` Especifica el nombre de red del servidor que se va a utilizar para la activación remota del agente. *remote_agent_server_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @use_interactive_resolver = ] 'use_interactive_resolver'` Permite resolver conflictos de forma interactiva para todos los artículos que permiten la resolución interactiva. *use_interactive_resolver* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
`[ @merge_job_name = ] 'merge_job_name'`El parámetro *\@ merge_job_name* está desusado y no se puede establecer. *merge_job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @hostname = ] 'hostname'` Invalida el valor devuelto por [host_name](../../t-sql/functions/host-name-transact-sql.md) cuando esta función se utiliza en la cláusula WHERE de un filtro con parámetros. *Hostname* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  Por motivos de rendimiento, se recomienda no aplicar funciones a los nombres de columna en las cláusulas de filtro de fila con parámetros, como `LEFT([MyColumn]) = SUSER_SNAME()`. Si utiliza [host_name](../../t-sql/functions/host-name-transact-sql.md) en una cláusula de filtro y reemplaza el valor host_name, podría ser necesario convertir los tipos de datos mediante [Convert](../../t-sql/functions/cast-and-convert-transact-sql.md). Para obtener más información acerca de las prácticas recomendadas para este caso, vea la sección "Reemplazar el valor de HOST_NAME()" del tema [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addmergesubscription** se utiliza en la replicación de mezcla.  
  
 Cuando un miembro del rol fijo de servidor **sysadmin** ejecuta **sp_addmergesubscription** para crear una suscripción de extracción, el trabajo de agente de mezcla se crea implícitamente y se ejecuta en la cuenta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio del agente. Se recomienda ejecutar [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md) y especificar las credenciales de una cuenta de Windows diferente específica del agente para **\@ job_login** y **\@ job_password**. Para más información, consulte [Modelo de seguridad del agente de replicación](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergesubscription-_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addmergesubscription**.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Resolución interactiva de conflictos](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_changemergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)  
  
  
