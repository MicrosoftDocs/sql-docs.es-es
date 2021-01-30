---
description: sp_add_alert (Transact-SQL)
title: sp_add_alert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_alert
- sp_add_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_alert
ms.assetid: d9b41853-e22d-4813-a79f-57efb4511f09
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b6d7d1e451949d17dc7e4637dc82b86c4225f06c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186044"
---
# <a name="sp_add_alert-transact-sql"></a>sp_add_alert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una alerta.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_alert [ @name = ] 'name'   
     [ , [ @message_id = ] message_id ]   
     [ , [ @severity = ] severity ]   
     [ , [ @enabled = ] enabled ]  
     [ , [ @delay_between_responses = ] delay_between_responses ]   
     [ , [ @notification_message = ] 'notification_message' ]   
     [ , [ @include_event_description_in = ] include_event_description_in ]   
     [ , [ @database_name = ] 'database' ]   
     [ , [ @event_description_keyword = ] 'event_description_keyword_pattern' ]   
     [ , { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ]   
     [ , [ @raise_snmp_trap = ] raise_snmp_trap ]   
     [ , [ @performance_condition = ] 'performance_condition' ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @wmi_namespace = ] 'wmi_namespace' ]  
     [ , [ @wmi_query = ] 'wmi_query' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] 'name'` El nombre de la alerta. El nombre aparece en el mensaje de correo electrónico o de buscapersonas enviado como respuesta a la alerta. Debe ser único y puede contener el carácter de porcentaje ( **%** ). *Name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @message_id = ] message_id` El número de error del mensaje que define la alerta. (Normalmente se corresponde con un número de error en la tabla **sysmessages** ). *message_id* es de **tipo int** y su valor predeterminado es **0**. Si se usa la *gravedad* para definir la alerta, *message_id* debe ser **0** o null.  
  
> [!NOTE]  
>  Solo los errores **sysmessages** escritos en el registro de aplicación de Microsoft Windows pueden hacer que se envíe una alerta.  
  
`[ @severity = ] severity` Nivel de gravedad (de **1** a **25**) que define la alerta. Cualquier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mensaje almacenado en la tabla **sysmessages** enviada al [!INCLUDE[msCoName](../../includes/msconame-md.md)] registro de aplicación de Windows con la gravedad indicada hace que se envíe la alerta. *Severity* es de **tipo int** y su valor predeterminado es 0. Si *message_id* se usa para definir la alerta, *Severity* debe ser **0**.  
  
`[ @enabled = ] enabled` Indica el estado actual de la alerta. *Enabled* es de **tinyint** y su valor predeterminado es 1 (habilitado). Si es **0**, la alerta no está habilitada y no se activa.  
  
`[ @delay_between_responses = ] delay_between_responses` El período de espera, en segundos, entre las respuestas a la alerta. *delay_between_responses* es de **tipo int** y su valor predeterminado es **0**, lo que significa que no hay ninguna espera entre las respuestas (cada aparición de la alerta genera una respuesta). La respuesta puede tener lugar de cualquiera de estas formas, o de ambas:  
  
-   Una o más notificaciones enviadas por correo electrónico o el buscapersonas  
  
-   Un trabajo que debe ejecutarse  
  
 Al establecer este valor, es posible evitar, por ejemplo, que se envíen mensajes de correo electrónico no deseados cuando una alerta se produce repetidamente en un período de tiempo breve.  
  
`[ @notification_message = ] 'notification_message'` Es un mensaje adicional opcional que se envía al operador como parte de la notificación por correo electrónico, **net send** o buscapersonas. *notification_message* es de tipo **nvarchar (512)** y su valor predeterminado es NULL. La especificación de *notification_message* resulta útil para agregar notas especiales como procedimientos correctores.  
  
`[ @include_event_description_in = ] include_event_description_in` Indica si la descripción del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] error debe incluirse como parte del mensaje de notificación. *include_event_description_in* es **tinyint**, su valor predeterminado es **5** (correo electrónico y **net send**) y puede tener uno o varios de estos valores combinados con un operador lógico **or** .  
  
> [!IMPORTANT]
>  Las opciones Buscapersonas y **net send** se quitarán del Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una versión futura de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar estas características en los nuevos trabajos de programación y planee modificar las aplicaciones que actualmente las utilizan.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|None|  
|**1**|Correo electrónico|  
|**2**|Buscapersonas|  
|**4**|**net send**|  
  
