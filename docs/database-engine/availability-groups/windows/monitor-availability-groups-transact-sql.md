---
title: Supervisión de grupos de disponibilidad con Transact-SQL (T-SQL)
description: Use instrucciones SELECT de Transact-SQL para supervisar los Grupos de disponibilidad Always On y sus réplicas y bases de datos.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- dynamic management views [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], listeners
- Availability Groups [SQL Server], databases
- catalog views [SQL Server], AlwaysOn Availability Groups
ms.assetid: 881a34de-8461-4811-8c62-322bf7226bed
author: cawrites
ms.author: chadam
ms.openlocfilehash: d2bccd9808ad397d7cc83285577aea3483b253c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352664"
---
# <a name="monitor-availability-groups-transact-sql"></a>Supervisar grupos de disponibilidad (Transact-SQL)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Para supervisar réplicas y grupos de disponibilidad y las bases de datos asociado utilizando [!INCLUDE[tsql](../../../includes/tsql-md.md)], [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] proporciona un conjunto de vistas de administración dinámica y de catálogo y propiedades del servidor. Mediante instrucciones SELECT de [!INCLUDE[tsql](../../../includes/tsql-md.md)] , puede utilizar las vistas para supervisar los grupos de disponibilidad y sus réplicas y bases de datos. La información devuelta para un grupo de disponibilidad determinado depende de si está conectado a la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospeda la réplica principal o una réplica secundaria.  
  
> [!TIP]  
>  Muchas de estas vistas se pueden unir utilizando las columnas ID para devolver información de varias vistas en una única consulta.  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] requieren el permiso VIEW ANY DEFINITION en la instancia de servidor. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] requieren el permiso VIEW SERVER STATE en el servidor.  
  
##  <a name="monitoring-the-always-on-availability-groups-feature-on-a-server-instance"></a><a name="AoAgFeatureOnSI"></a> Supervisar la característica de grupos de disponibilidad AlwaysOn en una instancia de servidor  
 Para supervisar la característica [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] en una instancia de servidor, utilice la siguiente función integrada:  
  
 Función[SERVERPROPERTY](../../../t-sql/functions/serverproperty-transact-sql.md)  
 Devuelve información del servidor acerca de si [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] está habilitado y, en caso afirmativo, si se ha iniciado en la instancia de servidor.  
  
 **Nombres de columna:** IsHadrEnabled, HadrManagerStatus  
  
