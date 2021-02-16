---
title: Referencia de objetos de sistema de grupos de disponibilidad
description: Referencia de los diversos objetos del sistema que se pueden usar cuando se trabaja con grupos de disponibilidad AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 04/03/2010
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: f712ec1fe11f5c975695f15e2d6d6bf4efdf4af5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341537"
---
# <a name="always-on-availability-group-system-object-reference"></a>Referencia de objetos de sistema de grupos de disponibilidad AlwaysOn

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]
    
Este tema sirve como página de referencia para todos los diversos objetos del sistema que se pueden utilizar cuando se trabaja con grupos de disponibilidad Always On. 

## <a name="system-catalog-views"></a>Vistas de catálogo del sistema

| Vista de catálogo del sistema | Descripción|
| :------ | :----------------------------- |
| [sys.availability_databases_cluster](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)   | Contiene una fila por cada base de datos de disponibilidad en la instancia de SQL Server que hospeda una réplica de disponibilidad para cualquier grupo de disponibilidad AlwaysOn en el clúster de clústeres de conmutación por error de Windows Server (WSFC), independientemente de si la base de datos de copia local se ha unido ya al grupo de disponibilidad. |
| [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  | Devuelve una fila para cada dirección IP asociada a cualquier escucha de grupo de disponibilidad AlwaysOn del clúster de clústeres de conmutación por error de Windows Server (WSFC). |
| [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)    | Para cada grupo de disponibilidad AlwaysOn, devuelve cero filas indicando que no hay ningún nombre de red asociado al grupo de disponibilidad, o devuelve una fila para cada configuración de escucha de grupo de disponibilidad en el clúster de clústeres de conmutación por error de Windows Server (WSFC).  |
| [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   | Devuelve una fila para cada grupo de disponibilidad para el que la instancia local de SQL Server hospeda una réplica de disponibilidad. Cada fila contiene una copia almacenada en caché de los metadatos del grupo de disponibilidad. |
| [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)    | Devuelve una fila por cada grupo de disponibilidad AlwaysOn en los clústeres de conmutación por error de Windows Server (WSFC). Cada fila contiene los metadatos del grupo de disponibilidad del clúster de WSFC. |
| [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)    | Devuelve una fila para la lista de enrutamiento de solo lectura de cada réplica de disponibilidad en un grupo de disponibilidad AlwaysOn en el clúster de conmutación por error de WSFC. |
| [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)    | Devuelve una fila para cada una de las réplicas de disponibilidad pertenecientes a un grupo de disponibilidad AlwaysOn del clúster de conmutación por error de WSFC. |
| &nbsp; | &nbsp; |

## <a name="system-dynamic-management-views"></a>Vistas de administración dinámica del sistema


| Vista de administración dinámica del sistema | Descripción|
| :------ | :----------------------------- |
| [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)   | Devuelve una fila por cada intento de reparación de página automática en cualquier base de datos de disponibilidad en una réplica de disponibilidad hospedada para cualquier grupo de disponibilidad por la instancia de servidor.  |
| [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)    | Devuelve una fila para cada grupo de disponibilidad AlwaysOn que posee una réplica de disponibilidad en la instancia local de SQL Server. Cada fila muestra los estados que definen el estado de un grupo de disponibilidad determinado. |
| [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)     | Devuelve una fila para cada réplica de disponibilidad (independientemente del estado de unión) de Grupos de disponibilidad AlwaysOn del clúster de clústeres de conmutación por error de Windows Server (WSFC). |
| [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)     | Devuelve una fila para cada réplica de disponibilidad AlwaysOn (independientemente de su estado de unión) de todos los Grupos de disponibilidad AlwaysOn (con independencia de la ubicación de la réplica) del clúster del servicio de clústeres de conmutación por error de Windows Server (WSFC). |
| [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)    | Devuelve una fila para cada réplica local y una fila para cada réplica remota en el mismo grupo de disponibilidad Always On que una réplica local. Cada fila contiene información sobre el estado de una réplica determinada. |
| [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)    | Devuelve una fila que expone el nombre del clúster e información sobre el cuórum. |
| [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)    | Devuelve una fila para cada uno de los miembros que constituyen el cuórum y la situación de cada uno de ellos. |
| [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)    | Devuelve una fila para cada miembro del clúster de WSFC que participa en la configuración de subred de un grupo de disponibilidad.  |
| [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)     | Devuelve una fila que contiene información destinada a proporcionar una visión general del estado de las bases de datos de disponibilidad de los Grupos de disponibilidad AlwaysOn de cada uno de los grupos de disponibilidad AlwaysOn del clúster del servicio de clústeres de conmutación por error de Windows Server (WSFC).  |
| [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)    | Devuelve una fila por cada base de datos que participa en un grupo de disponibilidad AlwaysOn para el que la instancia local de SQL Server hospeda una réplica de disponibilidad. |
| [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)    | Para cada instancia de SQL Server que hospeda una réplica de disponibilidad que se une a su grupo de disponibilidad AlwaysOn, devuelve el nombre del nodo de clústeres de conmutación por error de Windows Server (WSFC) que hospeda la instancia de servidor. |
| [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)    | Muestra la asignación de grupos de disponibilidad AlwaysOn que la instancia actual de SQL Server ha unido a tres identificadores únicos: un identificador de grupo de disponibilidad, un identificador de recurso de WSFC y un identificador de grupo de WSFC. |
| [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)    | Devuelve una fila que contiene la información de estado dinámico para cada agente de escucha TCP. |
| &nbsp; | &nbsp; |

## <a name="system-functions"></a>Funciones del sistema


| Función del sistema | Descripción|
| :------ | :----------------------------- |
| [sys.fn_hadr_is_primary_replica](../../../relational-databases/system-functions/sys-fn-hadr-is-primary-replica-transact-sql.md)  | Se usa para determinar si la réplica actual es la réplica principal. |
| [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)    | Se usa para determinar si la réplica actual es la réplica de copia de seguridad preferida. |
| [sys.fn_hadr_distributed_ag_replica](../../../relational-databases/system-functions/sys-fn-hadr-distributed-ag-replica-transact-sql.md)    | Se utiliza para asignar una réplica en un grupo de disponibilidad distribuido al grupo de disponibilidad local. |
| &nbsp; | &nbsp; |


  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

  
