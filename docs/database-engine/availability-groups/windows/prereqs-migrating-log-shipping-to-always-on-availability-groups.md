---
title: Requisitos previos para convertir el trasvase de registros en grupos de disponibilidad
description: Descripción de los requisitos previos necesarios para convertir el trasvase de registros en un grupo de disponibilidad Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: 2738ce65-205e-4682-92d8-dc7e37c58b2b
author: cawrites
ms.author: chadam
ms.openlocfilehash: b6b06ecbd40dd54c527c7d32a8943a57aecbeafa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348409"
---
# <a name="prerequisites-to-convert-log-shipping-to-always-on-availability-groups"></a>Requisitos previos para convertir el trasvase de registros en grupos de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  En este tema se describen los requisitos previos para convertir una base de datos principal de trasvase de registros junto con una o varias de sus bases de datos secundarias en una base de datos principal AlwaysOn y una o varias bases de datos secundarias.  
  
> [!NOTE]  
>  Puede configurar cualquier base de datos principal o secundaria (posiblemente legible) de un grupo de disponibilidad como una base de datos principal de trasvase de registros.  
  
  
##  <a name="availability-group-prerequisites"></a><a name="AGPrereqsRealAddress"></a> Requisitos previos de los grupos de disponibilidad  
 Para permitir que los trabajos de copia de seguridad se ejecuten en la réplica principal del grupo de disponibilidad, use las siguientes opciones de copia de seguridad de los grupos de disponibilidad AlwaysOn:  
  
|Propiedad|Configuración|  
|--------------|-------------|  
|Preferencia de la copia de seguridad automatizada del grupo de disponibilidad|Solo en la réplica principal|  
|Prioridad de copia de seguridad de la réplica principal.|>0|  
  
 **Para obtener más información:**  
  
 [Ver las propiedades del grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
 [Configurar la copia de seguridad en réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
##  <a name="log-shipping-prerequisites"></a><a name="LogShipPrereqs"></a> Requisitos previos del trasvase de registros  
  
-   La base de datos principal del trasvase de registros debe residir en la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospeda la réplica principal actual o inicial del grupo de disponibilidad.  
  
-   Para que una base de datos secundaria de trasvase de registros dada se convierta en una base de datos secundaria AlwaysOn, debe:  
  
    -   Usar el mismo nombre que la base de datos principal.  
  
    -   Residir en una instancia del servidor que hospeda una réplica secundaria para el grupo de disponibilidad.  
  
 Una vez que el trabajo de copia de seguridad se ejecute en la base de datos principal, deshabilite el trabajo de copia de seguridad y, una vez que el trabajo de restauración se haya ejecutado en una base de datos secundaria, deshabilite el trabajo de restauración.  
  
 Después de crear todas las bases de datos secundarias para el grupo de disponibilidad, si desea realizar copias de seguridad en las réplicas secundarias, debe configurar de nuevo la preferencia de copia de seguridad automatizada del grupo de disponibilidad.  
  
 **Para obtener más información:**  
  
 [Conversión de una configuración de trasvase de registros en un grupo de disponibilidad](/archive/blogs/sqlalwayson/converting-a-logshipping-configuration-to-availability-group) (blog de SQL Server)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Trasvase de registros**  
  
-   [Actualización del trasvase de registros a SQL Server 2016 &#40;Transact-SQL&#41;](../../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Quitar el trasvase de registros &#40;SQL Server&#41;](../../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
 **Grupos de disponibilidad AlwaysOn (SQL Server)**  
  
-   [Usar el Asistente para grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Usar el cuadro de diálogo Nuevo grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Crear un grupo de disponibilidad &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Crear un grupo de disponibilidad &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
-   [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Configurar la copia de seguridad en réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   **Blogs:**  
  
     [Convertir una configuración de trasvase de registros en un grupo de disponibilidad](/archive/blogs/sqlalwayson/converting-a-logshipping-configuration-to-availability-group)  
  
     [Agregue una base de datos principal de trasvase de registros y una base de datos secundaria a un grupo de disponibilidad](/archive/blogs/sqlalwayson/add-a-log-shipping-primary-database-and-secondary-databases-to-an-existing-availability-group)  
  
     [Blogs del equipo de AlwaysOn de SQL Server: blog oficial del equipo de AlwaysOn de SQL Server](/archive/blogs/sqlalwayson/)  
  
     [Blogs de los ingenieros de SQL Server de CSS](/archive/blogs/psssql/)  
  
-   **Notas del producto:**  
  
     [Guía de migración: migrar a grupos de disponibilidad AlwaysOn desde implementaciones anteriores que combinan creación de reflejo de la base de datos y trasvase de registros](/previous-versions/sql/sql-server-2012/jj635217(v=msdn.10))  
  
     [Notas del producto de Microsoft para SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Notas del producto del equipo de asesoramiento al cliente de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Herramientas para supervisar grupos de disponibilidad Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
