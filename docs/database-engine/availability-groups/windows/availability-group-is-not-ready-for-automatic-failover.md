---
title: Grupo de disponibilidad no preparado para conmutación automática por error
description: Aprenda a identificar las posibles causas de por qué un grupo de disponibilidad Always On no está preparado para la conmutación por error.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.agp3autofailover.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 28261014-342c-442a-bd89-6d04b8d4e8b7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6edcd5419260ac6bccf03739fe19f580c59e0718
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340891"
---
# <a name="always-on-availability-group-is-not-ready-for-automatic-failover"></a>El grupo de disponibilidad Always On no está preparado para la conmutación automática por error
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introducción  
  
|||  
|-|-|  
|**Nombre de directiva**|Preparación para la conmutación automática por error del grupo de disponibilidad|  
|**Problema**|Grupo de disponibilidad no preparado para conmutación automática por error.|  
|**Categoría**|**Critical)** (Crítico)|  
|**Faceta**|grupo de disponibilidad|  
  
## <a name="description"></a>Descripción  
 Esta directiva realiza comprobaciones para asegurarse de que el grupo de disponibilidad tenga al menos una réplica secundaria que esté preparada para la conmutación por error. La directiva está en mal estado y se genera una alerta cuando el modo de conmutación por error de la réplica principal es automático, pero ninguna de las réplicas secundarias del grupo de disponibilidad están listas para la conmutación por error.  
  
 La directiva está en un estado correcto cuando al menos una réplica secundaria está lista para la conmutación automática por error.
  
## <a name="possible-causes"></a>Causas posibles  
 Grupo de disponibilidad no preparado para la conmutación automática por error. La réplica principal está configurada para la conmutación automática por error; sin embargo, la réplica secundaria no está lista para la conmutación automática por error. La réplica secundaria configurada para la conmutación automática por error podría no estar disponible o su estado de sincronización de datos no es actualmente SYNCHRONIZED.  
  
## <a name="possible-solutions"></a>Soluciones posibles  
 Las siguientes son posibles soluciones para este problema:  
  
-   Compruebe que al menos una réplica secundaria está configurada como conmutación automática por error. Si no hay una réplica secundaria configurada como conmutación automática por error, actualice la configuración de una réplica secundaria para que sea el destino de la conmutación automática por error con confirmación sincrónica.  
  
-   Use la directiva para comprobar que los datos están en un estado de sincronización y el destino de la conmutación automática por error es SYNCHRONIZED y, a continuación, resuelva el problema en la réplica de disponibilidad.  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
