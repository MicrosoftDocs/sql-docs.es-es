---
description: sp_update_jobstep (Transact-SQL)
title: sp_update_jobstep (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_jobstep
- sp_update_jobstep_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_jobstep
ms.assetid: e158802c-c347-4a5d-bf75-c03e5ae56e6b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c102cee625bc81611bd67209306d6bc5f9398660
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183574"
---
# <a name="sp_update_jobstep-transact-sql"></a>sp_update_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia la configuración de un paso de un trabajo que se utiliza para realizar actividades automatizadas.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_update_jobstep   
     {   [@job_id =] job_id   
       | [@job_name =] 'job_name' } ,  
     [@step_id =] step_id  
     [ , [@step_name =] 'step_name' ]  
     [ , [@subsystem =] 'subsystem' ]   
     [ , [@command =] 'command' ]  
     [ , [@additional_parameters =] 'parameters' ]  
     [ , [@cmdexec_success_code =] success_code ]  
     [ , [@on_success_action =] success_action ]   
     [ , [@on_success_step_id =] success_step_id ]  
     [ , [@on_fail_action =] fail_action ]   
     [ , [@on_fail_step_id =] fail_step_id ]  
     [ , [@server =] 'server' ]   
     [ , [@database_name =] 'database' ]  
     [ , [@database_user_name =] 'user' ]   
     [ , [@retry_attempts =] retry_attempts ]  
     [ , [@retry_interval =] retry_interval ]   
     [ , [@os_run_priority =] run_priority ]  
     [ , [@output_file_name =] 'file_name' ]   
     [ , [@flags =] flags ]  
     [ ,  {   [ @proxy_id = ] proxy_id   
            | [ @proxy_name = ] 'proxy_name' }   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` Número de identificación del trabajo al que pertenece el paso. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL. Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo al que pertenece el paso. *job_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @step_id = ] step_id` Número de identificación del paso de trabajo que se va a modificar. Este número no puede modificarse. *step_id* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @step_name = ] 'step_name'` Es un nuevo nombre para el paso. *step_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subsystem = ] 'subsystem'` Subsistema utilizado por Microsoft SQL Server agente para ejecutar el *comando*. el *subsistema* es de tipo **nvarchar (40)** y su valor predeterminado es NULL.  
  
`[ @command = ] 'command'` Los comandos que se van a ejecutar a través *del subsistema*. el *comando* es de tipo **nvarchar (Max)** y su valor predeterminado es NULL.  
  
`[ @additional_parameters = ] 'parameters'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @cmdexec_success_code = ] success_code` El valor devuelto por un comando del subsistema **CmdExec** para indicar que el *comando* se ejecutó correctamente. *success_code* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @on_success_action = ] success_action` Acción que se va a realizar si el paso se realiza correctamente. *success_action* es de **tinyint**, su valor predeterminado es NULL y puede tener uno de estos valores.  
  
|Value|Descripción (acción)|  
|-----------|----------------------------|  
|**1**|Salir correctamente.|  
|**2**|Salir con error.|  
|**3**|Ir al paso siguiente.|  
|**4**|Vaya al paso *success_step_id.*|  
  
`[ @on_success_step_id = ] success_step_id` Número de identificación del paso de este trabajo que se va a ejecutar si el paso se realiza correctamente y *success_action* es **4**. *success_step_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @on_fail_action = ] fail_action` Acción que se va a realizar si se produce un error en el paso. *fail_action* es de **tinyint**, su valor predeterminado es NULL y puede tener uno de estos valores.  
  
|Value|Descripción (acción)|  
|-----------|----------------------------|  
|**1**|Salir correctamente.|  
|**2**|Salir con error.|  
|**3**|Ir al paso siguiente.|  
|**4**|Vaya al paso *fail_step_id * *.*|  
  
`[ @on_fail_step_id = ] fail_step_id` Número de identificación del paso de este trabajo que se va a ejecutar si se produce un error en el paso y *fail_action* es **4**. *fail_step_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @server = ] 'server'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]el *servidor* es de tipo **nvarchar (128)** y su valor predeterminado es NULL.  
  
`[ @database_name = ] 'database'` Nombre de la base de datos en la que se va a ejecutar un [!INCLUDE[tsql](../../includes/tsql-md.md)] paso. *Database* es de **tipo sysname**. No se permiten nombres incluidos entre corchetes ([ ]). El valor predeterminado es NULL.  
  
`[ @database_user_name = ] 'user'` Nombre de la cuenta de usuario que se va a usar al ejecutar un [!INCLUDE[tsql](../../includes/tsql-md.md)] paso. *User* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @retry_attempts = ] retry_attempts` Número de reintentos que se deben usar si se produce un error en este paso. *retry_attempts* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @retry_interval = ] retry_interval` Cantidad de tiempo en minutos entre reintentos. *retry_interval* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @os_run_priority = ] run_priority` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @output_file_name = ] 'file_name'` Nombre del archivo en el que se guarda la salida de este paso. *file_name* es de tipo **nvarchar (200)** y su valor predeterminado es NULL. Este parámetro solo es válido con comandos que se ejecutan en subsistemas [!INCLUDE[tsql](../../includes/tsql-md.md)] o CmdExec.  
  
 Para volver a establecer output_file_name en NULL, debe establecer *output_file_name* en una cadena vacía (' ') o en una cadena de caracteres en blanco, pero no puede usar la función **Char (32)** . Por ejemplo, establezca este argumento en una cadena vacía del modo siguiente:  
  
 **@output_file_name = ' '**  
  
`[ @flags = ] flags` Opción que controla el comportamiento. *Flags* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0** (valor predeterminado)|Sobrescribir el archivo de salida.|  
|**2**|Anexar al archivo de salida|  
|**4**|Escribir la salida del paso de trabajo Transact-SQL en el historial de pasos|  
|**8**|Escribir el registro en la tabla (sobrescribir el historial existente)|  
|**16**|Escribir el registro en la tabla (anexar al historial existente)|  
  
`[ @proxy_id = ] proxy_id` El número de identificación del proxy con el que se ejecuta el paso de trabajo. *proxy_id* es de tipo **int** y su valor predeterminado es NULL. Si no se especifica ningún *proxy_id* , no se especifica ningún *proxy_name* y no se especifica ningún *user_name* , el paso de trabajo se ejecuta como la cuenta de servicio para el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente.  
  
`[ @proxy_name = ] 'proxy_name'` Nombre del proxy con el que se ejecuta el paso de trabajo. *proxy_name* es de tipo **sysname y su** valor predeterminado es NULL. Si no se especifica ningún *proxy_id* , no se especifica ningún *proxy_name* y no se especifica ningún *user_name* , el paso de trabajo se ejecuta como la cuenta de servicio para el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_update_jobstep** se debe ejecutar desde la base de datos **msdb** .  
  
 La actualización de un paso de trabajo incrementa el número de la versión del trabajo.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo los miembros de **sysadmin** pueden actualizar un paso de trabajo que pertenece a otro usuario.  
  
 Si el paso de trabajo requiere acceso a un proxy, el creador del paso de trabajo debe tener acceso al proxy del paso de trabajo. Todos los subsistemas, excepto Transact-SQL, necesitan una cuenta de proxy. Los miembros de **sysadmin** tienen acceso a todos los servidores proxy y pueden utilizar la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta de servicio del agente para el proxy.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se modifica el número de reintentos para el primer paso del trabajo `Weekly Sales Data Backup`. Después de ejecutar este ejemplo, el número de reintentos es `10`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1,  
    @retry_attempts = 10 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Ver o modificar trabajos](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_delete_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
