---
description: sys.dm_workload_management_workload_groups_stats (Transact-SQL)
title: sys.dm_workload_management_workload_groups_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/02/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
author: ronortloff
ms.author: rortloff
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 01dbe51a5b82ea68ef1676876a9c0fa1633bf90c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99131463"
---
# <a name="sysdm_workload_management_workload_groups_stats-transact-sql"></a>sys.dm_workload_management_workload_groups_stats (Transact-SQL)
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Devuelve las estadísticas del grupo de cargas de trabajo y los valores efectivos del grupo de cargas de trabajo en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|group_id|**int**|Id. único del grupo de cargas de trabajo.||
|name|**sysname**|Nombre del grupo de cargas de trabajo||
|statistics_start_time|**datetime**|Hora de inicio de la recopilación de estadísticas para el grupo de cargas de trabajo.  El valor es cuando el grupo de cargas de trabajo se creó o cuando la instancia está en pausa o escalada.||
|total_request_count|**bigint**|El recuento acumulado de solicitudes completadas en el grupo de cargas de trabajo.||
|total_shared_resource_reqeusts|**bigint**|Recuento acumulado de solicitudes completadas en el grupo de cargas de trabajo que usaban recursos del grupo compartido.||
|total_queued_request_count|**bigint**|Recuento acumulado de solicitudes en cola después de alcanzar el límite de max_concurrency.||
|total_request_execution_timeouts|**bigint**|Recuento acumulado de solicitudes en el grupo de cargas de trabajo que han agotado el tiempo de espera antes de la finalización en función del valor de query_execution_timeout_sec.||
|effective_min_percentage_resource|**tinyint**|La configuración de min_percentage_resource efectiva permite considerar el nivel de servicio y la configuración del grupo de cargas de trabajo. El min_percentage_resource efectivo puede ajustarse más alto en los niveles de servicio más bajos.  Por ejemplo, en DW100c, el min_percentage_resource más bajo permitido es del 25%.  El min_percentage_resource se ajusta en 0% si el valor no se puede conceder en el nivel de servicio.  Por ejemplo, min_percentage_resource establece en 10% en DW6000c, tendría un effective_min_percentage_resource de 0% cuando se escaló a DW100c.||
|effective_cap_percentage_resource|**tinyint**|Cap_percentage_resource efectiva para el grupo de cargas de trabajo.  Si hay otros grupos de cargas de trabajo con min_percentage_resource > 0, el effective_cap_percentage_resource se reduce proporcionalmente.||
|effective_request_min_resource_grant_percent|**decimal (5, 2)**|El valor efectivo en tiempo de ejecución para request_min_resource_grant_percent del grupo de cargas de trabajo. El valor efectivo que considera el nivel de servicio y cómo se configura el grupo de cargas de trabajo.  Si min_percentage_resource se ajusta debido al nivel de servicio, effective_request_min_resource_grant_percent se ajustará en consecuencia.||
|effective_request_max_resource_grant_percent|**decimal (5, 2)**|El valor efectivo en tiempo de ejecución de request_max_resource_grant_percent del grupo de cargas de trabajo teniendo en cuenta la configuración de todos los grupos de cargas de trabajo.||
|||||

## <a name="see-also"></a>Vea también

 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
