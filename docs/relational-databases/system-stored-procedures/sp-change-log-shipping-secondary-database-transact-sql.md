---
description: sp_change_log_shipping_secondary_database (Transact-SQL)
title: sp_change_log_shipping_secondary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_log_shipping_secondary_database
- sp_change_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_database
ms.assetid: 3ebcf2f1-980f-4543-a84b-fbaeea54eeac
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 50383a1389c6327ae9caa6dcabf69bdcb8c720b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209358"
---
# <a name="sp_change_log_shipping_secondary_database-transact-sql"></a>sp_change_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia la configuración de la base de datos secundaria.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_change_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
[, [ @restore_delay = ] 'restore_delay']  
[, [ @restore_all = ] 'restore_all']  
[, [ @restore_mode = ] 'restore_mode']  
[, [ @disconnect_users = ] 'disconnect_users']  
[, [ @block_size = ] 'block_size']  
[, [ @buffer_count = ] 'buffer_count']  
[, [ @max_transfer_size = ] 'max_transfer_size']  
[, [ @restore_threshold = ] 'restore_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @restore_delay = ] 'restore_delay'` Cantidad de tiempo, en minutos, que el servidor secundario espera antes de restaurar un archivo de copia de seguridad determinado. *restore_delay* es de **tipo int** y no puede ser null. El valor predeterminado es 0.  
  
`[ @restore_all = ] 'restore_all'` Si se establece en 1, el servidor secundario restaura todas las copias de seguridad del registro de transacciones disponibles cuando se ejecuta el trabajo de restauración. De lo contrario, se detiene tras haber restaurado un archivo. *restore_all* es de **bit** y no puede ser null.  
  
`[ @restore_mode = ] 'restore_mode'` Modo de restauración para la base de datos secundaria.  
  
 0 = Restaurar registro con NORECOVERY.  
  
 1 = restaurar registro con STANDBY.  
  
 *restore* es de **bit** y no puede ser null.  
  
`[ @disconnect_users = ] 'disconnect_users'` Si se establece en 1, los usuarios se desconectarán de la base de datos secundaria cuando se realice una operación de restauración. Valor predeterminado = 0. *disconnect_users* es de **bit** y no puede ser null.  
  
`[ @block_size = ] 'block_size'` Tamaño, en bytes, que se utiliza como tamaño de bloque para el dispositivo de copia de seguridad. *BLOCK_SIZE* es de **tipo int** y su valor predeterminado es-1.  
  
`[ @buffer_count = ] 'buffer_count'` Número total de búferes utilizados por la operación de copia de seguridad o restauración. *buffer_count* es de **tipo int** y su valor predeterminado es-1.  
  
`[ @max_transfer_size = ] 'max_transfer_size'` Tamaño, en bytes, de la solicitud de entrada o salida máxima que emite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el dispositivo de copia de seguridad. *max_transfersize* es de **tipo int** y puede ser null.  
  
`[ @restore_threshold = ] 'restore_threshold'` Número de minutos que pueden transcurrir entre las operaciones de restauración antes de que se genere una alerta. *restore_threshold* es de **tipo int** y no puede ser null.  
  
`[ @threshold_alert = ] 'threshold_alert'` Es la alerta que se generará cuando se supere el umbral de restauración. *threshold_alert* es de **tipo int** y su valor predeterminado es 14421.  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` Especifica si se generará una alerta cuando se supere *restore_threshold*. 1 = habilitadas; 0 = deshabilitadas. *threshold_alert_enabled* es de **bit** y no puede ser null.  
  
`[ @history_retention_period = ] 'history_retention_period'` Es el período de tiempo en minutos en el que se retendrá el historial. *history_retention_period* es de **tipo int**. Si no se especifica ninguno, se usará un valor de 1440.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_change_log_shipping_secondary_database** se debe ejecutar desde la base de datos **maestra** en el servidor secundario. Este procedimiento almacenado hace lo siguiente:  
  
1.  Cambia la configuración del **log_shipping_secondary_database** registros según sea necesario.  
  
2.  Cambia el registro de supervisión local en **log_shipping_monitor_secondary** en el servidor secundario utilizando los argumentos proporcionados, si es necesario.  

## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se muestra el uso de **sp_change_log_shipping_secondary_database** para actualizar los parámetros de la base de datos secundaria para la base de datos **LogShipAdventureWorks**.  
  
```  
EXEC master.dbo.sp_change_log_shipping_secondary_database   
 @secondary_database =  'LogShipAdventureWorks'  
,  @restore_delay = 0  
,  @restore_all = 1  
,  @restore_mode = 0  
,  @disconnect_users = 0  
,  @threshold_alert = 14420  
,  @threshold_alert_enabled = 1  
,  @history_retention_period = 14420;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
