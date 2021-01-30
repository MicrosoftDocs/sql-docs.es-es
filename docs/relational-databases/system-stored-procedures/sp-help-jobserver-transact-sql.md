---
description: sp_help_jobserver (Transact-SQL)
title: sp_help_jobserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobserver
- sp_help_jobserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobserver
ms.assetid: 57971787-f9f5-4199-9f64-c2b61a308906
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4c5e5f8a4c7ec027c72452ea725ec6def715309a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200113"
---
# <a name="sp_help_jobserver-transact-sql"></a>sp_help_jobserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca del servidor para un trabajo dado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_jobserver  
     { [ @job_id = ] job_id   
     | [ @job_name = ] 'job_name' }  
     [ , [ @show_last_run_details = ] show_last_run_details ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` Número de identificación del trabajo del que se va a devolver información. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo del que se va a devolver información. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @show_last_run_details = ] show_last_run_details` Indica si la información de ejecución de la última ejecución forma parte del conjunto de resultados. *show_last_run_details* es de **tinyint** y su valor predeterminado es **0**. **0** no incluye información de última ejecución y **1** sí.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|Número de identificación del servidor de destino.|  
|**server_name**|**nvarchar(30)**|Nombre de equipo del servidor de destino.|  
|**enlist_date**|**datetime**|Fecha de alta del servidor de destino en el servidor maestro.|  
|**last_poll_date**|**datetime**|Fecha en que el servidor de destino sondeó por última vez el servidor maestro.|  
  
 Si **sp_help_jobserver** se ejecuta con *show_last_run_details* establecido en **1**, el conjunto de resultados tiene estas columnas adicionales.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**last_run_date**|**int**|Fecha del inicio de la última ejecución del trabajo en este servidor de destino.|  
|**last_run_time**|**int**|Hora del inicio de la última ejecución del trabajo en este servidor de destino|  
|**last_run_duration**|**int**|Duración del trabajo en su última ejecución en este servidor de destino (en segundos)|  
|**last_outcome_message**|**nvarchar(1024)**|Describe el último resultado del trabajo.|  
|**last_run_outcome**|**int**|Resultado del trabajo la última vez que se ejecutó en este servidor:<br /><br /> **0** = error<br /><br /> **1** = correcto<br /><br /> **3** = cancelado<br /><br /> **5** = desconocido|  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros de **SQLAgentUserRole** solo pueden ver información de los trabajos que les pertenecen.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve información acerca del trabajo `NightlyBackups`, incluida la información sobre la última ejecución.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobserver  
    @job_name = N'NightlyBackups',  
    @show_last_run_details = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_jobserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_jobserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
