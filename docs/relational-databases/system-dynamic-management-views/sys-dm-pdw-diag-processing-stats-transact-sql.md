---
description: sys.dm_pdw_diag_processing_stats (Transact-SQL)
title: sys.dm_pdw_diag_processing_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: df659c55-4f63-45f8-8afe-ce300031bc5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 0164a7f806afcacc35af0e2bffd2c6cbe69ee762
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99144248"
---
# <a name="sysdm_pdw_diag_processing_stats-transact-sql"></a>sys.dm_pdw_diag_processing_stats (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Muestra información relacionada con todos los eventos de diagnóstico internos que se pueden incorporar en sesiones de diagnóstico definidas por el administrador. Consulte esta vista para conocer las estadísticas de los subsistemas de diagnóstico y eventos que controlan el rellenado de todas las demás DMV. Hay un grupo de colas para cada proceso en cada nodo.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**pdw_node_id**|**int**|Nodo del dispositivo del que procede este registro.|  
|**process_id**|**int**|Identificador del proceso que ejecuta el envío de esta estadística.|  
|**target_name**|**nvarchar(255)**|Nombre de la cola.|  
|**queue_size**|**int**|Número de elementos de la cola de procesos. El tamaño de la cola suele ser 0. Un número positivo indica que el sistema está bajo esfuerzo y está creando trabajo pendiente de eventos. Un recuento positivo en las demás columnas significa que el sistema se ha dañado para esa cola concreta y cualquier DMV relacionada.|  
|**lost_events_count**|**bigint**|Número de eventos perdidos.|  
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
