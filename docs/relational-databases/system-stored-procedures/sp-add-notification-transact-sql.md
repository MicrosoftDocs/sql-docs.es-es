---
description: sp_add_notification (Transact-SQL)
title: sp_add_notification (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_notification_TSQL
- sp_add_notification
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_notification
ms.assetid: 0525e0a2-ed0b-4e69-8a4c-a9e3e3622fbd
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f09a5638e55ee6bd8e0dfd6e07defc92b0ed365d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208005"
---
# <a name="sp_add_notification-transact-sql"></a>sp_add_notification (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece una notificación para una alerta.  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_notification [ @alert_name = ] 'alert' ,   
    [ @operator_name = ] 'operator' ,   
    [ @notification_method = ] notification_method  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @alert_name = ] 'alert'` La alerta de esta notificación. la *alerta* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @operator_name = ] 'operator'` Operador al que se va a notificar cuando se produzca la alerta. *Operator* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @notification_method = ] notification_method` Método por el que se notifica al operador. *notification_method* es de **tinyint** y no tiene ningún valor predeterminado. *notification_method* puede ser uno o varios de estos valores combinados con un operador lógico **or** .  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Correo electrónico|  
|**2**|Buscapersonas|  
|**4**|**net send**|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_add_notification** se debe ejecutar desde la base de datos **msdb** .  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] proporciona una sencilla forma gráfica de administrar todo el sistema de alertas. Se recomienda utilizar [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] para configurar la infraestructura de alertas.  
  
 Para enviar una notificación como respuesta a una alerta, primero debe configurar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el envío de correo.  
  
 Si se produce algún error al enviar un mensaje de correo electrónico o una notificación por buscapersonas, el error se comunica en el registro de errores de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_add_notification**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se agrega una notificación de correo electrónico para la alerta especificada (`Test Alert`).  
  
> **Nota:** En este ejemplo se da por supuesto que `Test Alert` ya existe y que `François Ajenstat` es un nombre de operador válido.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_notification  
 @alert_name = N'Test Alert',  
 @operator_name = N'François Ajenstat',  
 @notification_method = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [sp_delete_notification &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-notification-transact-sql.md)   
 [sp_help_notification &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-notification-transact-sql.md)   
 [sp_update_notification &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-notification-transact-sql.md)   
 [sp_add_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
