---
title: El servicio de clúster WSFC está sin conexión | Microsoft Docs
description: Estado de clúster de WSFC comprueba el estado del clúster de conmutación por error de Windows Server. La directiva es incorrecta cuando el clúster está sin conexión o en el estado de cuórum forzado.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp1WSFCquorum.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: d502548d-ece6-4a42-9ded-2157d33e3d21
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7f72e53e13b98e79a7d4b5849966e9b3a62e518e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347526"
---
# <a name="wsfc-cluster-service-is-offline"></a>El servicio de clúster de WSFC está sin conexión

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]
    
## <a name="introduction"></a>Introducción  
  
|||  
|-|-|  
|**Nombre de directiva**|Estado de clúster de WSFC|  
|**Problema**|El servicio de clúster de WSFC está sin conexión.|  
|**Categoría**|**Critical)** (Crítico)|  
|**Faceta**|Instancia de SQL Server|  
  
## <a name="description"></a>Descripción  
 Esta directiva comprueba el estado del clúster de conmutación por error de Windows Server (WSFC). La directiva está en mal estado y se genera una alerta cuando el clúster de WSFC está sin conexión o en el estado de quorum forzado. Todos los grupos de disponibilidad hospedados en este clúster están sin conexión o se requiere una acción de recuperación ante desastres.  
  
 El estado de la directiva es correcto cuando el estado del clúster está en el quorum normal.  
  
> [!NOTE]  
>  Para esta versión de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], la información sobre las posibles causas y soluciones se encuentra en el artículo [El servicio de clúster de WSFC está desconectado](https://go.microsoft.com/fwlink/p/?LinkId=220849) en TechNet Wiki.  
  
## <a name="possible-causes"></a>Causas posibles  
 Esto puede deberse a un problema del servicio de clúster o a la pérdida del quorum en el clúster.  
  
## <a name="possible-solution"></a>Solución posible  
 Use la herramienta Administrador de clústeres para realizar el flujo de trabajo de quorum forzado o de recuperación ante desastres. Si no puede resolver el problema al realizar el quorum forzado o la recuperación ante desastres, póngase en contacto con el administrador de clústeres para que le ayude a solucionar este problema. Para obtener más información, vea [Forzar el inicio de un clúster WSFC sin un cuórum](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md) en los Libros en línea de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
