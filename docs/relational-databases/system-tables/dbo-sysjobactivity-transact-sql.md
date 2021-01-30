---
description: dbo.sysjobactivity (Transact-SQL)
title: dbo.sysjobactivity (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysjobactivity_TSQL
- dbo.sysjobactivity
- sysjobactivity
- sysjobactivity_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobactivity system table
ms.assetid: fd17cac9-5d1f-4b44-b2dc-ee9346d8bf1e
author: cawrites
ms.author: chadam
ms.openlocfilehash: c375367cdca6b7005d0c74f90654b7a6911070ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207027"
---
# <a name="dbosysjobactivity-transact-sql"></a>dbo.sysjobactivity (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Registra la actividad y el estado de los trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actual.  Esta tabla se almacena en la base de datos **msdb** .
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|IDENTIFICADOR de la sesión almacenada en la tabla **syssessions** de la base de datos **msdb** .|  
|**job_id**|**uniqueidentifier**|IDENTIFICADOR del trabajo.|  
|**run_requested_date**|**datetime**|Fecha y hora en que se solicitó la ejecución del trabajo.|  
|**run_requested_source**|**sysname(nvarchar(128))**|Solicitante de la ejecución del trabajo.<br /><br /> **1** = SOURCE_SCHEDULER<br /><br /> **2** = SOURCE_ALERTER<br /><br /> **3** = SOURCE_BOOT<br /><br /> **4** = SOURCE_USER<br /><br /> **6** = SOURCE_ON_IDLE_SCHEDULE|  
|**queued_date**|**datetime**|Fecha y hora en que el trabajo se puso en cola. Si el trabajo se ejecuta directamente, esta columna es NULL.|  
|**start_execution_date**|**datetime**|Fecha y hora de la programación de la ejecución del trabajo.|  
|**last_executed_step_id**|**int**|Id. del último paso de trabajo que se ejecutó.|  
|**last_executed_step_**<br /><br /> **date**|**datetime**|Fecha y hora en que empezó a ejecutarse el último paso de trabajo.|  
|**stop_execution_date**|**datetime**|Fecha y hora de finalización de la ejecución del trabajo.|  
|**job_history_id**|**int**|Se usa para identificar una fila en la tabla **sysjobhistory** .|  
|**next_scheduled_run_date**|**datetime**|Siguiente fecha y hora en que se ha programado la ejecución del trabajo.|  

## <a name="example"></a>Ejemplo
Este ejemplo devolverá el estado de tiempo de ejecución de todos los trabajos Agente SQL Server.  Ejecute la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].
```sql
SELECT sj.Name, 
    CASE
        WHEN sja.start_execution_date IS NULL THEN 'Not running'
        WHEN sja.start_execution_date IS NOT NULL AND sja.stop_execution_date IS NULL THEN 'Running'
        WHEN sja.start_execution_date IS NOT NULL AND sja.stop_execution_date IS NOT NULL THEN 'Not running'
    END AS 'RunStatus'
FROM msdb.dbo.sysjobs sj
JOIN msdb.dbo.sysjobactivity sja
ON sj.job_id = sja.job_id
WHERE session_id = (
    SELECT MAX(session_id) FROM msdb.dbo.sysjobactivity); 
```
  
## <a name="see-also"></a>Consulte también  
 [dbo.sysjobhistory &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
  
  
