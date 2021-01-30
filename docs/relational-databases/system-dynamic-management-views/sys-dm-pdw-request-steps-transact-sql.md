---
description: sys.dm_pdw_request_steps (Transact-SQL)
title: sys.dm_pdw_request_steps (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/28/2020
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: cc563e88-0d34-436e-b914-b60d6ee0d50b
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 08d39aff859ad993ec78fba73bfbad800945b461
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99139528"
---
# <a name="sysdm_pdw_request_steps-transact-sql"></a>sys.dm_pdw_request_steps (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene información sobre todos los pasos que componen una solicitud o consulta determinada en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . Muestra una fila por cada paso de consulta.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|request_id y step_index componen la clave para esta vista.<br /><br /> IDENTIFICADOR numérico único asociado a la solicitud.|Vea request_id en [sys.dm_pdw_exec_requests &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|request_id y step_index componen la clave para esta vista.<br /><br /> La posición de este paso en la secuencia de pasos que componen la solicitud.|de 0 a (n-1) para una solicitud con n pasos.|  
|plan_node_id|**int**|IDENTIFICADOR de nodo que corresponde al identificador de operador de ese paso en el plan de ejecución.|None|  
|operation_type|**nvarchar(35)**|Tipo de operación representada por este paso.|**Operaciones del plan de consulta de DMS:** ' ReturnOperation ', ' PartitionMoveOperation ', ' MoveOperation ', ' BroadcastMoveOperation ', ' ShuffleMoveOperation ', ' TrimMoveOperation ', ' CopyOperation ', ' DistributeReplicatedTableMoveOperation '<br /><br /> **Operaciones del plan de consulta SQL:** ' Operación ', ' RemoteOperation '<br /><br /> **Otras operaciones del plan de consulta:** 'MetaDataCreateOperation', 'RandomIDOperation'<br /><br /> **Operaciones externas para lecturas:** 'HadoopShuffleOperation', 'HadoopRoundRobinOperation', 'HadoopBroadcastOperation'<br /><br /> **Operaciones externas para MapReduce:** 'HadoopJobOperation', 'HdfsDeleteOperation'<br /><br /> **Operaciones externas para Escrituras:** 'ExternalExportDistributedOperation', 'ExternalExportReplicatedOperation', 'ExternalExportControlOperation'<br /><br /> Para obtener más información, vea "Descripción de los planes de consulta" en la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] . <br /><br />  Un plan de consulta también puede verse afectado por la configuración de la base de datos.  Active [las opciones de ALTER DATABASE Set](../../t-sql/statements/alter-database-transact-sql-set-options.md?bc=%252fazure%252fsql-data-warehouse%252fbreadcrumb%252ftoc.json&toc=%252fazure%252fsql-data-warehouse%252ftoc.json&view=azure-sqldw-latest&preserve-view=true) para obtener más información.|  
|distribution_type|**nvarchar(32)**|Tipo de distribución que se va a someter a este paso.|' AllNodes ', ' AllDistributions ', ' AllComputeNodes ', ' ComputeNode ', ' Distribution ', ' SubsetNodes ', ' SubsetDistributions ', ' unespecifiqued '|  
|location_type|**nvarchar(32)**|Dónde se está ejecutando el paso.|' Compute ', ' control ', ' DMS '|  
|status|**nvarchar(32)**|Estado de este paso.|Pending, running, complete, failed, UndoFailed, PendingCancel, Canceled, Undone, Aborted|  
|error_id|**nvarchar (36)**|IDENTIFICADOR único del error asociado a este paso, si existe.|Vea error_id de [sys.dm_pdw_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md). ES NULL si no se ha producido ningún error.|  
|start_time|**datetime**|Hora a la que se inició la ejecución del paso.|Menor o igual que la hora actual y mayor o igual que end_compile_time de la consulta a la que pertenece este paso. Para obtener más información sobre las consultas, vea [sys.dm_pdw_exec_requests &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|end_time|**datetime**|Hora a la que se completó la ejecución de este paso, se canceló o dio error.|Menor o igual que la hora actual y mayor o igual que start_time. Se establece en NULL para los pasos que se encuentran actualmente en ejecución o en cola.|  
|total_elapsed_time|**int**|Cantidad total de tiempo que se ha estado ejecutando el paso de la consulta, en milisegundos.|Entre 0 y la diferencia entre end_time y start_time. 0 para los pasos en cola.<br /><br /> Si total_elapsed_time supera el valor máximo de un entero, total_elapsed_time seguirá siendo el valor máximo. Esta condición generará la advertencia "se ha superado el valor máximo".<br /><br /> El valor máximo en milisegundos es equivalente a 24,8 días.|  
|row_count|**bigint**|Número total de filas cambiadas o devueltas por esta solicitud.|Número de filas afectadas por el paso.  Mayor o igual que cero para los pasos de la operación de datos.  -1 para los pasos que no operan en los datos.|  
|estimated_rows|**bigint**|Número total de filas de trabajo calculadas durante la compilación de la consulta.|Número de filas estimadas por el paso.  Mayor o igual que cero para los pasos de la operación de datos.  -1 para los pasos que no operan en los datos.|  
|.|**nvarchar(4000)**|Contiene el texto completo del comando de este paso.|Cualquier cadena de solicitud válida para un paso. NULL cuando la operación es del tipo MetaDataCreateOperation. Se trunca si hay más de 4000 caracteres.|  
  
 Para obtener información acerca de las filas máximas retenidas en esta vista, consulte la sección valores máximos de la vista del sistema en los valores mínimo y máximo de [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] .  
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
