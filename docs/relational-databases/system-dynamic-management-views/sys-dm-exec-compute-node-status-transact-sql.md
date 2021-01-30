---
description: sys.dm_exec_compute_node_status (Transact-SQL)
title: sys.dm_exec_compute_node_status (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_COMPUTE_NODE_STATUS_TSQL
- DM_EXEC_COMPUTE_NODE_STATUS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_compute_node_status
- sys.dm_exec_compute_node_status management view
ms.assetid: b606f91f-3a08-4a4f-bb57-32ae155b3738
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b3376db130372d5004d9b7724ac8ca5f491e136f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99106301"
---
# <a name="sysdm_exec_compute_node_status-transact-sql"></a>sys.dm_exec_compute_node_status (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Contiene información adicional sobre el rendimiento y el estado de todos los nodos de polybase. Muestra una fila por nodo.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|compute_node_id|`int`|Identificador numérico único asociado al nodo.|Único en todo el clúster de escalado horizontal, independientemente del tipo.|  
|process_id|`int`|||  
|process_name|`nvarchar(255)`|Nombre lógico del nodo.|Cualquier cadena de longitud adecuada.|  
|allocated_memory|`bigint`|Memoria total asignada en este nodo.||  
|available_memory|`bigint`|Memoria total disponible en este nodo.||  
|process_cpu_usage|`bigint`|Uso total de la CPU del proceso, en pasos.||  
|total_cpu_usage|`bigint`|Uso total de CPU, en pasos.||  
|thread_count|`bigint`|Número total de subprocesos en uso en este nodo.||  
|handle_count|`bigint`|Número total de identificadores en uso en este nodo.||  
|total_elapsed_time|`bigint`|Tiempo total transcurrido desde el inicio o el reinicio del sistema.|Tiempo total transcurrido desde el inicio o el reinicio del sistema. Si total_elapsed_time supera el valor máximo de un entero (24,8 días en milisegundos), se producirá un error de materialización debido al desbordamiento. El valor máximo en milisegundos es equivalente a 24,8 días.|  
|is_available|`bit`|Marca que indica si este nodo está disponible.||  
|sent_time|`datetime`|Última vez que envió un paquete de red||  
|received_time|`datetime`|Última vez que este nodo envió un paquete de red.||  
|error_id|`nvarchar(36)`|Identificador único del último error que se produjo en este nodo.||
|compute_pool_id|`int`|Identificador único para el grupo.|

## <a name="see-also"></a>Consulte también  
 [Solución de problemas de polybase con vistas de administración dinámica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
