---
description: sp_help_jobactivity (Transact-SQL)
title: sp_help_jobactivity (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobactivity_TSQL
- sp_help_jobactivity
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobactivity
ms.assetid: d344864f-b4d3-46b1-8933-b81dec71f511
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b399b08120b8153af01d0faab42633f5a8325142
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199652"
---
# <a name="sp_help_jobactivity-transact-sql"></a>sp_help_jobactivity (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra información acerca del estado en tiempo de ejecución de los trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_jobactivity { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @session_id = ] session_id ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` El número de identificación del trabajo. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @session_id = ] session_id` Identificador de sesión para el que se va a notificar información. *session_id* es de **tipo int** y su valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Devuelve el siguiente conjunto de resultados:  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Número de identificación de la sesión del Agente.|  
|**job_id**|**uniqueidentifier**|Identificador del trabajo.|  
|**job_name**|**sysname**|Nombre del trabajo.|  
|**run_requested_date**|**datetime**|Fecha para la que se solicitó la ejecución del trabajo.|  
|**run_requested_source**|**sysname**|Origen de la solicitud de ejecución del trabajo. Uno de los valores siguientes:<br /><br /> **1** = ejecutar según una programación<br /><br /> **2** = ejecutar en respuesta a una alerta<br /><br /> **3** = ejecutar al inicio<br /><br /> **4** = ejecutado por el usuario<br /><br /> **6** = ejecución en programación inactiva de CPU|  
|**queued_date**|**datetime**|Fecha en la que la solicitud se puso en cola. NULL si el trabajo se ejecutó directamente.|  
|**start_execution_date**|**datetime**|Fecha en la que el trabajo se asignó a un subproceso ejecutable.|  
|**last_executed_step_id**|**int**|Identificado de paso del paso de trabajo ejecutado más recientemente.|  
|**last_exectued_step_date**|**datetime**|Momento en que comenzó a ejecutarse el paso de trabajo ejecutado más recientemente.|  
|**stop_execution_date**|**datetime**|Momento en el que el trabajo dejó de ejecutarse.|  
|**next_scheduled_run_date**|**datetime**|La próxima vez que se programe la ejecución del trabajo.|  
|**job_history_id**|**int**|Identificador del historial de trabajos en la tabla del historial de trabajos.|  
|**message**|**nvarchar(1024)**|Mensaje generado durante la última ejecución del trabajo.|  
|**run_status**|**int**|Estado devuelto en la última ejecución del trabajo:<br /><br /> **0** = error<br /><br /> **1** = correcto<br /><br /> **3** = cancelado<br /><br /> **5** = estado desconocido|  
|**operator_id_emailed**|**int**|Número de Id. del operador notificado a través de correo electrónico al término del trabajo.|  
|**operator_id_netsent**|**int**|Número de ID. del operador notificado mediante **net send** al finalizar el trabajo.|  
|**operator_id_paged**|**int**|Número de Id. del operador notificado a través de buscapersonas al término del trabajo.|  
  
## <a name="remarks"></a>Observaciones  
 Este procedimiento proporciona una instantánea del estado actual de los trabajos. Los resultados devueltos representan la información disponible en el momento de procesar la solicitud.  
  
 El Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea un Id. de sesión cada vez que se inicia el servicio del Agente. El identificador de sesión se almacena en la tabla **msdb.dbo.syssesiones**.  
  
 Cuando no se proporciona ningún *session_id* , muestra información sobre la sesión más reciente.  
  
 Cuando no se proporciona ningún *job_name* o *job_id* , se muestra información de todos los trabajos.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo los miembros de **sysadmin** pueden ver la actividad de los trabajos que pertenecen a otros usuarios.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente muestra la actividad para todos los trabajos que el usuario actual está autorizado a ver.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobactivity ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
