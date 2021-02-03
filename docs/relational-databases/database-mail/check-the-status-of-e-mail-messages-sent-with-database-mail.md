---
description: Comprobar el estado de los mensajes de correo electrónico enviados con Correo electrónico de base de datos
title: Estado de los mensajes de correo electrónico enviados con Correo electrónico de base de datos
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- e-mail [SQL Server], status information
- mail [SQL Server], status information
- Database Mail [SQL Server], message status
- status information [Database Mail]
ms.assetid: eb290f24-b52f-46bc-84eb-595afee6a5f3
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
ms.openlocfilehash: 0b230cc2a880b16b62934c3a89af475dd5cecf96
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251134"
---
# <a name="check-the-status-of-e-mail-messages-sent-with-database-mail"></a>Comprobar el estado de los mensajes de correo electrónico enviados con Correo electrónico de base de datos
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo comprobar el estado del mensaje de correo electrónico enviado con Correo electrónico de base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   **Antes de empezar:**  
  
-   **Para ver el estado del correo electrónico enviado a través de Correo electrónico de base de datos con:**  [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
 Correo electrónico de base de datos conserva copias de los mensajes de correo electrónico salientes y las muestra en las vistas **sysmail_allitems**, **sysmail_sentitems**, **sysmail_unsentitems** y **sysmail_faileditems** de la base de datos **msdb** . El programa externo de Correo electrónico de base de datos registra la actividad y muestra ese registro por medio del registro de eventos de aplicación Windows y la vista **sysmail_event_log** de la base de datos **msdb** . Para comprobar el estado de un mensaje de correo electrónico, ejecute una consulta en esta vista. Los mensajes de correo electrónico tienen cuatro posibles estados: **enviado**, **no enviado**, **reintentando** y **error**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para ver el estado del correo electrónico enviado con Correo electrónico de base de datos**  
  
1.  Seleccione los mensajes que le interesen de la tabla **sysmail_allitems** por medio de **mailitem_id** o **sent_status**.  
  
2.  Para comprobar el estado devuelto por el programa externo para los mensajes de correo electrónico, combine las vistas **sysmail_allitems** y **sysmail_event_log** en la columna **mailitem_id** , como se muestra en la sección siguiente.  
  
     De forma predeterminada, el programa externo no registra información acerca de los mensajes que se enviaron correctamente. Para registrar todos los mensajes, establezca el nivel de registro en detallado mediante la página **Configurar parámetros del sistema** del **Asistente para configuración de Correo electrónico de base de datos**.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 En el siguiente ejemplo se muestra información acerca de los mensajes de correo electrónico enviados a `danw` que el programa externo no ha podido enviar correctamente. La instrucción incluye el asunto, la fecha y la hora en que el programa externo no pudo enviar el mensaje, así como el mensaje de error del registro de Correo electrónico de base de datos.  
  
```  
USE msdb ;  
GO  
  
-- Show the subject, the time that the mail item row was last  
-- modified, and the log information.  
-- Join sysmail_faileditems to sysmail_event_log   
-- on the mailitem_id column.  
-- In the WHERE clause list items where danw was in the recipients,  
-- copy_recipients, or blind_copy_recipients.  
-- These are the items that would have been sent  
-- to danw.  
  
SELECT items.subject,  
    items.last_mod_date  
    ,l.description FROM dbo.sysmail_faileditems as items  
INNER JOIN dbo.sysmail_event_log AS l  
    ON items.mailitem_id = l.mailitem_id  
WHERE items.recipients LIKE '%danw%'    
    OR items.copy_recipients LIKE '%danw%'   
    OR items.blind_copy_recipients LIKE '%danw%'  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Registro y auditorías del Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-log-and-audits.md)  
  
  
