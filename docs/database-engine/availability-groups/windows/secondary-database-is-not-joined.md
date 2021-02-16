---
title: Base de datos secundaria sin combinar | Microsoft Docs
description: El estado de combinación de la base de datos de disponibilidad comprueba el estado de combinación de la base de datos secundaria como parte de la administración basada en directivas para los grupos de disponibilidad Always On.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.drp2joined.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 10817e5e-75fa-42dd-baa2-359bea3ad051
author: cawrites
ms.author: chadam
ms.openlocfilehash: 39c9f6852fd1a1cc15a014b67ee693a3cb315787
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344498"
---
# <a name="secondary-database-is-not-joined"></a>Base de datos secundaria sin combinar
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Introducción  
  
|||  
|-|-|  
|**Nombre de directiva**|Estado de combinación de la base de datos de disponibilidad|  
|**Problema**|Base de datos secundaria sin combinar.|  
|**Categoría**|**Warning (ADVERTENCIA)**|  
|**Faceta**|Base de datos de disponibilidad|  
  
## <a name="description"></a>Descripción  
 Esta directiva comprueba el estado de la unión de la base de datos secundaria (también denominada “réplica de base de datos secundaria”). La directiva está en mal estado cuando la réplica de conjunto de datos no está combinada. De lo contrario, la directiva está en un estado correcto.  

## <a name="possible-causes"></a>Causas posibles  
 Esta base de datos secundaria no está combinada con el grupo de disponibilidad. La configuración de esta base de datos secundaria está incompleta.  
  
## <a name="possible-solution"></a>Solución posible  
 Use Transact-SQL, PowerShell o SQL Server Management Studio para combinar la réplica secundaria con el grupo de disponibilidad. Para obtener más información sobre cómo combinar las réplicas secundarias con los grupos de disponibilidad, vea [Combinar una réplica secundaria con un grupo de disponibilidad (SQL Server)](https://msdn.microsoft.com/library/ff878473\(SQL.110\).aspx).  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
