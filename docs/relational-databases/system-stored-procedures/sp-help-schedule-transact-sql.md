---
description: sp_help_schedule (Transact-SQL)
title: sp_help_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_schedule
- sp_help_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_schedule
ms.assetid: b2fc4ce1-0a8e-44d2-b206-7dc7b258d8c9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 24a0d594e02fad844d651933bab8d89bc9d35bb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193531"
---
# <a name="sp_help_schedule-transact-sql"></a>sp_help_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra información acerca de programaciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_schedule   
     [ @schedule_id = ] id ,  
     [ @schedule_name = ] 'schedule_name'   
     [ , [ @attached_schedules_only = ] attached_schedules_only ]  
     [ , [ @include_description = ] include_description ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @schedule_id = ] id` Identificador de la programación que se va a mostrar. *schedule_name* es de **tipo int** y no tiene ningún valor predeterminado. Se puede especificar *schedule_id* o *schedule_name* .  
  
`[ @schedule_name = ] 'schedule_name'` Nombre de la programación que se va a mostrar. *schedule_name* es de **tipo sysname** y no tiene ningún valor predeterminado. Se puede especificar *schedule_id* o *schedule_name* .  
  
`[ @attached_schedules_only = ] attached_schedules_only ]` Especifica si se van a mostrar solo las programaciones a las que está asociado un trabajo. *attached_schedules_only* es de **bit** y su valor predeterminado es **0**. Cuando *attached_schedules_only* es **0**, se muestran todas las programaciones. Cuando *attached_schedules_only* es **1**, el conjunto de resultados solo contiene las programaciones adjuntas a un trabajo.  
  
`[ @include_description = ] include_description` Especifica si se incluyen descripciones en el conjunto de resultados. *include_description* es de **bit** y su valor predeterminado es **0**. Cuando *include_description* es **0**, la columna de *schedule_description* del conjunto de resultados contiene un marcador de posición. Cuando *include_description* es **1**, la descripción de la programación se incluye en el conjunto de resultados.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Este procedimiento devuelve el siguiente conjunto de resultados:  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|Número de identificador de la programación.|  
|**schedule_uid**|**uniqueidentifier**|Identificador de la programación.|  
|**schedule_name**|**sysname**|Nombre de la programación.|  
|**enabled**|**int**|Indica si la programación está habilitada (**1**) o no (**0**).|  
|**freq_type**|**int**|Valor que indica cuándo se va a ejecutar el trabajo.<br /><br /> **1** = una vez<br /><br /> **4** = diariamente<br /><br /> **8** = semanalmente<br /><br /> **16** = mensualmente<br /><br /> **32** = mensualmente, con respecto al **freq_interval**<br /><br /> **64** = ejecutar cuando se inicia el servicio SQLServerAgent.|  
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
|**schedule_description**|**nvarchar(4000)**|Descripción de la programación en inglés (si se solicita).|  
|**job_count**|**int**|Devuelve el número de trabajos que hacen referencia a esta programación.|  
  
## <a name="remarks"></a>Observaciones  
 Cuando no se proporcionan parámetros, **sp_help_schedule** muestra información de todas las programaciones de la instancia.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros de **SQLAgentUserRole** solo pueden ver las programaciones que poseen.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-information-for-all-schedules-in-the-instance"></a>A. Mostrar información de todas las programaciones de la instancia  
 El ejemplo siguiente muestra información de todas las programaciones de la instancia.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_schedule ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-schedule"></a>B. Mostrar información de una programación específica  
 El ejemplo siguiente muestra información de la programación denominada `NightlyJobs`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)  
  
  
