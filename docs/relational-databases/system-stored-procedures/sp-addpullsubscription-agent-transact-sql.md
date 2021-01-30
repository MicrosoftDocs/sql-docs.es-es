---
description: sp_addpullsubscription_agent (Transact-SQL)
title: sp_addpullsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpullsubscription_agent
- sp_addpullsubscription_agent_TSQL
helpviewer_keywords:
- sp_addpullsubscription_agent
ms.assetid: b9c2eaed-6d2d-4b78-ae9b-73633133180b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 031911ae4e9f6f81e82ec9d0b4fb20c73d94a2cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206685"
---
# <a name="sp_addpullsubscription_agent-transact-sql"></a>sp_addpullsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
 
  Agrega un nuevo trabajo de agente programado utilizado para sincronizar una suscripción de extracción a una publicación transaccional. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addpullsubscription_agent [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]          , [ @publication = ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor = ] 'distributor' ]  
    [ , [ @distribution_db = ] 'distribution_db' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]  
    [ , [ @distributor_login = ] 'distributor_login' ]  
    [ , [ @distributor_password = ] 'distributor_password' ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
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
    [ , [ @distribution_jobid = ] distribution_jobid OUTPUT ]  
    [ , [ @encrypted_distributor_password = ] encrypted_distributor_password ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port = ] ftp_port ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]  
    [ , [ @working_directory = ] 'working_directory' ]  
    [ , [ @use_ftp = ] 'use_ftp' ]  
    [ , [ @publication_type = ] publication_type ]  
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @offloadagent = ] 'remote_agent_activation' ]  
    [ , [ @offloadserver = ] 'remote_agent_server_name']  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  

> [!NOTE]
> El nombre del servidor se puede especificar como `<Hostname>,<PortNumber>` . Es posible que tenga que especificar el número de puerto para la conexión cuando SQL Server se implementa en Linux o Windows con un puerto personalizado, y el servicio explorador está deshabilitado.
  
`[ @publisher_db = ] 'publisher_db'_` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname y su** valor predeterminado es NULL. los publicadores de Oracle omiten *publisher_db* .  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre de la instancia del suscriptor o el nombre del agente de escucha del AG si la base de datos del suscriptor se encuentra en un grupo de disponibilidad. *Subscriber* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Es el modo de seguridad que se va a utilizar al conectarse a un suscriptor durante la sincronización. *subscriber_security_mode* es de **tipo int y** su valor predeterminado es NULL. **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. **1** especifica la autenticación de Windows.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. El Agente de distribución siempre conecta con el suscriptor local mediante Autenticación de Windows. Si se especifica un valor distinto de NULL o **1** para este parámetro, se devuelve un mensaje de advertencia.  
  
`[ @subscriber_login = ] 'subscriber_login'` Es el inicio de sesión del suscriptor que se va a utilizar al conectarse a un suscriptor durante la sincronización. *subscriber_login* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. Si se especifica un valor para este parámetro, se devuelve un mensaje de advertencia, pero el valor se pasa por alto.  
  
`[ @subscriber_password = ] 'subscriber_password'` Es la contraseña del suscriptor. *subscriber_password* es necesario si *subscriber_security_mode* está establecido en **0**. *subscriber_password* es de **tipo sysname y su** valor predeterminado es NULL. Si se utiliza una contraseña de suscriptor, se cifra automáticamente.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. Si se especifica un valor para este parámetro, se devuelve un mensaje de advertencia, pero el valor se pasa por alto.  
  
`[ @distributor = ] 'distributor'` Es el nombre del distribuidor. *Distributor* es de **tipo sysname** y su valor predeterminado es el especificado por el *publicador*.  
  
`[ @distribution_db = ] 'distribution_db'` Es el nombre de la base de datos de distribución. *distribution_db* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @distributor_security_mode = ] distributor_security_mode` Es el modo de seguridad que se va a utilizar al conectarse a un distribuidor durante la sincronización. *distributor_security_mode* es de **tipo int** y su valor predeterminado es **1**. **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. **1** especifica la autenticación de Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` Es el inicio de sesión del distribuidor que se va a utilizar al conectarse a un distribuidor durante la sincronización. *distributor_login* es necesario si *distributor_security_mode* está establecido en **0**. *distributor_login* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @distributor_password = ] 'distributor_password'` Es la contraseña del distribuidor. *distributor_password* es necesario si *distributor_security_mode* está establecido en **0**. *distributor_password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  No utilice una contraseña en blanco. Utilice una contraseña segura. Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @optional_command_line = ] 'optional_command_line'` Es un símbolo del sistema opcional proporcionado al Agente de distribución. Por ejemplo, **-DefinitionFile** C:\Distdef.txt o **-CommitBatchSize** 10. *optional_command_line* es de tipo **nvarchar (4000)** y su valor predeterminado es una cadena vacía.  
  
`[ @frequency_type = ] frequency_type` Es la frecuencia con la que se programa el Agente de distribución. *frequency_type* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una vez|  
|**2** (predeterminado)|A petición|  
|**4**|Diario|  
|**8**|Cada semana|  
|**16**|Mensual|  
|**32**|Mensualmente relativa|  
|**64**|Iniciar automáticamente|  
|**128**|Periódica|  
  
