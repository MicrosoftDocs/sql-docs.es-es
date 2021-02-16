---
title: Página Seleccionar bases de datos (Asistente para nuevo grupo de disponibilidad y Asistente para agregar base de datos)
description: Describe la página "Seleccionar bases de datos" para el nuevo grupo de disponibilidad y los asistentes para agregar bases de datos que se encuentran en la GUI de SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.adddatabasewizard.selectdatabases.f1
- sql13.swb.newagwizard.selectdatabases.f1
ms.assetid: 929c5e15-d087-438d-b1f2-aa97c5f8bff8
author: cawrites
ms.author: chadam
ms.openlocfilehash: a25b2d75c6ee143f005677a250d59b4c0df32a22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352655"
---
# <a name="select-databases-page-new-availability-group-wizard-and-add-database-wizard"></a>Select Databases Page (New Availability Group Wizard and Add Database Wizard)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema de Ayuda se describen las opciones de la página **Especificar bases de datos** . Esta tema se aplica a [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] y [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
##  <a name="select-databases-options"></a><a name="PageOptions"></a> Seleccionar opciones de las bases de datos  
 La cuadrícula **Usar bases de datos en esta instancia de SQL Server** muestra cada base de datos de usuario local. Estas son sus columnas:  
  
 **Nombre**  
 Muestra el nombre de una base de datos de usuario local.  

 **Tamaño**  
 Muestra el tamaño de la base de datos, si el tamaño está disponible para el asistente.  
  
 **Estado**  
 Muestra un hipervínculo cuyo texto indica si una determinada base de datos cumple los requisitos previos para ser agregada a un grupo de disponibilidad. Si el estado es “**Cumple los requisitos previos**”, puede agregar la base de datos al grupo de disponibilidad. Si una base de datos no cumple todos los requisitos previos, el hipervínculo **Estado** proporciona una breve explicación de por qué la base de datos no es apta. Haga clic en el vínculo para obtener más información.  
  
 Puede dejar el asistente en la página **Seleccionar base de datos** mientras realiza acciones en una base de datos para que se cumpla un requisito previo. Cuando vuelva a la página **Seleccionar bases de datos** , haga clic en **Actualizar** para actualizar la cuadrícula.  
  
 **Contraseña**  
 Si la base de datos contiene una clave maestra de base de datos, escriba la contraseña para dicha clave.  
  
 **Actualizar**  
 Haga clic para actualizar la cuadrícula. Esto es útil después de realizar acciones en una base de datos para que se cumpla un requisito previo.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Usar el cuadro de diálogo Nuevo grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Usar el Asistente para agregar una base de datos al grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
  
