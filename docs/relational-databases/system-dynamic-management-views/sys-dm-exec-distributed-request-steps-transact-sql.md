---
description: sys.dm_exec_distributed_request_steps (Transact-SQL)
title: sys.dm_exec_distributed_request_steps (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_distributed_request_steps
- sys.dm_exec_distributed_request_steps management view
ms.assetid: 1954541d-b716-4e03-8fcc-7022f428e01d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23992d8ed0c2974c78f3fcd5db3686683b9d6ee3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160061"
---
# <a name="sysdm_exec_distributed_request_steps-transact-sql"></a>sys.dm_exec_distributed_request_steps (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Contiene información sobre todos los pasos que componen una determinada solicitud o consulta de polybase. Muestra una fila por cada paso de consulta.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**int**|execution_id y step_index componen la clave para esta vista. Identificador numérico único asociado a la solicitud.|Vea ID en [sys.dm_exec_requests &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|**int**|La posición de este paso en la secuencia de pasos que componen la solicitud.|de 0 a (n-1) para una solicitud con n pasos.|  
|operation_type|**nvarchar(128)**|Tipo de la operación representada por este paso.|' MoveOperation ', ' Operation ', ' RandomIDOperation ', ' RemoteOperation ', ' ReturnOperation ', ' ShuffleMoveOperation ', ' TempTablePropertiesOperation ', ' DropDiagnosticsNotifyOperation ', ' HadoopShuffleOperation ', ' HadoopBroadCastOperation ', ' HadoopRoundRobinOperation '|  
|distribution_type|**nvarchar(32)**|Dónde se está ejecutando el paso.|' AllComputeNodes ', ' AllDistributions ', ' ComputeNode ', ' Distribution ', ' AllNodes ', ' SubsetNodes ', ' SubsetDistributions ', ' unespecifiqued '.|  
|location_type|**nvarchar(32)**|Dónde se está ejecutando el paso.|' Compute ', ' Head ' o ' DMS '. Todos los pasos de movimiento de datos muestran ' DMS '.|  
|status|**nvarchar(32)**|Estado de este paso|' Pending ', ' Running ', ' complete ', ' failed ', ' UndoFailed ', ' PendingCancel ', ' Canceled ', ' Undone ', ' Aborted '|  
|error_id|**nvarchar (36)**|Identificador único del error asociado a este paso, si lo hubiera.|Vea el ID. de [sys.dm_exec_compute_node_errors &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md), NULL si no se ha producido ningún error.|  
|start_time|**datetime**|Hora a la que se inició la ejecución del paso|Menor o igual que la hora actual y mayor o igual que end_compile_time de la consulta a la que pertenece este paso.|  
|end_time|**datetime**|Hora a la que se completó la ejecución de este paso, se canceló o dio error.|Menor o igual que la hora actual y mayor o igual que start_time, se establece en NULL para los pasos que se encuentran actualmente en ejecución o en cola.|  
|total_elapsed_time|**int**|Cantidad total de tiempo que se ha estado ejecutando el paso de la consulta, en milisegundos|Entre 0 y la diferencia entre end_time y start_time. 0 para los pasos en cola.|  
|row_count|**bigint**|Número total de filas cambiadas o devueltas por esta solicitud|0 para los pasos que no cambiaron o devuelven datos, número de filas afectadas de otro modo. Establézcalo en-1 para los pasos de DMS.|  
|.|nvarchar(4000)|Contiene el texto completo del comando de este paso.|Cualquier cadena de solicitud válida para un paso. Se trunca si hay más de 4000 caracteres.|  
  
## <a name="see-also"></a>Consulte también  
 [Solución de problemas de polybase con vistas de administración dinámica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
