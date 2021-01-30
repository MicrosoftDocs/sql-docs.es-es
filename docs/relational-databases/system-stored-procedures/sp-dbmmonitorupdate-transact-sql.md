---
description: sp_dbmmonitorupdate (Transact-SQL)
title: sp_dbmmonitorupdate (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitorupdate
- sp_dbmmonitorupdate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorupdate
- database mirroring [SQL Server], monitoring
ms.assetid: 9ceb9611-4929-44ee-a406-c39ba2720fd5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7a18e87dd51da9869afa2b180a5fd55ef62c304e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203137"
---
# <a name="sp_dbmmonitorupdate-transact-sql"></a>sp_dbmmonitorupdate (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza la tabla de estado del monitor de creación de reflejos de la base de datos insertando una nueva fila de tabla para cada base de datos reflejada y trunca las filas más antiguas que el período de retención actual. El período de retención predeterminado es de 7 días (168 horas). Al actualizar la tabla, **sp_dbmmonitorupdate** evalúa las métricas de rendimiento.  
  
> [!NOTE]  
>  La primera vez que se ejecuta **sp_dbmmonitorupdate** , crea la tabla de estado de la creación de reflejo de la base de datos y el rol fijo de base de datos **dbm_monitor** en la base de datos **msdb** .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dbmmonitorupdate [ database_name ]  
```  
  
## <a name="arguments"></a>Argumentos  
 *database_name*  
 Nombre de la base de datos para la que se va a actualizar el estado de la creación de reflejos. Si no se especifica *database_name* , el procedimiento actualiza la tabla de estado para cada base de datos reflejada en la instancia del servidor.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_dbmmonitorupdate** solo se puede ejecutar en el contexto de la base de datos **msdb** .  
  
 Si una columna de la tabla de estado no se aplica al rol de un asociado, el valor es NULL en ese asociado. Una columna también tendrá un valor NULL si la información relevante no está disponible, como durante una conmutación por error o un reinicio del servidor.  
  
 Una vez que **sp_dbmmonitorupdate** crea el rol fijo de base de datos **dbm_monitor** en la base de datos **msdb** , los miembros del rol fijo de servidor **sysadmin** pueden agregar cualquier usuario al rol fijo de base de datos **dbm_monitor** . El rol **dbm_monitor** permite a sus miembros ver el estado de la creación de reflejo de la base de datos, pero no actualizarlo, pero no ver ni configurar eventos de creación de reflejo de la base de datos.  
  
 Al actualizar el estado de creación de reflejo de una base de datos, **sp_dbmmonitorupdate** inspecciona el último valor de cualquier métrica de rendimiento de creación de reflejo para la que se ha especificado un umbral de advertencia. Si el valor supera el umbral, el procedimiento agrega un evento informativo al registro de eventos. Todos los valores son promedios desde la última actualización. Para obtener más información, vea [Usar alertas y umbrales de advertencia de las métricas de rendimiento de la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se actualiza el estado de la creación de reflejos solo para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE msdb;  
EXEC sp_dbmmonitorupdate AdventureWorks2012 ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangealert &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)   
 [sp_dbmmonitorchangemonitoring &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitordropalert &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)   
 [sp_dbmmonitorhelpalert &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
  
