---
description: sysmail_sentitems (Transact-SQL)
title: sysmail_sentitems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_sentitems_TSQL
- sysmail_sentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_sentitems database mail view
ms.assetid: 16eb2a44-cebb-4cec-93ac-e2498c39989f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ef28d1d962fbdcfffd13dee4599b30617bbc36ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199712"
---
# <a name="sysmail_sentitems-transact-sql"></a>sysmail_sentitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contiene una fila por cada mensaje enviado por el Correo electrónico de base de datos. Use **sysmail_sentitems** si desea ver qué mensajes se enviaron correctamente.  
  
 Para ver todos los mensajes procesados por Correo electrónico de base de datos, use [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Para ver solo los mensajes con el estado de error, use [sysmail_faileditems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Para ver solo los mensajes no enviados o reintentos, utilice [sysmail_unsentitems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md). Para ver los datos adjuntos de correo electrónico, use [sysmail_mailattachments &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Identificador del elemento de correo en la cola de correo electrónico.|  
|**profile_id**|**int**|Identificador del perfil utilizado para enviar el mensaje.|  
|**destinatarios**|**ntext**|Direcciones de correo electrónico de los destinatarios de mensajes.|  
|**copy_recipients**|**ntext**|Direcciones de correo electrónico de los destinatarios que reciben copias del mensaje.|  
|**blind_copy_recipients**|**ntext**|Direcciones de correo electrónico de los destinatarios que reciben copias del mensaje pero cuyos nombres no aparecen en el encabezado del mensaje.|  
|**subject**|**nvarchar (510)**|Línea de asunto del mensaje.|  
|**body**|**ntext**|el cuerpo del mensaje.|  
|**body_format**|**VARCHAR (20)**|El formato del cuerpo del mensaje. Los valores posibles son **Text** y **html**.|  
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
|**send_request_user**|**sysname**|El usuario que envió el mensaje. Se trata del contexto de usuario del procedimiento del Correo electrónico de base de datos, no del campo De: del mensaje.|  
|**sent_account_id**|**int**|Identificador de la cuenta del Correo electrónico de base de datos utilizada para enviar el mensaje.|  
|**sent_status**|**VARCHAR (8)**|Estado del mensaje. Siempre se **envía** para esta vista.|  
|**sent_date**|**datetime**|Fecha y hora en que se envió el mensaje.|  
|**last_mod_date**|**datetime**|Fecha y hora de la modificación más reciente de la fila.|  
|**last_mod_user**|**sysname**|Usuario que realizó la modificación más reciente de la fila.|  
  
## <a name="remarks"></a>Observaciones  
 Al solucionar problemas del Correo electrónico de base de datos, puede que esta vista le ayude a identificar la naturaleza del problema, pues en ella se muestran los atributos de los mensajes enviados correctamente. El Correo electrónico de base de datos marca los mensajes como enviados cuando se envían correctamente a un servidor de correo SMTP. Normalmente, los mensajes de correo electrónico se reciben en pocos minutos, pero puede que se retrasen a causa de problemas con el servidor SMTP. El Correo electrónico de base de datos marca los mensajes como enviados cuando los acepta el servidor de correo SMTP. Los mensajes de correo electrónico con errores en el servidor de correo SMTP, como una dirección de correo electrónico de destinatario no válida, no se devuelven al Correo electrónico de base de datos. Estos mensajes se registran como enviados aunque no se hayan entregado. Este tipo de error se debe solucionar en el servidor SMTP. Asimismo, puede que el servidor de correo SMTP envíe a la dirección de respuesta de una cuenta del Correo electrónico de base de datos una notificación en la que se indica que no se puede entregar el mensaje.  
  
## <a name="permissions"></a>Permisos  
 Se concede al rol fijo de servidor **sysadmin** y al rol de base de datos **DatabaseMailUserRole** . Cuando lo ejecuta un miembro del rol fijo de servidor **sysadmin** , esta vista muestra todos los mensajes enviados. Los demás usuarios verán únicamente los mensajes que ellos han enviado.  
  
## <a name="see-also"></a>Consulte también  
 [Objetos de mensajería de Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail-messaging-objects.md)  
  
  
