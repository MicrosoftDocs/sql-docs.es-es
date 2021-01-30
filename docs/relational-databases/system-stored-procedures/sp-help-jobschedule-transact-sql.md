---
description: sp_help_jobschedule (Transact-SQL)
title: sp_help_jobschedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobschedule
- sp_help_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobschedule
ms.assetid: 2cded902-9272-4667-ac4b-a4f95a9f008e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9765feaa9e3237f1ccd8d3a6dcc203c88758aefa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200132"
---
# <a name="sp_help_jobschedule-transact-sql"></a>sp_help_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de la programación de los trabajos que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] utiliza para realizar actividades automatizadas.  
 
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_jobschedule { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @schedule_name = ] 'schedule_name' ]  
     [ , [ @schedule_id = ] schedule_id ]  
     [ , [ @include_description = ] include_description ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` El número de identificación del trabajo. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]
> Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.

`[ @schedule_name = ] 'schedule_name'` Nombre del elemento de programación del trabajo. *schedule_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @schedule_id = ] schedule_id` Número de identificación del elemento de programación del trabajo. *schedule_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @include_description = ] include_description` Especifica si se debe incluir la descripción de la programación en el conjunto de resultados. *include_description* es de **bit** y su valor predeterminado es **0**. Cuando *include_description* es **0**, la descripción de la programación no se incluye en el conjunto de resultados. Cuando *include_description* es **1**, la descripción de la programación se incluye en el conjunto de resultados.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|Número de identificador de la programación.|  
|**schedule_name**|**sysname**|Nombre de la programación.|  
|**enabled**|**int**|Indica si la programación está habilitada (**1**) o no (**0**).|  
|**freq_type**|**int**|Valor que indica cuándo se va a ejecutar el trabajo.<br /><br /> **1** = una vez<br /><br /> **4** = diariamente<br /><br /> **8** = semanalmente<br /><br /> **16** = mensualmente<br /><br /> **32** = mensualmente, con respecto al **freq_interval**<br /><br /> **64** = ejecutar cuando se inicia el servicio **SQLServerAgent** .|  
|**freq_interval**|**int**|Días en que se ejecuta el trabajo. El valor depende del valor de **freq_type**. Para obtener más información, vea [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_subday_type**|**int**|Unidades para **freq_subday_interval**. Para obtener más información, vea [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_subday_interval**|**int**|Número de períodos de **freq_subday_type** entre cada ejecución del trabajo. Para obtener más información, vea [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_relative_interval**|**int**|Repetición del trabajo programado del **freq_interval** cada mes. Para obtener más información, vea [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_recurrence_factor**|**int**|Número de meses entre las ejecuciones programadas del trabajo.|  
|**active_start_date**|**int**|Fecha en que se activó la programación.|  
|**active_end_date**|**int**|Fecha final de la programación.|  
|**active_start_time**|**int**|Hora del día en que se inicia la programación.|  
|**active_end_time**|**int**|Hora del día en que termina la programación.|  
|**date_created**|**datetime**|Fecha en que se creó la programación.|  
|**schedule_description**|**nvarchar(4000)**|Una descripción en Inglés de la programación que se deriva de los valores de las **programaciones demsdb.dbo.sys**. Cuando *include_description* es **0**, esta columna contiene texto que indica que no se solicitó la descripción.|  
|**next_run_date**|**int**|Fecha en que la programación hará que se vuelva a ejecutar el trabajo.|  
|**next_run_time**|**int**|Hora a la que la programación hará que se vuelva a ejecutar el trabajo.|  
|**schedule_uid**|**uniqueidentifier**|Identificador de la programación.|  
|**job_count**|**int**|Recuento de trabajos devueltos.|  
  
> **Nota: sp_help_jobschedule** devuelve valores del **dbo.sysjobschedules** y **dbo.sysprograma** las tablas del sistema en **msdb**. **sysjobschedules** se actualiza cada 20 minutos. Esto puede afectar a los valores devueltos por este procedimiento almacenado.  
  
## <a name="remarks"></a>Observaciones  
 Los parámetros de **sp_help_jobschedule** solo se pueden utilizar en determinadas combinaciones. Si se especifica *schedule_id* , no se puede especificar ni *job_id* ni *job_name* . De lo contrario, los parámetros *job_id* o *job_name* se pueden usar con *schedule_name*.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** . Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros de **SQLAgentUserRole** solo pueden ver las propiedades de las programaciones de trabajos que les pertenecen.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-the-job-schedule-for-a-specific-job"></a>A. Devolver la programación de un trabajo específico  
 En el ejemplo siguiente se devuelve información de la programación de un trabajo denominado `BackupDatabase`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule  
    @job_name = N'BackupDatabase' ;  
GO  
```  
  
### <a name="b-returning-the-job-schedule-for-a-specific-schedule"></a>B. Devolver la programación de un trabajo para una programación específica  
 En el ejemplo siguiente se devuelve información de la programación denominada `NightlyJobs` y el trabajo denominado `RunReports`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule   
    @job_name = N'RunReports',  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
### <a name="c-returning-the-job-schedule-and-schedule-description-for-a-specific-schedule"></a>C. Devolver la programación de un trabajo y la descripción de una programación específica  
 En el ejemplo siguiente se devuelve información de la programación denominada `NightlyJobs` y el trabajo denominado `RunReports`. El conjunto de resultados devuelto incluye una descripción de la programación.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule  
    @job_name = N'RunReports',  
    @schedule_name = N'NightlyJobs',  
    @include_description = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_update_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
