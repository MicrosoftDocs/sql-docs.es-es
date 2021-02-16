---
title: 'Asistente para grupos de disponibilidad: Página Validación'
description: En este tema se describen las opciones que se encuentran en la página Validación del Asistente para grupos de disponibilidad AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.validation.f1
- sql13.swb.adddatabasewizard.validation.f1
- sql13.swb.newagwizard.validation.f1
helpviewer_keywords:
- ', listeners'
ms.assetid: c8971556-240c-491a-bc86-9cc72f71a3dd
author: cawrites
ms.author: chadam
ms.openlocfilehash: b4d05d10c54806b7ffeec607ffc1b50e27bfe5a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355088"
---
# <a name="validation-page-always-on-availability-group-wizards"></a>Página Validación (asistentes para grupos de disponibilidad AlwaysOn)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
  En este tema de Ayuda se describen las opciones de la página **Validación** . Esta tema se aplica a [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)], [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)]y [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Utilice esta página para validar que el entorno admite todas las opciones de configuración realizada en las páginas anteriores del asistente.  
  
##  <a name="validation-page-options"></a><a name="PageOptions"></a> Opciones de la página Validación  
 **Resultados de la validación de grupos de disponibilidad.**  
 Esta cuadrícula muestra los resultados de cada paso de la validación completado. Las columnas de la cuadrícula son las siguientes:  
  
 **Nombre**  
 Muestra una frase que describe un paso concreto.  
  
 **Resultado**  
 Muestra uno de los siguientes texto de hipervínculo. Para obtener más información sobre el resultado del paso de la validación determinado, haga clic en el hipervínculo.  
  
|Resultado|Descripción|  
|------------|-----------------|  
|**Error**|Indica que se produjo un error en el paso de la validación. Haga clic en el vínculo para ver el mensaje de error.|  
|**Omitido**|Indica que el paso de la validación se omite porque no lo requieren las opciones seleccionadas. Haga clic en el vínculo para ver el motivo por el que un paso se ha omitido.|  
|**Success**|Indica que el paso de la validación se completó correctamente|  
|**Warning (ADVERTENCIA)**|Indica un problema potencial con la configuración del grupo de disponibilidad.  Haga clic en el vínculo para ver el mensaje de advertencia.|  
  
 **Volver a ejecutar la validación**  
 Haga clic para repetir los pasos de la validación si realiza un cambio fuera del asistente en respuesta a un error de validación.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Usar el cuadro de diálogo Nuevo grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Usar el Asistente para agregar una réplica al grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Usar el Asistente para agregar una base de datos al grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
