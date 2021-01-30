---
description: sp_stop_job (Transact-SQL)
title: sp_stop_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_stop_job_TSQL
- sp_stop_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_stop_job
ms.assetid: 64b4cc75-99a0-421e-b418-94e37595bbb0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b7f4c7010ae6b00f083e54bcd3b6ec152d5d66ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159788"
---
# <a name="sp_stop_job-transact-sql"></a>sp_stop_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Indica al Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que detenga la ejecución de un trabajo.  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_stop_job   
      [@job_name =] 'job_name'  
    | [@job_id =] job_id   
    | [@originating_server =] 'master_server'  
    | [@server_name =] 'target_server'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_name = ] 'job_name'` Nombre del trabajo que se va a detener. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @job_id = ] job_id` Número de identificación del trabajo que se va a detener. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @originating_server = ] 'master_server'` Nombre del servidor maestro. Si se especifica, se detienen todos los trabajos multiservidor. *master_server* es de tipo **nvarchar (128)** y su valor predeterminado es NULL. Especifique este parámetro solo cuando llame a **sp_stop_job** en un servidor de destino.  
  
> [!NOTE]  
>  Solo se puede especificar uno de los tres primeros parámetros.  
  
`[ @server_name = ] 'target_server'` Nombre del servidor de destino específico en el que se va a detener un trabajo multiservidor. *target_server* es de tipo **nvarchar (128)** y su valor predeterminado es NULL. Especifique este parámetro solo cuando llame a **sp_stop_job** en un servidor maestro para un trabajo multiservidor.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_stop_job** envía una señal de detención a la base de datos. Algunos procesos se pueden detener inmediatamente y otros deben llegar a un punto estable (o un punto de entrada a la ruta de acceso del código) antes de que se puedan detener. Algunas instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] de ejecución prolongada, como BACKUP, RESTORE y algunos comandos DBCC, pueden tardar mucho tiempo en finalizar. Cuando se están ejecutando, puede tardar un tiempo antes de que se cancele el trabajo. Si se detiene un trabajo, se registrará una entrada de trabajo cancelado en el historial de trabajos.  
  
 Si un trabajo está ejecutando un paso de tipo **CmdExec** o **PowerShell**, se forzará la finalización prematura del proceso que se está ejecutando (por ejemplo, MyProgram.exe). La finalización prematura puede provocar un comportamiento imprevisible como, por ejemplo, que los archivos que el proceso utiliza se mantengan abiertos. Por consiguiente, **sp_stop_job** debe usarse solo en circunstancias extremas Si el trabajo contiene pasos de tipo **CmdExec** o **PowerShell**.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros de **SQLAgentUserRole** y **SQLAgentReaderRole** solo pueden detener los trabajos que les pertenecen. Los miembros de **SQLAgentOperatorRole** pueden detener todos los trabajos locales, incluidos los que pertenecen a otros usuarios. Los miembros de **sysadmin** pueden detener todos los trabajos locales y multiservidor.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se detiene un trabajo denominado `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_stop_job  
    N'Weekly Sales Data Backup' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_delete_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_help_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_start_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)   
 [sp_update_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
