---
title: Uso de bases de datos independientes con grupos de disponibilidad
description: Obtenga información sobre el uso de una base de datos independiente con Grupos de disponibilidad Always On en SQL Server 2019 (15. x).
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- contained database, AlwaysOnAvailabilityGroups
ms.assetid: cacce3ae-e940-4566-8298-6607c4268e74
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4e0b08eac29037c5dcde26a955c01764e6f6f6dd
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584489"
---
# <a name="use-contained-databases-with-always-on-availability-groups"></a>Uso de bases de datos independientes con grupos de disponibilidad Always On 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Este tema contiene información sobre cómo utilizar una base de datos independiente con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] en [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Antes de agregar una base de datos independiente a un grupo de disponibilidad, asegúrese de que la opción de servidor **contained database authentication** está establecida en **1** en cada instancia de servidor que hospeda una réplica de disponibilidad para el grupo de disponibilidad. Para obtener más información, vea [contained database authentication (opción de configuración del servidor)](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md) y [Opciones de configuración de servidor &#40;SQL Server&#41;](../../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Opciones de configuración de servidor &#40;SQL Server&#41;](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Bases de datos independientes](../../../relational-databases/databases/contained-databases.md)  
  
  
