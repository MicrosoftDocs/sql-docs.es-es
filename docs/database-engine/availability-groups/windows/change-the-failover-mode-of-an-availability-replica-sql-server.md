---
title: Cambio del modo de conmutación por error de la réplica para un grupo de disponibilidad
description: Se describe cómo cambiar el modo de conmutación por error para una réplica en un grupo de disponibilidad Always On mediante Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 06/30/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- failover modes [SQL Server]
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], failover modes
- Availability Groups [SQL Server], configuring
ms.assetid: 619a826f-8e65-48eb-8c34-39497d238279
author: cawrites
ms.author: chadam
ms.openlocfilehash: a5f75b9de94e5826035c047bf1e99d0b9655a131
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343812"
---
# <a name="change-the-failover-mode-for-a-replica-within-an-always-on-availability-group"></a>Cambio del modo de conmutación por error para una réplica dentro de un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo cambiar el modo de conmutación por error de una réplica de disponibilidad de un grupo de disponibilidad AlwaysOn en [!INCLUDE[ssnoverison](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell. El modo de conmutación por error es una propiedad de réplica que determina el modo de conmutación por error para las réplicas que se ejecutan en modo de disponibilidad de confirmación sincrónica. Para obtener más información, vea [Conmutación por error y modos de conmutación por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) y [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
## <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Requisitos previos y restricciones  
  
-   Esta tarea solo se admite en las réplicas principales. Debe estar conectado a la instancia del servidor que hospeda la réplica principal.  
  
-   Las instancias de clúster de conmutación por error (FCI) de SQL Server no admiten la conmutación automática por error de grupos de disponibilidad, por lo tanto, todas las réplicas de disponibilidad hospedadas por un FCI solo se pueden configurar para la conmutación por error manual.  
  

##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para cambiar el modo de conmutación por error de una réplica de disponibilidad**  
  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal y expanda el árbol.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Haga clic en el grupo de disponibilidad cuya réplica desea cambiar.  
  
4.  Haga clic con el botón derecho en la réplica y haga clic en **Propiedades**.  
  
5.  En el cuadro de diálogo **Propiedades de réplica de disponibilidad** , use la lista desplegable **Modo de conmutación por error** para cambiar el modo de conmutación por error de esta réplica.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para cambiar el modo de conmutación por error de una réplica de disponibilidad**  
  
1.  Conéctese a la instancia del servidor que hospeda la réplica principal.  
  
2.  Use la instrucción [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) del siguiente modo:

    ```syntaxsql
    ALTER AVAILABILITY GROUP *group_name* MODIFY REPLICA ON '*server_name*'  
       WITH ( {  
             AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }
                | FAILOVER_MODE = { AUTOMATIC | MANUAL }
             }  )
    ```
    
    En el script anterior:

    - *nombre_grupo* es el nombre del grupo de disponibilidad.  
  
    - *server_name* es el nombre de equipo o el nombre de red del clúster de conmutación por error. En el caso de las instancias con nombre, agregue "\instance_name". Use el nombre que hospede la réplica que quiera modificar.
  
Para obtener más información sobre estos parámetros, vea [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md).  
  
En el ejemplo siguiente, escrito en la réplica principal del grupo de disponibilidad *MyAG* , se cambia el modo de conmutación por error a conmutación automática por error en la réplica de disponibilidad que se encuentra en la instancia del servidor predeterminada en un equipo denominado *COMPUTER01*.  
  
```sql
ALTER AVAILABILITY GROUP MyAG MODIFY REPLICA ON 'COMPUTER01' WITH  
    (FAILOVER_MODE = AUTOMATIC);  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
 **Para cambiar el modo de conmutación por error de una réplica de disponibilidad**  
  
1.  Cambie el directorio (**cd**) a la instancia del servidor que hospeda la réplica principal.  
  
2.  Use el cmdlet **Set-SqlAvailabilityReplica** con el parámetro **FailoverMode** . Cuando se establece una réplica en conmutación automática por error, puede que sea necesario usar el parámetro **AvailabilityMode** para cambiar la réplica al modo de disponibilidad de confirmación sincrónica.  
  
    Por ejemplo, el comando siguiente modifica la réplica `MyReplica` en el grupo de disponibilidad `MyAg` para utilizar el modo de disponibilidad de confirmación sincrónica y admitir la conmutación automática por error.  
  
    ```powershell
    Set-SqlAvailabilityReplica -AvailabilityMode "SynchronousCommit" -FailoverMode "Automatic" `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\Replicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Para configurar y usar el proveedor de SQL Server PowerShell**  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Conmutación por error y modos de conmutación por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)  
  
