---
description: sp_help_jobsteplog (Transact-SQL)
title: sp_help_jobsteplog (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobsteplog_TSQL
- sp_help_jobsteplog
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobsteplog
ms.assetid: 1a0be7b1-8f31-4b4c-aadb-586c0e00ed04
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0e5311127f4ff825f862cb3aef2317a2c53578cc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200097"
---
# <a name="sp_help_jobsteplog-transact-sql"></a>sp_help_jobsteplog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve los metadatos acerca de un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registro de pasos de trabajo del agente concreto. **sp_help_jobsteplog** no devuelve el registro real.  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_jobsteplog { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
     [ , [ @step_id = ] step_id ]  
     [ , [ @step_name = ] 'step_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] 'job_id'` Número de identificación del trabajo para el que se va a devolver información del registro de pasos de trabajo. *job_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @step_id = ] step_id` El número de identificación del paso en el trabajo. Si no se especifica, se incluirán todos los pasos del trabajo. *step_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @step_name = ] 'step_name'` Nombre del paso del trabajo. *step_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|Identificador único del trabajo.|  
|**job_name**|**sysname**|Nombre del trabajo.|  
|**step_id**|**int**|Identificador del paso en el trabajo. Por ejemplo, si el paso es el primer paso del trabajo, su *step_id* es 1.|  
|**step_name**|**sysname**|Nombre del paso del trabajo.|  
|**step_uid**|**uniqueidentifier**|Identificador único del paso en el trabajo (generado por el sistema).|  
|**date_created**|**datetime**|Fecha de creación del paso.|  
|**date_modified**|**datetime**|Fecha de la última modificación del paso.|  
|**log_size**|**float**|Tamaño del registro de pasos de trabajo, en megabytes (MB).|  
|**inicia**|**nvarchar(max)**|Salida del registro de pasos de trabajo.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_help_jobsteplog** está en la base de datos **msdb** .  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros de **SQLAgentUserRole** solo pueden ver los metadatos del registro de pasos de trabajo de los pasos de trabajo que poseen.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returns-job-step-log-information-for-all-steps-in-a-specific-job"></a>A. Devolver información del registro de pasos de trabajo para todos los pasos de un trabajo específico  
 En el ejemplo siguiente se devuelve toda la información del registro de pasos de trabajo del trabajo denominado `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobsteplog  
    @job_name = N'Weekly Sales Data Backup' ;  
GO  
```  
  
### <a name="b-return-job-step-log-information-about-a-specific-job-step"></a>B. Devolver información del registro de pasos de trabajo para un paso de trabajo específico  
 En el ejemplo siguiente se devuelve información del registro de pasos de trabajo para el primer paso de trabajo del trabajo denominado `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_delete_jobsteplog &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobsteplog-transact-sql.md)   
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
