---
description: sys.dm_exec_background_job_queue_stats (Transact-SQL)
title: sys.dm_exec_background_job_queue_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_background_job_queue_stats
- sys.dm_exec_background_job_queue_stats
- dm_exec_background_job_queue_stats_TSQL
- sys.dm_exec_background_job_queue_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue_stats dynamic management view
ms.assetid: 27f62ab5-46c4-417e-814d-8d6437034d1c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e75e8e70e8499c201a00a08fd9634f6abd8288e0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160134"
---
# <a name="sysdm_exec_background_job_queue_stats-transact-sql"></a>sys.dm_exec_background_job_queue_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila que proporciona estadísticas acumuladas para cada trabajo del procesador de consultas enviado para ejecución asincrónica (en segundo plano).  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_exec_background_job_queue_stats**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**queue_max_len**|**int**|Longitud máxima de la cola.|  
|**enqueued_count**|**int**|Número de solicitudes registradas correctamente en la cola.|  
|**started_count**|**int**|Número de solicitudes que han iniciado la ejecución.|  
|**ended_count**|**int**|Número de solicitudes a las que se ha dado servicio como correctas o erróneas.|  
|**failed_lock_count**|**int**|Número de solicitudes que han generado errores a causa de contención de bloqueos o interbloqueos.|  
|**failed_other_count**|**int**|Número de solicitudes que han generado errores a causa de otras razones.|  
|**failed_giveup_count**|**int**|Número de solicitudes que han generado errores porque se ha alcanzado el límite de reintentos.|  
|**enqueue_failed_full_count**|**int**|Número de intentos de poner en cola erróneos porque la cola está llena.|  
|**enqueue_failed_duplicate_count**|**int**|Número de intentos de poner en cola duplicados.|  
|**elapsed_avg_ms**|**int**|Tiempo promedio transcurrido por solicitud en milisegundos.|  
|**elapsed_max_ms**|**int**|Tiempo transcurrido de la solicitud más grande en milisegundos.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="remarks"></a>Observaciones  
 Esta vista devuelve información solo para los trabajos de estadísticas de actualización asincrónica. Para obtener más información sobre las estadísticas de actualización asincrónica, vea [estadísticas](../../relational-databases/statistics/statistics.md).  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="examples"></a>Ejemplos  
  
### <a name="a-determining-the-percentage-of-failed-background-jobs"></a>A. Determinar el porcentaje de trabajos en segundo plano que no han podido completarse  
 En el ejemplo siguiente se devuelve el porcentaje de trabajos en segundo plano que no han podido completarse para todas las consultas ejecutadas.  
  
```  
SELECT   
        CASE ended_count WHEN 0   
                THEN 'No jobs ended'   
                ELSE CAST((failed_lock_count + failed_giveup_count + failed_other_count) / CAST(ended_count AS float) * 100 AS varchar(20))   
        END AS [Percent Failed]  
FROM sys.dm_exec_background_job_queue_stats;  
GO  
```  
  
### <a name="b-determining-the-percentage-of-failed-enqueue-attempts"></a>B. Determinar el porcentaje de intentos de poner en cola que no han tenido éxito  
 En el ejemplo siguiente se devuelve el porcentaje de intentos de poner en cola que no han tenido éxito para todas las consultas ejecutadas.  
  
```  
SELECT   
        CASE enqueued_count WHEN 0   
                THEN 'No jobs posted'   
                ELSE CAST((enqueue_failed_full_count + enqueue_failed_duplicate_count) / CAST(enqueued_count + enqueue_failed_full_count + enqueue_failed_duplicate_count AS float) * 100 AS varchar(20))   
        END AS [Percent Enqueue Failed]  
FROM sys.dm_exec_background_job_queue_stats;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con ejecuciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


