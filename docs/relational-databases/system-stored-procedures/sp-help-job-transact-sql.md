---
description: sp_help_job (Transact-SQL)
title: sp_help_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_job_TSQL
- sp_help_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_job
ms.assetid: 8a8b6104-e0e4-4d07-a2c3-f4243ee0d6fa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 32ffb85143cf448742831071bee7d4b94a25ec57
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200156"
---
# <a name="sp_help_job-transact-sql"></a>sp_help_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de los trabajos que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza para realizar actividades automatizadas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_job { [ @job_id = ] job_id  
[ @job_name = ] 'job_name' }   
     [ , [ @job_aspect = ] 'job_aspect' ]   
     [ , [ @job_type = ] 'job_type' ]   
     [ , [ @owner_login_name = ] 'login_name' ]   
     [ , [ @subsystem = ] 'subsystem' ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @enabled = ] enabled ]   
     [ , [ @execution_status = ] status ]   
     [ , [ @date_comparator = ] 'date_comparison' ]   
     [ , [ @date_created = ] date_created ]   
     [ , [ @date_last_modified = ] date_modified ]   
     [ , [ @description = ] 'description_pattern' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` El número de identificación del trabajo. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Para ver un trabajo específico, se debe especificar *job_id* o *job_name* .  Omita *job_id* y *job_name* para devolver información sobre todos los trabajos.
  
`[ @job_aspect = ] 'job_aspect'` Atributo de trabajo que se va a mostrar. *job_aspect* es de tipo **VARCHAR (9)**, su valor predeterminado es NULL y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**ALL**|Información del aspecto del trabajo|  
|**TRABAJO**|Información del trabajo|  
|**ESTR**|Información de la programación|  
|**PASOS**|Información de los pasos del trabajo|  
|**DESTINOS**|Información de los destinos|  
  
`[ @job_type = ] 'job_type'` Tipo de trabajos que se van a incluir en el informe. *job_type* es de tipo **VARCHAR (12)** y su valor predeterminado es NULL. *job_type* puede ser **local** o **multiservidor**.  
  
`[ @owner_login_name = ] 'login_name'` Nombre de inicio de sesión del propietario del trabajo. *login_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subsystem = ] 'subsystem'` Nombre del subsistema. el *subsistema* es de tipo **nvarchar (40)** y su valor predeterminado es NULL.  
  
`[ @category_name = ] 'category'` Nombre de la categoría. *Category* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @enabled = ] enabled` Un número que indica si la información se muestra para los trabajos habilitados o los trabajos deshabilitados. *Enabled* es de **tinyint** y su valor predeterminado es NULL. **1** indica los trabajos habilitados y **0** indica los trabajos deshabilitados.  
  
`[ @execution_status = ] status` El estado de ejecución de los trabajos. *status* es de **tipo int**, su valor predeterminado es NULL y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Devuelve solo los trabajos que no están inactivos o suspendidos.|  
|**1**|En ejecución.|  
|**2**|En espera de subproceso.|  
|**3**|Entre reintentos.|  
|**4**|Activos.|  
|**5**|Suspendido.|  
|**7**|Realizando acciones de finalización.|  
  
`[ @date_comparator = ] 'date_comparison'` Operador de comparación que se va a usar en las comparaciones de *Date_Created* y *date_modified*. *date_comparison* es **Char (1)** y puede ser =, \<, or > .  
  
`[ @date_created = ] date_created` Fecha en que se creó el trabajo. *Date_Created* es de **tipo DateTime** y su valor predeterminado es NULL.  
  
`[ @date_last_modified = ] date_modified` Fecha en que se modificó el trabajo por última vez. *date_modified* es de **tipo DateTime** y su valor predeterminado es NULL.  
  
`[ @description = ] 'description_pattern'` La descripción del trabajo. *description_pattern* es de tipo **nvarchar (512)** y su valor predeterminado es NULL. *description_pattern* puede incluir los caracteres comodín SQL Server para la coincidencia de patrones.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Si no se especifica ningún argumento, **sp_help_job** devuelve este conjunto de resultados.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|Id. único del trabajo.|  
|**originating_server**|**nvarchar(30)**|Nombre del servidor del que proviene el trabajo.|  
|**name**|**sysname**|Nombre del trabajo.|  
|**enabled**|**tinyint**|Indica si el trabajo está habilitado para su ejecución.|  
|**description**|**nvarchar(512)**|Descripción del trabajo.|  
|**start_step_id**|**int**|Id. del paso del trabajo en el que debe comenzar la ejecución.|  
|**category**|**sysname**|Categoría del trabajo|  
|**owner**|**sysname**|Propietario del trabajo.|  
|**notify_level_eventlog**|**int**|**Máscara** de archivo que indica en qué circunstancias se debe registrar un evento de notificación en el registro de aplicación de Microsoft Windows. Puede ser uno de estos valores:<br /><br /> **0** = nunca<br /><br /> **1** = cuando un trabajo se realiza correctamente<br /><br /> **2** = cuando se produce un error en el trabajo<br /><br /> **3** = cada vez que se completa el trabajo (independientemente del resultado del trabajo)|  
|**notify_level_email**|**int**|**Máscara de máscara** que indica en qué circunstancias se debe enviar una notificación por correo electrónico cuando se completa un trabajo. Los valores posibles son los mismos que para **notify_level_eventlog**.|  
|**notify_level_netsend**|**int**|**Máscara** de red que indica en qué circunstancias se debe enviar un mensaje de red cuando se completa un trabajo. Los valores posibles son los mismos que para **notify_level_eventlog**.|  
|**notify_level_page**|**int**|**Máscara de máscara** que indica en qué circunstancias se debe enviar una página cuando se completa un trabajo. Los valores posibles son los mismos que para **notify_level_eventlog**.|  
|**notify_email_operator**|**sysname**|Nombre de correo electrónico del operador que recibe la notificación.|  
|**notify_netsend_operator**|**sysname**|Nombre del equipo o del usuario que se utiliza al enviar mensajes de red.|  
|**notify_page_operator**|**sysname**|Nombre del equipo o del usuario que se utiliza al enviar un mensaje a un localizador.|  
|**delete_level**|**int**|**Máscara de máscara** que indica en qué circunstancias se debe eliminar el trabajo cuando se completa un trabajo. Los valores posibles son los mismos que para **notify_level_eventlog**.|  
|**date_created**|**datetime**|Fecha en que se creó el trabajo.|  
|**date_modified**|**datetime**|Fecha en que se modificó el trabajo por última vez.|  
|**version_number**|**int**|Versión del trabajo (se actualiza automáticamente cada vez que el trabajo se modifica).|  
|**last_run_date**|**int**|Fecha de inicio de la última ejecución del trabajo.|  
|**last_run_time**|**int**|Hora de inicio de la última ejecución del trabajo.|  
|**last_run_outcome**|**int**|Resultado del trabajo la última vez que se ejecutó:<br /><br /> **0** = error<br /><br /> **1** = correcto<br /><br /> **3** = cancelado<br /><br /> **5** = desconocido|  
|**next_run_date**|**int**|Fecha de la próxima ejecución programada del trabajo.|  
|**next_run_time**|**int**|Hora de la próxima ejecución programada del trabajo.|  
|**next_run_schedule_id**|**int**|Número de identificación de la próxima ejecución programada.|  
|**current_execution_status**|**int**|Estado de ejecución actual:<br /><br /> **1** = en ejecución<br /><br /> **2** = esperando subproceso<br /><br /> **3** = entre reintentos<br /><br /> **4** = inactivo<br /><br /> **5** = suspendido<br /><br /> **6** = obsoleto<br /><br /> **7** = PerformingCompletionActions|  
|**current_execution_step**|**sysname**|Paso actual de ejecución del trabajo.|  
|**current_retry_attempt**|**int**|Si el trabajo se está ejecutando y el paso se ha intentado más de una vez, es el número actual de reintentos.|  
|**has_step**|**int**|Número de pasos que tiene el trabajo.|  
|**has_schedule**|**int**|Número de programaciones que tiene el trabajo.|  
|**has_target**|**int**|Número de servidores de destino que tiene el trabajo.|  
|**type**|**int**|Tipo del trabajo.<br /><br /> 1 = Trabajo local.<br /><br /> **2** = trabajo multiservidor.<br /><br /> **0** = el trabajo no tiene ningún servidor de destino.|  
  
 Si se especifica *job_id* o *job_name* , **sp_help_job** devuelve estos conjuntos de resultados adicionales para los pasos de trabajo, las programaciones de trabajos y los servidores de destino del trabajo.  
  
 Éste es el conjunto de resultados de los pasos del trabajo.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**step_id**|**int**|Identificador único del paso (en este trabajo).|  
|**step_name**|**sysname**|Nombre del paso.|  
|**subsistema**|**nvarchar(40)**|Subsistema en el que se ejecuta el comando del paso.|  
|**command**|**nvarchar (3200)**|Comando que se ejecuta.|  
|**flags**|**nvarchar(4000)**|**Máscara de máscara** de valores que controlan el comportamiento del paso.|  
|**cmdexec_success_code**|**int**|En el caso de un paso **CmdExec** , es el código de salida del proceso de un comando correcto.|  
|**on_success_action**|**nvarchar(4000)**|Qué hacer si el paso termina correctamente:<br /><br /> **1** = salir con éxito.<br /><br /> **2** = salir con error.<br /><br /> **3** = ir al paso siguiente.<br /><br /> **4** = ir al paso.|  
|**on_success_step_id**|**int**|Si **on_success_action** es **4**, indica el siguiente paso que se debe ejecutar.|  
|**on_fail_action**|**nvarchar(4000)**|Acción que se realiza si el paso da error. Los valores son los mismos que para **on_success_action**.|  
|**on_fail_step_id**|**int**|Si **on_fail_action** es **4**, indica el siguiente paso que se debe ejecutar.|  
|**servidor**|**sysname**|Reservado.|  
|**database_name**|**sysname**|Para pasos [!INCLUDE[tsql](../../includes/tsql-md.md)], la base de datos en que se ejecutará el comando.|  
|**database_user_name**|**sysname**|Para pasos [!INCLUDE[tsql](../../includes/tsql-md.md)], el contexto de usuario de la base de datos en que se ejecuta el comando.|  
|**retry_attempts**|**int**|Número máximo de veces que se vuelve a intentar el comando (si no termina correctamente) antes de determinar que el paso ha dado error.|  
|**retry_interval**|**int**|Intervalo (en minutos) entre los reintentos.|  
|**os_run_priority**|**VARCHAR (4000)**|Reservado.|  
|**output_file_name**|**VARCHAR (200)**|Archivo en el que se debe escribir la salida [!INCLUDE[tsql](../../includes/tsql-md.md)] del comando (solo y pasos **CmdExec** ).|  
|**last_run_outcome**|**int**|Resultado del paso la última vez que se ejecutó:<br /><br /> **0** = error<br /><br /> **1** = correcto<br /><br /> **3** = cancelado<br /><br /> **5** = desconocido|  
|**last_run_duration**|**int**|Duración (en segundos) del paso la última vez que se ejecutó.|  
|**last_run_retries**|**int**|Número de veces que se ha intentado el comando desde que se ejecutó el paso por última vez.|  
|**last_run_date**|**int**|Fecha en que se inició la ejecución del paso por última vez.|  
|**last_run_time**|**int**|Hora en que se inició la ejecución del paso por última vez.|  
|**proxy_id**|**int**|Proxy del paso de trabajo.|  
  
 Éste es el conjunto de resultados de las programaciones del trabajo.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|Identificador de la programación (único para todos los trabajos).|  
|**schedule_name**|**sysname**|Nombre de la programación (único solo para este trabajo).|  
|**enabled**|**int**|Indica si la programación está activa (**1**) o no (**0**).|  
|**freq_type**|**int**|Valor que indica cuándo se va a ejecutar el trabajo:<br /><br /> **1** = una vez<br /><br /> **4** = diariamente<br /><br /> **8** = semanalmente<br /><br /> **16** = mensualmente<br /><br /> **32** = mensualmente, con respecto al **freq_interval**<br /><br /> **64** = ejecutar cuando se inicia el servicio **SQLServerAgent** .|  
|**freq_interval**|**int**|Días en que se ejecuta el trabajo. El valor depende del valor de **freq_type**. Para obtener más información, vea [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_subday_type**|**Int**|Unidades para **freq_subday_interval**. Para obtener más información, vea [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_subday_interval**|**int**|Número de períodos de **freq_subday_type** entre cada ejecución del trabajo. Para obtener más información, vea [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_relative_interval**|**int**|Repetición del trabajo programado del **freq_interval** cada mes. Para obtener más información, vea [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_recurrence_factor**|**int**|Número de meses entre las ejecuciones programadas del trabajo.|  
|**active_start_date**|**int**|Fecha en que empieza la ejecución del trabajo.|  
|**active_end_date**|**int**|Fecha en que termina la ejecución del trabajo.|  
|**active_start_time**|**int**|Hora de inicio de la ejecución del trabajo en **active_start_date.**|  
|**active_end_time**|**int**|Tiempo de finalización de la ejecución del trabajo en **active_end_date**.|  
|**date_created**|**datetime**|Fecha en que se creó la programación.|  
|**schedule_description**|**nvarchar(4000)**|Descripción de la programación en inglés (si se solicita).|  
|**next_run_date**|**int**|Fecha en que la programación hará que se vuelva a ejecutar el trabajo.|  
|**next_run_time**|**int**|Hora a la que la programación hará que se vuelva a ejecutar el trabajo.|  
|**schedule_uid**|**uniqueidentifier**|Identificador de la programación.|  
|**job_count**|**int**|Devuelve el número de trabajos que hacen referencia a esta programación.|  
  
 Éste es el conjunto de resultados de los servidores de destino del trabajo.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|Identificador del servidor de destino.|  
|**server_name**|**nvarchar(30)**|Nombre de equipo del servidor de destino.|  
|**enlist_date**|**datetime**|Fecha de alta del servidor de destino en el servidor maestro.|  
|**last_poll_date**|**datetime**|Fecha en que el servidor de destino sondeó por última vez el servidor maestro.|  
|**last_run_date**|**int**|Fecha del inicio de la última ejecución del trabajo en este servidor de destino.|  
|**last_run_time**|**int**|Hora del inicio de la última ejecución del trabajo en este servidor de destino.|  
|**last_run_duration**|**int**|Duración de la última ejecución del trabajo en este servidor de destino.|  
|**last_run_outcome**|**tinyint**|Resultado del trabajo la última vez que se ejecutó en este servidor:<br /><br /> **0** = error<br /><br /> **1** = correcto<br /><br /> **3** = cancelado<br /><br /> **5** = desconocido|  
|**last_outcome_message**|**nvarchar(1024)**|Mensaje de resultado de la última ejecución del trabajo en este servidor de destino.|  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros de **SQLAgentUserRole** solo pueden ver los trabajos que les pertenecen. Los miembros de **sysadmin**, **SQLAgentReaderRole** y **SQLAgentOperatorRole** pueden ver todos los trabajos locales y multiservidor.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-list-information-for-all-jobs"></a>A. Mostrar información de todos los trabajos  
 Este ejemplo ejecuta el procedimiento `sp_help_job` sin parámetros para obtener la información de todos los trabajos actualmente definidos en la base de datos `msdb`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_job ;  
GO  
```  
  
### <a name="b-listing-information-for-jobs-matching-a-specific-criteria"></a>B. Mostrar información de los trabajos que coinciden con un criterio específico  
 En el siguiente ejemplo se muestra información sobre el trabajo para los trabajos multiservidor que pertenecen a `françoisa`, donde el trabajo se habilita y ejecuta.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_job   
   @job_type = N'MULTI-SERVER',  
   @owner_login_name = N'françoisa',  
   @enabled = 1,  
   @execution_status = 1 ;  
GO  
```  
  
### <a name="c-listing-all-aspects-of-information-for-a-job"></a>C. Mostrar todos los aspectos de la información de un trabajo  
 En el siguiente ejemplo se muestran todos los aspectos de la información para el trabajo `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_job  
    @job_name = N'NightlyBackups',  
    @job_aspect = N'ALL' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_delete_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_update_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
