---
description: sp_delete_job (Transact-SQL)
title: sp_delete_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_job
- sp_delete_job_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_job
ms.assetid: b85db6e4-623c-41f1-9643-07e5ea38db09
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ef2fdfc32a0b8b076037b3d75d2c88df9a5883aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186340"
---
# <a name="sp_delete_job-transact-sql"></a>sp_delete_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina un trabajo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_delete_job { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,  
     [ , [ @originating_server = ] 'server' ]   
     [ , [ @delete_history = ] delete_history ]  
     [ , [ @delete_unused_schedule = ] delete_unused_schedule ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` Es el número de identificación del trabajo que se va a eliminar. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Es el nombre del trabajo que se va a eliminar. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *job_id* o *job_name*; no se pueden especificar ambos.  
  
`[ @originating_server = ] 'server'` Para uso interno.  
  
`[ @delete_history = ] delete_history` Especifica si se debe eliminar el historial del trabajo. *delete_history* es de **bit** y su valor predeterminado es **1**. Cuando *delete_history* es **1**, se elimina el historial de trabajos del trabajo. Cuando *delete_history* es **0**, no se elimina el historial de trabajos.  
  
 Tenga en cuenta que cuando se elimina un trabajo y no se elimina el historial, la información histórica del trabajo no se mostrará en el historial de trabajos de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interfaz gráfica de usuario del agente, pero la información seguirá residiendo en la tabla **sysjobhistory** de la base de datos **msdb** .  
  
`[ @delete_unused_schedule = ] delete_unused_schedule` Especifica si se deben eliminar las programaciones adjuntas a este trabajo si no están adjuntas a ningún otro trabajo. *delete_unused_schedule* es de **bit** y su valor predeterminado es **1**. Cuando *delete_unused_schedule* es **1**, se eliminan las programaciones adjuntas a este trabajo si ningún otro trabajo hace referencia a la programación. Cuando *delete_unused_schedule* es **0**, no se eliminan las programaciones.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 El argumento **\@ originating_server** está reservado para uso interno.  
  
 El argumento **\@ delete_unused_schedule** proporciona compatibilidad con versiones anteriores de SQL Server quitando automáticamente las programaciones que no están adjuntas a ningún trabajo. Tenga en cuenta que este parámetro tiene como valor predeterminado el comportamiento compatible con versiones anteriores. Para conservar las programaciones que no están adjuntas a un trabajo, debe proporcionar el valor **0** como **\@ delete_unused_schedule** argumento.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ofrece un método gráfico sencillo para administrar trabajos y es el método recomendado para crear y administrar la infraestructura de trabajo.  
  
 Este procedimiento almacenado no puede eliminar planes de mantenimiento y tampoco puede eliminar trabajos que forman parte de planes de mantenimiento. En su lugar, utilice [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para eliminar planes de mantenimiento.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_delete_job** para eliminar cualquier trabajo. Un usuario que no sea miembro del rol fijo de servidor **sysadmin** solo puede eliminar los trabajos de los que sea propietario.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se elimina el trabajo `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_delete_job  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_help_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_update_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
