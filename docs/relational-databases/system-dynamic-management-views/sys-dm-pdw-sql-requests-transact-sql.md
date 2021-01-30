---
description: sys.dm_pdw_sql_requests (Transact-SQL)
title: sys.dm_pdw_sql_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 44e19609-902c-46cf-acdf-19ea75011365
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: fbb924edb6bd5d6b246ee986b7f600576a179a61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99138822"
---
# <a name="sysdm_pdw_sql_requests-transact-sql"></a>sys.dm_pdw_sql_requests (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene información sobre todos los SQL Server las distribuciones de consultas como parte de un paso de SQL en la consulta.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Identificador único de la consulta a la que pertenece esta distribución de consulta SQL.<br /><br /> request_id, step_index y distribution_id forman la clave de esta vista.|Vea request_id en [sys.dm_pdw_exec_requests &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Índice del paso de consulta del que forma parte esta distribución.<br /><br /> request_id, step_index y distribution_id forman la clave de esta vista.|Vea step_index en [sys.dm_pdw_request_steps &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|pdw_node_id|**int**|Identificador único del nodo en el que se ejecuta esta distribución de consulta.|Vea node_id en [sys.dm_pdw_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|distribution_id|**int**|Identificador único de la distribución en la que se ejecuta esta distribución de consultas.<br /><br /> request_id, step_index y distribution_id forman la clave de esta vista.|Vea distribution_id en [sys.pdw_distributions &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-distributions-transact-sql.md). Se establece en-1 para las solicitudes que se ejecutan en el ámbito del nodo, no en el ámbito de distribución.|  
|status|**nvarchar(32)**|Estado actual de la distribución de la consulta.|Pendiente, en ejecución, con errores, cancelado, completado, anulado, CancelSubmitted|  
|error_id|**nvarchar (36)**|Identificador único del error asociado a esta distribución de consulta, si existe.|Vea error_id en [sys.dm_pdw_errors &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md). Se establece en NULL si no se produjo ningún error.|  
|start_time|**datetime**|Hora a la que comenzó la distribución de la consulta.|Menor o igual que la hora actual y mayor o igual que start_time del paso de consulta al que pertenece esta distribución de consulta|  
|end_time|**datetime**|Hora a la que se completó la ejecución de la distribución de la consulta, se canceló o dio error.|Mayor o igual que la hora de inicio, o bien se establece en NULL si la distribución de consulta está en curso o en cola.|  
|total_elapsed_time|**int**|Representa el tiempo en milisegundos durante el que se ha estado ejecutando la distribución de la consulta.|Mayor o igual que 0. Igual a la diferencia de start_time y end_time para distribuciones de consulta completadas, con error o canceladas.<br /><br /> Si total_elapsed_time supera el valor máximo de un entero, total_elapsed_time seguirá siendo el valor máximo. Esta condición generará la advertencia "se ha superado el valor máximo".<br /><br /> El valor máximo en milisegundos es equivalente a 24,8 días.|  
|row_count|**bigint**|Número de filas modificadas o leídas por esta distribución de consulta.|-1 para las operaciones que no cambian o devuelven datos, como CREATE TABLE y DROP TABLE.|  
|spid|**int**|Identificador de sesión en la instancia de SQL Server que ejecuta la distribución de la consulta.||  
|.|**nvarchar(4000)**|Texto completo del comando para esta distribución de consulta.|Cualquier cadena de solicitud o consulta válida.|  
  
 Para obtener información acerca de las filas máximas retenidas en esta vista, consulte la sección de metadatos en el tema [límites de capacidad](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
