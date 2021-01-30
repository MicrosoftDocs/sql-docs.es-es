---
description: log_shipping_primary_databases (Transact-SQL)
title: log_shipping_primary_databases (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_primary_databases
- log_shipping_primary_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_primary_databases system table
ms.assetid: 56888756-a798-42be-9b5e-0f9aa05a2cc6
author: cawrites
ms.author: chadam
ms.openlocfilehash: ba2f633ec533ea83a1dbbcb0bffb9136c776283b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186538"
---
# <a name="log_shipping_primary_databases-transact-sql"></a>log_shipping_primary_databases (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Almacena un registro para la base de datos principal en una configuración de trasvase de registros. Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**primary_id**|**uniqueidentifier**|Id. de la base de datos principal para la configuración del trasvase de registros.|  
|**primary_database**|**sysname**|Nombre de la base de datos principal en la configuración del trasvase de registros.|  
|**backup_directory**|**nvarchar (500)**|Directorio donde se almacenan los archivos de copia de seguridad de registros de transacciones del servidor principal.|  
|**backup_share**|**nvarchar (500)**|Ruta UNC o de red al directorio de copia de seguridad.|  
|**backup_retention_period**|**int**|Tiempo, en minutos, durante el que un archivo de copia de seguridad de registros se retiene en el directorio de copia de seguridad antes de eliminarse.|  
|**backup_job_id**|**uniqueidentifier**|Id. del trabajo del Agente [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asociado al trabajo de copia de seguridad en el servidor principal.|  
|**monitor_server**|**sysname**|Nombre de la instancia del [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que se utiliza como servidor de supervisión en la configuración del trasvase de registros.|  
|**monitor_server_security_mode**|**bit**|Modo de seguridad utilizado para conectarse al servidor de supervisión.<br /><br /> 1 = Autenticación de Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.|  
|**last_backup_file**|**nvarchar (500)**|Ruta de acceso absoluta de la copia de seguridad más reciente del registro de transacciones.|  
|**last_backup_date**|**datetime**|Fecha y hora de la última operación de copia de seguridad del registro.|  
|**user_specified_monitor**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> **sp_help_log_shipping_primary_database** y **sp_help_log_shipping_secondary_primary** utilizar esta columna para controlar la visualización de la configuración del monitor en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .<br /><br /> 0 = al invocar cualquiera de estos dos procedimientos almacenados, el usuario no especificó un valor explícito para el parámetro **\@ monitor_server** .<br /><br /> 1 = El usuario especificó un valor explícito.|  
|**backup_compression**|**tinyint**|Indica si la configuración de trasvase de registros invalida el comportamiento de compresión de copia de seguridad del nivel servidor.<br /><br /> 0 = Deshabilitada. Nunca se comprimen las copias de seguridad de registros, independientemente de la configuración de la compresión de copia de seguridad configurada por el servidor.<br /><br /> 1 = Habilitada. Las copias de seguridad de registros siempre se comprimen, independientemente de la configuración de la compresión de copia de seguridad configurada por el servidor.<br /><br /> 2 = usa la configuración del servidor para [ver o establecer la opción de configuración del servidor opciones](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) de configuración del servidor de compresión de copia de seguridad predeterminada. Este es el valor predeterminado.<br /><br /> La compresión de copia de seguridad solo se admite en la edición Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_primary_database &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [Tablas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
