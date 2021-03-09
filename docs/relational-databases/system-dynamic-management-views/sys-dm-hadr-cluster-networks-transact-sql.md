---
description: sys.dm_hadr_cluster_networks (Transact-SQL)
title: sys.dm_hadr_cluster_networks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_cluster_networks
- sys.dm_hadr_cluster_networks_TSQL
- sys.dm_hadr_cluster_networks
- dm_hadr_cluster_networks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_cluster_networks dynamic management view
ms.assetid: ece32b15-d63f-4f93-92b7-e2930333e97a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a50a255745558f73a414fa00c782e5587aad7cab
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464513"
---
# <a name="sysdm_hadr_cluster_networks-transact-sql"></a>sys.dm_hadr_cluster_networks (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila para cada miembro del clúster de WSFC que participa en la configuración de subred de un grupo de disponibilidad. Puede utilizar esta vista de administración dinámica para validar la dirección IP virtual de red configurada para cada réplica de disponibilidad.  
  
 Clave principal: **member_name**  +  **network_subnet_IP**  +  **network_subnet_prefix_length**  
  
 > [!TIP]
 > A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , esta vista de administración dinámica admite Always on instancias de clúster de conmutación por error además de Always on grupos de disponibilidad.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**member_name**|**nvarchar(128)**|Un nombre de equipo de un nodo del clúster de WSFC.|  
|**network_subnet_ip**|**nvarchar (48)**|Dirección de IP de red de la subred a la que pertenece el equipo. Puede ser una dirección IPv4 o IPv6.|  
|**network_subnet_ipv4_mask**|**nvarchar (45)**|Máscara de subred de red que especifica la subred a la que pertenece la dirección IP. **network_subnet_ipv4_mask** para especificar el <DHCP network_subnet_option opciones de> en una cláusula with DHCP de la instrucción [Create Availability Group](../../t-sql/statements/create-availability-group-transact-sql.md) o [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .<br /><br /> NULL = subred IPv6.|  
||||  
|**network_subnet_prefix_length**|**int**|Longitud de prefijo de IP de red que especifica la subred a la que pertenece el equipo.|  
|**is_public**|**bit**|Si la red es privada o pública en el clúster de WSFC, puede ser:<br /><br /> 0 = Privada<br /><br /> 1 = Pública|  
|**is_ipv4**|**bit**|Tipo de la subred; puede ser:<br /><br /> 1 = IPv4<br /><br /> 0 = IPv6|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Clústeres de conmutación por error y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [sys.dm_os_cluster_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
