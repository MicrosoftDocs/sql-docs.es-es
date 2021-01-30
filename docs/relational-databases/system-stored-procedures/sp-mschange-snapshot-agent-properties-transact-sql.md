---
title: sp_MSchange_snapshot_agent_properties (T-SQL)
description: Describe el sp_MSchange_snapshot_agent_properties procedimiento almacenado que se usa para cambiar las propiedades de la Agente de instantáneas utilizada para Replicación de SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_snapshot_agent_properties_TSQL
- sp_MSchange_snapshot_agent_properties
helpviewer_keywords:
- sp_MSchange_snapshot_agent_properties
ms.assetid: 7947a788-3fd7-469f-84db-b03ba89a153c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f07673e9dd7b613e34a5f56d6fa4c3629d3e1540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174500"
---
# <a name="sp_mschange_snapshot_agent_properties-transact-sql"></a>sp_MSchange_snapshot_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia las propiedades de un trabajo Agente de instantáneas que se ejecuta en un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] distribuidor de o una versión posterior. Este procedimiento almacenado se utiliza para cambiar las propiedades cuando el publicador se ejecuta en una instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Este procedimiento almacenado se ejecuta en el distribuidor de la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_MSchange_snapshot_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @frequency_type= ] frequency_type  
        , [ @frequency_interval= ] frequency_interval  
        , [ @frequency_subday= ] frequency_subday  
        , [ @frequency_subday_interval= ] frequency_subday_interval  
        , [ @frequency_relative_interval= ] frequency_relative_interval  
        , [ @frequency_recurrence_factor= ] frequency_recurrence_factor  
        , [ @active_start_date= ] active_start_date  
        , [ @active_end_date= ] active_end_date  
        , [ @active_start_time_of_day= ] active_start_time_of_day  
        , [ @active_end_time_of_day= ] active_end_time_of_day  
        , [ @snapshot_job_name = ] 'snapshot_agent_name'  
        , [ @publisher_security_mode = ] publisher_security_mode  
        , [ @publisher_login = ] 'publisher_login'  
        , [ @publisher_password = ] 'publisher_password'   
        , [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
        , [ @publisher_type = ] 'publisher_type'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos de publicación. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @frequency_type = ] frequency_type` Es la frecuencia con la que se ejecuta el Agente de instantáneas. *frequency_type* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**2**|A petición|  
|**4**|Diario|  
|**8**|Cada semana|  
|**10**|Mensual|  
|**20**|Mensualmente, dependiendo del intervalo de frecuencia|  
|**40**|Cuando se inicia el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  
`[ @frequency_interval = ] frequency_interval` Es el valor que se va a aplicar a la frecuencia establecida por *frequency_type*. *frequency_interval* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @frequency_subday = ] frequency_subday` Son las unidades de *freq_subday_interval*. *frequency_subday* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**2**|Segundo|  
|**4**|Minute|  
|**8**|Hora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es el intervalo de *frequency_subday*. *frequency_subday_interval* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la fecha en la que se ejecuta el Agente de instantáneas. *frequency_relative_interval* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en la que se programa el Agente de instantáneas por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que el Agente de instantáneas deja de estar programado, con el formato AAAAMMDD. *active_end_date* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día a la que se programa el Agente de instantáneas por primera vez, con el formato HHMMSS. *active_start_time_of_day* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día en que el Agente de instantáneas deja de estar programado, con el formato HHMMSS. *active_end_time_of_day* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` Es el nombre de un nombre de trabajo de Agente de instantáneas existente si se está utilizando un trabajo existente. *snapshot_agent_name* es de tipo **nvarchar (100)** y no tiene ningún valor predeterminado.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Es el modo de seguridad que utiliza el agente al conectarse al publicador. *publisher_security_mode* es de **tipo int** y no tiene ningún valor predeterminado. **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación y **1** especifica la autenticación de Windows. Se debe especificar un valor de **0** para los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Es el inicio de sesión que se usa al conectarse al publicador. *publisher_login* es de **tipo sysname** y no tiene ningún valor predeterminado. se debe especificar *publisher_login* cuando *publisher_security_mode* es **0**. Si *publisher_login* es NULL y Publisher *_ * * security_mode* es **1**, se utilizará la cuenta de Windows especificada en *job_login* al conectarse al publicador.  
  
`[ @publisher_password = ] 'publisher_password'` Es la contraseña que se usa al conectarse al publicador. *publisher_password* es de tipo **nvarchar (524)** y no tiene ningún valor predeterminado.  
  
> [!IMPORTANT]  
>  No almacene información de autenticación en archivos de script. Para ayudar a mejorar la seguridad, se recomienda proporcionar nombres de inicio de sesión y contraseñas en tiempo de ejecución.  
  
`[ @job_login = ] 'job_login'` Es el inicio de sesión de la cuenta de Windows con la que se ejecuta el agente. *job_login* es de tipo **nvarchar (257)** y no tiene ningún valor predeterminado. Esta cuenta de Windows siempre se utiliza para conexiones del agente con el distribuidor. Es preciso proporcionar este parámetro al crear un nuevo trabajo del Agente de instantáneas. *No se puede cambiar para un no* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Publicador.*  
  
`[ @job_password = ] 'job_password'` Es la contraseña de la cuenta de Windows con la que se ejecuta el agente. *job_password* es de **tipo sysname** y no tiene ningún valor predeterminado. Es preciso proporcionar este parámetro al crear un nuevo trabajo del Agente de instantáneas.  
  
> [!IMPORTANT]  
>  No almacene información de autenticación en archivos de script. Para ayudar a mejorar la seguridad, se recomienda proporcionar nombres de inicio de sesión y contraseñas en tiempo de ejecución.  
  
`[ @publisher_type = ] 'publisher_type'` Especifica el tipo de publicador cuando el publicador no se está ejecutando en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *publisher_type* es de **tipo sysname** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**MSSQLSERVER**|Especifica un publicador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|Especifica un publicador estándar de Oracle.|  
|**ORACLE GATEWAY**|Especifica un publicador de puerta de enlace de Oracle.|  
  
 Para obtener más información acerca de las diferencias entre un publicador de Oracle y un publicador de puerta de enlace de Oracle, consulte [información general de publicación de Oracle](../../relational-databases/replication/non-sql/oracle-publishing-overview.md).  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_MSchange_snapshot_agent_properties** se utiliza en la replicación de instantáneas, la replicación transaccional y la replicación de mezcla.  
  
 Debe especificar todos los parámetros al ejecutar **sp_MSchange_snapshot_agent_properties**. Ejecute [sp_helppublication_snapshot](../../relational-databases/system-stored-procedures/sp-helppublication-snapshot-transact-sql.md) para devolver las propiedades actuales del trabajo de agente de instantáneas.  
  
 Cuando el publicador se ejecuta en una instancia de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o una versión posterior, debe utilizar [sp_changepublication_snapshot](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md) para cambiar las propiedades de un trabajo agente de instantáneas.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** en el distribuidor pueden ejecutar **sp_MSchange_snapshot_agent_properties**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)  
  
  
