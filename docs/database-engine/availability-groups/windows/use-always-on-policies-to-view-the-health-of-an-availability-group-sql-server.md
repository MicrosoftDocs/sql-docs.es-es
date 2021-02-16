---
title: Directivas para ver el estado del grupo de disponibilidad
description: Use las directivas de AlwaysOn o PowerShell para determinar el estado operativo de un grupo de disponibilidad AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 6f1bcbc3-1220-4071-8e53-4b957f5d3089
author: cawrites
ms.author: chadam
ms.openlocfilehash: b9b06dac9dc00fd405fd28f1428a0833d968fabe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340837"
---
# <a name="use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server"></a>Usar directivas de AlwaysOn para ver el estado de un grupo de disponibilidad (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo determinar el estado operativo de un grupo de disponibilidad AlwaysOn con una directiva de AlwaysOn en [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o PowerShell en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Para obtener información sobre la administración basada en directivas de AlwaysOn, vea [Directivas de AlwaysOn para problemas operativos con grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md).  
  
> [!IMPORTANT]  
>  Para las directivas de AlwaysOn, los nombres de categoría se usan como identificadores. La acción de cambiar el nombre de una categoría de AlwaysOn interrumpirá la funcionalidad de la evaluación de estado. Por consiguiente, los nombres de categoría de AlwaysOn no deben modificarse nunca.  
  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere permisos CONNECT TO, VIEW SERVER STATE y VIEW ANY DEFINITION.  
  
##  <a name="using-the-always-on-dashboard"></a><a name="SSMSProcedure"></a> Usar el panel AlwaysOn  
 **Para abrir el panel AlwaysOn**  
  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda una de las réplicas de disponibilidad. Para ver información acerca de todas las réplicas de disponibilidad en un grupo de disponibilidad, use la instancia del servidor que hospeda la réplica principal.  
  
2.  Haga clic en el nombre del servidor para expandir el árbol.  
  
3.  Expanda el nodo **Alta disponibilidad de AlwaysOn** .  
  
     Haga clic con el botón derecho en el nodo **Grupos de disponibilidad** o expanda este nodo y haga clic con el botón derecho en un grupo de disponibilidad específico.  
  
4.  Seleccione el comando de **Mostrar panel** .  
  
 Para más información sobre cómo usar el panel AlwaysOn, vea [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](~/database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md).  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
 **Use Always On policies to view the health of an availability group**  
  
1.  Establezca el valor predeterminado (**cd**) en una instancia del servidor que hospede una de las réplicas de disponibilidad. Para ver información acerca de todas las réplicas de disponibilidad en un grupo de disponibilidad, use la instancia del servidor que hospeda la réplica principal.  
  
2.  Use los siguientes cmdlets:  
  
     **Test-SqlAvailabilityGroup**  
     Evalúa el estado de un grupo de disponibilidad mediante la evaluación de las directivas de administración basada en directivas (PBM) de SQL Server. Debe tener permisos CONNECT, VIEW SERVER STATE, y VIEW ANY DEFINITION para ejecutar este cmdlet.  
  
     Por ejemplo, el comando siguiente muestra todos los grupos de disponibilidad con el estado de mantenimiento "Error" de la instancia del servidor `Computer\Instance`.  
  
    ```  
    Get-ChildItem SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups `   
    | Test-SqlAvailabilityGroup | Where-Object { $_.HealthState -eq "Error" }  
    ```  
  
     **Test-SqlAvailabilityReplica**  
     Evalúa el estado de las réplicas de disponibilidad mediante la evaluación de las directivas de administración basada en directivas (PBM) de SQL Server. Debe tener permisos CONNECT, VIEW SERVER STATE, y VIEW ANY DEFINITION para ejecutar este cmdlet.  
  
     Por ejemplo, el comando siguiente evalúa el estado de la réplica de disponibilidad `MyReplica` del grupo de disponibilidad `MyAg` y genera un breve resumen.  
  
    ```  
    Test-SqlAvailabilityReplica `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
     **Test-SqlDatabaseReplicaState**  
     Evalúa el estado de una base de datos de disponibilidad en todas las réplicas de disponibilidad mediante la evaluación de directivas de administración basada en directivas (PBM) de SQL Server.  
  
     Por ejemplo, el comando siguiente evalúa el estado de todas las bases de datos de disponibilidad del grupo de disponibilidad `MyAg` y genera un breve resumen de cada base de datos.  
  
    ```  
    Get-ChildItem SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\DatabaseReplicaStates `   
     | Test-SqlDatabaseReplicaState  
    ```  
  
     Estos cmdlets aceptan las siguientes opciones:  
  
    |Opción|Descripción|  
    |------------|-----------------|  
    |**AllowUserPolicies**|Ejecuta las directivas de usuario que se encuentran en las categorías de directiva de AlwaysOn.|  
    |**InputObject**|Una colección de objetos que representan grupos de disponibilidad, réplicas de disponibilidad o estados de bases de datos de disponibilidad (dependiendo del cmdlet que esté utilizando). El cmdlet calculará el estado de los objetos especificados.|  
    |**NoRefresh**|Cuando se establece este parámetro, el cmdlet no actualizará manualmente los objetos especificados por el parámetro **-Path** o **-InputObject** .|  
    |**Path**|La ruta de acceso al grupo de disponibilidad, una o varias réplicas de disponibilidad o el estado del clúster de réplica de la base de datos de disponibilidad (dependiendo del cmdlet que esté utilizando). Se trata de un parámetro opcional. Si no se especifica, el valor del valor predeterminado de este parámetro es la ubicación de trabajo actual.|  
    |**ShowPolicyDetails**|Muestra el resultado de cada evaluación de directiva realizada por este cmdlet. El cmdlet envía un objeto por evaluación de la directiva, y este objeto tiene campos que describen los resultados de la evaluación (si la directiva se ha superado o no, el nombre y la categoría de la directiva, etc.).|  
  
     Por ejemplo, el siguiente comando **Test-SqlAvailabilityGroup** especifica el parámetro **-ShowPolicyDetails** para mostrar el resultado de cada evaluación de directiva realizada por este cmdlet en cada directiva de administración basada en directivas (PBM) que se ejecutó en el grupo de disponibilidad `MyAg`.  
  
    ```  
    Test-SqlAvailabilityGroup `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\AgName `  
    -ShowPolicyDetails  
  
    ```  
  
    > [!NOTE]  
    >  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Para configurar y usar el proveedor de SQL Server PowerShell**  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
 **Blogs del equipo de Always On de SQL Server: supervisión del estado de Always On con PowerShell:**  
  
-   [Parte 1: información general básica de los cmdlets](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-1-basic-cmdlet-overview)  
  
-   [Parte 2: uso avanzado de cmdlet](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-2-advanced-cmdlet-usage)  
  
-   [Parte 3: una aplicación simple de supervisión](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-3-a-simple-monitoring-application)  
  
-   [Parte 4: integración con el Agente SQL Server](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-4-integration-with-sql-server-agent)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Administración de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [Supervisión de los grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Directivas de AlwaysOn para problemas operativos con grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)  
  
