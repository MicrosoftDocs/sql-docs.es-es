---
title: Administración de un grupo de disponibilidad (índice de contenido)
description: Índice de referencia con vínculos a artículos que describen los aspectos básicos de la administración de un grupo de disponibilidad AlwaysOn, como la modificación de propiedades, la adición o eliminación de réplicas y bases de datos, la conmutación por error, la configuración del agente de escucha, etc.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], managing
ms.assetid: 0b7542fa-235e-413d-81bf-3eff9ee07480
author: cawrites
ms.author: chadam
ms.openlocfilehash: 27cce2f1be58688e3cf4156206650931c060d01a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343900"
---
# <a name="administration-of-an-availability-group"></a>Administración de un grupo de disponibilidad
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
 La administración de un grupo de disponibilidad AlwaysOn en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] implica una o varias de las siguientes tareas:  
  
-   Modificar las propiedades de una réplica de disponibilidad existente, por ejemplo para cambiar el acceso de la conexión de cliente (para configurar réplicas secundarias legibles), cambiar el modo de conmutación por error, el modo de disponibilidad o el valor de tiempo de espera de la sesión.    
-   Agregar o quitar réplicas secundarias.    
-   Agregar o quitar una base de datos.    
-   Suspender o reanudar una base de datos.   
-   Realizar una conmutación por error manual planeada (una *conmutación por error manual*) o una conmutación por error manual forzada (una *conmutación por error forzada*).    
-   Crear o configurar un agente de escucha del grupo de disponibilidad.    
-   Administrar [réplicas secundarias legibles](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) para un grupo de disponibilidad determinado. Esto implica la configuración de una o varias replica el acceso de solo lectura al ejecutar con rol secundario, y la configuración de enrutamiento de solo lectura.    
-   Administrar [copias de seguridad en las réplicas secundarias](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md) para un grupo de disponibilidad determinado. Esto implica configurar desde donde prefiera que se ejecuten los trabajos de copia de seguridad y despues incluir en scripts los trabajos de copia de seguridad para implementar sus preferencias de copia de seguridad. necesita los trabajos de copia de seguridad de script para cada base de datos del grupo de la disponibilidad en cada instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospeda una réplica de disponibilidad.    
-   Eliminar un grupo de disponibilidad.    
-   Migración entre clústeres de grupos de disponibilidad AlwaysOn para la actualización del sistema operativo  
  
## <a name="configure-an-existing-availability-group"></a>Configuración de un grupo de disponibilidad existente
  
-   [Agregar una réplica secundaria a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Quitar una réplica secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Agregar una base de datos a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)    
-   [Quitar una base de datos secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [Quitar una base de datos principal de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
-   [Configurar la directiva de conmutación por error flexible para controlar las condiciones de la conmutación automática por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
 ## <a name="manage-an-availability-group"></a>Administración de un grupo de disponibilidad  
  
-   [Configurar la copia de seguridad en réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Realizar una conmutación por error manual planeada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Realizar una conmutación por error manual forzada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Quitar un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
 ## <a name="manage-an-availability-replica"></a>Administración de una réplica de disponibilidad  
  
-   [Agregar una réplica secundaria a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Combinar una réplica secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Quitar una réplica secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Cambiar el modo de disponibilidad de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Cambiar el modo de conmutación por error de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Configurar la copia de seguridad en réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Configurar el acceso de solo lectura en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Configurar el enrutamiento de solo lectura para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Cambiar el tiempo de espera de la sesión en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
## <a name="manage-an-availability-database"></a>Administración de una base de datos de disponibilidad  
  
-   [Agregar una base de datos a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
-   [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Quitar una base de datos principal de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
-   [Quitar una base de datos secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [Suspender una base de datos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md)  
  
-   [Reanudar una base de datos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
## <a name="monitor-an-availability-group"></a>Supervisión de un grupo de disponibilidad
  
-   [Herramientas para supervisar grupos de disponibilidad Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
 ## <a name="support-migrating-availability-groups-to-a-new-wsfc-cluster-cross-cluster-migration"></a>Compatibilidad con la migración de grupos de disponibilidad a un nuevo clúster de WSFC (migración entre clústeres)
  
-   [Cambiar el contexto de clúster de HADR de la instancia de servidor &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)  
  
-   [Poner sin conexión un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   **Blogs:**  
  
     [Blogs del equipo de Always On de SQL Server: el blog oficial del equipo de Always On de SQL Server](/archive/blogs/sqlalwayson/)    
[Blogs de los ingenieros de SQL Server de CSS](/archive/blogs/psssql/)  
  
-   **Vídeos:**  
  
     [Microsoft SQL Server Code-Named "Denali", Serie Always On, parte 1: Introducción a la solución de alta disponibilidad de próxima generación](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)    
     [Microsoft SQL Server Code-Named "Denali", Serie Always On, parte 2: Creación de una solución crítica de alta disponibilidad mediante Always On](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **Notas del producto:**  
  
     [Notas del producto de Microsoft para SQL Server 2012](https://msdn.microsoft.com/library/hh403491.aspx)    
     [Notas del producto del equipo de asesoramiento al cliente de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Consulte también  
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Configuración de una instancia del servidor para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)   
 [Creación y configuración de grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Secundarias activas: réplicas secundarias legibles &#40;grupos de disponibilidad Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Secundarias activas: copia de seguridad en las réplicas secundarias &#40;grupos de disponibilidad Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)   
 [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Directivas de AlwaysOn para problemas operativos con Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)   
 [Supervisión de los grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Grupos de disponibilidad Always On: interoperabilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Información general sobre instrucciones Transact-SQL para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [Información general de los cmdlets de PowerShell para grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
