---
description: sys.pdw_loader_backup_run_details (Transact-SQL)
title: sys.pdw_loader_backup_run_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 04fc004f-ee15-4d7a-be08-78357aa99b55
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 9f00da4659e4254b7f9a48bea10d201e10e5c9ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186123"
---
# <a name="syspdw_loader_backup_run_details-transact-sql"></a>sys.pdw_loader_backup_run_details (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene información detallada adicional, más allá de la información de [sys.pdw_loader_backup_runs &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-loader-backup-runs-transact-sql.md), acerca de las operaciones de copia de seguridad y restauración continuadas y completadas en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y sobre las operaciones de copia de seguridad, restauración y carga en curso y completadas en [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] . La información se conserva entre reinicios del sistema.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|run_id|**int**|Identificador único para una ejecución de restauración o copia de seguridad específica.<br /><br /> run_id y pdw_node_id forman la clave de esta vista.||  
|pdw_node_id|**int**|Identificador único de un nodo de dispositivo para el que este registro contiene detalles.<br /><br /> run_id y pdw_node_id forman la clave de esta vista.|Vea node_id en [sys.dm_pdw_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|status|**nvarchar (16)**|Estado actual de la ejecución.|' CANCELADO ', ' COMPLETED ', ' FAILED ', ' QUEUED ', ' RUNNING '|  
|start_time|**datetime**|Hora a la que se inició la operación en este nodo concreto.||  
|end_time|**datetime**|Hora a la que finalizó la operación en este nodo determinado, si existe.||  
|total_elapsed_time|**int**|Tiempo total que se ha estado ejecutando la operación en este nodo concreto.|Si total_elapsed_time supera el valor máximo de un entero (24,8 días en milisegundos), se producirá un error de materialización debido al desbordamiento.<br /><br /> El valor máximo en milisegundos es equivalente a 24,8 días.|  
|progreso|**int**|Progreso de la operación expresado en forma de porcentaje.|De 0 a 100|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
