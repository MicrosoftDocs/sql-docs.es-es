---
description: sp_addsubscriber (Transact-SQL)
title: sp_addsubscriber (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addsubscriber
- sp_addsubscriber_TSQL
helpviewer_keywords:
- sp_addsubscriber
ms.assetid: b8a584ea-2a26-4936-965b-b84f026e39c0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9839d1daa3316fa8cb47af0ed2e362eba194f414
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212007"
---
# <a name="sp_addsubscriber-transact-sql"></a>sp_addsubscriber (Transact-SQL)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

  Agrega un suscriptor nuevo a un publicador, habilitándolo para recibir publicaciones. Para las publicaciones de instantáneas y transaccionales, este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicaciones; para las publicaciones de combinación que utilizan un distribuidor remoto, lo hace en el distribuidor.  
  
> [!IMPORTANT]  
>  Este procedimiento almacenado ha quedado desusado. Ya no es necesario registrar de forma explícita un suscriptor en el publicador.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addsubscriber [ @subscriber = ] 'subscriber'  
    [ , [ @type = ] type ]   
    [ , [ @login = ] 'login' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @commit_batch_size = ] commit_batch_size ]  
    [ , [ @status_batch_size = ] status_batch_size ]  
    [ , [ @flush_frequency = ] flush_frequency ]  
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
    [ , [ @description = ] 'description' ]  
    [ , [ @security_mode = ] security_mode ]  
    [ , [ @encrypted_password = ] encrypted_password ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @subscriber = ] 'subscriber'` Es el nombre del servidor que se va a agregar como un suscriptor válido a las publicaciones de este servidor. *Subscriber* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @type = ] type` Es el tipo de suscriptor. el *tipo* es **tinyint** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0** (valor predeterminado)|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Suscriptor de|  
|**1**|Servidor del origen de datos ODBC|  
|**2**|Base de datos [!INCLUDE[msCoName](../../includes/msconame-md.md)] Jet|  
|**3**|Proveedor OLE DB|  
  
`[ @login = ] 'login'` Es el identificador de inicio de sesión para la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. *login* es de tipo **sysname** y su valor predeterminado es NULL.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @password = ] 'password'` Es la contraseña para la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. *password* es de tipo **nvarchar (524)** y su valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  No utilice una contraseña en blanco. Utilice una contraseña segura.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @commit_batch_size = ] commit_batch_size` Este parámetro está en desuso y se mantiene por compatibilidad con versiones anteriores de los scripts.  
  
> [!NOTE]  
>  Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @status_batch_size = ] status_batch_size` Este parámetro está en desuso y se mantiene por compatibilidad con versiones anteriores de los scripts.  
  
> [!NOTE]  
>  Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @flush_frequency = ] flush_frequency` Este parámetro está en desuso y se mantiene por compatibilidad con versiones anteriores de los scripts.  
  
> [!NOTE]  
>  Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @frequency_type = ] frequency_type` Es la frecuencia con la que se programa el agente de replicación. *frequency_type* es de **tipo int** y puede tener uno de estos valores.  
  
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
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @frequency_interval = ] frequency_interval` Es el valor que se aplica a la frecuencia establecida por *frequency_type*. *frequency_interval* es de **tipo int** y su valor predeterminado es 1.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la fecha del agente de replicación. Este parámetro se utiliza cuando *frequency_type* se establece en **32** (relativo mensual). *frequency_relative_interval* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1** (predeterminado)|First|  
|**2**|Segundo|  
|**4**|Tercero|  
|**8**|Cuarto|  
|**16**|Último|  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es **0**.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @frequency_subday = ] frequency_subday` Es la frecuencia con que se vuelve a programar durante el período definido. *frequency_subday* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**2**|Segundo|  
|**4** (valor predeterminado)|Minute|  
|**8**|Hora|  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es el intervalo de *frequency_subday*. *frequency_subday_interval* es de **tipo int** y su valor predeterminado es **5**.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día en que el agente de replicación se programa por primera vez, con el formato HHMMSS. *active_start_time_of_day* es de **tipo int** y su valor predeterminado es **0**.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día a la que el agente de replicación deja de estar programado, con el formato HHMMSS. *active_end_time_of_day* es de **tipo int** y su valor predeterminado es 235959, lo que significa 11:59:59 P.M. tal y como se mide en un reloj de 24 horas.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en la que el agente de replicación se programa por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y su valor predeterminado es 0.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que el agente de replicación deja de estar programado, con el formato AAAAMMDD. *active_end_date* es de **tipo int**, con un valor predeterminado de 99991231, que significa el 31 de diciembre de 9999.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @description = ] 'description'` Es una descripción de texto del suscriptor. la *Descripción* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @security_mode = ] security_mode` Es el modo de seguridad implementado. *security_mode* es de **tipo int** y su valor predeterminado es 1. **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. **1** especifica la autenticación de Windows.  
  
> [!NOTE]  
>  Este parámetro ha quedado desusado y solo se mantiene por compatibilidad de scripts con versiones anteriores. La propiedad se especifica ahora por suscripción al ejecutar [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Cuando se especifica un valor, se utilizará como valor predeterminado al crear suscripciones en este suscriptor. Además se devolverá un mensaje de advertencia.  
  
`[ @encrypted_password = ] encrypted_password` Este parámetro ha quedado desusado y solo se proporciona para la configuración de compatibilidad con versiones anteriores *encrypted_password* en cualquier valor, pero **0** producirá un error.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al publicar desde un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addsubscriber** se utiliza en la replicación de instantáneas, la replicación transaccional y la replicación de mezcla.  
  
 **sp_addsubscriber** no es necesario cuando el suscriptor solo tendrá suscripciones anónimas a publicaciones de combinación.  
  
 **sp_addsubscriber** escribe en la tabla de [MSsubscriber_info](../../relational-databases/system-tables/mssubscriber-info-transact-sql.md) de la base de datos de **distribución** .  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_addsubscriber**.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [sp_changesubscriber &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_dropsubscriber &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)  
  
  
