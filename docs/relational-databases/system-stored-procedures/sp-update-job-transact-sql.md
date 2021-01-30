---
description: sp_update_job (Transact-SQL)
title: sp_update_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_job
- sp_update_job_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_job
ms.assetid: cbdfea38-9e42-47f3-8fc8-5978b82e2623
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e559c50589681f7d00b1bd6048b1e23579213bee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184700"
---
# <a name="sp_update_job-transact-sql"></a>sp_update_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia los atributos de un trabajo.  
  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_update_job [ @job_id =] job_id | [@job_name =] 'job_name'  
     [, [@new_name =] 'new_name' ]   
     [, [@enabled =] enabled ]  
     [, [@description =] 'description' ]   
     [, [@start_step_id =] step_id ]  
     [, [@category_name =] 'category' ]   
     [, [@owner_login_name =] 'login' ]  
     [, [@notify_level_eventlog =] eventlog_level ]  
     [, [@notify_level_email =] email_level ]  
     [, [@notify_level_netsend =] netsend_level ]  
     [, [@notify_level_page =] page_level ]  
     [, [@notify_email_operator_name =] 'operator_name' ]  
     [, [@notify_netsend_operator_name =] 'netsend_operator' ]  
     [, [@notify_page_operator_name =] 'page_operator' ]  
     [, [@delete_level =] delete_level ]   
     [, [@automatic_post =] automatic_post ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` Número de identificación del trabajo que se va a actualizar. *job_id* es de tipo **uniqueidentifier**.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo. *job_name* es **nvarchar (128)**.  
  
> **Nota:** Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @new_name = ] 'new_name'` Nuevo nombre del trabajo. *new_name* es **nvarchar (128)**.  
  
`[ @enabled = ] enabled` Especifica si el trabajo está habilitado (**1**) o no (**0**). *Enabled* es **tinyint**.  
  
`[ @description = ] 'description'` La descripción del trabajo. la *Descripción* es **nvarchar (512)**.  
  
`[ @start_step_id = ] step_id` Número de identificación del primer paso que se va a ejecutar para el trabajo. *step_id* es de **tipo int**.  
  
`[ @category_name = ] 'category'` La categoría del trabajo. la *categoría* es **nvarchar (128)**.  
  
`[ @owner_login_name = ] 'login'` Nombre del inicio de sesión al que pertenece el trabajo. *login* es **nvarchar (128)** solo los miembros del rol fijo de servidor **sysadmin** pueden cambiar la propiedad del trabajo.  
  
`[ @notify_level_eventlog = ] eventlog_level` Especifica cuándo se debe colocar una entrada en el registro de aplicación de Microsoft Windows para este trabajo. *eventlog_level* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción (acción)|  
|-----------|----------------------------|  
|**0**|Nunca|  
|**1**|En caso de éxito|  
|**2**|En caso de error|  
|**3**|Siempre|  
  
`[ @notify_level_email = ] email_level` Especifica cuándo se debe enviar un mensaje de correo electrónico al finalizar este trabajo. *email_level* es de **tipo int**. *email_level* usa los mismos valores que *eventlog_level*.  
  
`[ @notify_level_netsend = ] netsend_level` Especifica cuándo se debe enviar un mensaje de red al finalizar este trabajo. *netsend_level* es de **tipo int**. *netsend_level* usa los mismos valores que *eventlog_level*.  
  
`[ @notify_level_page = ] page_level` Especifica cuándo se debe enviar una página al finalizar este trabajo. *page_level* es de **tipo int**. *page_level* usa los mismos valores que *eventlog_level*.  
  
`[ @notify_email_operator_name = ] 'operator_name'` Nombre del operador al que se envía el correo electrónico cuando se alcanza *email_level* . *email_name* es **nvarchar (128)**.  
  
`[ @notify_netsend_operator_name = ] 'netsend_operator'` Nombre del operador al que se envía el mensaje de red. *netsend_operator* es **nvarchar (128)**.  
  
`[ @notify_page_operator_name = ] 'page_operator'` Nombre del operador al que se envía una página. *page_operator* es **nvarchar (128)**.  
  
`[ @delete_level = ] delete_level` Especifica cuándo se debe eliminar el trabajo. *delete_value* es de **tipo int**. *delete_level* usa los mismos valores que *eventlog_level*.  
  
`[ @automatic_post = ] automatic_post` Sector.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_update_job** se debe ejecutar desde la base de datos **msdb** .  
  
 **sp_update_job** solo cambia la configuración para la que se proporcionan los valores de parámetro. Si se omite un parámetro, se conserva la configuración actual.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo los miembros de **sysadmin** pueden utilizar este procedimiento almacenado para editar los atributos de los trabajos que pertenecen a otros usuarios.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se cambia el nombre, la descripción y el estado de habilitación del trabajo `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_job  
    @job_name = N'NightlyBackups',  
    @new_name = N'NightlyBackups -- Disabled',  
    @description = N'Nightly backups disabled during server migration.',  
    @enabled = 0 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_delete_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_help_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
