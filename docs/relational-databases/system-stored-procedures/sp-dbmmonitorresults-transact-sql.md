---
description: sp_dbmmonitorresults (Transact-SQL)
title: sp_dbmmonitorresults (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitorresults
- sp_dbmmonitorresults_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorresults
- database mirroring [SQL Server], monitoring
ms.assetid: d575e624-7d30-4eae-b94f-5a7b9fa5427e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7af393da5482f8955c8e5eea20c9742ffb269baa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160614"
---
# <a name="sp_dbmmonitorresults-transact-sql"></a>sp_dbmmonitorresults (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve filas de estado para una base de datos supervisada de la tabla de estado en la que está almacenado el historial de supervisión de la creación de reflejo de la base de datos y permite elegir si el procedimiento obtiene antes el estado más reciente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dbmmonitorresults database_name   
   , rows_to_return  
    , update_status   
```  
  
## <a name="arguments"></a>Argumentos  
 *database_name*  
 Especifica la base de datos para la que se debe devolver el estado de la creación de reflejos.  
  
 *rows_to_return*  
 Especifica la cantidad de filas devueltas:  
  
 0 = Última fila  
  
 1 = Filas devueltas en las dos últimas horas  
  
 2 = Filas devueltas en las cuatro últimas horas  
  
 3 = Filas devueltas en las ocho últimas horas  
  
 4 = Filas devueltas el último día  
  
 5 = Filas devueltas los dos últimos días  
  
 6 = últimas 100 filas  
  
 7 = últimas 500 filas  
  
 8 = últimas 1.000 filas  
  
 9 = Último millón de filas  
  
 *update_status*  
 Especifica que, antes de devolver resultados, el procedimiento:  
  
 0 = No actualiza el estado de la base de datos. Los resultados se calculan utilizando únicamente las dos últimas filas, cuya antigüedad depende de cuándo se actualizó la tabla de estado.  
  
 1 = actualiza el estado de la base de datos mediante una llamada a **sp_dbmmonitorupdate** antes de calcular los resultados. Sin embargo, si la tabla de estado se ha actualizado en los 15 segundos anteriores o el usuario no es miembro del rol fijo de servidor **sysadmin** , **sp_dbmmonitorresults** se ejecuta sin actualizar el estado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Devuelve el número solicitado de filas de estado del historial para la base de datos especificada. Cada fila contiene la siguiente información:  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Nombre de una base de datos reflejada.|  
|**role**|**int**|Rol de creación de reflejos actual de la instancia del servidor:<br /><br /> 1 = Entidad de seguridad<br /><br /> 2 = Reflejo|  
|**mirroring_state**|**int**|Estado de la base de datos:<br /><br /> 0 = suspendido<br /><br /> 1 = desconectada<br /><br /> 2 = En proceso de sincronización<br /><br /> 3 = Pendiente de conmutación por error<br /><br /> 4 = Sincronizada|  
|**witness_status**|**int**|El estado de conexión del testigo en la sesión de creación de reflejo de la base de datos puede ser:<br /><br /> 0 = Desconocido<br /><br /> 1 = Conectado<br /><br /> 2 = Desconectado|  
|**log_generation_rate**|**int**|Cantidad (en kilobytes/seg.) de registro generado desde la actualización anterior del estado de la creación de reflejos de esta base de datos.|  
|**unsent_log**|**int**|Tamaño (en kilobytes) del registro no enviado en la cola de envío del servidor principal.|  
|**send_rate**|**int**|Tasa de envío (en kilobytes/seg.) del registro desde el servidor principal al servidor reflejado.|  
|**unrestored_log**|**int**|Tamaño (en kilobytes) de la cola de puesta al día en el servidor reflejado.|  
|**recovery_rate**|**int**|Tasa de puesta al día (en kilobytes/seg.) en el servidor reflejado.|  
|**transaction_delay**|**int**|Retardo total (en milisegundos) para todas las transacciones.|  
|**transactions_per_sec**|**int**|Número de transacciones por segundo en la instancia del servidor principal.|  
|**average_delay**|**int**|Retardo medio en la instancia del servidor principal para cada transacción a causa de la creación de reflejo de la base de datos. En modo de alto rendimiento (es decir, cuando se establece la propiedad SAFETY en OFF), este valor suele ser 0.|  
|**time_recorded**|**datetime**|Hora a la que la fila fue registrada por el monitor de creación de reflejo de la base de datos. Es la hora del reloj del sistema del servidor principal.|  
|**time_behind**|**datetime**|Hora aproximada del reloj del sistema del servidor principal a la que está asociada actualmente la base de datos reflejada. Este valor solo es significativo en la instancia del servidor principal.|  
|**local_time**|**datetime**|Hora del reloj del sistema en la instancia local del servidor a la que se actualizó esta fila.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_dbmmonitorresults** solo se puede ejecutar en el contexto de la base de datos **msdb** .  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** o al rol fijo de base de datos **dbm_monitor** en la base de datos **msdb** . El rol **dbm_monitor** permite a sus miembros ver el estado de la creación de reflejo de la base de datos, pero no actualizarlo, pero no ver ni configurar eventos de creación de reflejo de la base de datos.  
  
> [!NOTE]  
>  La primera vez que se ejecuta **sp_dbmmonitorupdate** , se crea el rol fijo de base de datos **dbm_monitor** en la base de datos **msdb** . Los miembros del rol fijo de servidor **sysadmin** pueden agregar cualquier usuario al rol fijo de base de datos **dbm_monitor** .  
  
## <a name="examples"></a>Ejemplos  
 El siguiente ejemplo devuelve las filas registradas durante las dos horas anteriores sin actualizar el estado de la base de datos.  
  
```  
USE msdb;  
EXEC sp_dbmmonitorresults AdventureWorks2012, 2, 0;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangemonitoring &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitoraddmonitoring &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)   
 [sp_dbmmonitordropmonitoring &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)  
  
  
