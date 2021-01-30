---
description: sp_addmergepushsubscription_agent (Transact-SQL)
title: sp_addmergepushsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepushsubscription_agent_TSQL
- sp_addmergepushsubscription_agent
helpviewer_keywords:
- sp_addmergepushsubscription_agent
ms.assetid: 808a1925-be46-4999-8d69-b3a83010ec81
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dce3538b3f6160ccf08502845b960a00165d5b2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209397"
---
# <a name="sp_addmergepushsubscription_agent-transact-sql"></a>sp_addmergepushsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Agrega un nuevo trabajo de agente que se utiliza para programar la sincronización de una suscripción de inserción con una publicación de combinación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
> [!IMPORTANT]  
>  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addmergepushsubscription_agent [ @publication =] 'publication'   
    [ , [ @subscriber = ] 'subscriber' ]   
    [ , [ @subscriber_db = ] 'subscriber_db' ]   
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]   
    [ , [ @subscriber_login = ] 'subscriber_login' ]   
    [ , [ @subscriber_password = ] 'subscriber_password' ]   
    [ , [ @publisher_security_mode = ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @job_name = ] 'job_name' ]   
    [ , [ @frequency_type = ] frequency_type ]   
    [ , [ @frequency_interval = ] frequency_interval ]   
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]   
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]   
    [ , [ @frequency_subday = ] frequency_subday ]   
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]   
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]   
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]   
    [ , [ @active_start_date = ] active_start_date ]   
    [ , [ @active_end_date = ] active_end_date ]   
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Es el modo de seguridad que se va a utilizar al conectarse a un suscriptor durante la sincronización. *subscriber_security_mode* es de **tipo int** y su valor predeterminado es 1. Si es **0**, especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. Si es **1**, especifica la autenticación de Windows.  
  
`[ @subscriber_login = ] 'subscriber_login'` Es el inicio de sesión del suscriptor que se va a utilizar al conectarse a un suscriptor durante la sincronización. *subscriber_login* es necesario si *subscriber_security_mode* está establecido en **0**. *subscriber_login* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscriber_password = ] 'subscriber_password'` Es la contraseña del suscriptor para la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. *subscriber_password* es necesario si *subscriber_security_mode* está establecido en **0**. *subscriber_password* es de **tipo sysname y su** valor predeterminado es NULL. Si se utiliza una contraseña de suscriptor, se cifra automáticamente.  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Es el modo de seguridad que se va a utilizar al conectarse a un publicador durante la sincronización. *publisher_security_mode* es de **tipo int** y su valor predeterminado es 1. Si es **0**, especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. Si es **1**, especifica la autenticación de Windows.  
  
`[ @publisher_login = ] 'publisher_login'` Es el inicio de sesión que se va a utilizar al conectarse a un publicador durante la sincronización. *publisher_login* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @publisher_password = ] 'publisher_password'` Es la contraseña que se usa al conectarse al publicador. *publisher_password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @job_login = ] 'job_login'` Es el inicio de sesión de la cuenta de Windows con la que se ejecuta el agente. *job_login* es de tipo **nvarchar (257)** y su valor predeterminado es NULL. Esta cuenta de Windows se utiliza siempre para las conexiones de agentes al distribuidor y para las conexiones al suscriptor y al publicador, cuando se utiliza la autenticación de Windows integrada.  
  
`[ @job_password = ] 'job_password'` Es la contraseña de la cuenta de Windows con la que se ejecuta el agente. *job_password* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @job_name = ] 'job_name'` Es el nombre de un trabajo del agente existente. *job_name* es de **tipo sysname y su** valor predeterminado es NULL. Este parámetro solo se especifica cuando se sincroniza la suscripción con un trabajo existente en lugar de utilizar un trabajo recién creado (la opción predeterminada). Si no es miembro del rol fijo de servidor **sysadmin** , debe especificar *job_login* y *job_password* al especificar *job_name*.  
  
`[ @frequency_type = ] frequency_type` Es la frecuencia con la que se programa el Agente de mezcla. *frequency_type* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una vez|  
|**2**|A petición|  
|**4**|Diario|  
|**8**|Cada semana|  
|**16**|Mensual|  
|**32**|Mensualmente relativa|  
|**64**|Iniciar automáticamente|  
|**128**|Periódica|  
|NULL (predeterminado)||  
  
> [!NOTE]  
>  Si se especifica un valor de **64** , el agente de mezcla se ejecutará en modo continuo. Esto corresponde a la configuración del parámetro **-Continuous** para el agente. Para más información, consulte [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md).  
  
`[ @frequency_interval = ] frequency_interval` Los días en los que se ejecuta el Agente de mezcla. *frequency_interval* es de **tipo int** y puede tener uno de los valores siguientes.  
  
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
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la fecha del Agente de mezcla. Este parámetro se utiliza cuando *frequency_type* se establece en **32** (relativo mensual). *frequency_relative_interval* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|First|  
|**2**|Segundo|  
|**4**|Tercero|  
|**8**|Cuarto|  
|**16**|Último|  
|NULL (predeterminado)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @frequency_subday = ] frequency_subday` Es la frecuencia con que se vuelve a programar durante el período definido. *frequency_subday* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**2**|Segundo|  
|**4**|Minute|  
|**8**|Hora|  
|NULL (predeterminado)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es el intervalo de *frequency_subday*. *frequency_subday_interval* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día a la que se programa el Agente de mezcla por primera vez, con el formato HHMMSS. *active_start_time_of_day* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día en que el Agente de mezcla deja de estar programado, con el formato HHMMSS. *active_end_time_of_day* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en la que se programa el Agente de mezcla por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que el Agente de mezcla deja de estar programado, con el formato AAAAMMDD. *active_end_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Especifica si la suscripción se puede sincronizar mediante el administrador de sincronización de Windows. *enabled_for_syncmgr* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si es **false**, la suscripción no está registrada en el administrador de sincronización. Si es **true**, la suscripción se registra con el administrador de sincronización y se puede sincronizar sin iniciar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addmergepushsubscription_agent** se utiliza en la replicación de mezcla y utiliza una funcionalidad similar a la de [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md).  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergepushsubscript_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addmergepushsubscription_agent**.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addmergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_changemergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)  
  
  
