---
description: sp_update_alert (Transact-SQL)
title: sp_update_alert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_alert_TSQL
- sp_update_alert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_alert
ms.assetid: 4bbaeaab-8aca-4c9e-abc1-82ce73090bd3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c9dce24d3f8dd20d2138ba153fee74212e603e6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184751"
---
# <a name="sp_update_alert-transact-sql"></a>sp_update_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza la configuración de una alerta existente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_update_alert   
     [ @name =] 'name'   
     [ , [ @new_name =] 'new_name']   
     [ , [ @enabled =] enabled]   
     [ , [ @message_id =] message_id]   
     [ , [ @severity =] severity]   
     [ , [ @delay_between_responses =] delay_between_responses]   
     [ , [ @notification_message =] 'notification_message']   
     [ , [ @include_event_description_in =] include_event_description_in]   
     [ , [ @database_name =] 'database']   
     [ , [ @event_description_keyword =] 'event_description_keyword']   
     [ , [ @job_id =] job_id | [@job_name =] 'job_name']   
     [ , [ @occurrence_count = ] occurrence_count]   
     [ , [ @count_reset_date =] count_reset_date]   
     [ , [ @count_reset_time =] count_reset_time]   
     [ , [ @last_occurrence_date =] last_occurrence_date]   
     [ , [ @last_occurrence_time =] last_occurrence_time]   
     [ , [ @last_response_date =] last_response_date]   
     [ , [ @last_response_time =] last_response _time]  
     [ , [ @raise_snmp_trap =] raise_snmp_trap]  
     [ , [ @performance_condition =] 'performance_condition' ]   
     [ , [ @category_name =] 'category']  
     [ , [ @wmi_namespace = ] 'wmi_namespace' ]  
     [ , [ @wmi_query = ] 'wmi_query' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] 'name'` Nombre de la alerta que se va a actualizar. *Name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @new_name = ] 'new_name'` Un nuevo nombre para la alerta. El nombre debe ser único. *new_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @enabled = ] enabled` Especifica si la alerta está habilitada (**1**) o no (**0**). *Enabled* es de **tinyint** y su valor predeterminado es NULL. Para poder activar una alerta, ésta debe estar habilitada.  
  
`[ @message_id = ] message_id` Un nuevo mensaje o número de error para la definición de la alerta. Normalmente, *message_id* corresponde a un número de error de la tabla **sysmessages** . *message_id* es de **tipo int** y su valor predeterminado es NULL. Solo se puede utilizar un identificador de mensaje si el valor de nivel de gravedad de la alerta es **0**.  
  
`[ @severity = ] severity` Un nuevo nivel de gravedad (de **1** a **25**) para la definición de la alerta. Cualquier [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mensaje enviado al registro de aplicación de Windows con la gravedad especificada activará la alerta. *Severity* es de **tipo int** y su valor predeterminado es NULL. Un nivel de gravedad solo se puede usar si el valor de ID. de mensaje de la alerta es **0**.  
  
`[ @delay_between_responses = ] delay_between_responses` El nuevo período de espera, en segundos, entre las respuestas a la alerta. *delay_between_responses* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @notification_message = ] 'notification_message'` Texto revisado de un mensaje adicional que se envía al operador como parte de la notificación por correo electrónico, **net send** o buscapersonas. *notification_message* es de tipo **nvarchar (512)** y su valor predeterminado es NULL.  
  
`[ @include_event_description_in = ] include_event_description_in` Especifica si la descripción del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] error del registro de aplicación de Windows debe incluirse en el mensaje de notificación. *include_event_description_in* es de **tinyint**, su valor predeterminado es NULL y puede ser uno o varios de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|None|  
|**1**|Correo electrónico|  
|**2**|Buscapersonas|  
|**4**|**net send**|  
|**7**|Todo|  
  
`[ @database_name = ] 'database'` Nombre de la base de datos en la que debe producirse el error para que se desencadene la alerta. *Database* es de **tipo sysname.** No se permiten nombres incluidos entre corchetes ([ ]). El valor predeterminado es NULL.  
  
