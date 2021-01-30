---
description: sp_addpublication_snapshot (Transact-SQL)
title: sp_addpublication_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpublication_snapshot_TSQL
- sp_addpublication_snapshot
helpviewer_keywords:
- sp_addpublication_snapshot
ms.assetid: 192b6214-df6e-44a3-bdd4-9d933a981619
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f7fb5fbea8a87d1bacb4a086825ecfe62b292f76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209369"
---
# <a name="sp_addpublication_snapshot-transact-sql"></a>sp_addpublication_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea el Agente de instantáneas en la publicación especificada. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
> [!IMPORTANT]  
>  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addpublication_snapshot [ @publication= ] 'publication'  
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
  
`[ @frequency_type = ] frequency_type` Es la frecuencia con la que se ejecuta el Agente de instantáneas. *frequency_type* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una vez.|  
|**4** (valor predeterminado)|Diariamente|  
|**8**|Semanalmente|  
|**16**|Mensual.|  
|**32**|Mensualmente, según el intervalo de frecuencia|  
|**64**|Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inicia el agente.|  
|**128**|Ejecutar cuando el equipo esté inactivo|  
  
`[ @frequency_interval = ] frequency_interval` Es el valor que se va a aplicar a la frecuencia establecida por *frequency_type*. *frequency_interval* es de **tipo int** y puede tener uno de los valores siguientes.  
  
|Valor de frequency_type|Efecto en frequency_interval|  
|------------------------------|-----------------------------------|  
|**1**|*frequency_interval* no se usa.|  
|**4** (valor predeterminado)|Cada *frequency_interval* días, con un valor predeterminado de Daily.|  
|**8**|*frequency_interval* es uno o más de los siguientes (combinados con un operador lógico [&#124; (OR bit a bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) :<br /><br /> **1** = Domingo &#124;<br /><br /> **2** = lunes &#124;<br /><br /> **4** = martes &#124;<br /><br /> **8** = miércoles &#124;<br /><br /> **16** = jueves &#124;<br /><br /> **32** = viernes &#124;<br /><br /> **64** = sábado|  
|**16**|En el *frequency_interval* día del mes.|  
|**32**|*frequency_interval* es uno de los siguientes:<br /><br /> **1** = Domingo &#124;<br /><br /> **2** = lunes &#124;<br /><br /> **3** = martes &#124;<br /><br /> **4** = miércoles &#124;<br /><br /> **5** = jueves &#124;<br /><br /> **6** = viernes &#124;<br /><br /> **7** = sábado &#124;<br /><br /> **8** = día &#124;<br /><br /> **9** = día de la semana &#124;<br /><br /> **10** = día del fin de semana|  
|**64**|*frequency_interval* no se usa.|  
|**128**|*frequency_interval* no se usa.|  
  
`[ @frequency_subday = ] frequency_subday` Es la unidad de *freq_subday_interval*. *frequency_subday* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**2**|Segundo|  
|**4** (valor predeterminado)|Minute|  
|**8**|Hora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es el intervalo de *frequency_subday*. *frequency_subday_interval* es de **tipo int** y su valor predeterminado es 5, lo que significa cada 5 minutos.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la fecha en la que se ejecuta el Agente de instantáneas. *frequency_relative_interval* es de **tipo int** y su valor predeterminado es 1.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es 0.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en la que se programa el Agente de instantáneas por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y su valor predeterminado es 0.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que el Agente de instantáneas deja de estar programado, con el formato AAAAMMDD. *active_end_date* es de **tipo int**, con un valor predeterminado de 99991231, que significa el 31 de diciembre de 9999.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día a la que se programa el Agente de instantáneas por primera vez, con el formato HHMMSS. *active_start_time_of_day* es de **tipo int** y su valor predeterminado es 0.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día en que el Agente de instantáneas deja de estar programado, con el formato HHMMSS. *active_end_time_of_day* es de **tipo int** y su valor predeterminado es 235959, lo que significa 11:59:59 P.M. tal y como se mide en un reloj de 24 horas.  
  
`[ @snapshot_job_name = ] 'snapshot_agent_name'` Es el nombre de un nombre de trabajo de Agente de instantáneas existente si se está utilizando un trabajo existente. *snapshot_agent_name* es de tipo **nvarchar (100)** y su valor predeterminado es NULL. Este parámetro es solo para uso interno y no se puede especificar al crear una publicación nueva. Si se especifica *snapshot_agent_name* , *job_login* y *job_password* deben ser null.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Es el modo de seguridad que utiliza el agente al conectarse al publicador. *publisher_security_mode* es de **smallint** y su valor predeterminado es 1. **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación y **1** especifica la autenticación de Windows. Se debe especificar un valor de **0** para los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Es el inicio de sesión que se usa al conectarse al publicador. *publisher_login* es de **tipo sysname y su** valor predeterminado es NULL. se debe especificar *publisher_login* cuando *publisher_security_mode* es **0**. Si *publisher_login* es NULL y *publisher_security_mode* es **1**, se utilizará la cuenta especificada en *job_login* al conectarse al publicador.  
  
`[ @publisher_password = ] 'publisher_password'` Es la contraseña que se usa al conectarse al publicador. *publisher_password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  No almacene información de autenticación en archivos de script. Para ayudar a mejorar la seguridad, se recomienda proporcionar nombres de inicio de sesión y contraseñas en tiempo de ejecución.  
  
`[ @job_login = ] 'job_login'` Es el inicio de sesión de la cuenta con la que se ejecuta el agente. En Azure SQL Instancia administrada, use una cuenta de SQL Server. *job_login* es de tipo **nvarchar (257)** y su valor predeterminado es NULL. Esta cuenta siempre se utiliza para las conexiones del agente con el distribuidor. Es preciso proporcionar este parámetro al crear un nuevo trabajo del Agente de instantáneas.  
  
> [!NOTE]
>  En el caso de los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe ser el mismo inicio de sesión especificado en [sp_adddistpublisher &#40;&#41;de TRANSACT-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md).  
  
`[ @job_password = ] 'job_password'` Es la contraseña de la cuenta de Windows con la que se ejecuta el agente. *job_password* es de **tipo sysname** y no tiene ningún valor predeterminado. Es preciso proporcionar este parámetro al crear un nuevo trabajo del Agente de instantáneas.  
  
> [!IMPORTANT]  
>  No almacene información de autenticación en archivos de script. Para ayudar a mejorar la seguridad, se recomienda proporcionar nombres de inicio de sesión y contraseñas en tiempo de ejecución.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al crear un agente de instantáneas en un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addpublication_snapshot** se utiliza en la replicación de instantáneas, la replicación transaccional y la replicación de mezcla.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addpublication-snapsh_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addpublication_snapshot**.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Crear y aplicar una instantánea](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [sp_addpublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication_snapshot &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md)   
 [sp_startpublication_snapshot &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-startpublication-snapshot-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
