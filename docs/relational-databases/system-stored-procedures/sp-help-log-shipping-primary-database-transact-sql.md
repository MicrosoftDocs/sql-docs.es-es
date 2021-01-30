---
description: sp_help_log_shipping_primary_database (Transact-SQL)
title: sp_help_log_shipping_primary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_primary_database_TSQL
- sp_help_log_shipping_primary_database
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_primary_database
ms.assetid: e711b01c-ef29-4eb6-a016-0e647e337818
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: bbae989332c0aa998699aacd4f8e9d16f61e0180
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200072"
---
# <a name="sp_help_log_shipping_primary_database-transact-sql"></a>sp_help_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Recupera parámetros de la base de datos principal.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_log_shipping_primary_database  
[ @database = ] 'database' OR  
[ @primary_id = ] 'primary_id'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @database = ] 'database'` Es el nombre de la base de datos principal de trasvase de registros. *Database* es de **tipo sysname**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @primary_id = ] 'primary_id'` IDENTIFICADOR de la base de datos principal para la configuración del trasvase de registros. *primary_id* es de tipo **uniqueidentifier** y no puede ser null.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|**primary_id**|Id. de la base de datos principal para la configuración del trasvase de registros.|  
|**primary_database**|Nombre de la base de datos principal en la configuración del trasvase de registros.|  
|**backup_directory**|Directorio donde se almacenan los archivos de copia de seguridad de registros de transacciones del servidor principal.|  
|**backup_share**|Ruta UNC o de red al directorio de copia de seguridad.|  
|**backup_retention_period**|Tiempo, en minutos, durante el que un archivo de copia de seguridad de registros se retiene en el directorio de copia de seguridad antes de eliminarse.|  
|**backup_compression**|Indica si la configuración del trasvase de registros utiliza la [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md).<br /><br /> **0** = deshabilitado. No se comprimen nunca las copias de seguridad de registros.<br /><br /> **1** = habilitada. Se comprimen siempre las copias de seguridad de registros.<br /><br /> **2** = utilice el valor de la [opción ver o establecer la opción de configuración del servidor compresión de copia de seguridad predeterminada](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Este es el valor predeterminado.<br /><br /> La compresión de copia de seguridad solo se admite en [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (o una versión posterior). En otras ediciones, el valor es 2 siempre.|  
|**backup_job_id**|Id. del trabajo del Agente [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asociado al trabajo de copia de seguridad en el servidor principal.|  
|**monitor_server**|Nombre de la instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que se utiliza como servidor de supervisión en la configuración del trasvase de registros.|  
|**monitor_server_security_mode**|Modo de seguridad utilizado para conectarse al servidor de supervisión.<br /><br /> 1 = Autenticación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.|  
|**backup_threshold**|Número de minutos permitido entre las operaciones de copia de seguridad antes de que se genere una alerta.|  
|**threshold_alert**|Alerta que se generará cuando se sobrepase el umbral de copia de seguridad.|  
|**threshold_alert_enabled**|Determina si las alertas de umbral de copia de seguridad están habilitadas.<br /><br /> **1** = habilitada.<br /><br /> **0** = deshabilitado.|  
|**last_backup_file**|Ruta de acceso absoluta de la copia de seguridad más reciente del registro de transacciones.|  
|**last_backup_date**|Fecha y hora de la última operación de copia de seguridad del registro.|  
|**last_backup_date_utc**|Fecha y hora de la última operación de copia de seguridad del registro de transacciones en la base de datos principal, expresadas en UTC (hora universal coordinada).|  
|**history_retention_period**|Cantidad de tiempo, en minutos, durante la que los registros de historial del trasvase de registros se mantienen en una base de datos principal determinada antes de ser eliminados.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_help_log_shipping_primary_database** se debe ejecutar desde la base de datos **maestra** del servidor principal.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se muestra el uso de **sp_help_log_shipping_primary_database** para recuperar la configuración de la base de datos principal para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```  
EXEC master.dbo.sp_help_log_shipping_primary_database @database=N'AdventureWorks2012';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
