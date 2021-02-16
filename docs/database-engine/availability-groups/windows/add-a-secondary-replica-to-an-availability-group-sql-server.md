---
title: Adición de una réplica secundaria a un grupo de disponibilidad
description: Obtenga información sobre cómo agregar una réplica secundaria a un grupo de disponibilidad Always On mediante Transact-SQL (T-SQL), PowerShell o el Asistente para grupo de disponibilidad de SQL Server Management Studio (SSMS).
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], configuring
ms.assetid: 6669dcce-85f9-495f-aadf-7f62cff4a9da
author: cawrites
ms.author: chadam
ms.openlocfilehash: cde589fa3ba6f1fd46dcf1b16077797cdb268dad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354396"
---
# <a name="add-a-secondary-replica-to-an-always-on-availability-group"></a>Adición de una réplica secundaria a un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo agregar una réplica secundaria a un grupo de disponibilidad AlwaysOn existente usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  

  
##  <a name="prerequisites-and-restrictions"></a><a name="PrerequisitesRestrictions"></a> Requisitos previos y restricciones  
  
-   Debe estar conectado a la instancia del servidor que hospeda la réplica principal.  
  
 Para obtener más información, vea [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  

##  <a name="security"></a><a name="Security"></a> Seguridad  
  
###  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.  

[!INCLUDE[Freshness](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para agregar una réplica**  
  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal y expanda el árbol.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Haga clic con el botón secundario en el grupo de disponibilidad y seleccione uno de los siguientes comandos:  
  
    -   Seleccione el comando **Agregar réplica** para iniciar el Asistente para agregar una réplica al grupo de disponibilidad. Para obtener más información, vea [Usar el Asistente para agregar una réplica al grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md).  
  
    -   O bien, seleccione el comando **Propiedades** para abrir el cuadro de diálogo **Propiedades de grupo de disponibilidad** . Los pasos para agregar una réplica en este cuadro de diálogo son los siguientes:  
  
        1.  En el panel **Réplicas de disponibilidad** del cuadro de diálogo, haga clic en el botón **Agregar** . Esto crea y selecciona una entrada de réplica en la que el campo Instancia de servidor en blanco está seleccionado.  
  
        2.  Escriba el nombre de una instancia de servidor que cumpla los requisitos previos para hospedar una réplica de disponibilidad.  
  
         Para agregar réplicas adicionales, repita los pasos anteriores. Cuando haya terminado de especificar las réplicas, haga clic en **Aceptar** para completar la operación.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para agregar una réplica**  
  
1.  Conéctese a la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospeda la réplica principal.  
  
2.  Agregue la nueva réplica secundaria al grupo de disponibilidad utilizando la cláusula de la instrucción ALTER AVAILABILITY GROUP. Las opciones ENDPOINT_URL, AVAILABILITY_MODE y FAILOVER_MODE son necesarias en una cláusula ADD REPLICA ON. Las demás opciones (BACKUP_PRIORITY, SECONDARY_ROLE, PRIMARY_ROLE y SESSION_TIMEOUT) son opcionales. Para obtener más información, vea [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md).  
  
     Por ejemplo, la siguiente instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)] crea una nueva réplica para un grupo de disponibilidad denominado `MyAG` en la instancia de servidor predeterminada hospedada por `COMPUTER04`, cuya dirección URL del extremo es `TCP://COMPUTER04.Adventure-Works.com:5022'`. Esta réplica admite la conmutación por error manual y el modo de disponibilidad de confirmación asincrónica.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG ADD REPLICA ON 'COMPUTER04'   
       WITH (  
             ENDPOINT_URL = 'TCP://COMPUTER04.Adventure-Works.com:5022',  
             AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
             FAILOVER_MODE = MANUAL  
             );  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
 **Para agregar una réplica**  
  
1.  Cambie el directorio (**cd**) a la instancia del servidor que hospeda la réplica principal.  
  
2.  Use el cmdlet **New-SqlAvailabilityReplica** .  
  
     Por ejemplo, el comando siguiente agrega una réplica de disponibilidad a un grupo de disponibilidad existente denominado `MyAg`. Esta réplica admite la conmutación por error manual y el modo de disponibilidad de confirmación asincrónica. En el rol secundario, esta réplica admitirá conexiones de acceso de lectura, lo que permite descargar a esta réplica del procesamiento de solo lectura.  
  
    ```  
    $agPath = "SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg"  
    $endpointURL = "TCP://PrimaryServerName.domain.com:5022"  
    $failoverMode = "Manual"  
    $availabilityMode = "AsynchronousCommit"  
    $secondaryReadMode = "AllowAllConnections"  
  
    New-SqlAvailabilityReplica -Name SecondaryServer\Instance `   
    -EndpointUrl $endpointURL `   
    -FailoverMode $failoverMode `   
    -AvailabilityMode $availabilityMode `   
    -ConnectionModeInSecondaryRole $secondaryReadMode `   
    -Path $agPath  
    ```  
  
    > [!NOTE]  
    >  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Para configurar y usar el proveedor de SQL Server PowerShell**  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-adding-a-secondary-replica"></a><a name="FollowUp"></a> Seguimiento: después de agregar una réplica secundaria  
 Para agregar una réplica para un grupo de disponibilidad existente, debe realizar los pasos siguientes:  
  
1.  Conéctese a la instancia del servidor que va a hospedar la nueva réplica secundaria.  
  
2.  Una la nueva réplica secundaria al grupo de disponibilidad. Para obtener más información, vea [Combinar una réplica secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
3.  Para cada base de datos del grupo de disponibilidad debe crear una base de datos secundaria en la instancia del servidor que hospeda la réplica secundaria. Para obtener más información, vea [Preparar manualmente una base de datos secundaria para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
4.  Una cada una de las bases de datos secundarias al grupo de disponibilidad. Para obtener más información, vea [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para administrar una réplica de disponibilidad**  
  
-   [Combinar una réplica secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Quitar una réplica secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Configurar el acceso de solo lectura en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Cambiar el modo de disponibilidad de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Cambiar el modo de conmutación por error de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Cambiar el tiempo de espera de la sesión en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
-   [Cambiar el tiempo de espera de la sesión en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Creación y configuración de grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
