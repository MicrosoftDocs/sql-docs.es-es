---
title: Generación y análisis de CLUSTER.LOG para grupos de disponibilidad
description: Obtenga más información sobre cómo generar y analizar el registro de clúster para un grupo de disponibilidad Always On.
ms.custom: seo-lt-2019
ms.date: 06/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 01a9e3c1-2a5f-4b98-a424-0ffc15d312cf
author: rothja
ms.author: jroth
ms.openlocfilehash: c674f16110b6c1161e543d7e6871fddceb02f6bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343757"
---
# <a name="generate-and-analyze-the-clusterlog-for-an-always-on-availability-group"></a>Generación y análisis de CLUSTER.LOG para un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Como recurso de clúster de conmutación por error, existen las interacciones externas entre SQL Server, el Servicio de clúster de conmutación por error de Windows Server (WSFC) y el DLL del recurso de SQL Server (hadrres.dll), que no se pueden supervisar en SQL Server. El registro de WSFC, CLUSTER.LOG, puede diagnosticar problemas en el clúster de WSFC o en el DLL de recursos de SQL Server. 
  
## <a name="generate-cluster-log"></a>Generar el registro del clúster  
 Puede generar los registros del clúster de dos maneras:  
  
1.  Ejecute el comando `cluster /log /g` en el símbolo del sistema. Este comando genera los registros del clúster en el directorio \windows\cluster\reports en cada nodo de WSFC. La ventaja de este método es que puede especificar el nivel de detalle en los registros generados mediante la opción `/level`. La desventaja es que no se puede especificar el directorio de destino para los registros de clúster generados. Para obtener más información, consulte [How to create the cluster.log in Windows Server 2008 Failover Clustering](https://techcommunity.microsoft.com/t5/failover-clustering/how-to-create-the-cluster-log-in-windows-server-2008-failover/ba-p/371283) (Cómo crear el archivo cluster.log en clústeres de conmutación por error de Windows Server 2008).  
  
2.  Use el cmdlet [Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10)) de PowerShell. La ventaja de este método es que le permite generar el registro de clúster desde todos los nodos a un directorio de destino en el nodo en que se ejecuta el cmdlet. La desventaja es que no le permite especificar el nivel de detalle en los registros generados.  
  
 Los siguientes comandos de PowerShell generan los registros de clúster desde todos los nodos del clúster de los últimos 15 minutos y los colocan en el directorio actual. Ejecute los comandos en una ventana de PowerShell con privilegios de administración.  
  
```powershell  
Import-Module FailoverClusters   
Get-ClusterLog -TimeSpan 15 -Destination .  
```  
  
## <a name="always-on-log-verbosity"></a>Nivel de detalle del registro Always On  
 Puede aumentar el nivel de detalle de los registros en CLUSTER.LOG para un grupo de disponibilidad. Para modificar el nivel de detalle, siga estos pasos:  
  
1.  En el menú **Inicio**, abra **Administrador de clústeres de conmutación por error**.  
  
2.  Expanda el clúster y el nodo **Servicios y aplicaciones** y después haga clic en el nombre del grupo de disponibilidad.  
  
3.  En el panel de detalles, haga clic con el botón derecho en el recurso del grupo de disponibilidad y haga clic en **Propiedades**.  
  
4.  Haga clic en la pestaña **Propiedades**.  
  
5.  Modifique la propiedad **VerboseLogging**. De forma predeterminada, **VerboseLogging** se establece en `0`, lo cual notifica la información, las advertencias y los errores. **VerboseLogging** se establece en un valor entre `0` y `2`.  
  
6.  Haga clic en **OK**.  
  
7.  Vuelva a hacer clic con el botón derecho en el recurso del grupo de disponibilidad y haga clic en **Dejar este recurso sin conexión**.  
  
8.  Vuelva a hacer clic con el botón derecho en el recurso del grupo de disponibilidad y haga clic en **Ponga este recurso en línea**.  
  
