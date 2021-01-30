---
description: sp_add_job (Transact-SQL)
title: sp_add_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_job_TSQL
- sp_add_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_job
ms.assetid: 6ca8fe2c-7b1c-4b59-b4c7-e3b7485df274
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39f083d083a38e4a581c507354efaf6548471e7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206794"
---
# <a name="sp_add_job-transact-sql"></a>sp_add_job (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Agrega un nuevo trabajo ejecutado por el servicio Agente SQL.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 
 > [!IMPORTANT]  
 > En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.
 
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_job [ @job_name = ] 'job_name'  
     [ , [ @enabled = ] enabled ]   
     [ , [ @description = ] 'description' ]   
     [ , [ @start_step_id = ] step_id ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @category_id = ] category_id ]   
     [ , [ @owner_login_name = ] 'login' ]   
     [ , [ @notify_level_eventlog = ] eventlog_level ]   
     [ , [ @notify_level_email = ] email_level ]   
     [ , [ @notify_level_netsend = ] netsend_level ]   
     [ , [ @notify_level_page = ] page_level ]   
     [ , [ @notify_email_operator_name = ] 'email_name' ]   
          [ , [ @notify_netsend_operator_name = ] 'netsend_name' ]   
     [ , [ @notify_page_operator_name = ] 'page_name' ]   
     [ , [ @delete_level = ] delete_level ]   
     [ , [ @job_id = ] job_id OUTPUT ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_name = ] 'job_name'` Nombre del trabajo. El nombre debe ser único y no puede contener el carácter de porcentaje ( **%** ). *job_name* es de tipo **nvarchar (128)** y no tiene ningún valor predeterminado.  
  
`[ @enabled = ] enabled` Indica el estado del trabajo agregado. *Enabled* es de **tinyint** y su valor predeterminado es 1 (habilitado). Si es **0**, el trabajo no está habilitado y no se ejecuta según su programación; sin embargo, se puede ejecutar manualmente.  
  
`[ @description = ] 'description'` La descripción del trabajo. la *Descripción* es de tipo **nvarchar (512)** y su valor predeterminado es NULL. Si se omite la *Descripción* , se usa "no hay descripción disponible".  
  
`[ @start_step_id = ] step_id` Número de identificación del primer paso que se va a ejecutar para el trabajo. *step_id* es de **tipo int** y su valor predeterminado es 1.  
  
`[ @category_name = ] 'category'` Categoría del trabajo. *Category* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @category_id = ] category_id` Mecanismo independiente del idioma para especificar una categoría de trabajo. *category_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @owner_login_name = ] 'login'` Nombre del inicio de sesión al que pertenece el trabajo. *login* es de **tipo sysname y su** valor predeterminado es null, que se interpreta como el nombre de inicio de sesión actual. Solo los miembros del rol fijo de servidor **sysadmin** pueden establecer o cambiar el valor de **\@ owner_login_name**. Si los usuarios que no son miembros del rol **sysadmin** establecen o cambian el valor de **\@ owner_login_name**, la ejecución de este procedimiento almacenado no se realiza correctamente y se devuelve un error.  
  
`[ @notify_level_eventlog = ] eventlog_level` Valor que indica cuándo se debe colocar una entrada en el registro de aplicación de Microsoft Windows para este trabajo. *eventlog_level* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Nunca|  
|**1**|En caso de éxito|  
|**2** (predeterminado)|En caso de error|  
|**3**|Siempre|  
  
`[ @notify_level_email = ] email_level` Valor que indica cuándo se debe enviar un mensaje de correo electrónico al finalizar este trabajo. *email_level* es de **tipo int** y su valor predeterminado es **0**, que indica que nunca. *email_level* usa los mismos valores que *eventlog_level*.  
  
`[ @notify_level_netsend = ] netsend_level` Valor que indica cuándo se debe enviar un mensaje de red al finalizar este trabajo. *netsend_level* es de **tipo int** y su valor predeterminado es **0**, que indica que nunca. *netsend_level* usa los mismos valores que *eventlog_level*.  
  
`[ @notify_level_page = ] page_level` Valor que indica cuándo se debe enviar una página al finalizar este trabajo. *page_level* es de **tipo int** y su valor predeterminado es **0**, que indica que nunca. *page_level* usa los mismos valores que *eventlog_level*.  
  
`[ @notify_email_operator_name = ] 'email_name'` Nombre de correo electrónico de la persona a la que se va a enviar un correo electrónico cuando se alcance *email_level* . *email_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @notify_netsend_operator_name = ] 'netsend_name'` Nombre del operador al que se envía el mensaje de red al finalizar este trabajo. *netsend_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @notify_page_operator_name = ] 'page_name'` Nombre de la persona a la que se va a paginar al finalizar este trabajo. *PAGE_NAME* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @delete_level = ] delete_level` Valor que indica cuándo se debe eliminar el trabajo. *delete_value* es de **tipo int** y su valor predeterminado es 0, lo que significa que nunca. *delete_level* usa los mismos valores que *eventlog_level*.  
  
