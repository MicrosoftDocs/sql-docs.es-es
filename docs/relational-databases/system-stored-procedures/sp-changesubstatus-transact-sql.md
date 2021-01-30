---
description: sp_changesubstatus (Transact-SQL)
title: sp_changesubstatus (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubstatus
- sp_changesubstatus_TSQL
helpviewer_keywords:
- sp_changesubstatus
ms.assetid: 9370e47a-d128-4f15-9224-1c3642770c39
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 89de8f8a4c5f1abb3a07551bcca6eac53e0b2e2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158968"
---
# <a name="sp_changesubstatus-transact-sql"></a>sp_changesubstatus (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cambia el estado de un suscriptor existente. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_changesubstatus [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
        , [ @status = ] 'status'  
    [ , [ @previous_status = ] 'previous_status' ]  
    [ , [ @destination_db = ] 'destination_db' ]  
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
    [ , [ @distribution_jobid = ] distribution_jobid ]  
    [ , [ @from_auto_sync = ] from_auto_sync ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @offloadagent= ] remote_agent_activation ]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] dts_package_location ]  
    [ , [ @skipobjectactivation = ] skipobjectactivation  
  [ , [ @distribution_job_name= ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es **%** . Si no se especifica la *publicación* , se verán afectadas todas las publicaciones.  
  
`[ @article = ] 'article'` Es el nombre del artículo. Debe ser único para la publicación. *article* es de **tipo sysname y su** valor predeterminado es **%** . Si no se especifica *article* , se verán afectados todos los artículos.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor del que se va a cambiar el estado. *Subscriber* es de **tipo sysname y su** valor predeterminado es **%** . Si no se especifica el *suscriptor* , se cambia el estado de todos los suscriptores al artículo especificado.  
  
`[ @status = ] 'status'` Es el estado de la suscripción en la tabla **syssubscriptions** . *status* es de **tipo sysname**, no tiene ningún valor predeterminado y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**active**|El suscriptor está sincronizado y recibe datos.|  
|**inactive**|Existe una entrada de suscriptor sin que haya una suscripción.|  
|**subscribed**|El suscriptor solicita datos, pero aún no está sincronizado.|  
  
`[ @previous_status = ] 'previous_status'` Es el estado anterior de la suscripción. *previous_status* es de **tipo sysname y su** valor predeterminado es NULL. Este parámetro permite cambiar cualquier suscripción que tenga actualmente ese estado, lo que permite a las funciones de grupo en un conjunto específico de suscripciones (por ejemplo, al establecer de nuevo todas las suscripciones activas en **suscrito**).  
  
`[ @destination_db = ] 'destination_db'` Es el nombre de la base de datos de destino. *destination_db* es de **tipo sysname y su** valor predeterminado es **%** .  
  
`[ @frequency_type = ] frequency_type` Es la frecuencia con la que se programa la tarea de distribución. *frequency_type* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @frequency_interval = ] frequency_interval` Es el valor que se va a aplicar a la frecuencia establecida por *frequency_type*. *frequency_interval* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la fecha de la tarea de distribución. Este parámetro se utiliza cuando *frequency_type* se establece en 32 (relativo mensual). *frequency_relative_interval* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|First|  
|**2**|Segundo|  
|**4**|Tercero|  
|**8**|Cuarto|  
|**16**|Último|  
|NULL (predeterminado)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @frequency_subday = ] frequency_subday` Es la frecuencia, en minutos, con la que se vuelve a programar durante el período definido. *frequency_subday* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**2**|Segundo|  
|**4**|Minute|  
|**8**|Hora|  
|NULL (predeterminado)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es el intervalo de *frequency_subday*. *frequency_subday_interval* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día en la que la tarea de distribución se programa por primera vez, con el formato HHMMSS. *active_start_time_of_day* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día a la que la tarea de distribución deja de estar programada, con el formato HHMMSS. *active_end_time_of_day* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en la que la tarea de distribución se programa por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que la tarea de distribución deja de estar programada, con el formato AAAAMMDD. *active_end_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @optional_command_line = ] 'optional_command_line'` Es un símbolo del sistema opcional. *optional_command_line* es de tipo **nvarchar (4000)** y su valor predeterminado es NULL.  
  
`[ @distribution_jobid = ] distribution_jobid` Es el ID. de trabajo del Agente de distribución en el distribuidor de la suscripción cuando se cambia el estado de la suscripción de inactiva a activa. En otros casos, no se define. Si interviene más de un agente de distribución en una sola llamada a este procedimiento almacenado, el resultado no se define. *distribution_jobid* es **binario (16)** y su valor predeterminado es NULL.  
  
`[ @from_auto_sync = ] from_auto_sync` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @ignore_distributor = ] ignore_distributor` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @offloadagent = ] remote_agent_activation`
 > [!NOTE]  
>  La activación remota del agente ha quedado desusada y ya no es compatible. Este parámetro solamente se admite por compatibilidad de scripts con versiones anteriores. Si se establece *remote_agent_activation* en un valor distinto de **0** , se genera un error.  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  La activación remota del agente ha quedado desusada y ya no es compatible. Este parámetro solamente se admite por compatibilidad de scripts con versiones anteriores. Si se establece *remote_agent_server_name* en cualquier valor distinto de NULL, se genera un error.  
  
`[ @dts_package_name = ] 'dts_package_name'` Especifica el nombre del paquete de servicios de transformación de datos (DTS) de. *dts_package_name* es de **tipo sysname y su** valor predeterminado es NULL. Por ejemplo, para un paquete denominado **DTSPub_Package** debe especificar `@dts_package_name = N'DTSPub_Package'` .  
  
`[ @dts_package_password = ] 'dts_package_password'` Especifica la contraseña del paquete. *dts_package_password* es de **tipo sysname y su** valor predeterminado es null, lo que especifica que la propiedad de contraseña se dejará sin cambios.  
  
> [!NOTE]  
>  Un paquete DTS debe tener una contraseña.  
  
`[ @dts_package_location = ] dts_package_location` Especifica la ubicación del paquete. *dts_package_location* es de **tipo int** y su valor predeterminado es **0**. Si es **0**, la ubicación del paquete se encuentra en el distribuidor. Si es **1**, la ubicación del paquete está en el suscriptor. La ubicación del paquete puede ser **Distributor** o **Subscriber**.  
  
`[ @skipobjectactivation = ] skipobjectactivation` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @distribution_job_name = ] 'distribution_job_name'` Es el nombre del trabajo de distribución. *distribution_job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al cambiar las propiedades de un artículo en un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_changesubstatus** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
 **sp_changesubstatus** cambia el estado del suscriptor en la tabla **syssubscriptions** con el estado cambiado. Si es necesario, actualiza el estado del artículo en la tabla **sysarticles** para indicar activo o inactivo. Si es necesario, establece la marca de replicación activada o desactivada en la tabla **sysobjects** de la tabla replicada.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , del rol fijo de base de datos **db_owner** o del creador de la suscripción pueden ejecutar **sp_changesubstatus**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_dropsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
