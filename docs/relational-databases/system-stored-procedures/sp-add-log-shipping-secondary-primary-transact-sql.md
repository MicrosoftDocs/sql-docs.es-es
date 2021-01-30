---
description: sp_add_log_shipping_secondary_primary (Transact-SQL)
title: sp_add_log_shipping_secondary_primary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_secondary_primary_TSQL
- sp_add_log_shipping_secondary_primary
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_secondary_primary
ms.assetid: bfbbbee2-c255-4a59-a963-47d6e980a8e2
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7675de9f5d3e322511af474fcf9495dad3e4454a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179780"
---
# <a name="sp_add_log_shipping_secondary_primary-transact-sql"></a>sp_add_log_shipping_secondary_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Configura información principal, agrega vínculos al monitor local y remoto y crea trabajos de copia y restauración en el servidor secundario de la base de datos principal especificada.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_log_shipping_secondary_primary  
 [ @primary_server = ] 'primary_server',   
[ @primary_database = ] 'primary_database',  
[ @backup_source_directory = ] 'backup_source_directory' ,   
[ @backup_destination_directory = ] 'backup_destination_directory'  
[ @copy_job_name = ] 'copy_job_name'  
[ @restore_job_name = ] 'restore_job_name'  
[, [ @file_retention_period = ] 'file_retention_period']  
[, [ @monitor_server = ] 'monitor_server']  
[, [ @monitor_server_security_mode = ] 'monitor_server_security_mode']  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @copy_job_id = ] 'copy_job_id' OUTPUT ]  
[, [ @restore_job_id = ] 'restore_job_id' OUTPUT ]  
[, [ @secondary_id = ] 'secondary_id' OUTPUT]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @primary_server = ] 'primary_server'` Nombre de la instancia principal de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en la configuración del trasvase de registros. *primary_server* es de **tipo sysname** y no puede ser null.  
  
`[ @primary_database = ] 'primary_database'` Es el nombre de la base de datos del servidor principal. *primary_database* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @backup_source_directory = ] 'backup_source_directory'` Directorio donde se almacenan los archivos de copia de seguridad del registro de transacciones del servidor principal. *backup_source_directory* es de tipo **nvarchar (500)** y no puede ser null.  
  
`[ @backup_destination_directory = ] 'backup_destination_directory'` Directorio del servidor secundario donde se copian los archivos de copia de seguridad. *backup_destination_directory* es de tipo **nvarchar (500)** y no puede ser null.  
  
`[ @copy_job_name = ] 'copy_job_name'` Nombre que se va a utilizar para el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trabajo del agente que se va a crear para copiar las copias de seguridad del registro de transacciones en el servidor secundario. *copy_job_name* es de **tipo sysname** y no puede ser null.  
  
`[ @restore_job_name = ] 'restore_job_name'` Es el nombre del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trabajo del agente en el servidor secundario que restaura las copias de seguridad en la base de datos secundaria. *restore_job_name* es de **tipo sysname** y no puede ser null.  
  
`[ @file_retention_period = ] 'file_retention_period'` Período de tiempo, en minutos, que se conserva un archivo de copia de seguridad en el servidor secundario en la ruta de acceso especificada por el @backup_destination_directory parámetro antes de ser eliminado. *history_retention_period* es de **tipo int** y su valor predeterminado es NULL. Si no se especifica ninguno, se usará un valor de 14420.  
  
`[ @monitor_server = ] 'monitor_server'` Es el nombre del servidor de supervisión. *Monitor_server* es de **tipo sysname**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @monitor_server_security_mode = ] 'monitor_server_security_mode'` El modo de seguridad usado para conectarse al servidor de supervisión.  
  
 1 = Autenticación de Windows.  
  
 0 = Autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *monitor_server_security_mode* es de **bit** y no puede ser null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Es el nombre de usuario de la cuenta usada para obtener acceso al servidor de supervisión.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Es la contraseña de la cuenta utilizada para obtener acceso al servidor de supervisión.  
  
`[ @copy_job_id = ] 'copy_job_id' OUTPUT` IDENTIFICADOR asociado al trabajo de copia en el servidor secundario. *copy_job_id* es de tipo **uniqueidentifier** y no puede ser null.  
  
`[ @restore_job_id = ] 'restore_job_id' OUTPUT` IDENTIFICADOR asociado al trabajo de restauración en el servidor secundario. *restore_job_id* es de tipo **uniqueidentifier** y no puede ser null.  
  
`[ @secondary_id = ] 'secondary_id' OUTPUT` El ID. del servidor secundario en la configuración del trasvase de registros. *secondary_id* es de tipo **uniqueidentifier** y no puede ser null.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_add_log_shipping_secondary_primary** se debe ejecutar desde la base de datos **maestra** en el servidor secundario. Este procedimiento almacenado hace lo siguiente:  
  
1.  Genera un Id. secundario para el servidor principal y la base de datos principal especificados.  
  
2.  Hace lo siguiente:  

    1.  Agrega una entrada para el identificador secundario en **log_shipping_secondary** utilizando los argumentos proporcionados.  
  
    2.  Crea un trabajo de copia para el Id. secundario que está deshabilitado.  
  
    3.  Establece el identificador de trabajo de copia de la entrada **log_shipping_secondary** en el identificador de trabajo del trabajo de copia.  
  
    4.  Crea un trabajo de restauración para el Id. secundario que está deshabilitado.  
  
    5.  Establezca el identificador del trabajo de restauración en la entrada **log_shipping_secondary** en el ID. de trabajo del trabajo de restauración.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se muestra el uso del **sp_add_log_shipping_secondary_primary** procedimiento almacenado para configurar la información de la base de datos principal [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en el servidor secundario.  
  
```  
EXEC master.dbo.sp_add_log_shipping_secondary_primary   
@primary_server = N'TRIBECA'   
,@primary_database = N'AdventureWorks'   
,@backup_source_directory = N'\\tribeca\LogShipping'   
,@backup_destination_directory = N''   
,@copy_job_name = N''   
,@restore_job_name = N''   
,@file_retention_period = 1440   
,@monitor_server = N'ROCKAWAY'   
,@monitor_server_security_mode = 1   
,@copy_job_id = @LS_Secondary__CopyJobId OUTPUT   
,@restore_job_id = @LS_Secondary__RestoreJobId OUTPUT   
,@secondary_id = @LS_Secondary__SecondaryId OUTPUT ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
