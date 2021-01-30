---
description: sysmail_mailattachments (Transact-SQL)
title: sysmail_mailattachments (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_mailattachments_TSQL
- sysmail_mailattachments
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_mailattachments database mail view
ms.assetid: aee87059-a4c1-459a-a95c-641b4e3f0e73
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: adffc878cb36a7719748f8e5214bb23dab16fee2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199698"
---
# <a name="sysmail_mailattachments-transact-sql"></a>sysmail_mailattachments (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada archivo adjunto enviado al Correo electrónico de base de datos. Utilice esta vista cuando desee información acerca de los datos adjuntos del Correo electrónico de base de datos. Para revisar todos los mensajes de correo electrónico procesados por Correo electrónico de base de datos use [sysmail_allitems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**attachment_id**|**int**|Identificador de los datos adjuntos.|  
|**mailitem_id**|**int**|Identificador del elemento de correo que incluía los datos adjuntos.|  
|**filename**|**nvarchar (520)**|Nombre de archivo de los datos adjuntos. Cuando **attach_query_result** es 1 y **query_attachment_filename** es null, correo electrónico de base de datos crea un nombre de archivo arbitrario.|  
|**filesize**|**int**|Tamaño de los datos adjuntos en bytes.|  
|**vincula**|**varbinary(max)**|Contenido de los datos adjuntos.|  
|**last_mod_date**|**datetime**|Fecha y hora de la modificación más reciente de la fila.|  
|**last_mod_user**|**sysname**|Usuario que realizó la modificación más reciente de la fila.|  
  
## <a name="remarks"></a>Observaciones  
 Al solucionar problemas del Correo electrónico de base de datos, utilice esta vista para ver las propiedades de los datos adjuntos.  
  
 Los datos adjuntos almacenados en las tablas del sistema pueden hacer que la base de datos **msdb** crezca. Utilice **sysmail_delete_mailitems_sp** para eliminar elementos de correo y sus datos adjuntos asociados. Para obtener más información, vea [crear un trabajo de Agente SQL Server para archivar mensajes de correo electrónico de base de datos y registros de eventos](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Permisos  
 Se concede al rol fijo de servidor **sysadmin** y al rol de base de datos **DatabaseMailUserRole** . Cuando lo ejecuta un miembro del rol fijo de servidor **sysadmin** , esta vista muestra todos los datos adjuntos. Todos los demás usuarios verán únicamente los archivos adjuntos que envíen ellos mismos.  
  
## <a name="see-also"></a>Consulte también  
 [sysmail_allitems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_faileditems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [sysmail_sentitems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md)   
 [sysmail_unsentitems &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md)   
 [sysmail_event_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)  
  
  
