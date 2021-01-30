---
description: sp_help_jobs_in_schedule (Transact-SQL)
title: sp_help_jobs_in_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobs_in_schedule_TSQL
- sp_help_jobs_in_schedule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobs_in_schedule
ms.assetid: 1168aa2c-136b-4ba3-b18e-9070d95a26fa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1890fc4b8aa3757b55dfc5ad7e33070126300897
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200144"
---
# <a name="sp_help_jobs_in_schedule-transact-sql"></a>sp_help_jobs_in_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información sobre los trabajos a los que está adjunta una programación concreta.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_jobs_in_schedule   
     [ @schedule_name = ] 'schedule_name' ,  
     [ @schedule_id = ] schedule_id   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @schedule_id = ] schedule_id` Identificador de la programación de la que se va a mostrar información. *schedule_id* es de **tipo int** y no tiene ningún valor predeterminado. Se puede especificar *schedule_id* o *schedule_name* .  
  
`[ @schedule_name = ] 'schedule_name'` Nombre de la programación para la que se va a mostrar información. *schedule_name* es de **tipo sysname** y no tiene ningún valor predeterminado. Se puede especificar *schedule_id* o *schedule_name* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Devuelve el siguiente conjunto de resultados:  
  
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
|**notify_level_eventlog**|**int**|Máscara de bits que indica en qué circunstancias se debe registrar un evento de notificación en el registro de aplicación de Microsoft Windows. Puede ser uno de estos valores:<br /><br /> **0** = nunca<br /><br /> **1** = cuando un trabajo se realiza correctamente<br /><br /> **2** = cuando se produce un error en el trabajo<br /><br /> **3** = cada vez que se completa el trabajo (independientemente del resultado del trabajo)|  
|**notify_level_email**|**int**|Máscara de bits que indica en qué circunstancias se debe enviar una notificación por correo electrónico cuando se completa un trabajo. Los valores posibles son los mismos que para **notify_level_eventlog**.|  
|**notify_level_netsend**|**int**|Máscara de bits que indica en qué circunstancias se debe enviar un mensaje de red cuando se completa un trabajo. Los valores posibles son los mismos que para **notify_level_eventlog**.|  
|**notify_level_page**|**int**|Máscara de bits que indica en qué circunstancias se debe enviar un mensaje a un localizador cuando se completa un trabajo. Los valores posibles son los mismos que para **notify_level_eventlog**.|  
|**notify_email_operator**|**sysname**|Nombre de correo electrónico del operador que recibe la notificación.|  
|**notify_netsend_operator**|**sysname**|Nombre del equipo o del usuario que se utiliza al enviar mensajes de red.|  
|**notify_page_operator**|**sysname**|Nombre del equipo o del usuario que se utiliza al enviar un mensaje a un localizador.|  
|**delete_level**|**int**|Máscara de bits que indica en qué circunstancias se debe eliminar un trabajo cuando se completa. Los valores posibles son los mismos que para **notify_level_eventlog**.|  
|**date_created**|**datetime**|Fecha en que se creó el trabajo.|  
|**date_modified**|**datetime**|Fecha en que se modificó el trabajo por última vez.|  
|**version_number**|**int**|Versión del trabajo (se actualiza automáticamente cada vez que el trabajo se modifica).|  
|**last_run_date**|**int**|Fecha de inicio de la última ejecución del trabajo.|  
|**last_run_time**|**int**|Hora de inicio de la última ejecución del trabajo.|  
|**last_run_outcome**|**int**|Resultado del trabajo la última vez que se ejecutó:<br /><br /> **0** = error<br /><br /> **1** = correcto<br /><br /> **3** = cancelado<br /><br /> **5** = desconocido|  
|**next_run_date**|**int**|Fecha de la próxima ejecución programada del trabajo.|  
|**next_run_time**|**int**|Hora de la próxima ejecución programada del trabajo.|  
|**next_run_schedule_id**|**int**|Número de identificación de la próxima ejecución programada.|  
|**current_execution_status**|**int**|Estado de ejecución actual.|  
|**current_execution_step**|**sysname**|Paso actual de ejecución del trabajo.|  
|**current_retry_attempt**|**int**|Si el trabajo se está ejecutando y el paso se ha intentado más de una vez, es el número actual de reintentos.|  
|**has_step**|**int**|Número de pasos que tiene el trabajo.|  
|**has_schedule**|**int**|Número de programaciones que tiene el trabajo.|  
|**has_target**|**int**|Número de servidores de destino que tiene el trabajo.|  
|**type**|**int**|Tipo de trabajo:<br /><br /> **1** = trabajo local.<br /><br /> **2** = trabajo multiservidor.<br /><br /> **0** = el trabajo no tiene ningún servidor de destino.|  
  
## <a name="remarks"></a>Observaciones  
 Este procedimiento muestra información acerca de los trabajos adjuntos a la programación especificada.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros de **SQLAgentUserRole** solo pueden ver el estado de los trabajos que les pertenecen.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestran los trabajos adjuntos a la programación `NightlyJobs`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_jobs_in_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)  
  
  
