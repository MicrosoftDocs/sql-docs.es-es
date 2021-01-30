---
description: sys.dm_hadr_name_id_map (Transact-SQL)
title: sys.dm_hadr_name_id_map (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_name_id_map
- sys.dm_hadr_name_id_map_TSQL
- dm_hadr_name_id_map
- dm_hadr_name_id_map_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_name_id_map dynamic management view
ms.assetid: e07bb8a9-37de-4a39-a257-950d7c3ae8fb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fb1d27d8754ae146c5e46edcaedae7acb1b6d9dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186523"
---
# <a name="sysdm_hadr_name_id_map-transact-sql"></a>sys.dm_hadr_name_id_map (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra la asignación de Grupos de disponibilidad Always On que la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ha unido a tres identificadores únicos: un identificador de grupo de disponibilidad, un identificador de recurso de WSFC y un identificador de grupo de WSFC. La finalidad de esta asignación es controlar el escenario en el que cambia el nombre del recurso o el grupo de WSFC.  
   
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**ag_name**|**nvarchar(256)**|Nombre del grupo de disponibilidad. Se trata de un nombre especificado por el usuario que debe ser único en el clúster de clústeres de conmutación por error de Windows Server (WSFC).|  
|**ag_id**|**uniqueidentifier**|Identificador único (GUID) del grupo de disponibilidad.|  
|**ag_resource_id**|**nvarchar(256)**|Identificador único del grupo de disponibilidad como un recurso del clúster de WSFC.|  
|**ag_group_id**|**nvarchar(256)**|Identificador de grupo WSFC único del grupo de disponibilidad.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Always On vistas y funciones de administración dinámica de grupos de disponibilidad &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Vistas de catálogo de grupos de disponibilidad AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
