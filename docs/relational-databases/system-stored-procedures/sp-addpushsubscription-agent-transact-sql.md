---
description: sp_addpushsubscription_agent (Transact-SQL)
title: sp_addpushsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpushsubscription_agent_TSQL
- sp_addpushsubscription_agent
helpviewer_keywords:
- sp_addpushsubscription_agent
ms.assetid: 1fdd2052-50d8-4318-8aa7-fc635d5cad18
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 396e90bb76296270bf25d236a7fd20cd34a063dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206665"
---
# <a name="sp_addpushsubscription_agent-transact-sql"></a>sp_addpushsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Agrega un nuevo trabajo del agente programado que se utiliza para sincronizar una suscripción de inserción con una publicación transaccional. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
> [!IMPORTANT]  
>  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addpushsubscription_agent [ @publication= ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
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
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @distribution_job_name = ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @subscriber_provider = ] 'subscriber_provider' ]   
    [ , [ @subscriber_datasrc = ] 'subscriber_datasrc' ]   
    [ , [ @subscriber_location = ] 'subscriber_location' ]  
    [ , [ @subscriber_provider_string = ] 'subscriber_provider_string' ]   
    [ , [ @subscriber_catalog = ] 'subscriber_catalog' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre de la instancia del suscriptor o el nombre del agente de escucha del AG si la base de datos del suscriptor es un grupo de disponibilidad. *Subscriber* es de **tipo sysname y su** valor predeterminado es NULL. 

> [!NOTE]
> El nombre del servidor se puede especificar como `<Hostname>,<PortNumber>` . Es posible que tenga que especificar el número de puerto para la conexión cuando SQL Server se implementa en Linux o Windows con un puerto personalizado, y el servicio explorador está deshabilitado.

`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de **tipo sysname y su** valor predeterminado es NULL. Para un suscriptor que no sea de SQL Server, especifique un valor de **(destino predeterminado)** para *subscriber_db*.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Es el modo de seguridad que se va a utilizar al conectarse a un suscriptor durante la sincronización. *subscriber_security_mode* es de **tipo int** y su valor predeterminado es 1. **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. **1** especifica la autenticación de Windows.  
  
> [!IMPORTANT]  
>  En el caso de las suscripciones de actualización en cola, utilice la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para las conexiones a los suscriptores y especifique otra cuenta para la conexión a cada suscriptor. Para el resto de las suscripciones, utilice la autenticación de Windows.  
  
`[ @subscriber_login = ] 'subscriber_login'` Es el inicio de sesión del suscriptor que se va a utilizar al conectarse a un suscriptor durante la sincronización. *subscriber_login* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscriber_password = ] 'subscriber_password'` Es la contraseña del suscriptor. *subscriber_password* es necesario si *subscriber_security_mode* está establecido en **0**. *subscriber_password* es de **tipo sysname y su** valor predeterminado es NULL. Si se utiliza una contraseña de suscriptor, se cifra automáticamente.  
  
> [!IMPORTANT]  
>  No utilice una contraseña en blanco. Utilice una contraseña segura. Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @job_login = ] 'job_login'` Es el inicio de sesión de la cuenta con la que se ejecuta el agente. En Azure SQL Instancia administrada use una cuenta de SQL Server. *job_login* es de tipo **nvarchar (257)** y su valor predeterminado es NULL. Esta cuenta de Windows se utiliza siempre para las conexiones del agente con el distribuidor y para las conexiones con el suscriptor al utilizar la Autenticación de Windows integrada.  
  
`[ @job_password = ] 'job_password'` Es la contraseña de la cuenta con la que se ejecuta el agente. *job_password* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @job_name = ] 'job_name'` Es el nombre de un trabajo del agente existente. *job_name* es de **tipo sysname y su** valor predeterminado es NULL. Este parámetro solamente se especifica cuando la suscripción se va a sincronizar mediante un trabajo existente en lugar de uno recién creado (el valor predeterminado). Si no es miembro del rol fijo de servidor **sysadmin** , debe especificar *job_login* y *job_password* al especificar *job_name*.  
  
`[ @frequency_type = ] frequency_type` Es la frecuencia con la que se programa el Agente de distribución. *frequency_type* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una vez|  
|**2**|A petición|  
|**4**|Diario|  
|**8**|Cada semana|  
|**16**|Mensual|  
|**32**|Mensualmente relativa|  
|**64** (valor predeterminado)|Iniciar automáticamente|  
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
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es 0.  
  
