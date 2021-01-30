---
description: sp_changepublication_snapshot (Transact-SQL)
title: sp_changepublication_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changepublication_snapshot_TSQL
- sp_changepublication_snapshot
helpviewer_keywords:
- sp_changepublication_snapshot
ms.assetid: 518a4618-3592-4edc-8425-cbc33cdff891
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e7dadc4e406c11ba93a5428a6dfc958ed96a36a5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211938"
---
# <a name="sp_changepublication_snapshot-transact-sql"></a>sp_changepublication_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cambia las propiedades del Agente de instantáneas para la publicación especificada. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
> [!IMPORTANT]  
>  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_changepublication_snapshot [ @publication= ] 'publication'  
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
    [ , [ @snapshot_job_name = ] 'snapshot_agent_name' ]  
    [ , [ @publisher_security_mode = ] publisher_security_mode ]  
    [ , [ @publisher_login = ] 'publisher_login' ]  
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]  
    [ , [ @job_password = ] 'job_password' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @frequency_type = ] frequency_type` Es la frecuencia con la que se programa el agente. *frequency_type* es de **tipo int** y puede tener uno de los valores siguientes.  
  
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
  
`[ @frequency_interval = ] frequency_interval` Especifica los días en los que se ejecuta el agente. *frequency_interval* es de **tipo int** y puede tener uno de los valores siguientes.  
  
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
  
`[ @frequency_subday = ] frequency_subday` Son las unidades de *freq_subday_interval*. *frequency_subday* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**2**|Segundo|  
|**4**|Minute|  
|**8**|Hora|  
|NULL (predeterminado)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es el intervalo de *frequency_subday*. *frequency_subday_interval* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la fecha en la que se ejecuta el Agente de instantáneas. *frequency_relative_interval* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en la que se programa el Agente de instantáneas por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que el Agente de instantáneas deja de estar programado, con el formato AAAAMMDD. *active_end_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día a la que se programa el Agente de instantáneas por primera vez, con el formato HHMMSS. *active_start_time_of_day* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día en que el Agente de instantáneas deja de estar programado, con el formato HHMMSS. *active_end_time_of_day* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` Es el nombre de un nombre de trabajo de Agente de instantáneas existente si se está utilizando un trabajo existente. *snapshot_agent_name* es de tipo **nvarchar (100)** y su valor predeterminado es NULL.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Es el modo de seguridad que utiliza el agente al conectarse al publicador. *publisher_security_mode* es de **smallint** y su valor predeterminado es NULL. **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación y **1** especifica la autenticación de Windows. Se debe especificar un valor de **0** para los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Es el inicio de sesión que se usa al conectarse al publicador. *publisher_login* es de **tipo sysname y su** valor predeterminado es NULL. se debe especificar *publisher_login* cuando *publisher_security_mode* es **0**. Si *publisher_login* es NULL y *publisher_security_mode* es **1**, se utilizará la cuenta de Windows especificada en *job_login* al conectarse al publicador.  
  
`[ @publisher_password = ] 'publisher_password'` Es la contraseña que se usa al conectarse al publicador. *publisher_password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  No utilice una contraseña en blanco. Utilice una contraseña segura. Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @job_login = ] 'job_login'` Es el inicio de sesión de la cuenta de Windows con la que se ejecuta el agente. *job_login* es de tipo **nvarchar (257)** y su valor predeterminado es NULL. Esta cuenta de Windows siempre se utiliza para conexiones del agente con el distribuidor. Es preciso proporcionar este parámetro al crear un nuevo trabajo del Agente de instantáneas. No se puede cambiar para un publicador que no sea de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @job_password = ] 'job_password'` Es la contraseña de la cuenta de Windows con la que se ejecuta el agente. *job_password* es de **tipo sysname y su** valor predeterminado es NULL. Es preciso proporcionar este parámetro al crear un nuevo trabajo del Agente de instantáneas.  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al crear un agente de instantáneas en un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_changepublication_snapshot** se utiliza en la replicación de instantáneas, la replicación transaccional y la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_changepublication_snapshot**.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar propiedades de publicación](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Cambiar las propiedades de la publicación y de los artículos](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addpublication_snapshot &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
