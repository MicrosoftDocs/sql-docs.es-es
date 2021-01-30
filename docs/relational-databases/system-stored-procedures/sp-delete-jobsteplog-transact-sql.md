---
description: sp_delete_jobsteplog (Transact-SQL)
title: sp_delete_jobsteplog (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobsteplog
- sp_delete_jobsteplog_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobsteplog
ms.assetid: e9ef4c99-abde-4038-b6a3-a25dcbaf0958
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17cdcb1ba86e214ce707ff084a0d921b3817cbe0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211145"
---
# <a name="sp_delete_jobsteplog-transact-sql"></a>sp_delete_jobsteplog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita todos los registros de paso de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se especifican con los argumentos. Utilice este procedimiento almacenado para mantener la tabla **sysjobstepslogs** en la base de datos **msdb** .  
  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_delete_jobsteplog { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
       [ , [ @step_id = ] step_id | [ @step_name = ] 'step_name' ]  
       [ , [ @older_than = ] 'date' ]  
       [ , [ @larger_than = ] 'size_in_bytes' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] 'job_id'` Número de identificación del trabajo que contiene el registro de paso de trabajo que se va a quitar. *job_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> **Nota:** Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @step_id = ] step_id` El número de identificación del paso en el trabajo para el que se va a eliminar el registro de paso de trabajo. Si no se incluye, se eliminarán todos los registros de paso de trabajo del trabajo a menos que se especifiquen **\@ older_than** o **\@ larger_than** . *step_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @step_name = ] 'step_name'` Nombre del paso del trabajo para el que se va a eliminar el registro de paso de trabajo. *step_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> **Nota:** Se puede especificar *step_id* o *step_name* , pero no se pueden especificar ambos.  
  
`[ @older_than = ] 'date'` Fecha y hora del registro de paso de trabajo más antiguo que desea conservar. Todos los registros de paso de trabajo con fecha anterior a esta fecha y hora se quitarán. *Date* es de **tipo DateTime** y su valor predeterminado es NULL. Se pueden especificar tanto **\@ older_than** como **\@ larger_than** .  
  
`[ @larger_than = ] 'size_in_bytes'` El tamaño en bytes del registro de pasos de trabajo más grande que desea conservar. Todos los registros de paso de trabajo con un tamaño superior a este se quitan. Se pueden especificar tanto **\@ larger_than** como **\@ older_than** .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_delete_jobsteplog** está en la base de datos **msdb** .  
  
 Si no se especifican argumentos excepto **\@ job_id** o **\@ job_name** , se eliminarán todos los registros de paso de trabajo para el trabajo especificado.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo los miembros de **sysadmin** pueden eliminar un registro de pasos de trabajo que pertenece a otro usuario.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-removing-all-job-step-logs-from-a-job"></a>A. Quitar todos los registros de paso de trabajo de un trabajo  
 En este ejemplo se quitan todos los registros de paso de trabajo correspondientes al trabajo `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup';  
GO  
```  
  
### <a name="b-removing-the-job-step-log-for-a-particular-job-step"></a>B. Quitar el registro de paso de trabajo de un paso de trabajo determinado  
 En este ejemplo se quita el registro de paso de trabajo correspondiente al paso 2 del trabajo `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 2;  
GO  
```  
  
### <a name="c-removing-all-job-step-logs-based-on-age-and-size"></a>C. Quitar todos los registros de paso de trabajo en función de la antigüedad y el tamaño  
 En este ejemplo se quitan todos los registros de paso de trabajo que son anteriores a las 12 del mediodía del 25 de octubre de 2005, cuyo volumen es superior a 100 megabytes (MB), del trabajo `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @older_than = '10/25/2005 12:00:00',  
    @larger_than = 104857600;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_help_jobsteplog &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobsteplog-transact-sql.md)   
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