`[ @event_description_keyword = ] 'event_description_keyword'` Secuencia de caracteres que se debe encontrar en la descripción del error en el registro de mensajes de error. Se pueden usar caracteres de coincidencia de patrón de la expresión LIKE de [!INCLUDE[tsql](../../includes/tsql-md.md)]. *event_description_keyword* es de tipo **nvarchar (100)** y su valor predeterminado es NULL. Este parámetro es útil para filtrar los nombres de objeto (por ejemplo, **% customer_table%**).  
  
`[ @job_id = ] job_id` El número de identificación del trabajo. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL. Si se especifica *job_id* , se debe omitir *job_name* .  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo que se ejecuta como respuesta a esta alerta. *job_name* es de **tipo sysname y su** valor predeterminado es NULL. Si se especifica *job_name* , se debe omitir *job_id* .  
  
`[ @occurrence_count = ] occurrence_count` Restablece el número de veces que se ha producido la alerta. *occurrence_count* es de **tipo int**, su valor predeterminado es NULL y solo se puede establecer en **0**.  
  
`[ @count_reset_date = ] count_reset_date` Restablece la fecha en que se restableció por última vez el recuento de repeticiones. *count_reset_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @count_reset_time = ] count_reset_time` Restablece la hora en que se restableció por última vez el recuento de repeticiones. *count_reset_time* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @last_occurrence_date = ] last_occurrence_date` Restablece la fecha de la última vez que se produjo la alerta. *last_occurrence_date* es de **tipo int**, su valor predeterminado es NULL y solo se puede establecer en **0**.  
  
`[ @last_occurrence_time = ] last_occurrence_time` Restablece la hora de la última vez que se produjo la alerta. *last_occurrence_time* es de **tipo int**, su valor predeterminado es NULL y solo se puede establecer en **0**.  
  
`[ @last_response_date = ] last_response_date` Restablece la fecha de la última respuesta a la alerta por parte del servicio SQLServerAgent. *last_response_date* es de **tipo int**, su valor predeterminado es NULL y solo se puede establecer en **0**.  
  
`[ @last_response_time = ] last_response_time` Restablece la hora en que el servicio SQLServerAgent respondió por última vez a la alerta. *last_response_time* es de **tipo int**, su valor predeterminado es NULL y solo se puede establecer en **0**.  
  
`[ @raise_snmp_trap = ] raise_snmp_trap` Sector.  
  
`[ @performance_condition = ] 'performance_condition'` Valor expresado en el formato **'**_itemcomparatorvalue_**'**. *performance_condition* es de tipo **nvarchar (512)**, su valor predeterminado es NULL y consta de estos elementos.  
  
|Elemento de formato|Descripción|  
|--------------------|-----------------|  
|*Item*|Objeto de rendimiento, contador de rendimiento o instancia con nombre del contador|  
|*Comparador*|Uno de estos operadores: **>** , **<** , **=**|  
|*Valor*|Valor numérico del contador|  
  
`[ @category_name = ] 'category'` El nombre de la categoría de alerta. *Category* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @wmi_namespace = ] 'wmi_namespace'` Espacio de nombres WMI para consultar los eventos. *wmi_namespace* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @wmi_query = ] 'wmi_query'` La consulta que especifica el evento WMI para la alerta. *wmi_query* es de tipo **nvarchar (512)** y su valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Solo los **sysmessages** escritos en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] registro de aplicación de Windows pueden activar una alerta.  
  
 **sp_update_alert** solo cambia la configuración de alerta para la que se proporcionan los valores de parámetro. Si se omite un parámetro, se conserva la configuración actual.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar este procedimiento almacenado, los usuarios deben ser miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se cambia el valor de habilitación de `Test Alert` a `0`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_alert  
    @name = N'Test Alert',  
    @enabled = 0 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sp_help_alert &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-alert-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
