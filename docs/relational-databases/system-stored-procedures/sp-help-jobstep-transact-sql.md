---
description: sp_help_jobstep (Transact-SQL)
title: sp_help_jobstep (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobstep_TSQL
- sp_help_jobstep
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobstep
ms.assetid: 4a13b804-45f2-4f82-987f-42d9a57dd6db
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c6a4a59a69612c37a6b6d9208e121b11ea448a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200104"
---
# <a name="sp_help_jobstep-transact-sql"></a>sp_help_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información para los pasos de un trabajo que el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza para realizar actividades automatizadas.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_jobstep { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @suffix = ] suffix ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] 'job_id'` Número de identificación del trabajo para el que se va a devolver información del trabajo. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @step_id = ] step_id` El número de identificación del paso en el trabajo. Si no se especifica, se incluirán todos los pasos del trabajo. *step_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @step_name = ] 'step_name'` Nombre del paso del trabajo. *step_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @suffix = ] suffix` Marca que indica si se anexa una descripción de texto a la columna **Flags** en la salida. el *sufijo* es de **bit** y su valor predeterminado es **0**. Si el *sufijo* es **1**, se anexa una descripción.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**step_id**|**int**|Identificador único del paso.|  
|**step_name**|**sysname**|Nombre del paso del trabajo.|  
|**subsistema**|**nvarchar(40)**|Subsistema en el que se ejecuta el comando del paso.|  
|**command**|**nvarchar(max)**|Comando que se ejecuta en el paso.|  
|**flags**|**int**|Máscara de bits que controla el comportamiento del paso.|  
|**cmdexec_success_code**|**int**|En el caso de un paso **CmdExec** , es el código de salida del proceso de un comando correcto.|  
|**on_success_action**|**tinyint**|Acción que se realiza si el paso termina correctamente:<br /><br /> **1** = salir del trabajo que informa de los trabajos correctos.<br /><br /> **2** = salir del trabajo que informa de un error.<br /><br /> **3** = ir al paso siguiente.<br /><br /> **4** = ir al paso.|  
|**on_success_step_id**|**int**|Si **on_success_action** es 4, indica el siguiente paso que se debe ejecutar.|  
|**on_fail_action**|**tinyint**|Qué hacer si el paso da error. Los valores son los mismos que **on_success_action**.|  
|**on_fail_step_id**|**int**|Si **on_fail_action** es 4, indica el siguiente paso que se debe ejecutar.|  
|**servidor**|**sysname**|Reservado.|  
|**database_name**|**sysname**|Para pasos [!INCLUDE[tsql](../../includes/tsql-md.md)], la base de datos en que se ejecuta el comando.|  
|**database_user_name**|**sysname**|Para pasos [!INCLUDE[tsql](../../includes/tsql-md.md)], el contexto de usuario de la base de datos en que se ejecuta el comando.|  
|**retry_attempts**|**int**|Número máximo de veces que se vuelve a intentar el comando (si no termina correctamente).|  
|**retry_interval**|**int**|Intervalo (en minutos) entre cada nuevo intento.|  
|**os_run_priority**|**int**|Reservado.|  
|**output_file_name**|**nvarchar(200)**|Archivo en el que se debe escribir la salida [!INCLUDE[tsql](../../includes/tsql-md.md)] del comando (solo para pasos de **PowerShell y PowerShell** ).|  
|**last_run_outcome**|**int**|Resultado del paso la última vez que se ejecutó:<br /><br /> **0** = error<br /><br /> **1** = correcto<br /><br /> **2** = reintento<br /><br /> **3** = cancelado<br /><br /> **5** = desconocido|  
|**last_run_duration**|**int**|Duración (hhmmss) del paso la última vez que se ejecutó.|  
|**last_run_retries**|**int**|Número de veces que se ha intentado el comando desde que se ejecutó el paso por última vez.|  
|**last_run_date**|**int**|Fecha en que se inició la ejecución del paso por última vez.|  
|**last_run_time**|**int**|Hora en que se inició la ejecución del paso por última vez.|  
|**proxy_id**|**int**|Proxy del paso de trabajo.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_help_jobstep** está en la base de datos **msdb** .  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros de **SQLAgentUserRole** solo pueden ver los pasos de trabajo de los trabajos que les pertenecen.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-return-information-for-all-steps-in-a-specific-job"></a>A. Devolver información de todos los pasos de un trabajo específico  
 En el ejemplo siguiente se devuelven todos los pasos del trabajo denominado `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobstep  
    @job_name = N'Weekly Sales Data Backup' ;  
GO  
```  
  
### <a name="b-return-information-about-a-specific-job-step"></a>B. Devolver información acerca de un paso de trabajo específico  
 En el ejemplo siguiente se devuelve información acerca del primer paso del trabajo denominado `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_update_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
