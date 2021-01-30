---
description: sysmail_faileditems (Transact-SQL)
title: sysmail_faileditems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_faileditems
- sysmail_faileditems_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_faileditems database mail view
ms.assetid: a31562c5-358e-4cfc-a72d-b3faccc53851
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 83e7bc1ebeaa78cd8e12325c7259c93139f9a200
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193017"
---
# <a name="sysmail_faileditems-transact-sql"></a>sysmail_faileditems (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada mensaje de Correo electrónico de base de datos con el estado de **error** . Utilice esta vista para determinar qué mensajes no se enviaron correctamente.  
  
 Para ver todos los mensajes procesados por Correo electrónico de base de datos, use [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Para ver solo los mensajes sin enviar, use [sysmail_unsentitems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md). Para ver solo los mensajes que se enviaron, use [sysmail_sentitems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md). Para ver los datos adjuntos de correo electrónico, use [sysmail_mailattachments &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Identificador del elemento de correo en la cola de correo electrónico.|  
|**profile_id**|**int**|Identificador del perfil que se usa para enviar el mensaje.|  
|**destinatarios**|**ntext**|Direcciones de correo electrónico de los destinatarios de mensajes.|  
|**copy_recipients**|**ntext**|Direcciones de correo electrónico de los destinatarios que reciben copias del mensaje.|  
|**blind_copy_recipients**|**ntext**|Direcciones de correo electrónico de los destinatarios que reciben copias del mensaje pero cuyos nombres no aparecen en el encabezado del mensaje.|  
|**subject**|**nvarchar (510)**|Línea de asunto del mensaje.|  
|**body**|**ntext**|el cuerpo del mensaje.|  
|**body_format**|**VARCHAR (20)**|El formato del cuerpo del mensaje. Los valores posibles son TEXT y HTML.|  
|**importance**|**VARCHAR (6)**|Parámetro de **importancia** del mensaje.|  
|**distinga**|**VARCHAR (12)**|Parámetro de **sensibilidad** del mensaje.|  
|**file_attachments**|**ntext**|Una lista delimitada por signos de punto y coma de nombres de archivo adjuntos al mensaje de correo electrónico.|  
|**Attachment_encoding**|**VARCHAR (20)**|Tipo de datos adjuntos.|  
|**Consultar**|**ntext**|Consulta ejecutada por el programa de correo.|  
|**execute_query_database**|**sysname**|Contexto de base de datos en el cual el programa de correo ejecutó la consulta.|  
|**attach_query_result_as_file**|**bit**|Si el valor es 0, los resultados de la consulta se incluyeron en el cuerpo del mensaje de correo electrónico, después del contenido del cuerpo. Si el valor es 1, los resultados se devolvieron como datos adjuntos.|  
|**query_result_header**|**bit**|Si el valor es 1, los resultados de la consulta contenían encabezados de columna. Si el valor es 0, los resultados de la consulta no contenían encabezados de columna.|  
|**query_result_width**|**int**|**Query_result_width** parámetro del mensaje.|  
|**query_result_separator**|**char(1)**|Carácter utilizado para separar columnas en la salida de la consulta.|  
|**exclude_query_output**|**bit**|**Exclude_query_output** parámetro del mensaje. Para obtener más información, vea [sp_send_dbmail &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).|  
|**append_query_error**|**bit**|**Append_query_error** parámetro del mensaje. El valor 0 indica que el Correo electrónico de base de datos no debe enviar el mensaje de correo electrónico si hay un error en la consulta.|  
|**send_request_date**|**datetime**|Fecha y hora en que se colocó el mensaje en la cola de correo electrónico.|  
|**send_request_user**|**sysname**|Usuario que envió el mensaje. Se trata del contexto de usuario del procedimiento del Correo electrónico de base de datos, no del campo De: del mensaje.|  
|**sent_account_id**|**int**|Identificador de la cuenta del Correo electrónico de base de datos utilizada para enviar el mensaje. Siempre es NULL para esta vista.|  
|**sent_status**|**VARCHAR (8)**|Estado del mensaje. Siempre **se produjo un error** en esta vista.|  
|**sent_date**|**datetime**|Fecha y hora en que se eliminó el mensaje de la cola de correo electrónico.|  
|**last_mod_date**|**datetime**|Fecha y hora de la modificación más reciente de la fila.|  
|**last_mod_user**|**sysname**|Usuario que realizó la modificación más reciente de la fila.|  
  
## <a name="remarks"></a>Observaciones  
 Use la vista **sysmail_faileditems** para ver qué mensajes no se enviaron por correo electrónico de base de datos. Al solucionar problemas del Correo electrónico de base de datos, puede que esta vista le ayude a identificar la naturaleza del problema, pues en ella se muestran los atributos de los mensajes no enviados. Para ver el motivo del error, vea la entrada del mensaje con errores en la [sysmail_event_log &#40;vista&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md) .  
  
## <a name="permissions"></a>Permisos  
 Se concede al rol fijo de servidor **sysadmin** y al rol de base de datos **DatabaseMailUserRole** . Cuando lo ejecuta un miembro del rol fijo de servidor **sysadmin** , esta vista muestra todos los mensajes con errores. Todos los demás usuarios verán únicamente los mensajes con error que envíen ellos mismos.  
  
  
