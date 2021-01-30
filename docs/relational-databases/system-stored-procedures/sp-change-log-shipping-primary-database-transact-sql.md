---
description: sp_change_log_shipping_primary_database (Transact-SQL)
title: sp_change_log_shipping_primary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_log_shipping_primary_database
- sp_change_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_primary_database
ms.assetid: 8c9dce6b-d2a3-4ca7-a832-8f59a5adb214
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e1bcd150ff810f5a4a16deb09a7ea72956ccd681
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181356"
---
# <a name="sp_change_log_shipping_primary_database-transact-sql"></a>sp_change_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia la configuración de la base de datos principal.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_change_log_shipping_primary_database [ @database = ] 'database'  
[, [ @backup_directory = ] 'backup_directory']   
[, [ @backup_share = ] 'backup_share']   
[, [ @backup_retention_period = ] 'backup_retention_period']  
[, [ @monitor_server_security_mode = ] 'monitor_server_security_mode']  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @backup_threshold = ] 'backup_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
[, [ @backup_compression = ] backup_compression_option ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @database = ] 'database'` Es el nombre de la base de datos del servidor principal. *primary_database* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @backup_directory = ] 'backup_directory'` Es la ruta de acceso a la carpeta de copia de seguridad del servidor principal. *backup_directory* es de tipo **nvarchar (500)**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @backup_share = ] 'backup_share'` Es la ruta de acceso de red al directorio de copia de seguridad del servidor principal. *backup_share* es de tipo **nvarchar (500)**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @backup_retention_period = ] 'backup_retention_period'` Es el período de tiempo, en minutos, que se conservará el archivo de copia de seguridad de registros en el directorio de copia de seguridad del servidor principal. *backup_retention_period* es de **tipo int**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @monitor_server_security_mode = ] 'monitor_server_security_mode'` El modo de seguridad usado para conectarse al servidor de supervisión.  
  
 1 = Autenticación de Windows.  
  
 0 = Autenticación de SQL Server.  
  
 *monitor_server_security_mode* es de **bit** y no puede ser null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Es el nombre de usuario de la cuenta usada para obtener acceso al servidor de supervisión.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Es la contraseña de la cuenta utilizada para obtener acceso al servidor de supervisión.  
  
`[ @backup_threshold = ] 'backup_threshold'` Es el período de tiempo, en minutos, después de la última copia de seguridad antes de que se produzca un error de *threshold_alert* . *backup_threshold* es de **tipo int** y su valor predeterminado es de 60 minutos.  
  
`[ @threshold_alert = ] 'threshold_alert'` La alerta que se generará cuando se supere el umbral de copia de seguridad. *threshold_alert* es de **tipo int** y no puede ser null.  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` Especifica si se genera una alerta cuando se supera *backup_threshold* .  
  
 1 = habilitada  
  
 0 = deshabilitada  
  
 *threshold_alert_enabled* es de **bit** y no puede ser null.  
  
`[ @history_retention_period = ] 'history_retention_period'` Es el período de tiempo en minutos en el que se conserva el historial. *history_retention_period* es de **tipo int**. Si no se especifica ninguno, se usa un valor de 14420.  
  
`[ @backup_compression = ] backup_compression_option` Especifica si la configuración del trasvase de registros utiliza [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md). Este parámetro solo se admite en [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (o una versión posterior).  
  
 0 = Deshabilitada. No se comprimen nunca las copias de seguridad de registros.  
  
 1 = Habilitada. Se comprimen siempre las copias de seguridad de registros.  
  
 2 = Utilice el valor de la [opción ver o establecer la opción de configuración del servidor compresión de copia de seguridad predeterminada](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Este es el valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_change_log_shipping_primary_database** se debe ejecutar desde la base de datos **maestra** del servidor principal. Este procedimiento almacenado hace lo siguiente:  
  
1.  Cambia la configuración del registro de **log_shipping_primary_database** , si es necesario.  
  
2.  Cambia el registro local de **log_shipping_monitor_primary** en el servidor principal con los argumentos proporcionados, si es necesario.  
  
3.  Si el servidor de supervisión es diferente del servidor principal, los cambios se registran en **log_shipping_monitor_primary** en el servidor de supervisión con los argumentos proporcionados, si es necesario.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se muestra el uso de **sp_change_log_shipping_primary_database** para actualizar la configuración asociada a la base de datos principal [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```  
EXEC master.dbo.sp_change_log_shipping_primary_database   
 @database = N'AdventureWorks'   
, @backup_directory = N'c:\LogShipping'   
, @backup_share = N'\\tribeca\LogShipping'   
, @backup_retention_period = 1440   
, @backup_threshold = 60   
, @threshold_alert = 0   
, @threshold_alert_enabled = 1   
, @history_retention_period = 1440   
,@monitor_server_security_mode = 1  
,@backup_compression = 1;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [log_shipping_primary_databases &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)  
  
  