`[ @frequency_subday = ] frequency_subday` Es la frecuencia con que se vuelve a programar durante el período definido. *frequency_subday* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**2**|Segundo|  
|**4** (valor predeterminado)|Minute|  
|**8**|Hora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es el intervalo de *frequency_subday*. *frequency_subday_interval* es de **tipo int** y su valor predeterminado es 5.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día a la que se programa el Agente de distribución por primera vez, con el formato HHMMSS. *active_start_time_of_day* es de **tipo int** y su valor predeterminado es 0.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día en que el Agente de distribución deja de estar programado, con el formato HHMMSS. *active_end_time_of_day* es de **tipo int** y su valor predeterminado es 235959.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en la que se programa el Agente de distribución por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y su valor predeterminado es 0.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que el Agente de distribución deja de estar programado, con el formato AAAAMMDD. *active_end_date* es de **tipo int** y su valor predeterminado es 99991231.  
  
`[ @dts_package_name = ] 'dts_package_name'` Especifica el nombre del paquete de servicios de transformación de datos (DTS) de. *dts_package_name* es de **tipo sysname y su** valor predeterminado es NULL. Por ejemplo, para especificar el nombre de paquete `DTSPub_Package`, el parámetro sería `@dts_package_name = N'DTSPub_Package'`.  
  
`[ @dts_package_password = ] 'dts_package_password'` Especifica la contraseña necesaria para ejecutar el paquete. *dts_package_password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Debe especificar una contraseña si se especifica *dts_package_name* .  
  
`[ @dts_package_location = ] 'dts_package_location'` Especifica la ubicación del paquete. *dts_package_location* es de tipo **nvarchar (12)** y su valor predeterminado es Distributor. La ubicación del paquete puede ser **Distributor** o **Subscriber**.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Indica si la suscripción se puede sincronizar mediante el [!INCLUDE[msCoName](../../includes/msconame-md.md)] Administrador de sincronización. *enabled_for_syncmgr* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si es **false**, la suscripción no está registrada en el administrador de sincronización. Si es **true**, la suscripción se registra con el administrador de sincronización y se puede sincronizar sin iniciar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @distribution_job_name = ] 'distribution_job_name'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscriber_provider = ] 'subscriber_provider'` Es el identificador de programación único (PROGID) con el que se registra el proveedor de OLE DB para el origen de datos que no [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es de. *subscriber_provider* es de **tipo sysname y su** valor predeterminado es NULL. *subscriber_provider* debe ser único para el proveedor de OLE DB instalado en el distribuidor. *subscriber_provider* solo se admite para suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @subscriber_datasrc = ] 'subscriber_datasrc'` Es el nombre del origen de datos tal y como lo entiende el proveedor de OLE DB. *subscriber_datasrc* es de tipo **nvarchar (4000)** y su valor predeterminado es NULL. *subscriber_datasrc* se pasa como la propiedad DBPROP_INIT_DATASOURCE para inicializar el proveedor de OLE DB. *subscriber_datasrc* solo se admite para suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @subscriber_location = ] 'subscriber_location'` Es la ubicación de la base de datos tal como la entiende el proveedor de OLE DB. *subscriber_location* es de tipo **nvarchar (4000)** y su valor predeterminado es NULL. *subscriber_location* se pasa como la propiedad DBPROP_INIT_LOCATION para inicializar el proveedor de OLE DB. *subscriber_location* solo se admite para suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @subscriber_provider_string = ] 'subscriber_provider_string'` Es la cadena de conexión específica del proveedor de OLE DB que identifica el origen de datos. *subscriber_provider_string* es de tipo **nvarchar (4000)** y su valor predeterminado es NULL. *subscriber_provider_string* se pasa a IDataInitialize o se establece como la propiedad DBPROP_INIT_PROVIDERSTRING para inicializar el proveedor de OLE DB. *subscriber_provider_string* solo se admite para suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @subscriber_catalog = ] 'subscriber_catalog'` Es el catálogo que se va a utilizar al realizar una conexión al proveedor de OLE DB. *subscriber_catalog* es de **tipo sysname y su** valor predeterminado es NULL. *subscriber_catalog* se pasa como la propiedad DBPROP_INIT_CATALOG para inicializar el proveedor de OLE DB. *subscriber_catalog* solo se admite para suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addpushsubscription_agent** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/sp-addpushsubscription-a_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addpushsubscription_agent**.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Crear una suscripción para un suscriptor que no sea de SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_addsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md)   
 [sp_dropsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
