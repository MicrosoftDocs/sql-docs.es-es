---
description: sys.dm_exec_compute_node_errors (Transact-SQL)
title: sys.dm_exec_compute_node_errors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.DM_EXEC_COMPUTE_NODE_ERRORS_TSQL
- DM_EXEC_COMPUTE_NODE_ERRORS
- DM_EXEC_COMPUTE_NODE_ERRORS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase
- PolyBase, views
- dm_exec_compute_node_errors
- sys.dm_exec_compute_node_errors management view
ms.assetid: 9a03c039-70e4-4974-95d8-d3fa45984ffb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f3b865b3b1b77371bd7dba30b5e61f0428efee24
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160092"
---
# <a name="sysdm_exec_compute_node_errors-transact-sql"></a>sys.dm_exec_compute_node_errors (Transact-SQL)

[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Devuelve los errores que se producen en los nodos de proceso de polybase.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|error_id|`nvarchar(36)`|Identificador numérico único asociado al error.|Único en todos los errores de consulta del sistema|  
|source|`nvarchar(255)`|Descripción del proceso o subproceso de origen||  
|tipo|`nvarchar(255)`|Tipo de error.||  
|create_time|`datetime`|La hora de la ocurrencia del error||  
|compute_node_id|`int`|Identificador del nodo de proceso específico|Vea compute_node_id de [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)|  
|rexecution_id|`nvarchar(36)`|Identificador de la consulta de polybase, si existe.||  
|spid|`int`|Identificador de la sesión de SQL Server||  
|thread_id|`int`|Identificador numérico del subproceso en el que se produjo el error.||  
|detalles|nvarchar(4000)|Descripción completa de los detalles del error.||
|compute_pool_id|`int`|Identificador único para el grupo.|

  
## <a name="see-also"></a>Consulte también  
 [Solución de problemas de polybase con vistas de administración dinámica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
