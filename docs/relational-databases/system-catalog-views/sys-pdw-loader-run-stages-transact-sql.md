---
description: sys.pdw_loader_run_stages (Transact-SQL)
title: sys.pdw_loader_run_stages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 255681e9-323c-42c0-a63c-1f05536efdd5
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: b0c0c6507a512e6eb02440735f4a7a9c41217ef8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206399"
---
# <a name="syspdw_loader_run_stages-transact-sql"></a>sys.pdw_loader_run_stages (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Contiene información sobre las operaciones de carga en curso y completadas en [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] . La información se conserva entre reinicios del sistema.  
  
| Nombre de columna | Tipo de datos | Descripción | Intervalo |
| ----------- | --------- | ----------- | ----- |
|run_id|**int**|Identificador único de una ejecución del cargador.||  
|fase|**nvarchar(30)**|La fase actual de la ejecución.|' CREATE_STAGING ', ' DMS_LOAD ', ' LOAD_INSERT ', ' LOAD_CLEANUP '|  
|request_id|**nvarchar(32)**|IDENTIFICADOR de la solicitud que se está ejecutando en esta fase.||  
|status|**nvarchar (16)**|Estado de esta fase.||  
|start_time|**datetime**|Hora a la que se inició la fase.||  
|end_time|**datetime**|Hora a la que finalizó la fase, si existe.|NULL si no se ha iniciado o está en curso.|  
|total_elapsed_time|**int**|Tiempo total que esta fase tardó en ejecutarse (o hasta ahora).|Si total_elapsed_time supera el valor máximo de un entero (24,8 días en milisegundos), se producirá un error de materialización debido al desbordamiento.<br /><br /> El valor máximo en milisegundos es equivalente a 24,8 días.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
