---
description: log_shipping_monitor_history_detail (Transact-SQL)
title: log_shipping_monitor_history_detail (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_monitor_history_detail_TSQL
- log_shipping_monitor_history_detail
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_history_detail system table
ms.assetid: 7080c888-323b-4206-a1ab-e6c51f9e2579
author: cawrites
ms.author: chadam
ms.openlocfilehash: a2b97aaf55c31f7ae79468d4cfa16fbff947f1fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186934"
---
# <a name="log_shipping_monitor_history_detail-transact-sql"></a>log_shipping_monitor_history_detail (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Almacena los detalles del historial para los trabajos de trasvase de registros. Esta tabla se almacena en la base de datos **msdb** .  
  
 Las tablas relacionadas con el historial y la supervisión también se utilizan en el servidor principal y los servidores secundarios.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**agent_id**|**uniqueidentifier**|Id. principal de la copia de seguridad o Id. secundario de la copia o restauración.|  
|**agent_type**|**tinyint**|Es el tipo de trabajo de trasvase de registros.<br /><br /> 0 = Copia de seguridad.<br /><br /> 1 = Copia.<br /><br /> 2 = Restauración.|  
|**session_id**|**int**|Id. de sesión para el trabajo de copia de seguridad, copia o restauración.|  
|**database_name**|**sysname**|Nombre de la base de datos asociada a este registro. Base de datos principal para copia de seguridad, base de datos secundaria para restauración o vacío para copia.|  
|**session_status**|**tinyint**|Estado de la sesión.<br /><br /> 0 = Iniciada.<br /><br /> 1 = En ejecución.<br /><br /> 2 = Correcta.<br /><br /> 3 = Error.<br /><br /> 4 = Advertencia.|  
|**log_time**|**datetime**|Fecha y hora en que se creó el registro.|  
|**log_time_utc**|**datetime**|Fecha y hora en que se creó el registro en hora universal coordinada.|  
|**message**|**nvarchar(max)**|Texto del mensaje.|  
  
## <a name="remarks"></a>Observaciones  
 Esta tabla contiene los detalles del historial para los agentes de trasvase de registros. Para identificar una sesión del agente, utilice las columnas **agent_id**, **agent_type** y **session_id**. Para ver los detalles del historial de la sesión del agente, ordene por **log_time**.  
  
 Además de almacenarse en el servidor de supervisión remoto, la información relacionada con el servidor principal se almacena en el servidor principal en la tabla de **log_shipping_monitor_history_detail** , y la información relacionada con un servidor secundario también se almacena en el servidor secundario de la tabla **log_shipping_monitor_history_detail** .  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_delete_log_shipping_primary_database &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_cleanup_log_shipping_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [Tablas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)   
 [log_shipping_monitor_error_detail &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)  
  
  
