---
description: sys.resource_governor_workload_groups (Transact-SQL)
title: sys.resource_governor_workload_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.resource_governor_workload_groups
- resource_governor_workload_groups_TSQL
- sys.resource_governor_workload_groups_TSQL
- resource_governor_workload_groups
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_workload_groups catalog view
ms.assetid: 619ba4b7-868f-4784-b527-ec1dfd703c4f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d89b9210d0da27b3b4d45e8f4d2652960cf0c1d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183631"
---
# <a name="sysresource_governor_workload_groups-transact-sql"></a>sys.resource_governor_workload_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la configuración del grupo de cargas de trabajo almacenada en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cada grupo de cargas de trabajo puede suscribirse a un único grupo de recursos de servidor.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|group_id|**int**|Id. único del grupo de cargas de trabajo. No admite valores NULL.|  
|name|**sysname**|Nombre del grupo de cargas de trabajo No admite valores NULL.|  
|importance|**sysname**|**Nota:** La importancia solo se aplica a los grupos de cargas de trabajo en el mismo grupo de recursos.<br /><br /> Es la importancia relativa de una solicitud en este grupo de cargas de trabajo. Importance es uno de los siguientes, siendo MEDIUM el valor predeterminado: LOW, MEDIUM, HIGH.<br /><br /> No admite valores NULL.|  
|request_max_memory_grant_percent|**int**|Concesión máxima de memoria, en porcentaje, para una solicitud única. El valor predeterminado es 25. No admite valores NULL.<br /><br /> **Nota:** Si esta configuración es superior al 50 por ciento, las consultas de gran tamaño se ejecutarán de una en una. Por consiguiente, existe un mayor riesgo de que se produzca un error de memoria insuficiente mientras se está ejecutando la consulta.|  
|request_max_cpu_time_sec|**int**|Límite máximo de uso de CPU, en segundos, para una única solicitud. El valor predeterminado, 0, especifica que no hay límite. No admite valores NULL.<br /><br /> **Nota:** Para obtener más información, vea [clase de eventos umbral de CPU superado](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).|  
|request_memory_grant_timeout_sec|**int**|Tiempo de espera de la concesión de memoria, en segundos, para una única solicitud. El valor predeterminado, 0, utiliza un cálculo interno basado en el costo de la consulta. No admite valores NULL.|  
|max_dop|**int**|Grado máximo de paralelismo para el grupo de cargas de trabajo. El valor predeterminado, 0, utiliza la configuración global. No admite valores NULL.<br /><br /> **Nodo:** Esta configuración invalidará la opción de consulta **maxdop**.|  
|group_max_requests|**int**|Número máximo de solicitudes simultáneas. El valor predeterminado, 0, especifica que no hay límite. No admite valores NULL.|  
|{1}pool_id{2}|**int**|Identificador del grupo de recursos de servidor que usa este grupo de cargas de trabajo.|  
|external_pool_id|**int**|**Válido para** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> IDENTIFICADOR del grupo de recursos externos que usa este grupo de cargas de trabajo.|  
  
## <a name="remarks"></a>Observaciones  
 La vista de catálogo muestra los metadatos almacenados. Para ver la configuración en memoria, use la vista de administración dinámica correspondiente, [sys.dm_resource_governor_workload_groups &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md).  
  
 La configuración almacenada puede ser diferente a la configuración en memoria si se ha cambiado la configuración del regulador de recursos pero sin aplicar la instrucción ALTER RESOURCE GOVERNOR RECONFIGURE.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW ANY DEFINITION para ver el contenido; requiere el permiso CONTROL SERVER para cambiar el contenido.  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Resource Governor vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)  
  
  
