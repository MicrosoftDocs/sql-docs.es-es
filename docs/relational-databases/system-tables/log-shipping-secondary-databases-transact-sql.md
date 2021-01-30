---
description: log_shipping_secondary_databases (Transact-SQL)
title: log_shipping_secondary_databases (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_secondary_databases_TSQL
- log_shipping_secondary_databases
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_secondary_databases system table
ms.assetid: ba2374af-86b8-480c-a10c-51e7c4e3ae23
author: cawrites
ms.author: chadam
ms.openlocfilehash: b3323cf05b7c799205d8f5351874c3f4ea439afb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160568"
---
# <a name="log_shipping_secondary_databases-transact-sql"></a>log_shipping_secondary_databases (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Almacena un registro por cada base de datos secundaria en una configuración de trasvase de registros. Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**secondary_database**|**sysname**|Nombre de la base de datos secundaria en la configuración del trasvase de registros.|  
|**secondary_id**|**uniqueidentifier**|Id. del servidor secundario en la configuración del trasvase de registros.|  
|**restore_delay**|**int**|Cantidad de tiempo, en minutos, que el servidor secundario esperará antes de restaurar un archivo de copia de seguridad determinado. El valor predeterminado es 0 minutos.|  
|**restore_all**|**bit**|Si se establece en 1, el servidor secundario restaura todas las copias de seguridad del registro de transacciones disponibles cuando se ejecuta el trabajo de restauración. De lo contrario, se detiene tras haber restaurado un archivo.|  
|**restore_mode**|**bit**|Modo de restauración para la base de datos secundaria.<br /><br /> 0 = restore log with NORECOVERY.<br /><br /> 1 = Restaurar registro con STANDBY.|  
|**disconnect_users**|**bit**|Si se establece en 1, los usuarios se desconectarán de la base de datos secundaria cuando se realice una operación de restauración. El valor predeterminado es 0.|  
|**block_size**|**int**|Tamaño, en bytes, que se utiliza como tamaño de bloque para el dispositivo de copia de seguridad.|  
|**buffer_count**|**int**|Número total de búferes utilizados por la operación de copia de seguridad o restauración.|  
|**max_transfer_size**|**int**|Tamaño, en bytes, de la solicitud de entrada o salida máxima emitida por [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al dispositivo de copia de seguridad.|  
|**last_restored_file**|**nvarchar (500)**|Nombre del último archivo de copia de seguridad restaurado en la base de datos secundaria.|  
|**last_restored_date**|**datetime**|Fecha y hora de la última operación de restauración en la base de datos secundaria.|  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_secondary_database &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [sp_help_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)   
 [log_shipping_secondary &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-secondary-transact-sql.md)   
 [Tablas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
