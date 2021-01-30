---
description: sp_attach_schedule (Transact-SQL)
title: sp_attach_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_attach_schedule_TSQL
- sp_attach_schedule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_attach_schedule
ms.assetid: 80c80eaf-cf23-4ed8-b8dd-65fe59830dd1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 050436b47193c13fe36214d45bb6bd12749a5eb0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199235"
---
# <a name="sp_attach_schedule-transact-sql"></a>sp_attach_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Configura una programación para un trabajo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_attach_schedule  
     { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,   
     { [ @schedule_id = ] schedule_id   
     | [ @schedule_name = ] 'schedule_name' }  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` Número de identificación del trabajo al que se agrega la programación. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo al que se agrega la programación. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @schedule_id = ] schedule_id` El número de identificación de la programación que se va a establecer para el trabajo. *schedule_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @schedule_name = ] 'schedule_name'` Nombre de la programación que se va a establecer para el trabajo. *schedule_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *schedule_id* o *schedule_name* , pero no se pueden especificar ambos.  
  
## <a name="remarks"></a>Observaciones  
 La programación y el trabajo deben tener el mismo propietario.  
  
 Se puede configurar una programación para más de un trabajo. Se puede ejecutar un trabajo en más de una programación.  
  
 Este procedimiento almacenado se debe ejecutar desde la base de datos **msdb** .  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Observe que el propietario del trabajo puede adjuntar un trabajo a una programación y separar un trabajo de una programación sin tener que ser asimismo el propietario de la programación. Sin embargo, no se puede eliminar una programación si la desasociación la dejaría sin trabajos, a menos que el autor de la llamada sea el propietario de la programación.  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprueba si el usuario es propietario del trabajo y la programación.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se crea una programación llamada `NightlyJobs`. Los trabajos que usan esta programación se ejecutan a diario cuando la hora del servidor es `01:00`. En el ejemplo se asocia la programación a los trabajos `BackupDatabase` y `RunReports`.  
  
> [!NOTE]  
>  En este ejemplo se da por supuesto que el trabajo `BackupDatabase` y el trabajo `RunReports` ya existen.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_add_schedule  
    @schedule_name = N'NightlyJobs' ,  
    @freq_type = 4,  
    @freq_interval = 1,  
    @active_start_time = 010000 ;  
GO  
  
EXEC sp_attach_schedule  
   @job_name = N'BackupDatabase',  
   @schedule_name = N'NightlyJobs' ;  
GO  
  
EXEC sp_attach_schedule  
   @job_name = N'RunReports',  
   @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)  
  
  
