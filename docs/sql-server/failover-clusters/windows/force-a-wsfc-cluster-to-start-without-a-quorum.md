---
title: Forzar el inicio de un clúster WSFC sin un quórum
description: Fuerce el inicio sin cuórum de un nodo de clústeres de conmutación por error de Windows Server, lo que puede ser necesario para recuperar los datos y volver a establecer la alta disponibilidad.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
ms.assetid: 4a121375-7424-4444-b876-baefa8fe9015
author: cawrites
ms.author: chadam
ms.openlocfilehash: dd232e6b5908ff26c47a4b7a945a68f919175418
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353548"
---
# <a name="force-a-wsfc-cluster-to-start-without-a-quorum"></a>Forzar el inicio de un clúster WSFC sin un quórum
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo forzar que un nodo de clúster de los clústeres de conmutación por error de Windows Server (WSFC) se inicie sin un quórum.  Esto puede ser necesario en los escenarios de recuperación de desastres y de múltiples subredes para recuperar datos y reestablecer por completo la alta disponibilidad para las instancias de clúster de conmutación por error de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] y [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   **Antes de empezar:**  [Recomendaciones](#Recommendations), [Seguridad](#Security)  
  
-   **Para forzar que un clúster se inicie sin un quórum con:**  [Usar el Administrador de clústeres de conmutación por error](#FailoverClusterManagerProcedure), [Usar PowerShell](#PowerShellProcedure), [Usar Net.exe](#CommandPromptProcedure)  
  
-   **Seguimiento:**  [Seguimiento: después de forzar que el clúster se inicie sin un cuórum](#FollowUp)  
  
##  <a name="before-you-start"></a><a name="BeforeYouBegin"></a> Antes de empezar  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
 Excepto donde se indique de forma explícita, los procedimientos de este tema deben funcionar si se ejecutan desde cualquier nodo del clúster de WSFC.  No obstante, se pueden obtener mejores resultados, y evitar programas de red, si se ejecutan estos pasos desde el nodo que se desea forzar para que se inicie sin un quórum.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 El usuario debe ser una cuenta de dominio que sea miembro del grupo Administradores en cada nodo del clúster de WSFC.  
  
##  <a name="using-failover-cluster-manager"></a><a name="FailoverClusterManagerProcedure"></a> Usar el Administrador de clústeres de conmutación por error  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Para forzar que un clúster se inicie sin un quórum  
  
1.  Abra el Administrador de clústeres de conmutación por error y conéctese al nodo de clúster deseado para forzarlo en línea.  
  
2.  En el panel **Acciones**, haga clic en **Forzar inicio de clúster** y después en **Sí, forzar el inicio del clúster**.  
  
3.  En el panel izquierda, en el árbol **Administrador de clústeres de conmutación por error** , haga clic en el nombre del clúster.  
  
4.  En el panel de resumen, confirme que el valor actual de **Configuración de cuórum** es:  **Advertencia: el clúster se ejecuta en estado ForceQuorum**.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Para forzar que un clúster se inicie sin un quórum  
  
1.  Inicie Windows PowerShell con derechos elevados mediante **Ejecutar como administrador**.  
  
2.  Importe el módulo `FailoverClusters` para habilitar los commandlets de clúster.  
  
3.  Use `Stop-ClusterNode` para asegurarse de que el servicio de clúster está detenido.  
  
4.  Use `Start-ClusterNode` con `-FixQuorum` para forzar que se inicie el servicio de clúster.  
  
5.  Use `Get-ClusterNode` con `-Property NodeWieght = 1` para establecer el valor que garantiza que el nodo es un miembro con derecho a voto del quórum.  
  
6.  Enviar las propiedades de nodo de clúster en un formato legible.  
  
### <a name="example-powershell"></a>Ejemplo (PowerShell)  
 En el siguiente ejemplo se fuerza al servicio de clúster del nodo AlwaysOnSrv02 a iniciarse sin un cuórum, se establece `NodeWeight = 1`y, luego, se enumera el estado de nodo de clúster a partir del nodo que se acaba de forzar.  
  
```powershell  
Import-Module FailoverClusters  
  
$node = "Always OnSrv02"  
Stop-ClusterNode -Name $node  
Start-ClusterNode -Name $node -FixQuorum  
  
(Get-ClusterNode $node).NodeWeight = 1  
  
$nodes = Get-ClusterNode -Cluster $node  
$nodes | Format-Table -property NodeName, State, NodeWeight  
  
```  
  
##  <a name="using-netexe"></a><a name="CommandPromptProcedure"></a> Usar Net.exe  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>Para forzar que un clúster se inicie sin un quórum  
  
1.  Use Escritorio remoto para conectarse al nodo de clúster deseado para forzarlo en línea.  
  
2.  Inicie un símbolo del sistema con privilegios elevados mediante **Ejecutar como administrador**.  
  
3.  Use **net.exe** para asegurarse de que el servicio de clúster local está detenido.  
  
4.  Use **net.exe** con `/forcequorum` para forzar que se inicie el servicio de clúster local.  
  
### <a name="example-netexe"></a>Ejemplo (Net.exe)  
 En el siguiente ejemplo se fuerza que un servicio de clúster de nodo se inicie sin un quórum, establece `NodeWeight = 1`y, a continuación, enumera el estado de nodo de clúster a partir del nodo que se acaba de forzar.  
  
```ms-dos  
net.exe stop clussvc  
net.exe start clussvc /forcequorum  
```  
  
##  <a name="follow-up-after-forcing-cluster-to-start-without-a-quorum"></a><a name="FollowUp"></a> Seguimiento: Después de forzar que el clúster se inicie sin un cuórum  
  
-   Debe evaluar y configurar de nuevo los valores de NodeWeight para construir correctamente un nuevo quórum antes de volver a poner otros nodos en línea. De lo contrario, el clúster puede volver a estar sin conexión.  
  
     Para obtener más información, vea [Configuración de los votos y modos de cuórum WSFC &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
-   Los procedimientos de este tema constituyen solo en paso para volver a poner en línea el clúster de WSFC si se produce un error de quórum no planificado.  Puede que también desee llevar a cabo pasos adicionales para impedir que otros nodos de clúster de WSFC interfieran en la nueva configuración de quórum.  
  
-   Otras características de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] como [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], creación de reflejo de la base de datos y trasvase de registros también pueden necesitar acciones posteriores para recuperar los datos y volver a establecer la alta disponibilidad por completo.  
  
     **Para obtener más información:**  
  
     [Realizar una conmutación por error manual forzada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
     [Forzar el servicio en una sesión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/force-service-in-a-database-mirroring-session-transact-sql.md)  
  
     [Conmutar por error a una base de datos secundaria de trasvase de registros &#40;SQL Server&#41;](../../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   [View Events and Logs for a Failover Cluster](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Cmdlet de clúster de conmutación por error Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
## <a name="see-also"></a>Consulte también  
 [Recuperación ante desastres del clúster WSFC mediante cuórum forzado &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)   
 [Configurar los valores de NodeWeight de quórum de clúster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)   
 [Cmdlets de clúster de conmutación por error en Windows PowerShell enumerados por tarea](https://technet.microsoft.com/library/ee619761\(WS.10\).aspx)  
  
