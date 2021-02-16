---
title: DMV y vistas de catálogo del sistema para grupos de disponibilidad
description: Colección de vistas de administración dinámica y vistas de catálogo que pueden ayudar a supervisar y diagnosticar el estado de un grupo de disponibilidad AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
ms.assetid: 4320a4a4-6183-462b-8bda-e7424e7cb706
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d1a45cf3572c1d533714ff7ebf96fe0b4f236fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349302"
---
# <a name="dynamic-management-views-and-system-catalog-views-always-on-availability-groups"></a>Vistas de administración dinámica y vistas de catálogo del sistema (Grupos de disponibilidad Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se muestran algunas de las consultas comunes en las vistas de administración dinámica (DMV) de Always On que puede usar para supervisar grupos de disponibilidad y solucionar problemas de ellos.  
  
> [!TIP]  
>  En el panel Always On puede configurar con facilidad la GUI para que muestre muchas de las DMV de las réplicas de disponibilidad y las bases de datos de disponibilidad si hace clic con el botón derecho en el encabezado de tabla correspondiente y selecciona la DMV que quiere mostrar u ocultar.  
  
 Para obtener más información sobre las DMV de los grupos de disponibilidad, vea [Funciones y vistas de administración dinámica de grupos de disponibilidad Always On &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md). Para obtener más información sobre las vistas de catálogo de los grupos de disponibilidad, vea [Vistas de catálogo de grupos de disponibilidad Always On &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md).  
  
## <a name="check-the-wsfc-cluster-node-configuration"></a>Comprobar la configuración del nodo de clúster WSFC  
 La siguiente consulta de Transact-SQL (T-SQL) recupera el estado de todos los nodos del clúster actual de clústeres de conmutación por error de Windows Server (WSFC).  
  
```sql  
use master  
go  
select * from sys.dm_hadr_cluster_members  
go  
```  
  
 Este conjunto de resultados informa del estado de cada nodo miembro del clúster WSFC actual. Si el cuórum se define como **Mayoría de recurso compartido de archivos y nodo**, se informa incluso del recurso compartido de archivos. Puede ver el estado de cada nodo, incluido el peso de voto de cada nodo (el valor [number_of_quorum_votes](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)).  
  
## <a name="explore-the-cluster-network"></a>Explorar la red del clúster  
 La consulta siguiente recupera la configuración de red del clúster WSFC actual.  
  
```sql  
select * from sys.dm_hadr_cluster_networks  
```  
  
 El conjunto de resultados contiene una fila para cada adaptador de red del clúster WSFC. Por ejemplo, en un clúster de dos nodos que contiene dos adaptadores de red en cada nodo, esta consulta devuelve cuatro filas.  
  
## <a name="explore-the-availability-groups"></a>Explorar los grupos de disponibilidad  
 La consulta siguiente recupera información sobre un grupo de disponibilidad.  
  
```sql  
select primary_replica, primary_recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_group_states  
go  
select * from sys.availability_groups  
go  
select * from sys.availability_groups_cluster  
go  
```  
  
 Las DMV [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md), [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) y [sys.availability_ groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) devuelven información sobre los grupos de disponibilidad del clúster WSFC actual. De hecho, [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) y [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) parecen devolver la misma información.  
  
 Pero [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) informa de metadatos del grupo de disponibilidad almacenados en el clúster WSFC, mientras que [sys.availability_groups &#40;Transact-SQL&#41; ](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) informa de metadatos del grupo de disponibilidad almacenados en caché en el espacio de proceso de SQL Server. Además, estas dos DMV notifican información de configuración, mientras que [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41; ](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md) informa de los estados actuales de los grupos de disponibilidad.  
  
> [!IMPORTANT]  
>  Esta nomenclatura se transfiere con las DMV que documentan las réplicas de disponibilidad y las bases de datos de disponibilidad.  
  
## <a name="explore-the-availability-replicas"></a>Explorar las réplicas de disponibilidad  
 La consulta siguiente recupera información sobre las réplicas de disponibilidad definidas en los grupos de disponibilidad.  
  
```sql  
select replica_id, role_desc, connected_state_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
select replica_server_name, replica_id, availability_mode_desc, endpoint_url from sys.availability_replicas  
go  
select replica_server_name, join_state_desc from sys.dm_hadr_availability_replica_cluster_states  
go  
```  
  
 Al igual que en las DMV del grupo de disponibilidad, hay tres DMV que informan sobre las réplicas de disponibilidad. [sys.dm_hadr_availability_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md) notifica información de estado sobre las réplicas de disponibilidad almacenadas en caché localmente en SQL Server, mientras que [sys.dm_hadr_availability_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) notifica información de estado sobre las réplicas de disponibilidad del clúster WSFC. Por último, [sys.availability_replicas](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) notifica datos de configuración sobre las réplicas de disponibilidad, que se almacenan en caché localmente en SQL Server.  
  
## <a name="explore-availability-replica-health"></a>Explorar el estado de una réplica de disponibilidad  
 La consulta siguiente recupera información de estado actual sobre las réplicas de disponibilidad.  
  
```sql  
select replica_id, role_desc, recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
```  
  
 Compare los resultados de consulta sobre la réplica principal y la réplica secundaria y observe que, en la réplica secundaria, solo se notifica información de estado de esa réplica y no de ninguna otra del grupo de disponibilidad.  
  
## <a name="explore-the-availability-databases"></a>Explorar las bases de datos de disponibilidad  
 La consulta siguiente recupera información sobre las réplicas de disponibilidad definidas en el grupo de disponibilidad. Puede observar el cambio en los resultados de consulta antes y después de suspender el movimiento de datos en una base de datos de disponibilidad.  
  
```sql
select * from sys.availability_databases_cluster  
go  
select group_database_id, database_name, is_failover_ready  from sys.dm_hadr_database_replica_cluster_states  
go  
select database_id, synchronization_state_desc, synchronization_health_desc, last_hardened_lsn, redo_queue_size, log_send_queue_size from sys.dm_hadr_database_replica_states  
go  
```  
  
 Una vez más, tres DMV de Always On informan sobre las bases de datos de disponibilidad. [sys.availability_databases_cluster](~/relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md) notifica información de configuración sobre las bases de datos de disponibilidad del clúster WSFC. [sys.dm_hadr_database_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) notifica información de estado sobre las réplicas de base de datos, que se almacenan en caché localmente en SQL Server. Contiene alguna información de estado importante, como la preparación para la conmutación por error de la réplica de disponibilidad. Por último, [sys.dm_hadr_database_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) es un conjunto de resultados muy detallado que proporciona información de identidad y estado sobre cada base de datos de disponibilidad, como información de progreso de LSN de los registros de las réplicas de base de datos principal y secundaria.  
  
## <a name="explore-availability-database-health"></a>Explorar el estado de una base de datos de disponibilidad  
 La consulta siguiente recupera información sobre el estado de cada base de datos de disponibilidad de las réplicas. Puede observar el cambio en los resultados de consulta antes y después de suspender el movimiento de datos en una base de datos de disponibilidad.  
  
```sql  
select dc.database_name, dr.database_id, dr.synchronization_state_desc,   
dr.suspend_reason_desc, dr.synchronization_health_desc  
from sys.dm_hadr_database_replica_states dr  join sys.availability_databases_cluster dc  
on dr.group_database_id=dc.group_database_id   
where is_local=1  
go  
```  
  
  
