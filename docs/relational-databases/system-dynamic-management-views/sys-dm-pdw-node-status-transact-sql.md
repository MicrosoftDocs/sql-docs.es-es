---
description: sys.dm_pdw_node_status (Transact-SQL)
title: sys.dm_pdw_node_status (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 8e263b65-81d0-49d0-8873-62ef424369d6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 25c53d9ae723cc04809f410a336a1e3b50e157eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142331"
---
# <a name="sysdm_pdw_node_status-transact-sql"></a>sys.dm_pdw_node_status (Transact-SQL)

[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Contiene información adicional (a través de [sys.dm_pdw_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md)) sobre el rendimiento y el estado de todos los nodos del dispositivo. Muestra una fila por nodo en el dispositivo.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Identificador numérico único asociado al nodo.<br /><br /> Clave para esta vista.|Único en todo el dispositivo, independientemente del tipo.|  
|process_id|**int**|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]||  
|process_name|**nvarchar(255)**|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]||  
|allocated_memory|**bigint**|Memoria total asignada en este nodo.||  
|available_memory|**bigint**|Memoria total disponible en este nodo.||  
|process_cpu_usage|**bigint**|Uso total de la CPU del proceso, en pasos.||  
|total_cpu_usage|**bigint**|Uso total de CPU, en pasos.||  
|thread_count|**bigint**|Número total de subprocesos en uso en este nodo.||  
|handle_count|**bigint**|Número total de identificadores en uso en este nodo.||  
|total_elapsed_time|**bigint**|Tiempo total transcurrido desde el inicio o el reinicio del sistema.|Tiempo total transcurrido desde el inicio o el reinicio del sistema. Si total_elapsed_time supera el valor máximo de un entero (24,8 días en milisegundos), se producirá un error de materialización debido al desbordamiento.<br /><br /> El valor máximo en milisegundos es equivalente a 24,8 días.|  
|is_available|**bit**|Marca que indica si este nodo está disponible.||  
|sent_time|**datetime**|Última vez que este nodo envió un paquete de red.||  
|received_time|**datetime**|Última vez que este nodo recibió un paquete de red.||  
|error_id|**nvarchar (36)**|Identificador único del último error que se produjo en este nodo.||  
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
