---
description: sys.resource_governor_resource_pools (Transact-SQL)
title: sys.resource_governor_resource_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.resource_governor_resource_pools
- resource_governor_resource_pools
- sys.resource_governor_resource_pools_TSQL
- resource_governor_resource_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_resource_pools catalog view
ms.assetid: 56793e9c-aa90-452e-88c6-d9b799239888
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fef5cb969c9dd0d3a0253362a2e3739e490db916
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203764"
---
# <a name="sysresource_governor_resource_pools-transact-sql"></a>sys.resource_governor_resource_pools (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la configuración del grupo de recursos de servidor almacenada en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cada fila de la vista determina la configuración de un grupo.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|{1}pool_id{2}|**int**|Identificador único del grupo de recursos de servidor. No admite valores NULL.|  
|name|**sysname**|Nombre del grupo de recursos de servidor. No admite valores NULL.|  
|min_cpu_percent|**int**|Ancho banda de la CPU promedio garantizado para todas las solicitudes en el grupo de recursos de servidor cuando hay contención de CPU. No admite valores NULL.|  
|max_cpu_percent|**int**|Ancho banda de la CPU promedio máximo permitido para todas las solicitudes en el grupo de recursos de servidor cuando hay contención de CPU. No admite valores NULL.|  
|min_memory_percent|**int**|Ancho banda de la memoria garantizado para todas las solicitudes en el grupo de recursos de servidor. No se comparte con otros grupos de recursos de servidor. No admite valores NULL.|  
|max_memory_percent|**int**|Porcentaje de la memoria total del servidor que puede utilizarse en las solicitudes en este grupo de recursos de servidor. No admite valores NULL. El máximo efectivo depende de los mínimos del grupo. Por ejemplo, max_memory_percent puede estar establecido en 100, pero el máximo efectivo es menor.|  
|cap_cpu_percent|**int**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> El límite máximo de ancho de banda de la CPU que recibirán todas las solicitudes en el grupo de recursos. Limita el ancho de banda máximo de la CPU en el nivel especificado. El intervalo permitido para el valor es de 1 a 100.|  
|min_iops_per_volume|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Las operaciones de E/S mínima por segundo (IOPS) por la configuración de volumen para este grupo. 0 = sin reserva. No puede ser NULL.|  
|max_iops_per_volume|**int**|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> Las operaciones de E/S máxima por segundo (IOPS) por la configuración de volumen para este grupo. 0 = ilimitado. No puede ser NULL.|  
  
## <a name="remarks"></a>Observaciones  
 La vista de catálogo muestra los metadatos almacenados. Para ver la configuración en memoria, use la vista de administración dinámica correspondiente, [sys.dm_resource_governor_resource_pools &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW ANY DEFINITION para ver el contenido; requiere el permiso CONTROL SERVER para cambiar el contenido.  
  
## <a name="see-also"></a>Consulte también  
 [Resource Governor vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)   
 [sys.dm_resource_governor_resource_pools &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)   
 [Regulador de recursos](../../relational-databases/resource-governor/resource-governor.md)   
 [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)  
  
  
