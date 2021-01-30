---
description: sys.dm_pdw_dms_workers (Transact-SQL)
title: sys.dm_pdw_dms_workers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0a284d18-3c46-4ffa-bcc9-689e660ee8b4
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 735ab65bc39b4f4ffab93f1b2bf8a705c3d895c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99143263"
---
# <a name="sysdm_pdw_dms_workers-transact-sql"></a>sys.dm_pdw_dms_workers (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene información sobre todos los trabajadores que completan los pasos de DMS.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Consulta de la que forma parte este trabajador de DMS.<br /><br /> request_id, step_index y dms_step_index forman la clave de esta vista.|Vea request_id en [sys.dm_pdw_exec_requests &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Paso de consulta del que forma parte este trabajador de DMS.<br /><br /> request_id, step_index y dms_step_index forman la clave de esta vista.|Vea step_index en [sys.dm_pdw_request_steps &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|dms_step_index|**int**|Paso del plan DMS en el que se ejecuta este trabajador.<br /><br /> request_id, step_index y dms_step_index forman la clave de esta vista.||  
|pdw_node_id|**int**|Nodo en el que se está ejecutando el trabajo.|Vea node_id en [sys.dm_pdw_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|distribution_id|**Int**|Distribución en la que se ejecuta el trabajador, si existe.|Vea distribution_id en [sys.pdw_distributions &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-distributions-transact-sql.md).|  
|tipo|**nvarchar(32)**|Tipo de subproceso de trabajo de DMS que esta entrada representa.|' DIRECT_CONVERTER ', ' DIRECT_READER ', ' FILE_READER ', ' HASH_CONVERTER ', ' HASH_READER ', ' ROUNDROBIN_CONVERTER ', ' EXPORT_READER ', ' EXTERNAL_READER ', ' EXTERNAL_WRITER ', ' PARALLEL_COPY_READER ', ' REJECT_WRITER ', ' ESCRITOR '|  
|status|**nvarchar(32)**|Estado del trabajo de DMS.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|bytes_per_sec|**bigint**|Rendimiento de lectura o escritura en el último segundo.|Mayor o igual que 0. ES NULL si la consulta se canceló o produjo un error antes de que se pudiera ejecutar el trabajo.|  
|bytes_processed|**bigint**|Número total de bytes procesados por este trabajador.|Mayor o igual que 0. ES NULL si la consulta se canceló o produjo un error antes de que se pudiera ejecutar el trabajo.|  
|rows_processed|**bigint**|Número de filas leídas o escritas para este trabajador.|Mayor o igual que 0. ES NULL si la consulta se canceló o produjo un error antes de que se pudiera ejecutar el trabajo.|  
|start_time|**datetime**|Hora a la que se inició la ejecución de este trabajador.|Mayor o igual que la hora de inicio del paso de consulta al que pertenece este trabajador. Vea [sys.dm_pdw_request_steps &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|end_time|**datetime**|Hora a la que finalizó la ejecución, se produjo un error o se canceló.|NULL para los trabajos en cola o en curso. De lo contrario, mayor que start_time.|  
|total_elapsed_time|**int**|Tiempo total invertido en la ejecución, en milisegundos.|Mayor o igual que 0.<br /><br /> Tiempo total transcurrido desde el inicio o el reinicio del sistema. Si total_elapsed_time supera el valor máximo de un entero (24,8 días en milisegundos), se producirá un error de materialización debido al desbordamiento.<br /><br /> El valor máximo en milisegundos es equivalente a 24,8 días.|  
|cpu_time|**bigint**|Tiempo de CPU consumido por este trabajo, en milisegundos.|Mayor o igual que 0.|  
|query_time|**int**|Período de tiempo antes de que SQL comience a devolver filas al subproceso, en milisegundos.|Mayor o igual que 0.|  
|buffers_available|**int**|Número de búferes sin usar.| ES NULL si la consulta se canceló o produjo un error antes de que se pudiera ejecutar el trabajo.|  
|sql_spid|**int**|Identificador de sesión en la instancia de SQL Server que realiza el trabajo para este trabajo de DMS.||  
|dms_cpid|**int**|IDENTIFICADOR de proceso del subproceso real que se está ejecutando.||  
|error_id|**nvarchar (36)**|Identificador único del error que se produjo durante la ejecución de este trabajo, si existe.|Vea error_id en [sys.dm_pdw_request_steps &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|source_info|**nvarchar(4000)**|Para un lector, especificación de las tablas y columnas de origen.||  
|destination_info|**nvarchar(4000)**|Para un escritor, especificación de las tablas de destino.||  
  
 Para obtener información acerca de las filas máximas retenidas en esta vista, consulte la sección de metadatos en el tema [límites de capacidad](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
