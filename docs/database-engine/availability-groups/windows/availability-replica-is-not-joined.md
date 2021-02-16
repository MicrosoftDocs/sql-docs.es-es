---
title: La réplica de disponibilidad no se ha unido a un grupo de disponibilidad
description: Obtenga información sobre cómo identificar las posibles causas de por qué una réplica no se ha unido a un grupo de disponibilidad Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
f1_keywords:
- sql13.swb.agdashboard.arp4joined.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 9c0d10b1-9e12-430c-83b9-ca2bd0a3afc4
author: cawrites
ms.author: chadam
ms.openlocfilehash: d80170723101e1e78e70d06f91653e4592b3560d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343826"
---
# <a name="availability-replica-is-not-joined-to-an-always-on-availability-group"></a>La réplica de disponibilidad no se ha unido a un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introducción  
  
|||  
|-|-|  
|**Nombre de directiva**|Estado de unión de réplica de disponibilidad|  
|**Problema**|La réplica de disponibilidad no está unida.|  
|**Categoría**|**Warning (ADVERTENCIA)**|  
|**Faceta**|réplica de disponibilidad|  
  
## <a name="description"></a>Descripción  
 Esta directiva comprueba el estado de unión de la réplica de disponibilidad. La directiva está en mal estado cuando la réplica de disponibilidad se agrega al grupo de disponibilidad, pero no se une correctamente. De lo contrario, la directiva está en un estado correcto.  
  
## <a name="possible-causes"></a>Causas posibles  
 La réplica secundaria no se ha unido al grupo de disponibilidad. Para que una réplica de disponibilidad se una correctamente al grupo de disponibilidad, el estado de la unión debe ser Instancia independiente unida (1) o Clúster de conmutación por error unido (2).  
  
## <a name="possible-solution"></a>Solución posible  
 Use Transact-SQL, PowerShell o SQL Server Management Studio para combinar la réplica secundaria con el grupo de disponibilidad. Para obtener más información sobre cómo combinar las réplicas secundarias con los grupos de disponibilidad, vea [Combinar una réplica secundaria con un grupo de disponibilidad (SQL Server)](https://msdn.microsoft.com/library/ff878473\(SQL.110\).aspx).  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
