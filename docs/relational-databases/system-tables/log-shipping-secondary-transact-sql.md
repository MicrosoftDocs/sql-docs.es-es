---
description: log_shipping_secondary (Transact-SQL)
title: log_shipping_secondary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_secondary
- log_shipping_secondary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_secondary system table
ms.assetid: 69723419-4544-49c6-a517-adb30ffa5741
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0b04537e3559828764eac63f4cea3d46afd6f49e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160576"
---
# <a name="log_shipping_secondary-transact-sql"></a>log_shipping_secondary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Almacena un registro por cada identificador secundario. Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**secondary_id**|**uniqueidentifier**|Id. del servidor secundario en la configuración del trasvase de registros.|  
|**primary_server**|**sysname**|Nombre de la instancia principal del motor de base de datos de SQL Server en la configuración del trasvase de registros.|  
|**primary_database**|**sysname**|Nombre de la base de datos principal en la configuración del trasvase de registros.|  
|**backup_source_directory**|**nvarchar (500)**|Directorio donde se almacenan los archivos de copia de seguridad de registros de transacciones del servidor principal.|  
|**backup_destination_directory**|**nvarchar (500)**|Directorio del servidor secundario donde se copian los archivos de copia de seguridad.|  
|**file_retention_period**|**int**|Tiempo, en minutos, durante el que un archivo de copia de seguridad se retiene en el servidor secundario antes de eliminarse.|  
|**copy_job_id**|**uniqueidentifier**|Id. asociado al trabajo de copia en el servidor secundario.|  
|**restore_job_id**|**uniqueidentifier**|Id. asociado al trabajo de restauración en el servidor secundario.|  
|**monitor_server**|**sysname**|Nombre de la instancia del [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que se utiliza como servidor de supervisión en la configuración del trasvase de registros.|  
|**monitor_server_security_mode**|**bit**|Modo de seguridad utilizado para conectarse al servidor de supervisión.<br /><br /> 1 = Autenticación de Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.|  
|**last_copied_file**|**nvarchar (500)**|Nombre del último archivo de copia de seguridad copiado en el servidor secundario.|  
|**last_copied_date**|**datetime**|Fecha y hora de la última operación de copia en el servidor secundario.|  
  
## <a name="remarks"></a>Observaciones  
 Varias bases de datos secundarias del mismo servidor secundario de una base de datos principal comparten algunas opciones de configuración en la tabla **log_shipping_secondary** . Si se modifica un valor compartido en alguna de ellas, este valor se modifica para todas.  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_secondary_database &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md)   
 [sp_change_log_shipping_secondary_database &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-log-shipping-secondary-database-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [sp_help_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)   
 [Tablas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
