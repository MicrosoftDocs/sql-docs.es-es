---
description: sysmail_allitems (Transact-SQL)
title: sysmail_allitems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_allitems_TSQL
- sysmail_allitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_allitems database mail view
ms.assetid: 21fb8432-7677-4435-902f-64a58bba4cbb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 25b60bb95655cea1d4e460b46a0e21faf28a3103
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199728"
---
# <a name="sysmail_allitems-transact-sql"></a>sysmail_allitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contiene una fila por cada mensaje procesado por el Correo electrónico de base de datos. Utilice esta vista cuando desee ver el estado de todos los mensajes.  
  
 Para ver solo los mensajes con el estado de error, use [sysmail_faileditems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Para ver solo los mensajes sin enviar, use [sysmail_unsentitems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md). Para ver solo los mensajes que se enviaron, use [sysmail_sentitems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Identificador del elemento de correo en la cola de correo electrónico.|  
|**profile_id**|**int**|Identificador del perfil utilizado para enviar el mensaje.|  
|**destinatarios**|**ntext**|Direcciones de correo electrónico de los destinatarios de mensajes.|  
|**copy_recipients**|**ntext**|Direcciones de correo electrónico de los destinatarios que reciben copias del mensaje.|  
|**blind_copy_recipients**|**ntext**|Direcciones de correo electrónico de los destinatarios que reciben copias del mensaje pero cuyos nombres no aparecen en el encabezado del mensaje.|  
|**subject**|**nvarchar (510)**|Línea de asunto del mensaje.|  
|**body**|**ntext**|el cuerpo del mensaje.|  
|**body_format**|**VARCHAR (20)**|El formato del cuerpo del mensaje. Los valores posibles son TEXT y HTML.|  
|**importance**|**VARCHAR (6)**|Parámetro de **importancia** del mensaje.|  
|**distinga**|**VARCHAR (12)**|Parámetro de **sensibilidad** del mensaje.|  
|**file_attachments**|**ntext**|Una lista delimitada por signos de punto y coma de nombres de archivo adjuntos al mensaje de correo electrónico.|  
|**attachment_encoding**|**VARCHAR (20)**|Tipo de datos adjuntos.|  
|**consulta**|**ntext**|Consulta ejecutada por el programa de correo.|  
|**execute_query_database**|**sysname**|Contexto de base de datos en el cual el programa de correo ejecutó la consulta.|  
|**attach_query_result_as_file**|**bit**|Si el valor es 0, los resultados de la consulta se incluyeron en el cuerpo del mensaje de correo electrónico, después del contenido del cuerpo. Si el valor es 1, los resultados se devolvieron como datos adjuntos.|  
|**query_result_header**|**bit**|Si el valor es 1, los resultados de la consulta contenían encabezados de columna. Si el valor es 0, los resultados de la consulta no contenían encabezados de columna.|  
|**query_result_width**|**int**|**Query_result_width** parámetro del mensaje.|  
|**query_result_separator**|**char(1)**|Carácter utilizado para separar columnas en la salida de la consulta.|  
|**exclude_query_output**|**bit**|**Exclude_query_output** parámetro del mensaje. Para obtener más información, vea [sp_send_dbmail &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).|  
|**append_query_error**|**bit**|**Append_query_error** parámetro del mensaje. El valor 0 indica que el Correo electrónico de base de datos no debe enviar el mensaje de correo electrónico si hay un error en la consulta.|  
|**send_request_date**|**datetime**|Fecha y hora en que se colocó el mensaje en la cola de correo electrónico.|  
|**send_request_user**|**sysname**|Usuario que envió el mensaje. Se trata del contexto de usuario del procedimiento del Correo electrónico de base de datos, no del campo De: del mensaje.|  
|**sent_account_id**|**int**|Identificador de la cuenta del Correo electrónico de base de datos utilizada para enviar el mensaje.|  
|**sent_status**|**VARCHAR (8)**|Estado del mensaje. Los valores posibles son:<br /><br /> **enviado** : se envió el mensaje de correo electrónico.<br /><br /> no **enviado** : el correo electrónico de base de datos sigue intentando enviar el mensaje.<br /><br /> **reintentando** -correo electrónico de base de datos no pudo enviar el mensaje, pero está intentando enviarlo de nuevo.<br /><br /> **error** : el correo electrónico de base de datos no pudo enviar el mensaje.|  
|**sent_date**|**datetime**|Fecha y hora en que se envió el mensaje.|  
|**last_mod_date**|**datetime**|Fecha y hora de la modificación más reciente de la fila.|  
|**last_mod_user**|**sysname**|Usuario que realizó la modificación más reciente de la fila.|  
  
## <a name="remarks"></a>Observaciones  
 Use la vista **sysmail_allitems** para ver el estado de todos los mensajes procesados por correo electrónico de base de datos. Al solucionar problemas del Correo electrónico de base de datos, puede que esta vista le ayude a identificar la naturaleza del problema, pues en ella se muestran los atributos de los mensajes enviados comparados con los de los mensajes no enviados.  
  
 Las tablas del sistema que expone esta vista contienen todos los mensajes y pueden hacer que la base de datos **msdb** crezca. Elimine periódicamente los mensajes antiguos de la vista para reducir el tamaño de las tablas. Para obtener más información, vea [crear un trabajo de Agente SQL Server para archivar mensajes de correo electrónico de base de datos y registros de eventos](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Permisos  
 Se concede al rol fijo de servidor **sysadmin** y al rol de base de datos **DatabaseMailUserRole** . Cuando lo ejecuta un miembro del rol fijo de servidor **sysadmin** , esta vista muestra todos los mensajes. Todos los demás usuarios solo verán los mensajes que enviaron.  
  
  
