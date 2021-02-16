---
title: Eliminación de una réplica secundaria de un grupo de disponibilidad
description: 'Pasos para quitar una réplica secundaria a un grupo de disponibilidad Always On mediante Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio. '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removesecondaryar.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], configuring
ms.assetid: 35ddc8b6-3e7c-4417-9a0a-d4987a09ddf7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51211eafbc847a5ad7daae6fecc128a489116cd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344605"
---
# <a name="remove-a-secondary-replica-from-an-availability-group-sql-server"></a>Quitar una réplica secundaria de un grupo de disponibilidad (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se explica cómo quitar una réplica secundaria de un grupo de disponibilidad AlwaysOn con [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
 
   
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Esta tarea solo se admite en la réplica principal.    
-   Solo se puede quitar una réplica secundaria de un grupo de disponibilidad.  
  
## <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Debe estar conectado a la instancia del servidor que hospeda la réplica principal del grupo de disponibilidad.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para quitar una réplica secundaria**  
  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal y expanda el árbol.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Seleccione el grupo de disponibilidad y expanda el nodo **Réplicas de disponibilidad** .  
  
4.  Este paso depende de si desea quitar varias réplicas o solo una, del modo siguiente:  
  
    -   Para quitar varias réplicas, use el panel **Detalles del Explorador de objetos** para ver y seleccionar todas las réplicas que desea quitar. Para obtener más información, vea [Usar los detalles del Explorador de objetos para supervisar los grupos de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Para quitar una sola réplica, selecciónela en el panel **Explorador de objetos** o el panel **Detalles del Explorador de objetos** .  
  
5.  Haga clic con el botón derecho en la réplica o réplicas secundarias seleccionadas y seleccione **Quitar del grupo de disponibilidad** en el menú de comandos.  
  
6.  En el cuadro de diálogo **Quitar réplicas secundarias del grupo de disponibilidad** , para quitar todas las réplicas secundarias enumeradas, haga clic en **Aceptar**. Si no desea quitar todas las réplicas enumeradas, haga clic en **Cancelar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para quitar una réplica secundaria**  
  
1.  Conéctese a la instancia del servidor que hospeda la réplica principal.  
  
2.  Use la instrucción [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) del siguiente modo:  
  
     ALTER AVAILABILITY GROUP *group_name* REMOVE REPLICA ON '*instance_name*' [,...*n*]  
  
     donde *group_name* es el nombre del grupo de disponibilidad e *instance_name* es la instancia del servidor donde se encuentra la réplica secundaria.  
  
     En el ejemplo siguiente se quita una réplica secundaria del grupo de disponibilidad *MyAG* . La réplica secundaria de destino se encuentra en una instancia del servidor denominada *HADR_INSTANCE* en un equipo denominado *COMPUTER02*.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG REMOVE REPLICA ON 'COMPUTER02\HADR_INSTANCE';  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
 **Para quitar una réplica secundaria**  
  
1.  Cambie el directorio (**cd**) a la instancia del servidor que hospeda la réplica principal.  
  
2.  Use el cmdlet **Remove-SqlAvailabilityReplica** .  
  
     Por ejemplo, el comando siguiente quita la réplica de disponibilidad en el servidor `MyReplica` del grupo de disponibilidad denominado `MyAg`.  Este comando debe ejecutarse en la instancia del servidor que hospeda la réplica principal del grupo de disponibilidad.  
  
    ```  
    Remove-SqlAvailabilityReplica `   
    -Path SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Para configurar y usar el proveedor de SQL Server PowerShell**  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-removing-a-secondary-replica"></a><a name="PostBestPractices"></a> Seguimiento: después de quitar una réplica secundaria  
 Si especifica una réplica que no está actualmente disponible, cuando la réplica se ponga en línea, detectará que se ha quitado.  
  
 Quitar una réplica produce la detención de la recepción de datos Después de que una réplica secundaria confirma que se ha quitado del almacén global, la réplica quita la configuración del grupo disponibilidad de las bases de datos, que permanecen en la instancia del servidor local en estado RECOVERING.  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Agregar una réplica secundaria a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)   
 [Quitar un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
