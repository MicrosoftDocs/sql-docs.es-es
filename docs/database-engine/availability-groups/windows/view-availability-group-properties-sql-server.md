---
title: Ver las propiedades del grupo de disponibilidad
description: 'Vea las propiedades de un grupo de disponibilidad AlwaysOn con SQL Server Management Studio (SSMS), Transact-SQL (T-SQL) o SQL PowerShell. '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server]
ms.assetid: 61243c87-bd62-4510-863f-2a8f347caf1f
author: cawrites
ms.author: chadam
ms.openlocfilehash: d7c82b6f567872a32615a21b49a42007fbfca7f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350754"
---
# <a name="view-availability-group-properties-sql-server"></a>Ver las propiedades del grupo de disponibilidad (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo pueden verse las propiedades de un grupo disponibilidad AlwaysOn con [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)] en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para ver y cambiar las propiedades de un grupo de disponibilidad**  
  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal y expanda el árbol.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Haga clic con el botón derecho en el grupo de disponibilidad cuyas propiedades quiere ver y seleccione el comando **Propiedades** .  
  
4.  En el cuadro de diálogo **Propiedades de grupo de disponibilidad** , use las páginas **General** y **Preferencias de copia de seguridad** para ver y, en algunos casos, cambiar las propiedades del grupo de disponibilidad seleccionado. Para obtener más información, vea [Propiedades de grupo de disponibilidad: Nuevo grupo de disponibilidad &#40;página General&#41;](../../../database-engine/availability-groups/windows/availability-group-properties-new-availability-group-general-page.md) y [Propiedades de grupo de disponibilidad: Nuevo grupo de disponibilidad &#40;página Preferencias de copia de seguridad&#41;](../../../database-engine/availability-groups/windows/availability-group-properties-new-availability-group-backup-preferences-page.md).  
  
     Use la página de **Permisos** para ver los inicios de sesión, roles y permisos explícitos actuales asociados al grupo de disponibilidad. Para más información, consulte [Permissions or Securables Page](../../../relational-databases/security/permissions-or-securables-page.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para ver las propiedades y el estado de un grupo de disponibilidad**  
  
 Para consultar las propiedades y los estados de los grupos de disponibilidad en los que la instancia de servidor hospeda una réplica de disponibilidad, use las vistas siguientes:  
  
 [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
 Devuelve una fila para cada grupo de disponibilidad para el que la instancia local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] hospeda una réplica de disponibilidad. Cada fila contiene una copia almacenada en caché de los metadatos del grupo de disponibilidad.  
  
 **Nombres de columna:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference y automated_backup_preference_desc  
  
 [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)  
 Devuelve una fila para cada grupo de disponibilidad del clúster de WSFC. Cada fila contiene los metadatos del grupo de disponibilidad procedentes del clúster de clústeres de conmutación por error de Windows Server (WSFC).  
  
 **Nombres de columna:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference y automated_backup_preference_desc  
  
 [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
 Devuelve una fila para cada grupo de disponibilidad que posee una réplica de disponibilidad en la instancia local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Cada fila muestra los estados que definen el estado de un grupo de disponibilidad determinado.  
  
 **Nombres de columna:** group_id, primary_replica, primary_recovery_health, primary_recovery_health_desc, secondary_recovery_health, secondary_recovery_health_desc, synchronization_health y synchronization_health_desc  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para obtener más información acerca de los grupos de disponibilidad**  
  
-   [Ver las propiedades de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Ver las propiedades del agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
-   [Directivas de AlwaysOn para problemas operativos con grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)  
  
-   [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
-   [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
 **Para configurar un grupo de disponibilidad existente**  
  
-   [Agregar una réplica secundaria a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Quitar una réplica secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Agregar una base de datos a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
-   [Quitar una base de datos secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Quitar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
-   [Quitar una base de datos principal de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
-   [Quitar un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
 **Para realizar la conmutación por error manual de un grupo de disponibilidad**  
  
-   [Realizar una conmutación por error manual planeada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Realizar una conmutación por error manual forzada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Directivas de AlwaysOn para problemas operativos con grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)  
  
  
