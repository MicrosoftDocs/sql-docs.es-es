---
title: Suspensión de la base de datos de disponibilidad para un grupo de disponibilidad
description: Identifique las posibles causas de la suspensión de una base de datos en un grupo de disponibilidad Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.drp1notsuspended.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 6baee70f-848c-4e86-b20d-78875c0f82cb
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5843b82c3be84acf24e04ab9dac4b616bed857a6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641240"
---
# <a name="availability-database-is-suspended-for-an-availability-group"></a>Suspensión de la base de datos de disponibilidad para un grupo de disponibilidad
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introducción  
  
|||  
|-|-|  
|**Nombre de directiva**|Estado de suspensión de la base de datos de disponibilidad|  
|**Problema**|Base de datos de disponibilidad suspendida.|  
|**Categoría**|**Warning (ADVERTENCIA)**|  
|**Faceta**|Base de datos de disponibilidad|  
  
## <a name="description"></a>Descripción  
 Esta directiva comprueba el estado del movimiento de datos de la base de datos secundaria (también denominada “réplica de base de datos secundaria”). La directiva está en mal estado cuando se suspende el movimiento de datos. De lo contrario, la directiva está en un estado correcto.  
  
> [!NOTE]  
>  Para esta versión de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], la información sobre las posibles causas y soluciones se encuentra en el artículo [Base de datos de disponibilidad suspendida](https://go.microsoft.com/fwlink/p/?LinkId=220860) en TechNet Wiki.  
  
## <a name="possible-causes"></a>Causas posibles  
 La sincronización de datos de esta base de datos de disponibilidad podría haberse suspendido debido al siguiente:  
  
-   Debido a un error, el sistema puede haber suspendido la sincronización de datos.  
  
-   El administrador de la base de datos puede haber suspendido la sincronización de datos con fines de mantenimiento.  
  
## <a name="possible-solution"></a>Solución posible  
 Reanude la sincronización de datos. Si el problema persiste, compruebe el grupo de disponibilidad en el registro de eventos y, a continuación, diagnostique por qué el sistema suspendió el movimiento de datos.  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
