---
description: sp_addmergepullsubscription_agent (Transact-SQL)
title: sp_addmergepullsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepullsubscription_agent
- sp_addmergepullsubscription_agent_TSQL
helpviewer_keywords:
- sp_addmergepullsubscription_agent
ms.assetid: a2f4b086-078d-49b5-8971-8a1e3f6a6feb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ecc9b2fded678671bc608235fa3080f2ed31573
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209430"
---
# <a name="sp_addmergepullsubscription_agent-transact-sql"></a>sp_addmergepullsubscription_agent (Transact-SQL)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Agrega un nuevo trabajo de agente utilizado para programar la sincronización de una suscripción de extracción a una publicación de combinación. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addmergepullsubscription_agent [ [ @name = ] 'name' ]   
        , [ @publisher = ] 'publisher'   
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication =] 'publication'   
    [ , [ @publisher_security_mod e= ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @publisher_encrypted_password = ] publisher_encrypted_password ]   
    [ , [ @subscriber = ] 'subscriber' ]   
    [ , [ @subscriber_db = ] 'subscriber_db' ]   
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]   
    [ , [ @subscriber_login = ] 'subscriber_login' ]   
    [ , [ @subscriber_password= ] 'subscriber_password' ]   
    [ , [ @distributor = ] 'distributor' ]   
    [ , [ @distributor_security_mode = ] distributor_security_mode ]   
    [ , [ @distributor_login = ] 'distributor_login' ]   
    [ , [ @distributor_password = ] 'distributor_password' ]   
    [ , [ @encrypted_password = ] encrypted_password ]   
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
    [ , [ @optional_command_line = ] 'optional_command_line' ]   
    [ , [ @merge_jobid = ] merge_jobid ]   
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]   
    [ , [ @ftp_address = ] 'ftp_address' ]   
    [ , [ @ftp_port = ] ftp_port ]   
    [ , [ @ftp_login = ] 'ftp_login' ]   
    [ , [ @ftp_password = ] 'ftp_password' ]    
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]   
    [ , [ @working_directory = ] 'working_directory' ]   
    [ , [ @use_ftp = ] 'use_ftp' ]   
    [ , [ @reserved = ] 'reserved' ]   
    [ , [ @use_interactive_resolver = ] 'use_interactive_resolver' ]   
    [ , [ @offloadagent = ] 'remote_agent_activation' ]   
    [ , [ @offloadserver = ] 'remote_agent_server_name']   
    [ , [ @job_name = ] 'job_name' ]   
    [ , [ @dynamic_snapshot_location = ] 'dynamic_snapshot_location' ]  
    [ , [ @use_web_sync = ] use_web_sync ]  
        [ , [ @internet_url = ] 'internet_url' ]  
    [ , [ @internet_login = ] 'internet_login' ]  
        [ , [ @internet_password = ] 'internet_password' ]  
    [ , [ @internet_security_mode = ] internet_security_mode ]  
        [ , [ @internet_timeout = ] internet_timeout ]  
    [ , [ @hostname = ] 'hostname' ]  
        [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] 'name'` Es el nombre del agente. *Name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @publisher = ] 'publisher'` Es el nombre del servidor del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Es el modo de seguridad que se va a utilizar al conectarse a un publicador durante la sincronización. *publisher_security_mode* es de **tipo int** y su valor predeterminado es 1. Si es **0**, especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. Si es **1**, especifica la autenticación de Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Es el inicio de sesión que se va a utilizar al conectarse a un publicador durante la sincronización. *publisher_login* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @publisher_password = ] 'publisher_password'` Es la contraseña que se usa al conectarse al publicador. *publisher_password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Cuando sea posible, pida a los usuarios que especifiquen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @publisher_encrypted_password = ]publisher_encrypted_password` Ya no se admite la configuración de *publisher_encrypted_password* . Si se intenta establecer este parámetro de **bits** en **1** se producirá un error.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Es el modo de seguridad que se va a utilizar al conectarse a un suscriptor durante la sincronización. *subscriber_security_mode* es de **tipo int** y su valor predeterminado es 1. Si es **0**, especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. Si es **1**, especifica la autenticación de Windows.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. El Agente de mezcla siempre conecta con el suscriptor local mediante Autenticación de Windows. Si se especifica un valor para este parámetro, se devolverá un mensaje de advertencia, pero el valor se pasará por alto.  
  
`[ @subscriber_login = ] 'subscriber_login'` Es el inicio de sesión del suscriptor que se va a utilizar al conectarse a un suscriptor durante la sincronización. *subscriber_login* es necesario si *subscriber_security_mode* está establecido en **0**. *subscriber_login* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. Si se especifica un valor para este parámetro, se devolverá un mensaje de advertencia, pero el valor se pasará por alto.  
  
