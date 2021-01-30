---
description: sysmail_unsentitems (Transact-SQL)
title: sysmail_unsentitems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_unsentitems_TSQL
- sysmail_unsentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_unsentitems database mail view
ms.assetid: 993c12da-41e5-4e53-a188-0323feb70c67
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c532febf9074456ec1806fdb9d7ad0eed9b9b26a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199684"
---
# <a name="sysmail_unsentitems-transact-sql"></a>sysmail_unsentitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contiene una fila por cada mensaje de Correo electrónico de base de datos con el estado sin **Enviar** o de **reintento** . Los mensajes con el estado unsent o retrying siguen en la cola de correo y pueden ser enviados en cualquier momento. Los mensajes pueden tener el estado sin **Enviar** por los siguientes motivos:  
  
-   El mensaje es nuevo y, aunque se ha colocado en la cola de correo, el Correo electrónico de base de datos está ocupado con otros mensajes y aún no ha alcanzado éste.  
  
-   El programa externo del Correo electrónico de base de datos no está en ejecución y no se envía ningún mensaje.  
  
 Los mensajes pueden tener el estado **reintentando** por los siguientes motivos:  
  
-   El Correo electrónico de base de datos intentó enviar el mensaje, pero no se pudo contactar con el servidor de correo SMTP. El Correo electrónico de base de datos seguirá intentando enviar el mensaje con otras cuentas propias asignadas al perfil que envió el mensaje. Si ninguna cuenta puede enviar el correo, Correo electrónico de base de datos esperará durante el período de tiempo configurado para el parámetro de **intervalo entre reintentos de cuenta** y, a continuación, intentará volver a enviar el mensaje. Correo electrónico de base de datos usa el parámetro **reintentos de cuenta** para determinar cuántas veces se intentará enviar el mensaje. Los mensajes conservan el estado de **reintento** , siempre y cuando correo electrónico de base de datos esté intentando enviar el mensaje.  
  
 Utilice esta vista cuando desee ver cuántos mensajes hay en espera para ser enviados y cuánto tiempo llevan en la cola de correo. Normalmente, el número de mensajes sin **Enviar** será bajo. Realice una prueba comparativa durante las operaciones normales para determinar un número razonable de mensajes en la cola para las operaciones.  
  
 Para ver todos los mensajes procesados por Correo electrónico de base de datos, use [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md). Para ver solo los mensajes con el estado de error, use [sysmail_faileditems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md). Para ver solo los mensajes que se enviaron, use [sysmail_sentitems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|Identificador del elemento de correo en la cola de correo electrónico.|  
|**profile_id**|**int**|Identificador del perfil que se usa para enviar el mensaje.|  
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
|**send_request_user**|**sysname**|Usuario que envió el mensaje. Este es el contexto de usuario del procedimiento del correo electrónico de base de datos, no del campo **de** del mensaje.|  
|**sent_account_id**|**int**|Identificador de la cuenta del Correo electrónico de base de datos utilizada para enviar el mensaje. Siempre es NULL para esta vista.|  
|**sent_status**|**VARCHAR (8)**|Se **desenviará** Si correo electrónico de base de datos no ha intentado enviar el correo. Volverá a **intentarlo** Si correo electrónico de base de datos no pudo enviar el mensaje, pero lo intentará de nuevo.|  
|**sent_date**|**datetime**|Fecha y hora en las que el Correo electrónico de base de datos intentó por última vez enviar el mensaje. Será NULL si el Correo electrónico de base de datos no ha intentado enviar el mensaje.|  
|**last_mod_date**|**datetime**|Fecha y hora de la modificación más reciente de la fila.|  
|**last_mod_user**|**sysname**|Usuario que realizó la modificación más reciente de la fila.|  
  
## <a name="remarks"></a>Observaciones  
 Al solucionar problemas del Correo electrónico de base de datos, puede que esta vista le ayude a identificar la naturaleza del problema, pues en ella se muestra el número de mensajes a la espera de ser enviados y el tiempo que llevan esperando. Si no se está enviando ningún mensaje, puede que el programa externo del Correo electrónico de base de datos no esté en ejecución o que un problema de red impida que el Correo electrónico de base de datos se ponga en contacto con los servidores SMTP. Si muchos de los mensajes sin enviar tienen el mismo **profile_id**, puede haber un problema con el servidor SMTP. Considere la posibilidad de agregar cuentas adicionales al perfil. Si se envían los mensajes pero pasan demasiado tiempo en la cola, puede que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] necesite más recursos para procesar el volumen de mensajes que requiere.  
  
## <a name="permissions"></a>Permisos  
 Se concede al rol fijo de servidor **sysadmin** y al rol de base de datos **DatabaseMailUserRole** . Cuando lo ejecuta un miembro del rol fijo de servidor **sysadmin** , esta vista muestra todos los mensajes no **enviados** o de **reintento** . Todos los demás usuarios solo verán los mensajes no **enviados** o de **reintento** que enviaron.  
  
  
