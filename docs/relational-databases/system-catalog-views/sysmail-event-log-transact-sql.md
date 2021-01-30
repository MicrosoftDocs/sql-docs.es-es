---
description: sysmail_event_log (Transact-SQL)
title: sysmail_event_log (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_event_log
- sysmail_event_log_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_event_log database mail view
ms.assetid: 440bc409-1188-4175-afc4-c68e31e44fed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f5c3435be5885aeedbedb7868a2883dc34ff0dff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199758"
---
# <a name="sysmail_event_log-transact-sql"></a>sysmail_event_log (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada mensaje de Windows o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelto por el sistema del Correo electrónico de base de datos. (El mensaje en este contexto hace referencia a un mensaje como un mensaje de error, no un mensaje de correo electrónico). Configure el parámetro de **nivel de registro** mediante el cuadro de diálogo **Configurar parámetros del sistema** del asistente para configuración de Correo electrónico de base de datos, o el [sysmail_configure_sp](../../relational-databases/system-stored-procedures/sysmail-configure-sp-transact-sql.md) procedimiento almacenado para determinar qué mensajes se devuelven.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**Log_id**|**int**|Identificador de elementos del registro.|  
|**event_type**|**VARCHAR (11)**|Tipo de aviso insertado en el registro. Los valores posibles son errores, advertencias, mensajes informativos, mensajes de operación correcta y otros mensajes internos.|  
|**log_date**|**datetime**|Fecha y hora en que se realiza la entrada de registro.|  
|**description**|**nvarchar(max)**|Texto del mensaje que se registra.|  
|**process_id**|**int**|Id. de proceso del programa externo del Correo electrónico de base de datos. Suele cambiar cada vez que se inicia el programa externo del Correo electrónico de base de datos.|  
|**mailitem_id**|**int**|Identificador del elemento de correo en la cola de correo electrónico. Su valor será NULL si el mensaje no está relacionado con un elemento de correo electrónico determinado.|  
|**account_id**|**int**|**ACCOUNT_ID** de la cuenta relacionada con el evento. Su valor será NULL si el mensaje no está relacionado con una cuenta de correo electrónico determinada.|  
|**last_mod_date**|**datetime**|Fecha y hora de la modificación más reciente de la fila.|  
|**last_mod_user**|**sysname**|Usuario que realizó la modificación más reciente de la fila. En el caso de los mensajes de correo electrónico, se trata del usuario que envió el mensaje. En el caso de los mensajes generados por el programa externo del Correo electrónico de base de datos, se trata del contexto de usuario del programa.|  
  
## <a name="remarks"></a>Observaciones  
 Para solucionar problemas de Correo electrónico de base de datos, busque en la vista de **sysmail_event_log** los eventos relacionados con errores de correo electrónico. Algunos mensajes, como los de error del programa externo del Correo electrónico de base de datos, no están asociados con mensajes de correo electrónico determinados. Para buscar errores relacionados con mensajes de correo electrónico específicos, busque el **mailitem_id** del mensaje de error en la vista de **sysmail_faileditems** y, a continuación, busque en el **sysmail_event_log** los mensajes relacionados con ese **mailitem_id**. Cuando se devuelve un error de **sp_send_dbmail**, el correo electrónico no se envía al sistema correo electrónico de base de datos y el error no se muestra en esta vista.  
  
 Si se producen errores en los intentos de entrega de cuentas individuales, el Correo electrónico de base de datos conservará los mensajes de error durante los reintentos hasta que la entrega del elemento de correo se realice correctamente o provoque un error. En caso de éxito final, todos los errores acumulados se registran como advertencias independientes, incluido el **ACCOUNT_ID**. Por tanto, puede que aparezcan advertencias aunque se haya enviado el mensaje. En caso de que se produzca un error de entrega, todas las advertencias anteriores se registran como un mensaje de error sin un **ACCOUNT_ID**, ya que se ha producido un error en todas las cuentas.  
  
## <a name="permissions"></a>Permisos  
 Debe ser miembro del rol fijo de servidor **sysadmin** o del rol de base de datos **DatabaseMailUserRole** para tener acceso a esta vista. Los miembros de **DatabaseMailUserRole** que no son miembros del rol **sysadmin** solo pueden ver los eventos de los mensajes de correo electrónico que envían.  
  
## <a name="see-also"></a>Consulte también  
 [sysmail_faileditems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [Programa externo Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-external-program.md)  
  
  