`[ @subscriber_password = ] 'subscriber_password'` Es la contraseña del suscriptor para la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. *subscriber_password* es necesario si *subscriber_security_mode* está establecido en **0**. *subscriber_password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. Si se especifica un valor para este parámetro, se devolverá un mensaje de advertencia, pero el valor se pasará por alto.  
  
`[ @distributor = ] 'distributor'` Es el nombre del distribuidor. *Distributor* es de **tipo sysname y su** valor predeterminado es *Publisher*; es decir, el publicador también es el distribuidor.  
  
`[ @distributor_security_mode = ] distributor_security_mode` Es el modo de seguridad que se va a utilizar al conectarse a un distribuidor durante la sincronización. *distributor_security_mode* es de **tipo int** y su valor predeterminado es 0. **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. **1** especifica la autenticación de Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` Es el inicio de sesión del distribuidor que se va a utilizar al conectarse a un distribuidor durante la sincronización. *distributor_login* es necesario si *distributor_security_mode* está establecido en **0**. *distributor_login* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @distributor_password = ] 'distributor_password'` Es la contraseña del distribuidor. *distributor_password* es necesario si *distributor_security_mode* está establecido en **0**. *distributor_password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Cuando sea posible, pida a los usuarios que especifiquen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @encrypted_password = ] encrypted_password` Ya no se admite la configuración de *encrypted_password* . Si se intenta establecer este parámetro de **bits** en **1** se producirá un error.  
  
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
  
`[ @frequency_interval = ] frequency_interval` El día o los días en los que se ejecuta el Agente de mezcla. *frequency_interval* es de **tipo int** y puede tener uno de estos valores.  
  
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
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la fecha del Agente de mezcla. Este parámetro se utiliza cuando *frequency_type* se establece en **32** (relativo mensual). *frequency_relative_interval* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|First|  
|**2**|Segundo|  
|**4**|Tercero|  
|**8**|Cuarto|  
|**16**|Último|  
|NULL (predeterminado)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @frequency_subday = ] frequency_subday` Es la frecuencia con que se vuelve a programar durante el período definido. *frequency_subday* es de **tipo int** y puede tener uno de estos valores.  
  
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
  
`[ @optional_command_line = ] 'optional_command_line'` Es un símbolo del sistema opcional que se proporciona al Agente de mezcla. *optional_command_line* es de tipo **nvarchar (255)** y su valor predeterminado es ' '. Se puede utilizar para proporcionar parámetros adicionales al Agente de mezcla, como en el siguiente ejemplo, en el que se aumenta el tiempo de espera predeterminado de las consultas a `600` segundos.  
  
```  
@optional_command_line = N'-QueryTimeOut 600'  
```  
  
`[ @merge_jobid = ] merge_jobid` Es el parámetro de salida del ID. de trabajo. *merge_jobid* es **binario (16)** y su valor predeterminado es NULL.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Especifica si la suscripción se puede sincronizar mediante el administrador de sincronización de Windows. *enabled_for_syncmgr* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si es **false**, la suscripción no está registrada en el administrador de sincronización. Si es **true**, la suscripción se registra con el administrador de sincronización y se puede sincronizar sin iniciar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @ftp_address = ] 'ftp_address'` Solo para la compatibilidad con versiones anteriores.  
  
`[ @ftp_port = ] ftp_port` Solo para la compatibilidad con versiones anteriores.  
  
`[ @ftp_login = ] 'ftp_login'` Solo para la compatibilidad con versiones anteriores.  
  
`[ @ftp_password = ] 'ftp_password'` Solo para la compatibilidad con versiones anteriores.  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'` Especifica la ubicación desde la que se van a recopilar los archivos de instantánea. *alternate_snapshot_folder* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. Si es NULL, los archivos de instantáneas se recopilarán desde la ubicación predeterminada especificada por el publicador.  
  
`[ @working_directory = ] 'working_directory'` Es el nombre del directorio de trabajo utilizado para almacenar temporalmente archivos de datos y de esquema para la publicación cuando se utiliza FTP para transferir archivos de instantáneas. *working_directory* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @use_ftp = ] 'use_ftp'` Especifica el uso de FTP en lugar del protocolo típico para recuperar instantáneas. *use_ftp* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @use_interactive_resolver = ] 'use_interactive_resolver' ]` Usa el solucionador interactivo para resolver conflictos de todos los artículos que permiten la resolución interactiva. *use_interactive_resolver* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
`[ @offloadagent = ] 'remote_agent_activation'`
 > [!NOTE]  
