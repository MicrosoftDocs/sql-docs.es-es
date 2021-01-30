---
description: sp_help_notification (Transact-SQL)
title: sp_help_notification (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_notification
- sp_help_notification_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_notification
ms.assetid: 0273457f-9d2a-4a6f-9a16-6a6bf281cba0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d37d091fdb380f0a08f3f0064f2ce408f439eee8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199984"
---
# <a name="sp_help_notification-transact-sql"></a>sp_help_notification (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Presenta una lista de alertas de un operador dado o una lista de operadores para una alerta dada.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_notification  
     [ @object_type = ] 'object_type' ,  
     [ @name = ] 'name' ,  
     [ @enum_type = ] 'enum_type' ,   
     [ @notification_method = ] notification_method   
     [ , [ @target_name = ] 'target_name' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @object_type = ] 'object_type'` Tipo de información que se va a devolver. *object_type* es **Char (9)** y no tiene ningún valor predeterminado. *object_type* pueden ser alertas, que enumeran las alertas asignadas al nombre del operador proporcionado *,* o a los operadores, que enumera los operadores responsables del nombre de alerta proporcionado *.*  
  
`[ @name = ] 'name'` Un nombre de operador (si *object_type* es Operators) o un nombre de alerta (si *object_type* es Alerts). *Name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @enum_type = ] 'enum_type'` La información *object_type* que se devuelve. *enum_type* es real en la mayoría de los casos. *enum_type* es **Char (10)**, no tiene ningún valor predeterminado y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|ACTUAL|Muestra solo el *object_types* asociado al *nombre*.|  
|ALL|Enumera todos los *object_types* incluidos los que no están asociados con *el nombre*.|  
|TARGET|Muestra solo el *object_types* que coincide con el *target_name* proporcionado, independientemente de la asociación con el *nombre*.|  
  
`[ @notification_method = ] notification_method` Valor numérico que determina las columnas del método de notificación que se van a devolver. *notification_method* es **tinyint** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Correo electrónico: devuelve solo el **use_email** columna.|  
|**2**|Buscapersonas: devuelve solo el **use_pager** columna.|  
|**4**|NetSend: devuelve solo el **use_netsend** columna.|  
|**7**|Todas: devuelve todas las columnas.|  
  
`[ @target_name = ] 'target_name'` Nombre de alerta que se va a buscar (si *object_type* es alertas) o un nombre de operador que se va a buscar (si *object_type* es Operators). *target_name* solo es necesario si *enum_type* es Target. *target_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-valves"></a>Válvulas del código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Si *object_type* es **Alerts**, el conjunto de resultados muestra todas las alertas de un operador determinado.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**alert_id**|**int**|Número de identificador de la alerta.|  
|**alert_name**|**sysname**|Nombre de la alerta.|  
|**use_email**|**int**|Se utiliza el correo electrónico para notificar al operador:<br /><br /> **1** = Sí<br /><br /> **0** = No|  
|**use_pager**|**int**|Se utiliza un buscapersonas para notificar al operador:<br /><br /> **1** = Sí<br /><br /> **0** = No|  
|**use_netsend**|**int**|Se utiliza un mensaje de red para notificar al operador:<br /><br /> **1** = Sí<br /><br /> **0** = No|  
|**has_email**|**int**|Número de notificaciones por correo electrónico enviadas para esta alerta.|  
|**has_pager**|**int**|Número de notificaciones por buscapersonas enviadas para esta alerta.|  
|**has_netsend**|**int**|Número de notificaciones de **net send** enviadas para esta alerta.|  
  
 Si **object_type** es **Operators**, el conjunto de resultados muestra todos los operadores de una alerta determinada.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**operator_id**|**int**|Número de identificación del operador.|  
|**operator_name**|**sysname**|Nombre del operador.|  
|**use_email**|**int**|Para enviar la notificación al operador se utiliza el correo electrónico:<br /><br /> **1** = Sí<br /><br /> **0** = No|  
|**use_pager**|**int**|Para enviar la notificación al operador se utiliza un buscapersonas:<br /><br /> **1** = Sí<br /><br /> **0** = No|  
|**use_netsend**|**int**|Para enviar la notificación al operador se utiliza un mensaje de red:<br /><br /> **1** = Sí<br /><br /> **0** = No|  
|**has_email**|**int**|El operador tiene una dirección de correo electrónico:<br /><br /> **1** = Sí<br /><br /> **0** = No|  
|**has_pager**|**int**|El operador tiene una dirección de buscapersonas:<br /><br /> **1** = Sí<br /><br /> **0** = No|  
|**has_netsend**|**int**|El operador tiene configurada la notificación mediante net send.<br /><br /> **1** = Sí<br /><br /> **0** = No|  
  
## <a name="remarks"></a>Observaciones  
 Este procedimiento almacenado se debe ejecutar desde la base de datos **msdb** .  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar este procedimiento almacenado, un usuario debe ser miembro del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-alerts-for-a-specific-operator"></a>A. Presentar las alertas de un operador específico  
 En el ejemplo siguiente se devuelven todas las alertas para las que el operador `François Ajenstat` recibe algún tipo de notificación.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_notification   
    @object_type = N'ALERTS',  
    @name = N'François Ajenstat',  
    @enum_type = N'ACTUAL',  
    @notification_method = 7 ;  
GO  
```  
  
### <a name="b-listing-operators-for-a-specific-alert"></a>B. Presentar los operadores de una alerta específica  
 En el ejemplo siguiente se devuelven todos los operadores que reciben algún tipo de notificación de la alerta `Test Alert`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_notification  
    @object_type = N'OPERATORS',  
    @name = N'Test Alert',  
    @enum_type = N'ACTUAL',  
    @notification_method = 7 ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_notification &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_delete_notification &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-notification-transact-sql.md)   
 [sp_update_notification &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-notification-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
