---
description: sys.dm_exec_external_work (Transact-SQL)
title: sys.dm_exec_external_work (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_EXTERNAL_WORK
- DM_EXEC_EXTERNAL_WORK_TSQL
- SYS.DM_EXEC_EXTERNAL_WORK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_external_work management view
- dm_exec_external_work management view
- PolyBase,views
- PolyBase
ms.assetid: 7597d97b-1fde-4135-ac35-4af12968f300
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 10f7f8ae36a35a39a303d9548b828768282df3f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159991"
---
# <a name="sysdm_exec_external_work-transact-sql"></a>sys.dm_exec_external_work (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Devuelve información sobre la carga de trabajo por trabajador en cada nodo de proceso.  
  
 Consulte sys.dm_exec_external_work para identificar el trabajo que se ha actualizado para comunicarse con el origen de datos externo (por ejemplo, Hadoop o SQL Server externos).  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|`nvarchar(32)`|Identificador único para la consulta de polybase asociada.|Vea *request_ID* en [Sys.dm_exec_requests &#40;&#41;de Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|`int`|Solicitud que está realizando este trabajador.|Vea *step_index* en  [Sys.dm_exec_requests &#40;&#41;de Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|dms_step_index|`int`|Paso del plan DMS en el que se está ejecutando este trabajador.|Vea [sys.dm_exec_dms_workers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md).|  
|compute_node_id|`int`|Nodo en el que se está ejecutando el trabajo.|Vea [sys.dm_exec_compute_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|tipo|`nvarchar(60)`|Tipo de trabajo externo.|' División de archivos '|  
|work_id|`int`|IDENTIFICADOR de la división real.|Mayor o igual que 0.|  
|input_name|`nvarchar(4000)`|Nombre de la entrada que se va a leer|Nombre de archivo cuando se usa Hadoop.|  
|read_location|`bigint`|Desplazamiento o ubicación de lectura.|Desplazamiento del archivo que se va a leer.|  
|bytes_processed|`bigint`|Número total de bytes asignados para el procesamiento de datos por parte de este trabajador. Es posible que esto no represente necesariamente los datos totales devueltos por la consulta. |Mayor o igual que 0.|  
|length|`bigint`|Longitud de la división o el bloque HDFS en el caso de Hadoop|Definible por el usuario. El valor predeterminado es 64M|  
|status|`nvarchar(32)`|Estado del trabajador|Pending, Processing, done, failed, Aborted|  
|start_time|`datetime`|Inicio del trabajo||  
|end_time|`datetime`|Fin del trabajo||  
|total_elapsed_time|`int`|Tiempo total en milisegundos||
|compute_pool_id|`int`|Identificador único para el grupo.|

## <a name="see-also"></a>Consulte también  
 [Solución de problemas de polybase con vistas de administración dinámica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
