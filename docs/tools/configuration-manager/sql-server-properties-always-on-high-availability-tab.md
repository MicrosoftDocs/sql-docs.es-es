---
title: Propiedades de SQL Server (pestaña Alta disponibilidad de AlwaysOn)
description: Para usar los grupos de disponibilidad como solución de alta disponibilidad y recuperación ante desastres, active la característica Grupos de disponibilidad Always On en SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: d8630923-a600-4f1c-aca1-027453a3ec82
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 2de80bdd32d62d43a3316365029ec46a165e2df6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345109"
---
# <a name="sql-server-properties-always-on-high-availability-tab"></a>Propiedades de SQL Server (pestaña Alta disponibilidad de AlwaysOn)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Use la pestaña **Alta disponibilidad de AlwaysOn** del cuadro de diálogo **Propiedades de SQL Server** del Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para habilitar o deshabilitar la característica Grupos de disponibilidad AlwaysOn de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. La habilitación de los Grupos de disponibilidad AlwaysOn es un requisito previo para que una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] use los grupos de disponibilidad como solución de alta disponibilidad y recuperación ante desastres.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
 Para tener habilitados los Grupos de disponibilidad AlwaysOn, una instancia del servidor debe cumplir los requisitos previos siguientes:  
  
-   La instancia de servidor debe residir en un nodo de clústeres de conmutación por error de Windows Server (WSFC).  
  
-   Para pertenecer al mismo grupo de disponibilidad, las instancias deben estar en el mismo clúster de WSFC. Un grupo de disponibilidad no puede abarcar varios clústeres de WSFC.  
  
-   La instancia del servidor debe ejecutar una edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que admita [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
-   Habilite los Grupos de disponibilidad AlwaysOn para las distintas instancias del servidor de una en una. Después de habilitar los Grupos de disponibilidad AlwaysOn, espere hasta que se haya reiniciado el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antes de habilitar la instancia del servidor siguiente.  
  
> [!NOTE]  
>  Para más información sobre las características admitidas y sobre los requisitos previos, restricciones y recomendaciones adicionales para [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], vea los Libros en pantalla de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
## <a name="dialog-options"></a>Opciones del cuadro de diálogo  
 **Nombre del clúster de conmutación por error de Windows**  
 Muestra el nombre del clúster de WSFC en el que el equipo local es un nodo.  
  
 **Habilitar los grupos de disponibilidad AlwaysOn**  
 Use esta casilla para habilitar o deshabilitar los Grupos de disponibilidad AlwaysOn en esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tal como se indica abajo:  
  
-   Si esta casilla está vacía, los Grupos de disponibilidad AlwaysOn estarán deshabilitados. Para habilitar los Grupos de disponibilidad AlwaysOn, seleccione esta casilla, haga clic en **Aceptar** y reinicie manualmente el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Si esta casilla ya está seleccionada, los Grupos de disponibilidad AlwaysOn ya están habilitados. Para deshabilitar los Grupos de disponibilidad AlwaysOn, desactive la casilla y haga clic en **Aceptar**. Esto hace que se reinicie la instancia del servidor.  
  
    > [!TIP]  
    >  Después de deshabilitar los Grupos de disponibilidad AlwaysOn, debe quitar todas las réplicas de disponibilidad locales de la instancia del servidor. Si quita la última réplica de un grupo de disponibilidad determinado, también debe quitar el grupo.  
  
## <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario  
  
> [!NOTE]  
>  Para más información sobre los pasos que se deben seguir después de deshabilitar los Grupos de disponibilidad AlwaysOn y si quiere saber más sobre cómo crear y configurar grupos de disponibilidad, vea los Libros en pantalla de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
  
