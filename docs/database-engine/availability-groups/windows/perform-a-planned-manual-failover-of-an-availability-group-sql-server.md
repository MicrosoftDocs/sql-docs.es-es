---
title: Realización de una conmutación por error manual planeada de un grupo de disponibilidad
description: Este tema se describe cómo realizar una conmutación por error manual planeada de un grupo de disponibilidad Always On.
ms.custom: seodec18
ms.date: 10/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.manualfailover.f1
helpviewer_keywords:
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 419f655d-3f9a-4e7d-90b9-f0bab47b3178
author: cawrites
ms.author: chadam
ms.openlocfilehash: e5efd8802ea57a6575f9e960b44ffffd9a32f83e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350784"
---
# <a name="perform-a-planned-manual-failover-of-an-always-on-availability-group-sql-server"></a>Realización de una conmutación por error manual planeada de un grupo de disponibilidad Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
En este tema se describe cómo realizar una conmutación por error manual sin pérdida de datos (una *conmutación por error manual planeada*) en un grupo de disponibilidad AlwaysOn mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Un grupo de disponibilidad realiza la conmutación por error en el nivel de réplica de disponibilidad. Una conmutación por error planeada, al igual que cualquier conmutación por error de un grupo de disponibilidad AlwaysOn, realiza la transición de una réplica secundaria a un rol principal. Al mismo tiempo, la conmutación por error realiza la transición de la réplica principal antigua al rol secundario.  
  
Una conmutación por error manual planeada solo se admite cuando la réplica principal y la réplica secundaria de destino se ejecutan en modo de confirmación sincrónica y están sincronizadas. Una conmutación por error manual planeada conserva todos los datos de las bases de datos secundarias que se unen al grupo de disponibilidad en la réplica secundaria de destino. Después de que la réplica principal antigua realiza la transición al rol secundario, sus bases de datos se convierten en bases de datos secundarias. A continuación, pueden empezar a sincronizarse con las bases de datos principales. Después de que todas realicen la transición al estado SYNCHRONIZED, la nueva réplica secundaria es apta para actuar como destino de una conmutación por error manual planeada futura.  
  
