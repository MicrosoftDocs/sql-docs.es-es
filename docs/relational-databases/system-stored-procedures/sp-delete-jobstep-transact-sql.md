---
description: sp_delete_jobstep (Transact-SQL)
title: sp_delete_jobstep (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobstep
- sp_delete_jobstep_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobstep
ms.assetid: 421ede8e-ad57-474a-9fb9-92f70a3e77e3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9b795b6526941518e476390dba158f56e18040da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186332"
---
# <a name="sp_delete_jobstep-transact-sql"></a>sp_delete_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita un paso de un trabajo.  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_delete_jobstep { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,   
     [ @step_id = ] step_id   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` El número de identificación del trabajo del que se quitará el paso. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo del que se quitará el paso. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> **Nota:** Se debe especificar *job_id* o *job_name* ; no se pueden especificar ambos.  
  
`[ @step_id = ] step_id` Número de identificación del paso que se va a quitar. *step_id* es de **tipo int** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Si se quita un paso de un trabajo, se actualizan automáticamente los otros pasos del trabajo que hacen referencia al paso eliminado.  
  
 Para obtener más información acerca de los pasos asociados a un trabajo determinado, ejecute **sp_help_jobstep**.  
  
> **Nota:** La llamada a **sp_delete_jobstep** con un valor *step_id* de cero elimina todos los pasos del trabajo.  
  
 Microsoft SQL Server Management Studio ofrece un método gráfico sencillo para administrar trabajos y es el método recomendado para crear y administrar la infraestructura de trabajo.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo los miembros de **sysadmin** pueden eliminar un paso de trabajo que pertenece a otro usuario.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se quita el paso de trabajo `1` del trabajo `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Ver o modificar trabajos](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_add_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_update_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
