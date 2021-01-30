---
description: sys.dm_pdw_dms_external_work (Transact-SQL)
title: sys.dm_pdw_dms_external_work (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 47345015-f861-451e-97c4-6e1cb81d1922
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: cfdccad0daadd45b16d2e910eed8c7b194182daf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99143625"
---
# <a name="sysdm_pdw_dms_external_work-transact-sql"></a>sys.dm_pdw_dms_external_work (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] vista del sistema que contiene información sobre todos los pasos del servicio de movimiento de datos (DMS) para operaciones externas.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Consulta que está usando este trabajo de DMS.<br /><br /> request_id, step_index y dms_step_index forman la clave de esta vista.|Igual que request_id en [sys.dm_pdw_exec_requests &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Paso de consulta que invoca a este trabajo de DMS.<br /><br /> request_id, step_index y dms_step_index forman la clave de esta vista.|Igual que step_index en [sys.dm_pdw_request_steps &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|dms_step_index|**int**|Paso actual del plan DMS.<br /><br /> request_id, step_index y dms_step_index forman la clave de esta vista.|Igual que dms___step_index en [sys.dm_pdw_dms_workers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql.md).|  
|pdw_node_id|**int**|Nodo que ejecuta el trabajo de DMS.|Igual que node_id en [sys.dm_pdw_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|tipo|**nvarchar(60)**|Tipo de operación externa que se está ejecutando en este nodo.<br /><br /> La división de archivos es una operación en un archivo de Hadoop externo que se ha dividido en varios supuestos más pequeños.|' DIVISIÓN DE ARCHIVOS '|  
|work_id|**int**|IDENTIFICADOR de división del archivo.|Mayor o igual que 0.<br /><br /> Único por nodo de proceso.|  
|input_name|**nvarchar(60)**|Nombre de cadena de la entrada que se lee.|Para un archivo de Hadoop, este es el nombre de archivo de Hadoop.|  
|read_location|**bigint**|Desplazamiento de la ubicación de lectura.||  
|estimated_bytes_processed|**bigint**|Número de bytes procesados por este trabajador.|Mayor o igual que 0.|  
|length|**bigint**|Número de bytes en la división del archivo.<br /><br /> En el caso de Hadoop, es el tamaño del bloque HDFS.|Definido por el usuario. El valor predeterminado es 64 MB.|  
|status|**nvarchar(32)**|Estado del trabajo.|Pending, Processing, done, failed, Aborted|  
|start_time|**datetime**|Hora a la que se inició la ejecución de este trabajador.|Mayor o igual que la hora de inicio del paso de consulta al que pertenece este trabajador. Vea [sys.dm_pdw_request_steps &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|end_time|**datetime**|Hora a la que finalizó la ejecución, se produjo un error o se canceló.|NULL para los trabajos en cola o en curso. De lo contrario, mayor que start_time.|  
|total_elapsed_time|**int**|Tiempo total invertido en la ejecución, en milisegundos.|Mayor o igual que 0.<br /><br /> Si total_elapsed_time supera el valor máximo de un entero, total_elapsed_time seguirá siendo el valor máximo. Esta condición generará la advertencia "se ha superado el valor máximo".<br /><br /> El valor máximo en milisegundos es equivalente a 24,8 días.|  
  
 Para obtener información acerca de las filas máximas retenidas en esta vista, consulte la sección de metadatos en el tema [límites de capacidad](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)  
  