## <a name="availability-group-resource-events"></a>Eventos de recursos del grupo de disponibilidad  
 En la siguiente tabla se muestran los diferentes tipos de eventos que puede ver en CLUSTER.LOG y que pertenecen al recurso del grupo de disponibilidad. Para obtener más información sobre el Subsistema de hospedaje de recursos (RHS) y el Monitor de control de recursos (RCM) en WSFC, consulte [Resource Hosting Subsystem (RHS) In Windows Server 2008 Failover Clusters](/archive/blogs/askcore/resource-hosting-subsystem-rhs-in-windows-server-2008-failover-clusters) (Subsistema de hospedaje de recursos (RHS) en clústeres de conmutación por error de Windows Server 2008).  
  
|Identificador|Source|Ejemplo de CLUSTER.LOG|  
|----------------|------------|------------------------------|  
|Mensajes precedidos por `[RES]` y `[hadrag]`|hadrres.dll (DLL de recursos Always On)|00002cc4.00001264::2011/08/05-13:47:42.543 INFO  [RES] Grupo de disponibilidad \<ag> de SQL Server: `[hadrag]` Solicitud sin conexión.<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.558 ERR   [RES] Grupo de disponibilidad \<ag> de SQL Server: `[hadrag]` El subproceso de la concesión ha terminado<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.605 INFO  [RES] Grupo de disponibilidad \<ag> de SQL Server: `[hadrag]` Instrucción SQL libre<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.902 INFO  [RES] Grupo de disponibilidad \<ag> de SQL Server: `[hadrag]` Desconecte de SQL Server|  
|Mensajes precedidos por `[RHS]`|RHS. EXE (Subsistema de hospedaje de recursos, proceso de host de hadrres.dll)|00000c40.00000a34::2011/08/10-18:42:29.498 INFO [RHS] El recurso ag está sin conexión. RHS está a punto de informar del estado del recurso a RCM.|  
|Mensajes precedidos por `[RCM]`|Supervisión del control de recursos (servicio de clúster)|000011d0.00000f80::2011/08/05-13:47:42.480 INFO  [RCM] rcm::RcmGroup::Move: Primero se devuelve el grupo "ag" al estado sin conexión...<br /><br /> 000011d0.00000f80::2011/08/05-13:47:42.496 INFO [RCM] TransitionToState(ag) en línea --> OfflineCallIssued.|  
|RcmApi/ClusAPI|Una llamada API, que significa principalmente que SQL Server está solicitando la acción|000011d0.00000f80::2011/08/05-13:47:42.465 INFO [RCM] rcm::RcmApi::MoveGroup: (ag, 2)|  
  
## <a name="debug-always-on-resource-dll-in-isolation"></a>Depurar el DLL del recurso Always On de forma aislada  
 Es un procedimiento de depuración recomendado para configurar el clúster de modo que ejecute el DLL del recurso Always On (hadrres.dll) de forma aislada de otros DLL de recursos. De forma predeterminada, el clúster de WSFC ejecuta todos los archivos DLL de recursos en una sola instancia de rhs.exe. Esto hace que todos los recursos del clúster compartan la misma instancia de rhs.exe. Si intenta depurar hadrres.dll con un depurador, al poner en pausa en el punto de interrupción, puede provocar que otros recursos que compartan la instancia rhs.exe también se pongan en pausa. Además, al ejecutar varios grupos de disponibilidad en el mismo clúster, la misma configuración puede hacer que todos los grupos de disponibilidad se pongan en pausa en el punto de interrupción para depurar un grupo de disponibilidad.  
  
 Para aislar un grupo de disponibilidad desde otros DLL de recursos de clúster, incluidos otros grupos de disponibilidad, haga lo siguiente para ejecutar hadrres.dll dentro de un proceso rhs.exe independiente:  
  
1.  Abra el **Editor del Registro** y desplácese hasta la clave siguiente: HKEY_LOCAL_MACHINE\Cluster\Resources. Esta clave contiene las claves para todos los recursos, cada uno con un GUID diferente.  
  
2.  Busque la clave de recurso que contenga un valor **Nombre** que coincida con el nombre del grupo de disponibilidad.  
  
3.  Cambie el valor **SeparateMonitor** a **1**.  
  
4.  Reinicie el servicio en clúster para el grupo de disponibilidad en el clúster de WSFC.  
  