`[ @database_name = ] 'database'` La base de datos en la que debe producirse el error para que se desencadene la alerta. Si no se proporciona la *base de datos*, la alerta se activa independientemente de dónde se haya producido el error. *Database* es de **tipo sysname**. No se permiten nombres incluidos entre corchetes ([ ]). El valor predeterminado es NULL.  
  
`[ @event_description_keyword = ] 'event_description_keyword_pattern'` Secuencia de caracteres que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe ser como la descripción del error. Se pueden usar caracteres de coincidencia de patrón de la expresión LIKE de [!INCLUDE[tsql](../../includes/tsql-md.md)]. *event_description_keyword_pattern* es de tipo **nvarchar (100)** y su valor predeterminado es NULL. Este parámetro es útil para filtrar los nombres de objeto (por ejemplo, **% customer_table%**).  
  
`[ @job_id = ] job_id` Número de identificación del trabajo que se va a ejecutar como respuesta a esta alerta. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo que se va a ejecutar como respuesta a esta alerta. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @raise_snmp_trap = ] raise_snmp_trap` No se implementa en la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versión 7,0. *raise_snmp_trap* es de **tinyint** y su valor predeterminado es 0.  
  
`[ @performance_condition = ] 'performance_condition'` Es un valor expresado en el formato '*itemcomparatorvalue*'. *performance_condition* es de tipo **nvarchar (512)** y su valor predeterminado es null, y consta de estos elementos.  
  
|Elemento de formato|Descripción|  
|--------------------|-----------------|  
|*Item*|Objeto de rendimiento, contador de rendimiento o instancia con nombre del contador|  
|*Comparador*|Uno de estos operadores: >, < o =|  
|*Valor*|Valor numérico del contador|  
  
`[ @category_name = ] 'category'` El nombre de la categoría de alerta. *Category* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @wmi_namespace = ] 'wmi_namespace'` Espacio de nombres WMI para consultar los eventos. *wmi_namespace* es de **tipo sysname y su** valor predeterminado es NULL. Solo se admiten espacios de nombres del servidor local.  
  
`[ @wmi_query = ] 'wmi_query'` La consulta que especifica el evento WMI para la alerta. *wmi_query* es de tipo **nvarchar (512)** y su valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_add_alert** se debe ejecutar desde la base de datos **msdb** .  
  
 Estas son las circunstancias en las que los errores y mensajes que generan [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y las aplicaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se envían al registro de aplicación Windows y que, por lo tanto, pueden generar alertas:  
  
-   Errores de **Sys. Messages** de gravedad 19 o superior  
  
-   Cualquier instrucción RAISERROR llamada con la sintaxis WITH LOG  
  
-   Cualquier error **Sys. Messages** modificado o creado mediante **sp_altermessage**  
  
-   Cualquier evento registrado mediante **xp_logevent**  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] proporciona una forma gráfica y fácil de administrar todo el sistema de alertas, y constituye el método recomendado para configurar una infraestructura de alertas.  
  
 Si una alerta no funciona correctamente, compruebe lo siguiente:  
  
-   El servicio Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando.  
  
-   El evento ha aparecido en el registro de aplicación Windows.  
  
-   La alerta está habilitada.  
  
-   Los eventos generados durante **xp_logevent** se producen en la base de datos maestra. Por tanto, **xp_logevent** no desencadena una alerta a menos que el valor de **\@database_name** de la alerta sea **'master'** o NULL.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_add_alert**.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente muestra cómo agregar una alerta (Test Alert) que ejecute el trabajo `Back up the AdventureWorks2012 Database` al activarse.  
  
> [!NOTE]  
>  En el ejemplo se da por supuesto que el mensaje 55001 y el trabajo `Back up the AdventureWorks2012 Database` ya existen. El siguiente es solo un ejemplo.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_alert  
    @name = N'Test Alert',  
    @message_id = 55001,   
   @severity = 0,   
   @notification_message = N'Error 55001 has occurred. The database will be backed up...',   
   @job_name = N'Back up the AdventureWorks2012 Database' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_notification &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_altermessage &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [sp_delete_alert &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md)   
 [sp_help_alert &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-alert-transact-sql.md)   
 [sp_update_alert &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)   
 [sys.sysperfinfo &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