##  <a name="monitoring-availability-groups-on-the-wsfc-cluster"></a><a name="WSFC"></a> Supervisar grupos de disponibilidad en el clúster de WSFC  
 Para supervisar el clúster de clústeres de conmutación por error de Windows Server (WSFC) que hospeda una instancia de servidor local habilitada para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], utilice las vistas siguientes:  
  
 [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)  
 Si el nodo de clústeres de conmutación por error de Windows Server (WSFC) que hospeda una instancia de SQL Server con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] habilitado tiene cuórum de WSFC, **sys.dm_hadr_cluster** devuelve una fila que expone el nombre del clúster e información sobre el cuórum. Si el nodo de WSFC no tiene el quórum, no se devuelve ninguna fila.  
  
 **Nombres de columna:** cluster_name, quorum_type, quorum_type_desc, quorum_state, quorum_state_desc  
  
 [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
 Si el nodo de WSFC que hospeda la instancia habilitada para AlwaysOn de SQL Server tiene cuórum de WSFC, devuelve una fila para cada uno de los miembros que constituyen el cuórum y el estado de cada uno de ellos.  
  
 **Nombres de columna:** member_name, member_type, member_type_desc, member_state, member_state_desc, number_of_quorum_votes  
  
 [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
 Devuelve una fila para cada miembro que participa en la configuración de subred de un grupo de disponibilidad. Puede utilizar esta vista de administración dinámica para validar la dirección IP virtual de red configurada para cada réplica de disponibilidad.  
  
 **Nombres de columna:** member_name, network_subnet_ip, network_subnet_ipv4_mask, network_subnet_prefix_length, is_public, is_ipv4  
  
 **Clave principal:** member_name + network_subnet_IP + network_subnet_prefix_length  
  
 [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)  
 Para cada instancia de SQL Server que hospeda una réplica de disponibilidad que se une a su grupo de disponibilidad AlwaysOn, devuelve el nombre del nodo de clústeres de conmutación por error de Windows Server (WSFC) que hospeda la instancia de servidor. Esta vista de administración dinámica tiene las siguientes aplicaciones:  
  
-   Esta vista de administración dinámica es útil para detectar un grupo de disponibilidad con varias réplicas de disponibilidad que se hospedan en el mismo nodo de WSFC, que es una configuración no admitida que puede producirse después de una conmutación por error de FCI si el grupo de disponibilidad está configurado incorrectamente.  
  
-   Cuando varias instancias de SQL Server se hospedan en el mismo nodo de WSFC, el archivo DLL de recursos utiliza esta vista de administración dinámica para determinar la instancia de SQL Server a la que se va a conectar.  
  
 **Nombres de columna:** ag_resource_id, instance_name, node_name  
  
 [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)  
 Muestra la asignación de grupos de disponibilidad AlwaysOn que la instancia actual de SQL Server ha unido a tres identificadores únicos: un identificador de grupo de disponibilidad, un identificador de recurso de WSFC y un identificador de grupo de WSFC. La finalidad de esta asignación es controlar el escenario en el que cambia el nombre del recurso o el grupo de WSFC.  
  
 **Nombres de columna:** ag_name, ag_id, ag_resource_id, ag_group_id  
  
> [!NOTE]  
>  Vea también **sys.dm_hadr_availability_replica_cluster_nodes** y **sys.dm_hadr_availability_replica_cluster_states** en la sección [Supervisar réplicas de disponibilidad](#AvReplicas) y **sys.availability_databases_cluster** y **sys.dm_hadr_database_replica_cluster_states** en la sección [Supervisar las bases de datos de disponibilidad](#AvDbs) más adelante en este tema.  
  
 Para obtener información sobre los clústeres de WSFC y [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vea [Clústeres de conmutación por error de Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md) y [Clústeres de conmutación por error y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
  
##  <a name="monitoring-availability-groups"></a><a name="AvGroups"></a> Supervisar grupos de disponibilidad  
 Para supervisar los grupos de disponibilidad para los que la instancia de servidor hospeda una réplica de disponibilidad, utilice las vistas siguientes:  
  
 [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
 Devuelve una fila para cada grupo de disponibilidad para el que la instancia local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] hospeda una réplica de disponibilidad. Cada fila contiene una copia almacenada en caché de los metadatos del grupo de disponibilidad.  
  
 **Nombres de columna:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference y automated_backup_preference_desc  
  
 [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)  
 Devuelve una fila para cada grupo de disponibilidad del clúster de WSFC. Cada fila contiene los metadatos del grupo de disponibilidad procedentes del clúster de clústeres de conmutación por error de Windows Server (WSFC).  
  
 **Nombres de columna:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference y automated_backup_preference_desc  
  
 [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
 Devuelve una fila para cada grupo de disponibilidad que posee una réplica de disponibilidad en la instancia local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Cada fila muestra los estados que definen el estado de un grupo de disponibilidad determinado.  
  
 **Nombres de columna:** group_id, primary_replica, primary_recovery_health, primary_recovery_health_desc, secondary_recovery_health, secondary_recovery_health_desc, synchronization_health y synchronization_health_desc  
  
##  <a name="monitoring-availability-replicas"></a><a name="AvReplicas"></a> Supervisar réplicas de disponibilidad  
 Para supervisar réplicas de disponibilidad, utilice las siguientes vistas y función del sistema:  
  
 [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)  
 Devuelve una fila para cada una de las réplicas de disponibilidad en cada grupo de disponibilidad para el que la instancia local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] hospeda una réplica de disponibilidad.  
  
 **Nombres de columna:** replica_id, group_id, replica_metadata_id, replica_server_name, owner_sid, endpoint_url, availability_mode, availability_mode_desc, failover_mode, failover_mode_desc, session_timeout, primary_role_allow_connections, primary_role_allow_connections_desc, secondary_role_allow_connections, secondary_role_allow_connections_desc, create_date, modify_date, backup_priority, read_only_routing_url  
  
 [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
 Devuelve una fila para la lista de enrutamiento de solo lectura de cada réplica de disponibilidad en un grupo de disponibilidad AlwaysOn en el clúster de conmutación por error de WSFC.  
  
 **Nombres de columna:** replica_id, routing_priority, read_only_replica_id  
  
 [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)  
 Devuelve una fila para cada réplica de disponibilidad (independientemente del estado de unión) de los grupos de disponibilidad AlwaysOn del clúster de clústeres de conmutación por error de Windows Server (WSFC).  
  
 **Nombres de columna:** group_name, replica_server_name, node_name  
  
 [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
 Devuelve una fila para cada réplica (independientemente del estado de unión) de todos los grupos de disponibilidad AlwaysOn (independientemente de la ubicación de la réplica) del clúster de clústeres de conmutación por error de Windows Server (WSFC).  
  
 **Nombres de columna:** replica_id, replica_server_name, group_id, join_state, join_state_desc  
  
 [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)  
 Devuelve una fila que muestra el estado de cada réplica de disponibilidad local y una fila para cada réplica de disponibilidad remota en el mismo grupo de disponibilidad.  
  
 **Nombres de columna:** replica_id, group_id, is_local, role, role_desc, operational_state, operational_state_desc, connected_state, connected_state_desc, recovery_health, recovery_health_desc, synchronization_health, synchronization_health_desc, last_connect_error_number, last_connect_error_description y last_connect_error_timestamp  
  
 [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
 Determina si la réplica actual es la réplica de copia de seguridad preferida.  
  
> [!NOTE]  
>  Para obtener más información sobre los contadores de rendimiento para réplicas de disponibilidad (el objeto de rendimiento **SQLServer:Availability Replica**  ), vea [SQL Server, réplica de disponibilidad](../../../relational-databases/performance-monitor/sql-server-availability-replica.md).  
  
##  <a name="monitoring-availability-databases"></a><a name="AvDbs"></a> Supervisar las bases de datos de disponibilidad  
 Para supervisar las bases de datos de disponibilidad, utilice las vistas siguientes:  
  
 [sys.availability_databases_cluster](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)  
 Contiene una fila por cada base de datos de la instancia de SQL Server que forma parte de todos los grupos de disponibilidad AlwaysOn del clúster, independientemente de si la base de datos de copia local ya se ha unido al grupo de disponibilidad.  
  
> [!NOTE]  
>  Cuando una base de datos se agrega a un grupo de disponibilidad, la base de datos principal se une automáticamente al grupo. Las bases de datos secundarias se deben preparar en cada réplica secundaria para poder unirse al grupo de disponibilidad.  
  
 **Nombres de columna:** group_id, group_database_id, database_name  
  
 [sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
 Contiene una fila por cada base de datos de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si una base de datos pertenece a una réplica de disponibilidad, la fila de esa base de datos muestra el GUID de la réplica y el identificador único de la base de datos dentro de su grupo de disponibilidad.  
  
 **[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] :** replica_id, group_database_id  
  
 [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
 Devuelve una fila por cada intento de reparación de página automática en cualquier base de datos de disponibilidad en una réplica de disponibilidad hospedada para cualquier grupo de disponibilidad por la instancia de servidor. Esta vista contiene las filas para los últimos intentos de reparación de página automática en una base de datos principal o secundaria determinada, con un máximo de 100 filas por base de datos. En cuanto una base de datos alcanza el máximo, la fila del siguiente intento de reparación de página automática reemplazará una de las entradas existentes.  
  
 **Nombres de columna:** database_id, file_id, page_id, error_type, page_status, modification_time  
  
 [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
 Devuelve una fila por cada base de datos que participa en un grupo de disponibilidad para el que la instancia local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] hospeda una réplica de disponibilidad.  
  
 **Nombres de columna:** database_id, group_id, replica_id, group_database_id, is_local, synchronization_state, synchronization_state_desc, is_commit_participant, synchronization_health, synchronization_health_desc, database_state, database_state_desc, is_suspended, suspend_reason, suspend_reason_desc, recovery_lsn, truncation_lsn, last_sent_lsn, last_sent_time, last_received_lsn, last_received_time, last_hardened_lsn, last_hardened_time, last_redone_lsn, last_redone_time, log_send_queue_size, log_send_rate, redo_queue_size, redo_rate, filestream_send_rate, end_of_log_lsn, last_commit_lsn, last_commit_time, low_water_mark_for_ghosts  
  
 [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
 Devuelve una fila que contiene información para proporcionar una visión general del estado de las bases de datos de disponibilidad de cada grupo de disponibilidad del clúster de clústeres de conmutación por error de Windows Server (WSFC). Esta vista de administración dinámica es útil cuando se planea o responde a una conmutación por error o para detectar qué réplica de un grupo de disponibilidad soporta el truncamiento del registro de una base de datos principal dada.  
  
 **Nombres de columna:** replica_id, group_database_id, database_name, is_failover_ready, is_pending_secondary_suspend, is_database_joined, recovery_lsn, truncation_lsn  
  
> [!NOTE]  
>  La ubicación de la réplica principal es el origen autorizado para un grupo de disponibilidad.  
  
> [!NOTE]  
>  Para obtener más información sobre los contadores de rendimiento de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] para bases de datos de disponibilidad (el objeto de rendimiento **SQLServer:Database Replica** ), vea [SQL Server, réplica de base de datos](../../../relational-databases/performance-monitor/sql-server-database-replica.md). Además, para supervisar la actividad del registro de transacciones en las bases de datos de disponibilidad, use los contadores siguientes del objeto de rendimiento **SQLServer:Databases**: **Tiempo de escritura de vaciados de registros (ms)** , **Vaciados de registro/s.** , **Errores de caché del grupo de registros/s.** , **Lecturas de disco del grupo de registros/s** y **Solicitudes del grupo de registros/s**. Para más información, vea [SQL Server, Databases Object](../../../relational-databases/performance-monitor/sql-server-databases-object.md).  
  
##  <a name="monitoring-availability-group-listeners"></a><a name="AGlisteners"></a> Supervisar agentes de escucha del grupo de disponibilidad  
 Para supervisar los agentes de escucha del grupo de disponibilidad en las subredes del clúster de WSFC, utilice las vistas siguientes:  
  
 [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
 Devuelve una fila para cada dirección IP virtual conforme que está actualmente en línea para un agente de escucha del grupo de disponibilidad.  
  
 **Nombres de columna:** listener_id, ip_address, ip_subnet_mask, is_dhcp, network_subnet_ip, network_subnet_prefix_length, network_subnet_ipv4_mask, state, state_desc  
  
 [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
 Para un grupo de disponibilidad determinado, devuelve cero filas que indican que no hay ningún nombre de red asociado al grupo de disponibilidad o devuelve una fila por cada configuración de agente de escucha del grupo de disponibilidad del clúster de WSFC.  
  
 **Nombres de columna:** group_id, listener_id, dns_name, port, is_conformant, ip_configuration_string_from_cluster  
  
 [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
 Devuelve una fila que contiene la información de estado dinámico para cada agente de escucha TCP.  
  
 **Nombres de columna:** listener_id, ip_address, is_ipv4, port, type, type_desc, state, state_desc, start_time  
  
 **Clave principal:** listener_id  
  
 Para obtener información sobre los agentes de escucha de grupo de disponibilidad, vea [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Tareas de supervisión de grupos de disponibilidad AlwaysOn:**  
  
-   [Usar los detalles del Explorador de objetos para supervisar los grupos de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)  
  
-   [Ver las propiedades del grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
-   [Ver las propiedades de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Ver las propiedades del agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
 **Referencia de supervisión de grupos de disponibilidad AlwaysOn (Transact-SQL):**  
  
-   [SERVERPROPERTY &#40;Transact-SQL&#41;](../../../t-sql/functions/serverproperty-transact-sql.md)  
  
-   [sys.availability_group_listener_ip_addresses &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
  
-   [sys.availability_group_listeners &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
  
-   [sys.availability_databases_cluster &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)  
  
-   [sys.availability_groups &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
  
-   [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_nodes &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
-   [sys.dm_hadr_auto_page_repair &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
  
-   [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_cluster &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)  
  
-   [sys.dm_hadr_cluster_members &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
  
-   [sys.dm_hadr_cluster_networks &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_instance_node_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)  
  
-   [sys.dm_hadr_name_id_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)  
  
-   [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
-   [sys.dm_tcp_listener_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
  
-   [sys.fn_hadr_backup_is_preferred_replica  &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
  
 **Contadores de rendimiento de AlwaysOn:**  
  
-   [SQL Server, réplica de disponibilidad](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)  
  
-   [SQL Server, réplica de base de datos](../../../relational-databases/performance-monitor/sql-server-database-replica.md)  
  
-   [SQL Server, Databases Object](../../../relational-databases/performance-monitor/sql-server-databases-object.md)  
  
 **Administración basada en directivas para grupos de disponibilidad AlwaysOn**  
  
-   [Usar directivas de AlwaysOn para ver el estado de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
-   [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Consulte también  
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Herramientas para supervisar grupos de disponibilidad Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
  
