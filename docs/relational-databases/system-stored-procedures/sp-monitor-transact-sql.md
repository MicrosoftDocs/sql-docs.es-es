---
description: sp_monitor (Transact-SQL)
title: sp_monitor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_monitor_TSQL
- sp_monitor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_monitor
ms.assetid: cb628496-2f9b-40e4-b018-d0831c4cb018
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f046c3e6fe81c7dac489fcdd88237ecb2d3747d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195463"
---
# <a name="sp_monitor-transact-sql"></a>sp_monitor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra estadísticas sobre [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_monitor  
```  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|**last_run**|Hora **sp_monitor** la última vez que se ejecutó.|  
|**current_run**|Tiempo **sp_monitor** se está ejecutando.|  
|**segundos**|Número de segundos transcurridos desde que se ejecutó **sp_monitor** .|  
|**cpu_busy**|Número de segundos durante los que la CPU del equipo servidor ha realizado trabajos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**io_busy**|Número de segundos que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha invertido en realizar operaciones de E/S.|  
|**activos**|Número de segundos durante los que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha estado inactivo.|  
|**packets_received**|Número de paquetes de entrada que ha leído [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**packets_sent**|Número de paquetes de salida escritos por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**packet_errors**|Número de errores que ha encontrado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al leer y escribir paquetes.|  
|**total_read**|Número de lecturas que ha realizado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**total_write**|Número de escrituras que ha realizado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**total_errors**|Número de errores que ha encontrado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al leer y escribir.|  
|**connections**|Número de inicios de sesión o intentos de inicio de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="remarks"></a>Observaciones  
 A través de una serie de funciones, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza el seguimiento del trabajo que ha realizado. Al ejecutar **sp_monitor** se muestran los valores actuales devueltos por estas funciones y se muestra cuánto han cambiado desde la última vez que se ejecutó el procedimiento.  
  
 Para cada columna, la estadística se imprime con el formato *número*(*número*)-*número*% o *número*(*número*). El primer *número* hace referencia al número de segundos (por **cpu_busy**, **io_busy** e **inactivo**) o el número total (para las demás variables) desde que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se reinició. El *número* entre paréntesis hace referencia al número de segundos o al número total transcurridos desde la última vez que se ejecutó **sp_monitor** . El porcentaje es el porcentaje de tiempo transcurrido desde la última vez que se ejecutó **sp_monitor** . Por ejemplo, si el informe muestra **cpu_busy** como 4250 (215)-68%, la CPU ha estado ocupada 4250 segundos desde que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inició por última vez, 215 segundos desde la última vez que se ejecutó **sp_monitor** , y el 68% del tiempo total transcurrido desde la última vez que se ejecutó la **sp_monitor** .  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se informa acerca la ocupación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```console
USE master  
EXEC sp_monitor  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```console
last_run       current_run                   seconds
-----------    --------------------------    ---------
Mar 29 1998    11:55AM Apr 4 1998 2:22 PM    561

cpu_busy           io_busy     idle
---------------    ---------   --------------
190(0)-0%          187(0)-0%   148(556)-99%

packets_received       packets_sent    packet_errors
----------------       ------------    -------------
16(1)                  20(2)           0(0)

total_read     total_write   total_errors    connections
-----------    -----------   -------------   -----------
141(0)         54920(127)    0(0)            4(0)
```
  
## <a name="see-also"></a>Consulte también  
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
