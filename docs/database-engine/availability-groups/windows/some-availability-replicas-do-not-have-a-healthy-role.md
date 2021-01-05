---
title: Algunas réplicas de disponibilidad no tienen un rol en buen estado | Microsoft Docs
description: El estado de rol de las réplicas de disponibilidad comprueba si hay alguna réplica de disponibilidad que no esté en un rol correcto.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp6allroleshealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 7ec5b337-7201-4a66-a541-7560f8b18784
author: cawrites
ms.author: chadam
ms.openlocfilehash: db542dca2f4c200764e5065bff155c908fafcd25
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641427"
---
# <a name="some-availability-replicas-do-not-have-a-healthy-role"></a>Algunas réplicas de disponibilidad no tienen un rol en buen estado
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introducción  
  
|||  
|-|-|  
|**Nombre de directiva**|Estado del rol de réplicas de disponibilidad|  
|**Problema**|Algunas réplicas de disponibilidad no tienen un rol en buen estado.|  
|**Categoría**|**Warning (ADVERTENCIA)**|  
|**Faceta**|grupo de disponibilidad|  
  
## <a name="description"></a>Descripción  
 Esta directiva acumula el estado de conexión de todas las réplicas de disponibilidad y comprueba si hay alguna réplica de disponibilidad que no está en un rol correcto. La directiva está en mal estado cuando una réplica de disponibilidad no es principal ni secundaria. De lo contrario, la directiva está en un estado correcto.  
  
> [!NOTE]  
>  En esta versión de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], la información sobre las posibles causas y soluciones se encuentra en el artículo [Algunas réplicas de disponibilidad no tienen un rol correcto](https://go.microsoft.com/fwlink/p/?LinkId=220854) en la wiki de TechNet.  
  
## <a name="possible-causes"></a>Causas posibles  
 En este grupo de disponibilidad, al menos una réplica de disponibilidad no tiene actualmente el rol principal o secundario.  
  
## <a name="possible-solution"></a>Solución posible  
 Use el estado de la directiva de réplica de disponibilidad para buscar la réplica de disponibilidad cuyo rol no es principal ni secundario, y después resuelva el problema en la réplica de disponibilidad.  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
