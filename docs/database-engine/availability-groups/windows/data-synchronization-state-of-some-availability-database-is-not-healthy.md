---
title: El estado de sincronización de datos de alguna base de datos de disponibilidad no está en buen estado
description: Identifique las posibles causas de por qué el estado de sincronización de datos de alguna base de datos en un grupo de disponibilidad Always On no es correcto.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.drp3datasynchealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 89f95d15-33c6-4768-bccd-9dbf8c4f49a9
author: cawrites
ms.author: chadam
ms.openlocfilehash: b95864974db04a3ce32c2e86b48680b24955d88d
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584411"
---
# <a name="data-synchronization-state-of-some-availability-database-is-not-healthy"></a>El estado de sincronización de datos de alguna base de datos de disponibilidad no está en buen estado
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introducción  
  
|||  
|-|-|  
|**Nombre de directiva**|Estado de sincronización de datos de las réplicas de disponibilidad|  
|**Problema**|El estado de sincronización de datos de alguna base de datos de disponibilidad no es correcto.|  
|**Categoría**|**Warning (ADVERTENCIA)**|  
|**Faceta**|réplica de disponibilidad|  
  
## <a name="description"></a>Descripción  
 Esta directiva comprueba el estado de sincronización de datos de la base de datos de disponibilidad (también denominada “réplica de base de datos”). La directiva está en mal estado cuando el estado de sincronización de datos es NOT SYNCHRONIZING o cuando no es SYNCHRONIZED para la réplica de base de datos de confirmación sincrónica.  
  
> [!NOTE]  
>  Para esta versión de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], la información sobre las posibles causas y soluciones se encuentra en [El estado de sincronización de datos de la base de datos de disponibilidad no es correcto](https://go.microsoft.com/fwlink/p/?LinkId=220863) en la wiki de TechNet.  
  
## <a name="possible-causes"></a>Causas posibles  
 Al menos una base de datos de disponibilidad en la réplica tiene un estado incorrecto de sincronización de datos. Si se trata de una réplica de disponibilidad de confirmación asincrónica, todas las bases de datos de disponibilidad deben estar en el estado SYNCHRONIZING. Si se trata de una réplica de disponibilidad de confirmación sincrónica, todas las bases de datos de disponibilidad deben estar en el estado SYNCHRONIZED. Este problema puede deberse a lo siguiente:  
  
-   La réplica de disponibilidad puede estar desconectada.  
  
-   El movimiento de datos puede haberse suspendido.  
  
-   La base de datos no está accesible.  
  
-   Puede haber un problema de retraso temporal debido a la latencia de red o a la carga en la réplica principal o secundaria.  
  
## <a name="possible-solution"></a>Solución posible  
 Resuelva cualquier problema de suspensión de conexión o de movimiento de datos. Compruebe los eventos correspondientes a este problema mediante SQL Server Management Studio y busque el error de la base de datos. Realice los siguientes pasos para solucionar problemas del error concreto.  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