> [!NOTE]  
>  Cuando *delete_level* es **3**, el trabajo se ejecuta solo una vez, independientemente de las programaciones definidas para el trabajo. Además, si un trabajo se elimina a sí mismo, también se elimina todo el historial de trabajos.  
  
`[ @job_id = ] _job_idOUTPUT` El número de identificación del trabajo asignado al trabajo si se ha creado correctamente. *job_id* es una variable de salida de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **\@ originating_server** existe en **sp_add_job,** pero no aparece en argumentos. **\@ originating_server** está reservado para uso interno.  
  
 Una vez que se ha ejecutado **sp_add_job** para agregar un trabajo, **sp_add_jobstep** se puede usar para agregar pasos que realizan las actividades del trabajo. **sp_add_jobschedule** se puede utilizar para crear la programación que el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio del agente utiliza para ejecutar el trabajo. Use **sp_add_jobserver** para establecer la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instancia en la que se ejecuta el trabajo y **sp_delete_jobserver** para quitar el trabajo de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instancia.  
  
 Si el trabajo se va a ejecutar en uno o varios servidores de destino en un entorno multiservidor, utilice **sp_apply_job_to_targets** para establecer los servidores de destino o los grupos de servidores de destino para el trabajo. Para quitar trabajos de los servidores de destino o de los grupos de servidores de destino, use **sp_remove_job_from_targets**.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ofrece un método gráfico sencillo para administrar trabajos y es el método recomendado para crear y administrar la infraestructura de trabajo.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar este procedimiento almacenado, los usuarios deben ser miembros del rol fijo de servidor **sysadmin** o tener uno de los siguientes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] roles fijos de base de datos del agente, que residen en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para obtener información sobre los permisos específicos asociados a cada uno de estos roles fijos de base de datos, vea [Agente SQL Server roles fijos de base de datos](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden establecer o cambiar el valor de **\@ owner_login_name**. Si los usuarios que no son miembros del rol **sysadmin** establecen o cambian el valor de **\@ owner_login_name**, la ejecución de este procedimiento almacenado no se realiza correctamente y se devuelve un error.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-adding-a-job"></a>A. Agregar un trabajo  
 En este ejemplo se agrega un nuevo trabajo denominado `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_job  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-adding-a-job-with-pager-e-mail-and-net-send-information"></a>B. Agregar un trabajo con la información de buscapersonas, correo electrónico y net send  
 En este ejemplo se crea un trabajo denominado `Ad hoc Sales Data Backup` que notifica a `François Ajenstat` (por medio de un mensaje emergente de red, buscapersonas o correo electrónico) si se produce algún error, y se elimina el trabajo si éste se realiza correctamente.  
  
> [!NOTE]  
>  En este ejemplo se da por supuesto que ya existen un operador denominado `François Ajenstat` y un inicio de sesión denominado `françoisa`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_job  
    @job_name = N'Ad hoc Sales Data Backup',   
    @enabled = 1,  
    @description = N'Ad hoc backup of sales data',  
    @owner_login_name = N'françoisa',  
    @notify_level_eventlog = 2,  
    @notify_level_email = 2,  
    @notify_level_netsend = 2,  
    @notify_level_page = 2,  
    @notify_email_operator_name = N'François Ajenstat',  
    @notify_netsend_operator_name = N'François Ajenstat',   
    @notify_page_operator_name = N'François Ajenstat',  
    @delete_level = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_add_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_add_jobserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_apply_job_to_targets &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)   
 [sp_delete_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_delete_jobserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_remove_job_from_targets &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-remove-job-from-targets-transact-sql.md)   
 [sp_help_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_help_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [sp_update_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
