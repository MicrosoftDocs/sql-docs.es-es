---
description: sp_help_alert (Transact-SQL)
title: sp_help_alert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_alert
- sp_help_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_alert
ms.assetid: 850cef4e-6348-4439-8e79-fd1bca712091
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b39195e5f1dd21fead42a05850f9b5e5e5874b52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208942"
---
# <a name="sp_help_alert-transact-sql"></a>sp_help_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Presenta información acerca de las alertas definidas en el servidor.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_alert [ [ @alert_name = ] 'alert_name' ]   
     [ , [ @order_by = ] 'order_by' ]   
     [ , [ @alert_id = ] alert_id ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @legacy_format = ] legacy_format ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @alert_name = ] 'alert_name'` El nombre de la alerta. *alert_name* es **nvarchar (128)**. Si no se especifica *alert_name* , se devuelve información sobre todas las alertas.  
  
`[ @order_by = ] 'order_by'` El criterio de ordenación que se va a usar para generar los resultados. *order_by* es de **tipo sysname y su** valor predeterminado es N '*Name*'.  
  
`[ @alert_id = ] alert_id` El número de identificación de la alerta de la que se va a notificar información. *alert_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @category_name = ] 'category'` La categoría de la alerta. *Category* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @legacy_format = ] legacy_format` Indica si se va a generar un conjunto de resultados heredado. *legacy_format* es de **bit** y su valor predeterminado es **0**. Cuando *legacy_format* es **1**, **sp_help_alert** devuelve el conjunto de resultados devuelto por **sp_help_alert** en Microsoft SQL Server 2000.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Cuando **\@ legacy_format** es **0**, **sp_help_alert** genera el siguiente conjunto de resultados.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificador entero único asignado por el sistema.|  
|**name**|**sysname**|Nombre de la alerta (por ejemplo, demo: registro de **msdb** completo).|  
|**event_source**|**nvarchar(100**|Origen del evento. Siempre será **MSSQLSERVER** para la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versión 7,0|  
|**event_category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**event_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**message_id**|**int**|Número del mensaje de error que define la alerta. (Normalmente se corresponde con un número de error en la tabla **sysmessages** ). Si se usa la gravedad para definir la alerta, **message_id** es **0** o null.|  
|**severity**|**int**|Nivel de gravedad (de **9** a **25**, **110**, **120**, **130** o **140**) que define la alerta.|  
|**enabled**|**tinyint**|Estado de si la alerta está habilitada (**1**) o no (**0**). Las alertas no habilitadas no se envían.|  
|**delay_between_responses**|**int**|Intervalo de espera, en segundos, entre las respuestas a la alerta.|  
|**last_occurrence_date**|**int**|Fecha de la última vez que se produjo la alerta.|  
|**last_occurrence_time**|**int**|Hora de la última vez que se produjo la alerta.|  
|**last_response_date**|**int**|Fecha de la última respuesta a la alerta por parte del servicio **SQLServerAgent** .|  
|**last_response_time**|**int**|Hora de la última respuesta a la alerta por parte del servicio **SQLServerAgent** .|  
|**notification_message**|**nvarchar(512)**|Mensaje adicional opcional enviado al operador como parte de la notificación por correo electrónico o buscapersonas.|  
|**include_event_description**|**tinyint**|Indica si la descripción del error de SQL Server del registro de aplicación de Microsoft Windows se tiene que incluir en el mensaje de notificación.|  
|**database_name**|**sysname**|Base de datos en la que debe ocurrir el error para que se desencadene la alerta. Si el nombre de la base de datos es NULL, la alerta se desencadena independientemente de dónde haya ocurrido el error.|  
|**event_description_keyword**|**nvarchar(100**|Descripción del error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el registro de aplicación Windows que debe ser similar al flujo de caracteres suministrada.|  
|**occurrence_count**|**int**|Número de veces que ha ocurrido la alerta.|  
|**count_reset_date**|**int**|Fecha en que se restableció por última vez el **occurrence_count** .|  
|**count_reset_time**|**int**|Hora en que se restableció por última vez el **occurrence_count** .|  
|**job_id**|**uniqueidentifier**|Número de identificación del trabajo que se ejecutará en respuesta a una alerta.|  
|**job_name**|**sysname**|Nombre del trabajo que se ejecutará en respuesta a una alerta.|  
|**has_notification**|**int**|Distinto de cero si la alerta se notifica a uno o varios operadores. El valor es uno de los siguientes (con OR):<br /><br /> **1**= tiene una notificación por correo electrónico<br /><br /> **2**= tiene notificación por buscapersonas<br /><br /> **4**= tiene una notificación de **net send** .|  
|**flags**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**performance_condition**|**nvarchar(512)**|Si el **tipo** es **2**, esta columna muestra la definición de la condición de rendimiento; de lo contrario, la columna es NULL.|  
|**category_name**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Siempre será '[Sin clasificar]' en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0.|  
|**wmi_namespace**|**sysname**|Si el **tipo** es **3**, esta columna muestra el espacio de nombres para el evento WMI.|  
|**wmi_query**|**nvarchar(512)**|Si el **tipo** es **3**, esta columna muestra la consulta para el evento WMI.|  
|**type**|**int**|Tipo del evento:<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerta de evento<br /><br /> **2**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerta de rendimiento<br /><br /> **3** = alerta de evento WMI|  
  
 Cuando **\@ legacy_format** es **1**, **sp_help_alert** genera el siguiente conjunto de resultados.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificador entero único asignado por el sistema.|  
|**name**|**sysname**|Nombre de la alerta (por ejemplo, demo: registro de **msdb** completo).|  
|**event_source**|**nvarchar(100**|Origen del evento. Siempre será **MSSQLSERVER** para la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versión 7,0|  
|**event_category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**event_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**message_id**|**int**|Número del mensaje de error que define la alerta. (Normalmente se corresponde con un número de error en la tabla **sysmessages** ). Si se usa la gravedad para definir la alerta, **message_id** es **0** o null.|  
|**severity**|**int**|Nivel de gravedad (de **9** a **25**, **110**, **120**, **130** o 1 **40**) que define la alerta.|  
|**enabled**|**tinyint**|Estado de si la alerta está habilitada (**1**) o no (**0**). Las alertas no habilitadas no se envían.|  
|**delay_between_responses**|**int**|Intervalo de espera, en segundos, entre las respuestas a la alerta.|  
|**last_occurrence_date**|**int**|Fecha de la última vez que se produjo la alerta.|  
|**last_occurrence_time**|**int**|Hora de la última vez que se produjo la alerta.|  
|**last_response_date**|**int**|Fecha de la última respuesta a la alerta por parte del servicio **SQLServerAgent** .|  
|**last_response_time**|**int**|Hora de la última respuesta a la alerta por parte del servicio **SQLServerAgent** .|  
|**notification_message**|**nvarchar(512)**|Mensaje adicional opcional enviado al operador como parte de la notificación por correo electrónico o buscapersonas.|  
|**include_event_description**|**tinyint**|Indica si la descripción del error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del registro de aplicación Windows se tiene que incluir en el mensaje de notificación.|  
|**database_name**|**sysname**|Base de datos en la que debe ocurrir el error para que se desencadene la alerta. Si el nombre de la base de datos es NULL, la alerta se desencadena independientemente de dónde haya ocurrido el error.|  
|**event_description_keyword**|**nvarchar(100**|Descripción del error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el registro de aplicación Windows que debe ser similar al flujo de caracteres suministrada.|  
|**occurrence_count**|**int**|Número de veces que ha ocurrido la alerta.|  
|**count_reset_date**|**int**|Fecha en que se restableció por última vez el **occurrence_count** .|  
|**count_reset_time**|**int**|Hora en que se restableció por última vez el **occurrence_count** .|  
|**job_id**|**uniqueidentifier**|Número de identificación del trabajo.|  
|**job_name**|**sysname**|Un trabajo que se ejecuta como respuesta a una alerta.|  
|**has_notification**|**int**|Distinto de cero si la alerta se notifica a uno o varios operadores. El valor es uno de los siguientes (unidos con OR):<br /><br /> **1**= tiene una notificación por correo electrónico<br /><br /> **2**= tiene notificación por buscapersonas<br /><br /> **4**= tiene una notificación de **net send** .|  
|**flags**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)].|  
|**performance_condition**|**nvarchar(512)**|Si el **tipo** es **2**, esta columna muestra la definición de la condición de rendimiento. Si el **tipo** es **3**, esta columna muestra la consulta para el evento WMI. De lo contrario, la columna es NULL.|  
|**category_name**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Siempre será '**[sin clasificar]**' para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7,0.|  
|**type**|**int**|Tipo de alerta:<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerta de evento<br /><br /> **2**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerta de rendimiento<br /><br /> **3** = alerta de evento WMI|  
  
## <a name="remarks"></a>Observaciones  
 **sp_help_alert** se debe ejecutar desde la base de datos **msdb** .  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. A otros usuarios debe concederse el rol fijo de base de datos **SQLAgentOperatorRole** en la base de datos **msdb** .  
  
 Para obtener más información sobre **SQLAgentOperatorRole**, vea [Agente SQL Server roles fijos de base de datos](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se proporciona información sobre la alerta `Demo: Sev. 25 Errors`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_alert @alert_name = 'Demo: Sev. 25 Errors';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sp_update_alert &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
