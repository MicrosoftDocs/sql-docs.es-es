---
title: 'Grupos de disponibilidad Always On: interoperabilidad'
description: Se describen las diferentes características que pueden y no pueden funcionar junto con un grupo de disponibilidad Always On.
ms.custom: seodec18
ms.date: 04/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], interoperability
ms.assetid: daf87f90-2623-42ca-912c-b8f07d210510
author: cawrites
ms.author: chadam
ms.openlocfilehash: fea5426d48b9808612062fccae4cc7c1228a2e1d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343947"
---
# <a name="always-on-availability-groups-interoperability-sql-server"></a>Grupos de disponibilidad Always On: interoperabilidad (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

En este tema se documenta la interoperabilidad de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con otras características de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

## <a name="features-that-interoperate-with-always-on-availability-groups"></a><a name="Interop"></a> Características que interoperan con los grupos de disponibilidad Always On

En la tabla siguiente se enumeran las características de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que interoperan con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Los vínculos de la columna **Más información** indican que existen consideraciones de interoperabilidad sobre una determinada característica.

|Característica|Más información|
|:------|:---------------|
|captura de datos modificados|[Replicación, seguimiento de cambios, captura de datos modificados y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|seguimiento de cambios|[Replicación, seguimiento de cambios, captura de datos modificados y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|Bases de datos independientes|[Bases de datos independientes con grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/contained-databases-with-always-on-availability-groups-sql-server.md)|
|Clave de cifrado de la base de datos|[Bases de datos cifradas con grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md)|
|Instantáneas de base de datos|[Instantáneas de base de datos con grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)|
|FILESTREAM y FileTable|[FILESTREAM y FileTable con grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/filestream-and-filetable-with-always-on-availability-groups-sql-server.md)|
|Búsqueda de texto completo|Nota: Los índices de texto completo se sincronizan con las bases de datos secundarias Always On.|
|Trasvase de registros|[Requisitos previos para migrar desde grupos de trasvase de registros a grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)|
|Almacén remoto de blobs (RBS)|[Almacén remoto de blobs &#40;RBS&#41; y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)|
|Replicación|[Configurar la replicación para grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)<br /><br /> [Mantener una base de datos de publicación AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)<br /><br /> [Replicación, seguimiento de cambios, captura de datos modificados y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)<br /><br /> [Suscriptores de replicación y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)|
|Analysis Services|[Analysis Services con grupos de disponibilidad AlwaysOn](../../../database-engine/availability-groups/windows/analysis-services-with-always-on-availability-groups.md)|
|Reporting Services|Use réplicas secundarias de solo lectura como un origen de datos de notificación y reduzca la carga en la réplica de lectura-escritura principal.<br /><br /> [Reporting Services con Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)|
|Service Broker|[Service Broker con grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/service-broker-with-always-on-availability-groups-sql-server.md)|
|Agente SQL Server|&nbsp;|

## <a name="features-that-interoperate-with-always-on-availability-groups-with-restrictions"></a><a name="restrictions"></a> Características que interoperan con los grupos de disponibilidad AlwaysOn con restricciones

Las siguientes características interoperan con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con restricciones específicas. Para obtener detalles, consulte los temas vinculados.

- Transacciones entre bases de datos o transacciones distribuidas ([!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] y Windows Server 2016). Para obtener más información, vea [Transacciones entre bases de datos no compatibles para la creación de reflejo de la base de datos o grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).
- El [Recopilador de datos del sistema de estadísticas de consulta](../../../relational-databases/data-collection/system-data-collection-set-reports.md#Query) no se puede ejecutar de forma confiable en un entorno con elementos secundarios no legibles. Para utilizar el recopilador de datos del sistema de estadísticas de consulta, establezca todas las réplicas del grupo de disponibilidad secundaria para permitir el [acceso de lectura](configure-read-only-access-on-an-availability-replica-sql-server.md). 

## <a name="features-that-do-not-interoperate-with-always-on-availability-groups"></a><a name="NoInterop"></a> Características que no interoperan con los grupos de disponibilidad AlwaysOn

[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] no interopera con las siguientes características:

- Creación de reflejo de base de datos. Para obtener más información, vea [Transacciones entre bases de datos y transacciones distribuidas para la creación de reflejo de la base de datos o grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).

## <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado

- **Blogs:**

  [Guía de migración: migración a clústeres de conmutación por error y grupos de disponibilidad de SQL Server 2012 desde implementaciones anteriores de agrupación en clústeres y creación de reflejo](/archive/blogs/sqlalwayson/now-available-migration-guide-migrating-to-sql-server-2012-failover-clustering-and-availability-groups-from-prior-clustering-and-mirroring-deployments)
  [Blogs del equipo de Always On de SQL Server: blog oficial del equipo de Always On de SQL Server](/archive/blogs/sqlalwayson/)
  [Blogs de los ingenieros de SQL Server de CSS](/archive/blogs/psssql/)

- **Notas del producto:**

  [Guía de migración: migración a grupos de disponibilidad Always On desde implementaciones anteriores que combinan la creación de reflejo de la base de datos y el trasvase de registros](/previous-versions/sql/sql-server-2012/jj635217(v=msdn.10))
  [Guía de soluciones de Microsoft SQL Server Always On para la alta disponibilidad y la recuperación ante desastres](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))
  [Notas del producto de Microsoft para SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)
  [Notas del producto del equipo de asesoramiento al cliente de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)

## <a name="see-also"></a>Consulte también

[Información general de los grupos de disponibilidad Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
[Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)