> [!NOTE]  
>  Si se especifica un valor de **64** , el agente de distribución se ejecutará en modo continuo. Esto corresponde a la configuración del parámetro **-Continuous** para el agente. Para más información, consulte [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
`[ @frequency_interval = ] frequency_interval` Es el valor que se va a aplicar a la frecuencia establecida por *frequency_type*. *frequency_interval* es de **tipo int** y su valor predeterminado es 1.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la fecha del Agente de distribución. Este parámetro se utiliza cuando *frequency_type* se establece en **32** (relativo mensual). *frequency_relative_interval* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1** (predeterminado)|First|  
|**2**|Segundo|  
|**4**|Tercero|  
|**8**|Cuarto|  
|**16**|Último|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es **1**.  
  
`[ @frequency_subday = ] frequency_subday` Es la frecuencia con que se vuelve a programar durante el período definido. *frequency_subday* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1** (predeterminado)|Una sola vez|  
|**2**|Segundo|  
|**4**|Minute|  
|**8**|Hora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es el intervalo de *frequency_subday*. *frequency_subday_interval* es de **tipo int** y su valor predeterminado es **1**.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día a la que se programa el Agente de distribución por primera vez, con el formato HHMMSS. *active_start_time_of_day* es de **tipo int** y su valor predeterminado es **0**.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día en que el Agente de distribución deja de estar programado, con el formato HHMMSS. *active_end_time_of_day* es de **tipo int** y su valor predeterminado es **0**.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en la que se programa el Agente de distribución por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y su valor predeterminado es **0**.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que el Agente de distribución deja de estar programado, con el formato AAAAMMDD. *active_end_date* es de **tipo int** y su valor predeterminado es **0**.  
  
`[ @distribution_jobid = ] _distribution_jobidOUTPUT` Es el identificador de la Agente de distribución para este trabajo. *distribution_jobid* es **binario (16)**, su valor predeterminado es NULL y es un parámetro de salida.  
  
`[ @encrypted_distributor_password = ] encrypted_distributor_password` Ya no se admite la configuración de *encrypted_distributor_password* . Si se intenta establecer este parámetro de **bits** en **1** se producirá un error.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Indica si la suscripción se puede sincronizar mediante el [!INCLUDE[msCoName](../../includes/msconame-md.md)] Administrador de sincronización. *enabled_for_syncmgr* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si es **false**, la suscripción no está registrada en el administrador de sincronización. Si es **true**, la suscripción se registra con el administrador de sincronización y se puede sincronizar sin iniciar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @ftp_address = ] 'ftp_address'` Solo para la compatibilidad con versiones anteriores.  
  
`[ @ftp_port = ] ftp_port` Solo para la compatibilidad con versiones anteriores.  
  
`[ @ftp_login = ] 'ftp_login'` Solo para la compatibilidad con versiones anteriores.  
  
`[ @ftp_password = ] 'ftp_password'` Solo para la compatibilidad con versiones anteriores.  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'_` Especifica la ubicación de la carpeta alternativa de la instantánea. *alternate_snapshot_folder* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @working_directory = ] 'working_director'` Es el nombre del directorio de trabajo utilizado para almacenar los archivos de datos y de esquema para la publicación. *working_directory* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. El nombre se debe especificar en el formato UNC.  
  
`[ @use_ftp = ] 'use_ftp'` Especifica el uso de FTP en lugar del protocolo regular para recuperar las instantáneas. *use_ftp* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
`[ @publication_type = ] publication_type` Especifica el tipo de replicación de la publicación. *publication_type* es de **tinyint** con un valor predeterminado de **0**. Si es **0**, la publicación es un tipo de transacción. Si es **1**, la publicación es un tipo de instantánea. Si es **2**, la publicación es un tipo de combinación.  
  
`[ @dts_package_name = ] 'dts_package_name'` Especifica el nombre del paquete DTS. *dts_package_name* es de **tipo sysname y su** valor predeterminado es NULL. Por ejemplo, para especificar un paquete de `DTSPub_Package`, el parámetro sería `@dts_package_name = N'DTSPub_Package'`.  
  
`[ @dts_package_password = ] 'dts_package_password'` Especifica la contraseña del paquete, si la hay. *dts_package_password* es de **tipo sysname y su** valor predeterminado es null, lo que significa que una contraseña no está en el paquete.  
  
> [!NOTE]  
>  Debe especificar una contraseña si se especifica *dts_package_name* .  
  
`[ @dts_package_location = ] 'dts_package_location'` Especifica la ubicación del paquete. *dts_package_location* es de tipo **nvarchar (12)** y su valor predeterminado es **Subscriber**. La ubicación del paquete puede ser **Distributor** o **Subscriber**.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @offloadagent = ] 'remote_agent_activation'`
 > [!NOTE]  
>  La activación remota del agente ha quedado desusada y ya no es compatible. Este parámetro solamente se admite por compatibilidad de scripts con versiones anteriores. Si se establece *remote_agent_activation* en un valor distinto de **false** , se generará un error.  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  La activación remota del agente ha quedado desusada y ya no es compatible. Este parámetro solamente se admite por compatibilidad de scripts con versiones anteriores. Si se establece *remote_agent_server_name* en cualquier valor que no sea NULL, se generará un error.  
  
`[ @job_name = ] 'job_name'` Es el nombre de un trabajo del agente existente. *job_name* es de **tipo sysname y su** valor predeterminado es NULL. Este parámetro solamente se especifica cuando la suscripción se va a sincronizar mediante un trabajo existente en lugar de uno recién creado (el valor predeterminado). Si no es miembro del rol fijo de servidor **sysadmin** , debe especificar *job_login* y *job_password* al especificar *job_name*.  
  
`[ @job_login = ] 'job_login'` Es el inicio de sesión de la cuenta de Windows con la que se ejecuta el agente. *job_login* es de tipo **nvarchar (257)** y no tiene ningún valor predeterminado. Esta cuenta de Windows siempre se utiliza para las conexiones del agente con el suscriptor.  
  
`[ @job_password = ] 'job_password'` Es la contraseña de la cuenta de Windows con la que se ejecuta el agente. *job_password* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addpullsubscription_agent** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-a_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addpullsubscription_agent**.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_change_subscription_properties &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
