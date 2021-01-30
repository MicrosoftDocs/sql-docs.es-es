---
description: sp_help_log_shipping_secondary_database (Transact-SQL)
title: sp_help_log_shipping_secondary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_secondary_database
- sp_help_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_secondary_database
ms.assetid: 11ce42ca-d3f1-44c8-9cac-214ca8896b9a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5fc81b903438d874a80399563527747ada9ef43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199632"
---
# <a name="sp_help_log_shipping_secondary_database-transact-sql"></a>sp_help_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Este procedimiento almacenado recupera la configuración de una o más bases de datos secundarias.  
  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database' OR  
[ @secondary_id = ] 'secondary_id'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @secondary_database = ] 'secondary_database'` Es el nombre de la base de datos secundaria. *secondary_database* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @secondary_id = ] 'secondary_id'` El ID. del servidor secundario en la configuración del trasvase de registros. *secondary_id* es de tipo **uniqueidentifier** y no puede ser null.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|**secondary_id**|Id. del servidor secundario en la configuración del trasvase de registros.|  
|**primary_server**|Nombre de la instancia principal del [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en la configuración del trasvase de registros.|  
|**primary_database**|Nombre de la base de datos principal en la configuración del trasvase de registros.|  
|**backup_source_directory**|Directorio donde se almacenan los archivos de copia de seguridad de registros de transacciones del servidor principal.|  
|**backup_destination_directory**|Directorio del servidor secundario donde se copian los archivos de copia de seguridad.|  
|**file_retention_period**|Tiempo, en minutos, durante el que un archivo de copia de seguridad se retiene en el servidor secundario antes de eliminarse.|  
|**copy_job_id**|Id. asociado al trabajo de copia en el servidor secundario.|  
|**restore_job_id**|Id. asociado al trabajo de restauración en el servidor secundario.|  
|**monitor_server**|Nombre de la instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que se utiliza como servidor de supervisión en la configuración del trasvase de registros.|  
|**monitor_server_security_mode**|Modo de seguridad utilizado para conectarse al servidor de supervisión.<br /><br /> 1 = Autenticación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.|  
|**secondary_database**|Nombre de la base de datos secundaria en la configuración del trasvase de registros.|  
|**restore_delay**|Cantidad de tiempo, en minutos, que espera el servidor secundario antes de restaurar un archivo de copia de seguridad dado. El valor predeterminado es 0 minutos.|  
|**restore_all**|Si se establece en 1, el servidor secundario restaura todas las copias de seguridad disponibles del registro de transacciones cuando se ejecuta el trabajo de restauración. De lo contrario, se detiene tras haber restaurado un archivo.|  
|**restore_mode**|Modo de restauración para la base de datos secundaria.<br /><br /> 0 = restore log with NORECOVERY.<br /><br /> 1 = Restaurar registro con STANDBY.|  
|**disconnect_users**|Si se establece en 1, los usuarios se desconectarán de la base de datos secundaria cuando se realice una operación de restauración. Valor predeterminado = 0.|  
|**block_size**|Tamaño, en bytes, que se utiliza como tamaño de bloque para el dispositivo de copia de seguridad.|  
|**buffer_count**|Número total de búferes utilizados por la operación de copia de seguridad o restauración.|  
|**max_transfer_size**|Tamaño, en bytes, de la solicitud de entrada o salida máxima emitida por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al dispositivo de copia de seguridad.|  
|**restore_threshold**|Número de minutos permitido entre las operaciones de restauración antes de que se genere una alerta.|  
|**threshold_alert**|Alerta que se generará cuando se sobrepase el umbral de restauración.|  
|**threshold_alert_enabled**|Determina si las alertas de umbral de restauración están habilitadas.<br /><br /> 1 = Habilitada.<br /><br /> 0 = Deshabilitada.|  
|**last_copied_file**|Nombre del último archivo de copia de seguridad copiado en el servidor secundario.|  
|**last_copied_date**|Fecha y hora de la última operación de copia en el servidor secundario.|  
|**last_copied_date_utc**|Fecha y hora de la última operación de copia en el servidor secundario, expresadas en hora universal coordinada (UTC).|  
|**last_restored_file**|Nombre del último archivo de copia de seguridad restaurado en la base de datos secundaria.|  
|**last_restored_date**|Fecha y hora de la última operación de restauración en la base de datos secundaria.|  
|**last_restored_date_utc**|Fecha y hora de la última operación de restauración en la base de datos secundaria, expresadas en hora universal coordinada (UTC).|  
|**history_retention_period**|Cantidad de tiempo, en minutos, durante la que los registros de historial del trasvase de registros se mantienen en una base de datos secundaria determinada antes de ser eliminados.|  
|**last_restored_latency**|Período de tiempo, en minutos, transcurrido desde que se creó la copia de seguridad de registros en el servidor primario hasta que se restauró en el servidor secundario.<br /><br /> El valor inicial es NULL.|  
  
## <a name="remarks"></a>Observaciones  
 Si incluye el parámetro *secondary_database* , el conjunto de resultados contendrá información acerca de esa base de datos secundaria; Si incluye el parámetro *secondary_id* , el conjunto de resultados contendrá información acerca de todas las bases de datos secundarias asociadas a ese ID. secundario.  
  
 **sp_help_log_shipping_secondary_database** se debe ejecutar desde la base de datos **maestra** en el servidor secundario.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="see-also"></a>Consulte también  
 [sp_help_log_shipping_secondary_primary &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)   
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
