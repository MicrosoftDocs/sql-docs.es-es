---
description: sp_add_log_shipping_primary_database (Transact-SQL)
title: sp_add_log_shipping_primary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_primary_database
- sp_add_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_primary_database
ms.assetid: 69531611-113f-46b5-81a6-7bf496d0353c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4c967127366c935b42ac33158982c6734114b2d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206722"
---
# <a name="sp_add_log_shipping_primary_database-transact-sql"></a>sp_add_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Configura la base de datos principal de una configuración de trasvase de registros, incluido el trabajo de copia de seguridad, el registro de monitor local y el registro de monitor remoto.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_log_shipping_primary_database [ @database = ] 'database',   
[ @backup_directory = ] 'backup_directory',   
[ @backup_share = ] 'backup_share',   
[ @backup_job_name = ] 'backup_job_name',   
[, [ @backup_retention_period = ] backup_retention_period]  
[, [ @monitor_server = ] 'monitor_server']  
[, [ @monitor_server_security_mode = ] monitor_server_security_mode]  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @backup_threshold = ] backup_threshold ]   
[, [ @threshold_alert = ] threshold_alert ]   
[, [ @threshold_alert_enabled = ] threshold_alert_enabled ]   
[, [ @history_retention_period = ] history_retention_period ]  
[, [ @backup_job_id = ] backup_job_id OUTPUT ]  
[, [ @primary_id = ] primary_id OUTPUT]  
[, [ @backup_compression = ] backup_compression_option ]  
  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @database = ] 'database'` Es el nombre de la base de datos principal de trasvase de registros. *Database* es de **tipo sysname**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @backup_directory = ] 'backup_directory'` Es la ruta de acceso a la carpeta de copia de seguridad del servidor principal. *backup_directory* es de tipo **nvarchar (500)**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @backup_share = ] 'backup_share'` Es la ruta de acceso de red al directorio de copia de seguridad del servidor principal. *backup_share* es de tipo **nvarchar (500)**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @backup_job_name = ] 'backup_job_name'` Es el nombre del trabajo de Agente SQL Server en el servidor principal que copia la copia de seguridad en la carpeta de copia de seguridad. *backup_job_name* es de **tipo sysname** y no puede ser null.  
  
`[ @backup_retention_period = ] backup_retention_period` Es el período de tiempo, en minutos, que se conservará el archivo de copia de seguridad de registros en el directorio de copia de seguridad del servidor principal. *backup_retention_period* es de **tipo int**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @monitor_server = ] 'monitor_server'` Es el nombre del servidor de supervisión. *Monitor_server* es de **tipo sysname**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @monitor_server_security_mode = ] monitor_server_security_mode` El modo de seguridad usado para conectarse al servidor de supervisión.  
  
 1 = Autenticación de Windows.  
  
 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. *monitor_server_security_mode* es de **bit** y no puede ser null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Es el nombre de usuario de la cuenta usada para obtener acceso al servidor de supervisión.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Es la contraseña de la cuenta utilizada para obtener acceso al servidor de supervisión.  
  
`[ @backup_threshold = ] backup_threshold` Es el período de tiempo, en minutos, después de la última copia de seguridad antes de que se produzca un error de *threshold_alert* . *backup_threshold* es de **tipo int** y su valor predeterminado es de 60 minutos.  
  
`[ @threshold_alert = ] threshold_alert` Es la alerta que se generará cuando se supere el umbral de copia de seguridad. *threshold_alert* es de **tipo int** y su valor predeterminado es 14.420.  
  
`[ @threshold_alert_enabled = ] threshold_alert_enabled` Especifica si se generará una alerta cuando se supere *backup_threshold* . El valor cero (0), valor predeterminado, significa que la alerta está deshabilitada y no se generará. *threshold_alert_enabled* es **bit**.  
  
`[ @history_retention_period = ] history_retention_period` Es el período de tiempo en minutos en el que se retendrá el historial. *history_retention_period* es de **tipo int** y su valor predeterminado es NULL. Si no se especifica ninguno, se usará un valor de 14420.  
  
`[ @backup_job_id = ] backup_job_id OUTPUT` El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ID. de trabajo del agente asociado al trabajo de copia de seguridad en el servidor principal. *backup_job_id* es de tipo **uniqueidentifier** y no puede ser null.  
  
`[ @primary_id = ] primary_id OUTPUT` IDENTIFICADOR de la base de datos principal para la configuración del trasvase de registros. *primary_id* es de tipo **uniqueidentifier** y no puede ser null.  
  
`[ @backup_compression = ] backup_compression_option` Especifica si la configuración del trasvase de registros utiliza [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md). Este parámetro solo se admite en [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (o una versión posterior).  
  
 0 = Deshabilitada. No se comprimen nunca las copias de seguridad de registros.  
  
 1 = Habilitada. Se comprimen siempre las copias de seguridad de registros.  
  
 2 = Utilice el valor de la [opción ver o establecer la opción de configuración del servidor compresión de copia de seguridad predeterminada](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Este es el valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_add_log_shipping_primary_database** se debe ejecutar desde la base de datos **maestra** del servidor principal. Este procedimiento almacenado realiza las siguientes funciones:  
  
1.  Genera un identificador principal y agrega una entrada para la base de datos principal en la tabla **log_shipping_primary_databases** con los argumentos proporcionados.  
  
2.  Crea un trabajo de copia de seguridad para la base de datos principal que está deshabilitada.  
  
3.  Establece el identificador de trabajo de copia de seguridad de la **log_shipping_primary_databases** entrada en el ID. de trabajo del trabajo de copia de seguridad.  
  
4.  Agrega un registro de monitor local en la tabla **log_shipping_monitor_primary** en el servidor principal con los argumentos proporcionados.  
  
5.  Si el servidor de supervisión es diferente del servidor principal, agrega un registro de monitor en **log_shipping_monitor_primary** en el servidor de supervisión con los argumentos proporcionados.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se agrega la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] como la base de datos principal en una configuración de trasvase de registros.  
  
```  
DECLARE @LS_BackupJobId AS uniqueidentifier ;  
DECLARE @LS_PrimaryId AS uniqueidentifier ;  
  
EXEC master.dbo.sp_add_log_shipping_primary_database   
@database = N'AdventureWorks'   
,@backup_directory = N'c:\lsbackup'   
,@backup_share = N'\\tribeca\lsbackup'   
,@backup_job_name = N'LSBackup_AdventureWorks'   
,@backup_retention_period = 1440  
,@monitor_server = N'rockaway'   
,@monitor_server_security_mode = 1   
,@backup_threshold = 60   
,@threshold_alert = 0   
,@threshold_alert_enabled = 0   
,@history_retention_period = 1440   
,@backup_job_id = @LS_BackupJobId OUTPUT   
,@primary_id = @LS_PrimaryId OUTPUT   
,@overwrite = 1   
,@backup_compression = 0;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
