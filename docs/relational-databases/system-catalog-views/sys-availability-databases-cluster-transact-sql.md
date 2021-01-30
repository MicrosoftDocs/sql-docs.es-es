---
description: sys.availability_databases_cluster (Transact-SQL)
title: sys.availability_databases_cluster (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.availability_databases_cluster_TSQL
- sys.availability_databases_cluster
- availability_databases_cluster_TSQL
- availability_databases_cluster
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.availability_databases_cluster catalog view
- Availability Groups [SQL Server], databases
ms.assetid: 8d9c57e5-7f39-4315-b466-92748231140a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2725e29dfff7f9d78db4cf866055bd2199e4c48e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165302"
---
# <a name="sysavailability_databases_cluster-transact-sql"></a>sys.availability_databases_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada base de datos de disponibilidad en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que hospeda una réplica de disponibilidad para cualquier grupo de disponibilidad de Always on en el clúster de clústeres de conmutación por error de Windows Server (WSFC), independientemente de si la base de datos de copia local ya se ha unido al grupo de disponibilidad.  
  
> [!NOTE]  
>  Cuando una base de datos se agrega a un grupo de disponibilidad, la base de datos principal se une automáticamente al grupo. Las bases de datos secundarias se deben preparar en cada réplica secundaria para poder unirse al grupo de disponibilidad.   
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificador único del grupo de disponibilidad dentro del grupo de disponibilidad en que la base de datos participa, si hay alguno.<br /><br /> NULL = la base de datos no forma parte de una réplica de disponibilidad en el grupo de disponibilidad.|  
|**group_database_id**|**uniqueidentifier**|Identificador único de la base de datos dentro del grupo de disponibilidad en que la base de datos participa, si hay alguno. **group_database_id** es el mismo para esta base de datos en la réplica principal y en cada réplica secundaria en la que la base de datos se ha unido al grupo de disponibilidad.<br /><br /> NULL = la base de datos no forma parte de una réplica de disponibilidad en ningún grupo de disponibilidad.|  
|**database_name**|**sysname**|Nombre de la base de datos que se agregó al grupo de disponibilidad.|  
  
## <a name="permissions"></a>Permisos  
 Si el autor de la llamada de **Sys.availability_databases_cluster** no es el propietario de la base de datos, los permisos mínimos necesarios para ver la fila correspondiente son el permiso ALTER any Database o View any Database de nivel de servidor, o el permiso CREATE DATABASE en la base de datos **maestra** .  
  
## <a name="see-also"></a>Consulte también  
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.dm_hadr_database_replica_states &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)   
 [sys.dm_hadr_database_replica_cluster_states &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