> [!NOTE]  
>  Si las replicas principal y secundaria están ambas configuradas para el modo de conmutación automática por error, una vez que la réplica secundaria se sincroniza, también puede servir como destino de una conmutación automática por error. Para más información, consulte [Modos de disponibilidad &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
   
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de empezar 

>[!IMPORTANT]
>Existen procedimientos específicos para la conmutación por error de un grupo de disponibilidad de escalado de lectura sin administrador de clústeres. Cuando un grupo de disponibilidad tiene CLUSTER_TYPE = NONE, siga los procedimientos que se describen en [Conmutación por error de la réplica principal en un grupo de disponibilidad de escalado de lectura](#fail-over-the-primary-replica-on-a-read-scale-availability-group).

###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones 
  
- Un comando de conmutación por error realiza la devolución en cuanto la réplica secundaria de destino haya aceptado el comando. Sin embargo, la recuperación de la base de datos se produce de forma asincrónica cuando el grupo de disponibilidad ha terminado la conmutación por error. 
- Puede que la coherencia entre las distintas bases de datos del grupo de disponibilidad no se mantenga en la conmutación por error. 
  
    > [!NOTE] 
    >  La compatibilidad con transacciones distribuidas y entre bases de datos varía según la versión de SQL Server y del sistema operativo. Para más información, consulte [Transacciones entre bases de datos y transacciones distribuidas para la creación de reflejo de la base de datos o grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md). 
  
###  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Requisitos previos y restricciones 
  
-   La réplica secundaria de destino y la réplica principal deben ejecutarse en modo de disponibilidad de confirmación sincrónica. 
-   Actualmente, la réplica secundaria de destino debe estar sincronizada con la réplica principal. Todas las bases de datos secundarias de esta réplica secundaria deben estar unidas al grupo de disponibilidad. También deben estar sincronizadas con sus bases de datos principales correspondientes (es decir, las bases de datos secundarias locales deben estar SINCRONIZADAS). 
  
    > [!TIP] 
    >  Para determinar la preparación para la conmutación por error de una réplica secundaria, consulte la columna **is_failover_ready** en la vista e administración dinámica [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md). O bien, puede examinar la columna **Preparación de la conmutación por error** del [panel de grupos AlwaysOn](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md). 
-   Esta tarea solo se admite en la réplica secundaria de destino. Debe estar conectado a la instancia del servidor que hospeda la réplica secundaria de destino. 
  
###  <a name="security"></a><a name="Security"></a> Seguridad 
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos 
 Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad. También se requieren los permisos CONTROL AVAILABILITY GROUP, ALTER ANY AVAILABILITY GROUP o CONTROL SERVER. 
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Usar SQL Server Management Studio 
 Para realizar la conmutación por error manual de un grupo de disponibilidad: 
  
1. En el Explorador de objetos, conéctese a una instancia de servidor que hospede una réplica secundaria del grupo de disponibilidad que es necesario conmutar por error. Expanda el árbol de servidores. 
  
2. Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** . 
  
3. Haga clic con el botón derecho en el grupo de disponibilidad que se va a conmutar por error y seleccione el comando **Conmutación por error**. 
  
4. Se inicia el asistente para la conmutación por error de grupos de disponibilidad. Para más información, consulte [Usar el Asistente para conmutación por error del grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md). 
  
##  <a name="use-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL 
 Para realizar la conmutación por error manual de un grupo de disponibilidad: 
  
1. Conéctese a la instancia del servidor que hospeda la réplica secundaria de destino. 
  
2. Use la instrucción [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) del siguiente modo: 
  
     ALTER AVAILABILITY GROUP *group_name* FAILOVER 
  
     En la instrucción, *group_name* es el nombre del grupo de disponibilidad. 
  
     En el ejemplo siguiente se realiza manualmente una conmutación por error del grupo de disponibilidad *MyAg* a la réplica secundaria conectada: 
  
    ```  
    ALTER AVAILABILITY GROUP MyAg FAILOVER;  
    ```  
  
##  <a name="use-powershell"></a><a name="PowerShellProcedure"></a> Usar de PowerShell 
 Para realizar la conmutación por error manual de un grupo de disponibilidad: 
  
1. Cambie el directorio (**cd**) a la instancia del servidor que hospeda la réplica secundaria de destino. 
  
2. Use el cmdlet **Switch-SqlAvailabilityGroup** . 
  
    > [!NOTE] 
    >  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Obtener ayuda de SQL Server PowerShell](../../../powershell/sql-server-powershell.md). 
  
     En el ejemplo siguiente, se realiza manualmente una conmutación por error del grupo de disponibilidad *MyAg* a la réplica secundaria que tiene la ruta de acceso especificada: 
  
    ```  
    Switch-SqlAvailabilityGroup -Path SQLSERVER:\Sql\SecondaryServer\InstanceName\AvailabilityGroups\MyAg  
    ```  
  
    Para configurar y usar el proveedor de SQL Server PowerShell: 
  
    -   [Proveedor de PowerShell de SQL Server](../../../powershell/sql-server-powershell-provider.md) 
    -   [Obtener ayuda de SQL Server PowerShell](../../../powershell/sql-server-powershell.md) 

##  <a name="follow-up-after-you-manually-fail-over-an-availability-group"></a><a name="FollowUp"></a> Seguimiento: después de conmutar por error manualmente un grupo de disponibilidad 
 Si la conmutación por error se produjo fuera del grupo de disponibilidad de [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)], ajuste los votos de cuórum de los nodos de clúster de Windows Server para reflejar la nueva configuración del grupo de disponibilidad. Para más información, consulte [Clústeres de conmutación por error de Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md). 

<a name = "ReadScaleOutOnly"><a/>

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Conmutación por error de la réplica principal en un grupo de disponibilidad de escalado de lectura

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## <a name="see-also"></a>Consulte también 

 * [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) 
 * [Conmutación por error y modos de conmutación por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) 
 * [Realizar una conmutación por error manual forzada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md) 
  
