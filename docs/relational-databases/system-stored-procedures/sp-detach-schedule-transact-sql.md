---
description: sp_detach_schedule (Transact-SQL)
title: sp_detach_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_detach_schedule
- sp_detach_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_detach_schedule
ms.assetid: 9a1fc335-1bef-4638-a33a-771c54a5dd19
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 513287fadf671cc645ffa210e96f56615e237983
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200749"
---
# <a name="sp_detach_schedule-transact-sql"></a>sp_detach_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita una asociación entre una programación y un trabajo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_detach_schedule   
     { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,  
       { [ @schedule_id = ] schedule_id | [ @schedule_name = ] 'schedule_name' } ,  
     [ @delete_unused_schedule = ] delete_unused_schedule   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` Número de identificación del trabajo del que se va a quitar la programación. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo del que se va a quitar la programación. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @schedule_id = ] schedule_id` El número de identificación de la programación que se va a quitar del trabajo. *schedule_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @schedule_name = ] 'schedule_name'` Nombre de la programación que se va a quitar del trabajo. *schedule_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *schedule_id* o *schedule_name* , pero no se pueden especificar ambos.  
  
`[ @delete_unused_schedule = ] delete_unused_schedule` Especifica si se deben eliminar las programaciones de trabajos no utilizadas. *delete_unused_schedule* es de **bit** y su valor predeterminado es **0**, lo que significa que se conservarán todas las programaciones, incluso si no hay ningún trabajo que haga referencia a ellas. Si se establece en **1**, las programaciones de trabajos sin usar se eliminan si no hay ningún trabajo que haga referencia a ellas.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Observe que el propietario del trabajo puede adjuntar un trabajo a una programación y separar un trabajo de una programación sin tener que ser asimismo el propietario de la programación. Sin embargo, una programación no se puede eliminar si la separación la dejase sin trabajos, a menos que el autor de la llamada sea el propietario de la programación.  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza una comprobación para determinar si el usuario es propietario de la programación. Solo los miembros del rol fijo de servidor **sysadmin** pueden separar las programaciones de los trabajos que pertenecen a otro usuario.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se quita una asociación entre una programación `'NightlyJobs'` y un trabajo `'BackupDatabase'`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_detach_schedule  
    @job_name = 'BackupDatabase',  
    @schedule_name = 'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)  
  
  
