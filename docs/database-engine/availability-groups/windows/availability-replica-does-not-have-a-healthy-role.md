---
title: Rol incorrecto de una réplica para un grupo de disponibilidad
description: Identifique las posibles causas de por qué una réplica de disponibilidad no tiene un rol en buen estado dentro de un grupo de disponibilidad AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
f1_keywords:
- sql13.swb.agdashboard.arp1rolehealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: ebb2c9f4-2097-4688-b4fb-8f0571047317
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1f74f004d65f4fb5191ab562142204898fc3453d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343856"
---
# <a name="availability-replica-does-not-have-a-healthy-role-for-an-always-on-availability-group"></a>La réplica de disponibilidad no tiene un rol en buen estado para un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introducción  
  
|||  
|-|-|  
|**Nombre de directiva**|Estado del rol de réplica de disponibilidad|  
|**Problema**|La réplica de disponibilidad no tiene un rol en buen estado.|  
|**Categoría**|**Critical)** (Crítico)|  
|**Faceta**|réplica de disponibilidad|  
  
## <a name="description"></a>Descripción  
 Esta directiva comprueba el estado del rol de la réplica de disponibilidad. La directiva está en mal estado cuando el rol de la réplica de disponibilidad no es principal ni secundario. De lo contrario, la directiva está en un estado correcto.  
  
## <a name="possible-causes"></a>Causas posibles  
 El rol de esta réplica de disponibilidad está en mal estado. La réplica no tiene el rol principal o secundario.  
  
## <a name="possible-solution-information_still_to_come"></a>Solución posible: Information_still_to_come  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
