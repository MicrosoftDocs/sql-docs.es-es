---
description: sys.dm_exec_external_work (Transact-SQL)
title: sys.dm_exec_external_work (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/10/2021
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
ms.openlocfilehash: ba2068fa58bdf711deecf90612ee4bf1e47d374b
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622893"
---
# <a name="sysdm_exec_external_work-transact-sql"></a>sys.dm_exec_external_work (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

Devuelve información sobre la carga de trabajo por trabajador en cada nodo de proceso.  
  
Consulta `sys.dm_exec_external_work` para identificar el trabajo que se ha actualizado para comunicarse con el origen de datos externo (por ejemplo, Hadoop o MongoDB).  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|`nvarchar(32)`|Identificador único para la consulta de polybase asociada.|Vea *request_ID* en [Sys.dm_exec_requests &#40;&#41;de Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|`int`|Solicitud que está realizando este trabajador.|Vea *step_index* en  [Sys.dm_exec_requests &#40;&#41;de Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|dms_step_index|`int`|Paso del plan DMS en el que se está ejecutando este trabajador.|Vea [sys.dm_exec_dms_workers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md).|  
|compute_node_id|`int`|Nodo en el que se está ejecutando el trabajo.|Vea [sys.dm_exec_compute_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|type|`nvarchar(60)`|Tipo de trabajo externo.|' División de archivos ' (para Hadoop y Azure Storage)<br/><br/>' ODBC Data Split ' (para otros orígenes de datos externos) |  
|work_id|`int`|IDENTIFICADOR de la división real.|Mayor o igual que 0.|  
|input_name|`nvarchar(4000)`|Nombre de la entrada que se va a leer|Nombre de archivo (con path) cuando se usa Hadoop o Azure Storage. Para otros orígenes de datos externos, es la concatenación de la ubicación del origen de datos externo y la ubicación de la tabla externa: `scheme://DataSourceHostname[:port]/[DatabaseName.][SchemaName.]TableName`|  
|read_location|`bigint`|Desplazamiento de la ubicación de lectura.| `0` en el número de bytes en el archivo menos 1.<br/><br/>`NULL` para almacenamiento que no es de Hadoop o que no es de Azure. |  
|read_command|`nvarchar(4000)`|Consulta que se envía al origen de datos externo. Incluido en [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)].|Texto que representa la consulta. Para Hadoop y Azure Storage devuelve `NULL` .|
|bytes_processed|`bigint`|Número total de bytes asignados para el procesamiento de datos por parte de este trabajador. Este valor no puede representar necesariamente el total de datos devueltos por la consulta. |Mayor o igual que 0.|  
|length|`bigint`|Longitud de la división o el bloque HDFS para Hadoop|Definible por el usuario. El valor predeterminado es 64M|  
|status|`nvarchar(32)`|Estado del trabajador|Pending, Processing, done, failed, Aborted|  
|start_time|`datetime`|Inicio del trabajo||  
|end_time|`datetime`|Fin del trabajo||  
|total_elapsed_time|`int`|Tiempo total en milisegundos||
|compute_pool_id|`int`|Identificador único del grupo en el que se está ejecutando el trabajo. Solo se aplica a SQL Server clúster de Big Data. Vea [Sys.dm_exec_compute_pools (Transact-SQL)](sys-dm-exec-compute-pools.md).|Devuelve `0` para [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] en Windows y Linux.|

## <a name="see-also"></a>Consulte también  
 [Solución de problemas de polybase con vistas de administración dinámica](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
