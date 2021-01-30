---
description: sysmail_delete_log_sp (Transact-SQL)
title: sysmail_delete_log_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_log_sp_TSQL
- sysmail_delete_log_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_log_sp
ms.assetid: e94b37a1-70ad-46a5-86c0-721892156f7c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1f3921d220f367d15849eab080369db986d8f53e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160014"
---
# <a name="sysmail_delete_log_sp-transact-sql"></a>sysmail_delete_log_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina eventos del registro del Correo electrónico de base de datos. Elimina todos los eventos del registro o los que cumplen criterios de fecha o tipo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_delete_log_sp  [ [ @logged_before = ] 'logged_before' ]  
    [, [ @event_type = ] 'event_type' ]  
  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @logged_before = ] 'logged_before'` Elimina las entradas hasta la fecha y hora especificadas por el argumento *logged_before* . *logged_before* es de **tipo DateTime** y su valor predeterminado es NULL. NULL indica todas las fechas.  
  
`[ @event_type = ] 'event_type'` Elimina las entradas de registro del tipo especificado como *event_type*. *event_type* es de tipo **VARCHAR (15)** y no tiene ningún valor predeterminado. Las entradas válidas son **Success**, **Warning**, **error** e **informativo**. NULL indica todos los tipos de evento.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Use el **sysmail_delete_log_sp** procedimiento almacenado para eliminar de forma permanente las entradas del registro de correo electrónico de base de datos. Un argumento opcional permite eliminar solo los registros antiguos indicando la fecha y la hora. Se eliminarán los eventos anteriores a ese argumento. Un argumento opcional permite eliminar solo los eventos de un tipo determinado, especificado como el argumento **event_type** .  
  
 Al eliminar entradas del registro del Correo electrónico de base de datos no se eliminan las entradas de mensajes de correo electrónico de las tablas del Correo electrónico de base de datos. Utilice [sysmail_delete_mailitems_sp](../../relational-databases/system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md) para eliminar el correo electrónico de las tablas de correo electrónico de base de datos.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden obtener acceso a este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-deleting-all-events"></a>A. Eliminar todos los eventos  
 En el ejemplo siguiente se eliminan todos los eventos del registro del Correo electrónico de base de datos.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp ;  
GO  
```  
  
### <a name="b-deleting-the-oldest-events"></a>B. Eliminar los eventos más antiguos  
 En el ejemplo siguiente se eliminan los eventos del registro del Correo electrónico de base de datos anteriores al 9 de octubre de 2005.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp  
    @logged_before = 'October 9, 2005' ;  
GO  
```  
  
### <a name="c-deleting-all-events-of-a-certain-type"></a>C. Eliminar todos los eventos de un tipo determinado  
 En el ejemplo siguiente se eliminan los mensajes de operación correcta del registro del Correo electrónico de base de datos.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp  
    @event_type = 'success' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sysmail_event_log &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)   
 [sysmail_delete_mailitems_sp &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md)   
 [Crear un trabajo del Agente SQL Server para archivar mensajes y registros de eventos del Correo electrónico de base de datos](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)  
  
  
