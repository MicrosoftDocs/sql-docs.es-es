---
title: Panel del grupo de disponibilidad en SSMS
description: Obtenga más información sobre la página "Opciones" del panel de grupos de disponibilidad Always On de SQL Server en SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Alwayson.Dashboard
ms.assetid: 4369b588-e982-4b57-80a1-beb2e879ce0b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6a7cfa4984ede1f6f6963006b5a5d5675a78dac8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349250"
---
# <a name="options-sql-server-always-on-dashboard-page"></a>Opciones (SQL Server AlwaysOn, página del panel)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Use la página **Panel AlwaysOn de SQL Server** del cuadro de diálogo **Opciones de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]** para configurar el panel de AlwaysOn.  
  
 **Para acceder a esta página:**  
  
 En el menú **Herramientas** , haga clic en **Opciones**, expanda la carpeta **SQL Server Always On** y luego haga clic en **Panel**.  
  
## <a name="on-this-page"></a>En esta página  
 **Activar actualización automática.**  
 Haga clic para habilitar la actualización automática. Las opciones son:  
  
-   El campo **Intervalo de actualización (en segundos)** muestra el número de segundos que pasarán antes de cada actualización del panel. El valor predeterminado es 30. Cuando se habilita la actualización automática, se puede editar este campo para cambiar el intervalo de actualización.  
  
-   El **Número de reintentos de conexión** muestra el número de veces que el panel intentará conectarse a una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospeda una réplica de disponibilidad para un grupo de disponibilidad que el panel está supervisando. El valor predeterminado es 65535. Cuando se habilita la actualización automática, se puede editar este campo para cambiar el número de reintentos de conexión.  
  
 **Habilite la directiva de AlwaysOn definida por el usuario.**  
 Si ha definido su propia directiva de AlwaysOn, haga clic en esta opción para habilitar la directiva.  
  
## <a name="see-also"></a>Consulte también  
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
