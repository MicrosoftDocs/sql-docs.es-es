---
description: sys.dm_exec_distributed_requests (Transact-SQL)
title: sys.dm_exec_distributed_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_DISTRIBUTED_REQUESTS
- DM_EXEC_DISTRIBUTED_REQUESTS_TSQL
- SYS.DM_EXEC_DISTRIBUTED_REQUESTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- sys.dm_exec_distributed_sql_requests management view
- PolyBase
- dm_exec_distributed_sql_requests management view
ms.assetid: c041d416-d8c6-435e-a563-6a310abd33e3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b7ba034a1ed93fdc28631c7660295980d034e71a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193592"
---
# <a name="sysdm_exec_distributed_requests-transact-sql"></a>sys.dm_exec_distributed_requests (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Contiene información sobre todas las solicitudes que se encuentran actualmente o que están activas recientemente en las consultas de polybase. Muestra una fila por solicitud o consulta.  
  
 En función de la sesión y el identificador de solicitud, un usuario puede recuperar las solicitudes distribuidas reales generadas que se ejecutarán a través de sys.dm_exec_distributed_requests. Por ejemplo, una consulta que implique tablas SQL externas y normales se descomponerá en varias instrucciones o solicitudes ejecutadas en los distintos nodos de proceso. Para realizar un seguimiento de los pasos distribuidos en todos los nodos de proceso, se introduce un identificador de ejecución "global" que se puede usar para realizar el seguimiento de todas las operaciones en los nodos de proceso asociados a una solicitud y un operador determinados, respectivamente.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|sql_handle|**varbinary (64)**|Clave para esta vista. Identificador numérico único asociado a la solicitud.|Único en todas las solicitudes del sistema.|  
|execution_id|**nvarchar (32**|Identificador numérico único asociado a la sesión en la que se ejecutó esta consulta.||  
|status|**nvarchar (32**|Estado actual de la solicitud.|"Pending", "authoring", "AcquireSystemResources", "inicializando", "Plan", "Parse", "AquireResources", "Running", "canceling", "Complete", "Failed", "Canceled".|  
|error_id|**nvarchar (36)**|Identificador único del error asociado a la solicitud, si existe.|Se establece en NULL si no se produjo ningún error.|  
|start_time|**datetime**|Hora a la que se inició la ejecución de la solicitud.|0 para las solicitudes en cola; de lo contrario, la fecha y hora válidas es menor o igual que la hora actual.|  
|end_time|**datetime**|Hora a la que el motor completó la compilación de la solicitud.|Null para las solicitudes en cola o activas; de lo contrario, una fecha y hora válida menor o igual que la hora actual.|  
|total_elapsed_time|**int**|Tiempo transcurrido en la ejecución desde que se inició la solicitud, en milisegundos.|Entre 0 y la diferencia entre start_time y end_time. Si total_elapsed_time supera el valor máximo de un entero, total_elapsed_time seguirá siendo el valor máximo. Esta condición generará la advertencia "se ha superado el valor máximo". El valor máximo en milisegundos es equivalente a 24,8 días.|  
  
## <a name="see-also"></a>Consulte también  
 [Solución de problemas de polybase con vistas de administración dinámica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