>  La activación remota del agente ha quedado desusada y ya no es compatible. Este parámetro solamente se admite por compatibilidad de scripts con versiones anteriores. Si se establece *remote_agent_activation* en un valor distinto de **false** , se generará un error.  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  La activación remota del agente ha quedado desusada y ya no es compatible. Este parámetro solamente se admite por compatibilidad de scripts con versiones anteriores. Si se establece *remote_agent_server_name* en cualquier valor que no sea NULL, se generará un error.  
  
`[ @job_name = ] 'job_name' ]` Es el nombre de un trabajo del agente existente. *job_name* es de **tipo sysname y su** valor predeterminado es NULL. Este parámetro solamente se especifica cuando la suscripción se va a sincronizar mediante un trabajo existente en lugar de uno recién creado (el valor predeterminado). Si no es miembro del rol fijo de servidor **sysadmin** , debe especificar *job_login* y *job_password* al especificar *job_name*.  
  
`[ @dynamic_snapshot_location = ] 'dynamic_snapshot_location' ]` La ruta de acceso a la carpeta donde se leerán los archivos de instantáneas si se va a usar una instantánea de datos filtrados. *dynamic_snapshot_location* es de tipo **nvarchar (260)** y su valor predeterminado es NULL. Para obtener más información, consulte [Filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
`[ @use_web_sync = ] use_web_sync` Indica que la sincronización web está habilitada. *use_web_sync* es de **bit** y su valor predeterminado es 0. **1** especifica que la suscripción de extracción se puede sincronizar a través de Internet mediante http.  
  
`[ @internet_url = ] 'internet_url'` Es la ubicación del agente de escucha de replicación (REPLISAPI.DLL) para la sincronización Web. *internet_url* es de tipo **nvarchar (260)** y su valor predeterminado es NULL. *internet_url* es una dirección URL completa, con el formato `http://server.domain.com/directory/replisapi.dll` . Si el servidor está configurado para escuchar en un puerto que no sea el 80, también se debe proporcionar el número de puerto con el formato `http://server.domain.com:portnumber/directory/replisapi.dll`, donde `portnumber` representa al puerto.  
  
`[ @internet_login = ] 'internet_login'` Es el inicio de sesión que utiliza el Agente de mezcla al conectarse al servidor Web que hospeda la sincronización Web mediante la autenticación básica HTTP. *internet_login* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @internet_password = ] 'internet_password'` Es la contraseña que utiliza el Agente de mezcla al conectarse al servidor Web que hospeda la sincronización Web mediante la autenticación básica HTTP. *internet_password* es de tipo **nvarchar (524)** y su valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
`[ @internet_security_mode = ] internet_security_mode` Es el método de autenticación utilizado por el Agente de mezcla al conectarse al servidor Web durante la sincronización Web mediante HTTPS. *internet_security_mode* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Se utiliza Autenticación básica.|  
|**1** (predeterminado)|Se utiliza Autenticación integrada de Windows.|  
  
> [!NOTE]  
>  Se recomienda utilizar la autenticación básica con sincronización web. Para usar la sincronización Web, debe establecer una conexión TLS con el servidor Web. Para más información, consulte [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md).  
  
`[ @internet_timeout = ] internet_timeout` Es el período de tiempo, en segundos, antes de que expire una solicitud de sincronización Web. *internet_timeout* es de **tipo int** y su valor predeterminado es **300** segundos.  
  
`[ @hostname = ] 'hostname'` Invalida el valor de HOST_NAME () cuando esta función se usa en la cláusula WHERE de un filtro con parámetros. *hostname* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @job_login = ] 'job_login'` Es el inicio de sesión de la cuenta de Windows con la que se ejecuta el agente. *job_login* es de tipo **nvarchar (257)** y no tiene ningún valor predeterminado. Esta cuenta de Windows siempre se utiliza para las conexiones del agente al suscriptor y para las conexiones al distribuidor y al publicador cuando se utiliza Autenticación integrada de Windows.  
  
`[ @job_password = ] 'job_password'` Es la contraseña de la cuenta de Windows con la que se ejecuta el agente. *job_password* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
> [!IMPORTANT]  
>  No almacene información de autenticación en archivos de script. Para una mayor seguridad, los nombres de inicio de sesión y las contraseñas se deben proporcionar en tiempo de ejecución.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addmergepullsubscription_agent** se utiliza en la replicación de mezcla y utiliza una funcionalidad similar a la de [sp_addpullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md).  
  
 Para obtener un ejemplo de cómo especificar correctamente la configuración de seguridad al ejecutar **sp_addmergepullsubscription_agent**, vea [crear una suscripción de extracción](../../relational-databases/replication/create-a-pull-subscription.md).  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_addmergepullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_1_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addmergepullsubscription_agent**.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addmergepullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
