---
description: sp_help_log_shipping_monitor_secondary (Transact-SQL)
title: sp_help_log_shipping_monitor_secondary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_monitor_secondary
- sp_help_log_shipping_monitor_secondary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_monitor_secondary
ms.assetid: 3ac091ea-c9a8-4c05-a0b6-1ccf4e001339
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7a16b96c6edd21e2af3a78b5fa8f91e2c6b98880
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200055"
---
# <a name="sp_help_log_shipping_monitor_secondary-transact-sql"></a>sp_help_log_shipping_monitor_secondary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información relativa a una base de datos secundaria desde las tablas de supervisión.  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_log_shipping_monitor_secondary  
[ @secondary_server = ] 'secondary_server',  
[ @secondary_database = ] 'secondary_database'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @secondary_server = ] 'secondary_server'` Es el nombre del servidor secundario. *secondary_server* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @secondary_database = ] 'secondary_database'` Es el nombre de la base de datos secundaria. *secondary_database* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Columna|Descripción|  
|------------|-----------------|  
|**secondary_server**|Nombre de la instancia secundaria de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en la configuración del trasvase de registros.|  
|**secondary_database**|Nombre de la base de datos secundaria en la configuración del trasvase de registros.|  
|**secondary_id**|Id. del servidor secundario en la configuración del trasvase de registros.|  
|**primary_server**|Nombre de la instancia principal del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en la configuración del trasvase de registros.|  
|**primary_database**|Nombre de la base de datos principal en la configuración del trasvase de registros.|  
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
  
## <a name="remarks"></a>Observaciones  
 **sp_help_log_shipping_monitor_secondary** se debe ejecutar desde la base de datos **maestra** del servidor de supervisión.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="see-also"></a>Vea también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
