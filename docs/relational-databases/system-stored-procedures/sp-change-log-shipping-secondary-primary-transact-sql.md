---
description: sp_change_log_shipping_secondary_primary (Transact-SQL)
title: sp_change_log_shipping_secondary_primary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_log_shipping_secondary_primary
- sp_change_log_shipping_secondary_primary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_primary
ms.assetid: 5bcb4df7-6df3-4f2b-9207-b97b5addf2a6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8352706512e71a5cba3a0b22befe2b7416d276bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209350"
---
# <a name="sp_change_log_shipping_secondary_primary-transact-sql"></a>sp_change_log_shipping_secondary_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia la configuración de la base de datos secundaria.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_change_log_shipping_secondary_primary  
[ @primary_server = ] 'primary_server',  
[ @primary_database = ] 'primary_database',  
[, [ @backup_source_directory = ] 'backup_source_directory']  
[, [ @backup_destination_directory = ] 'backup_destination_directory']  
[, [ @file_retention_period = ] file_retention_period]  
[, [ @monitor_server_security_mode = ] monitor_server_security_mode]  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @primary_server = ] 'primary_server'` Nombre de la instancia principal de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en la configuración del trasvase de registros. *primary_server* es de **tipo sysname** y no puede ser null.  
  
`[ @primary_database = ] 'primary_database'` Es el nombre de la base de datos del servidor principal. *primary_database* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @backup_source_directory = ] 'backup_source_directory'` Directorio donde se almacenan los archivos de copia de seguridad del registro de transacciones del servidor principal. *backup_source_directory* es de tipo **nvarchar (500)** y no puede ser null.  
  
`[ @backup_destination_directory = ] 'backup_destination_directory'` Directorio del servidor secundario donde se copian los archivos de copia de seguridad. *backup_destination_directory* es de tipo **nvarchar (500)** y no puede ser null.  
  
`[ @file_retention_period = ] 'file_retention_period'` Es el período de tiempo en minutos en el que se retendrá el historial. *history_retention_period* es de **tipo int** y su valor predeterminado es NULL. Si no se especifica ninguno, se usará un valor de 14420.  
  
`[ @monitor_server_security_mode = ] 'monitor_server_security_mode'` El modo de seguridad usado para conectarse al servidor de supervisión.  
  
 1 = Autenticación de Windows;  
  
 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación. *monitor_server_security_mode* es de **bit** y no puede ser null.  
  
`[ @monitor_server_login = ] 'monitor_server_login'` Es el nombre de usuario de la cuenta usada para obtener acceso al servidor de supervisión.  
  
`[ @monitor_server_password = ] 'monitor_server_password'` Es la contraseña de la cuenta utilizada para obtener acceso al servidor de supervisión.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_change_log_shipping_secondary_primary** se debe ejecutar desde la base de datos **maestra** en el servidor secundario. Este procedimiento almacenado hace lo siguiente:  
  
1.  Cambia la configuración del **log_shipping_secondary** registros según sea necesario.  
  
2.  Si el servidor de supervisión es diferente del servidor secundario, cambia el registro del monitor en **log_shipping_monitor_secondary** en el servidor de supervisión con los argumentos proporcionados, si es necesario.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
