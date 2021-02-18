---
title: 'Grupos de disponibilidad: una solución de alta disponibilidad y recuperación ante desastres'
description: Los grupos de disponibilidad Always On son una solución de alta disponibilidad y recuperación ante desastres de SQL Server que proporcionan una alternativa de calidad empresarial a la creación de reflejo de la base de datos, con más funciones. Obtenga información sobre los conceptos básicos y las funciones de esta característica.
ms.custom: seodec18
ms.date: 04/23/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], about
- secondary replicas, see Availability Groups [SQL Server]
- availability [SQL Server]
- AlwaysOn [SQL Server], see Availability Groups [SQL Server]
- Availability Groups [SQL Server]
ms.assetid: aa427606-8422-4656-b205-c9e665ddc8c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: c38eb3d70f724dc90e14aa77641dddd781233a62
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343871"
---
# <a name="always-on-availability-groups-a-high-availability-and-disaster-recovery-solution"></a>Grupos de disponibilidad Always On: una solución de alta disponibilidad y recuperación ante desastres
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  La característica [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] es una solución de alta disponibilidad y de recuperación ante desastres que proporciona una alternativa empresarial a la creación de reflejo de la base de datos. Incorporada en [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] maximiza la disponibilidad de un conjunto de bases de datos de usuario para una empresa. Un *grupo de disponibilidad* admite un entorno de conmutación por error para un conjunto discreto de bases de datos de usuario, conocido como *bases de datos de disponibilidad*, que realizan la conmutación por error conjuntamente. Un grupo de disponibilidad admite un conjunto de bases de datos principales de lectura y escritura y de uno a ocho conjuntos de bases de datos secundarias correspondientes. Opcionalmente, las bases de datos secundarias pueden estar disponibles para el acceso de solo lectura o para algunas operaciones de copia de seguridad.  
  
 Un grupo de disponibilidad realiza la conmutación por error en el nivel de réplica de disponibilidad. Las conmutaciones por error no se deben a problemas de bases de datos como que una base de datos pase a ser sospechosa debido a la pérdida de un archivo de datos, la eliminación de una base de datos o los daños de un registro de transacciones.  
 
 >[!NOTE]
 >Grupos de disponibilidad Always On es el nombre completo y formal para esta característica de disponibilidad. La abreviatura es AG, no AOAG ni AAG. 
  
##  <a name="benefits"></a><a name="Benefits"></a> Ventajas  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] proporciona un amplio conjunto de opciones que mejoran la disponibilidad de las bases de datos y que permiten el uso de recursos mejorado. Los componentes clave son los siguientes:  
  
-   Admite hasta nueve réplicas de disponibilidad. Una *réplica de disponibilidad* es una instancia de un grupo de disponibilidad hospedado en una instancia específica de SQL Server y que mantiene una copia local de cada base de datos de disponibilidad perteneciente al grupo de disponibilidad. Cada grupo de disponibilidad admite una réplica principal y hasta ocho réplicas secundarias. Para obtener más información, vea [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
    > [!IMPORTANT]  
    >  Cada réplica de disponibilidad debe residir en otro nodo de un único clúster de clústeres de conmutación por error de Windows Server (WSFC). Para obtener más información sobre los requisitos previos, las restricciones y las recomendaciones para los grupos de disponibilidad, vea [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
-   Admite modos de disponibilidad alternativos como los siguientes:  
  
    -   *Modo de confirmación asincrónica*. Este modo de disponibilidad es una solución de recuperación de desastres que funciona bien cuando las réplicas de disponibilidad se distribuyen sobre distancias considerables.  
  
    -   *Modo de confirmación sincrónica*. Este modo de disponibilidad resalta alta disponibilidad y protección de datos del rendimiento, a costa de aumentar la latencia de las transacciones. Un grupo de disponibilidad determinado puede admitir hasta cinco réplicas de disponibilidad de confirmación sincrónica, incluida la réplica principal actual.  
  
     Para obtener más información, vea [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). 

     [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] aumenta el número máximo de réplicas sincrónicas a 5, de las 3 que eran en [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)]. Puede configurar este grupo de cinco réplicas para habilitar la conmutación automática por error dentro del grupo. Hay una réplica principal, además de cuatro réplicas secundarias sincrónicas.
  
-   Admite varias formas de conmutación por error de un grupo de disponibilidad: conmutación automática por error, conmutación por error manual planeada (suele denominarse simplemente "conmutación por error manual") y conmutación por error manual forzada (suele denominarse simplemente "conmutación por error forzada"). Para obtener más información, vea [Conmutación por error y modos de conmutación por error &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
-   Permite configurar una réplica de disponibilidad determinada que admite una o dos de las funciones secundarias activas siguientes:  
  
    -   Tener acceso de conexión de solo lectura, que permite conexiones de solo lectura a la réplica para obtener acceso y leer sus bases de datos cuando se ejecuta como una réplica secundaria. Para obtener más información, vea [Secundarias activas: réplicas secundarias legibles &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
    -   Realizar operaciones de copia de seguridad en sus bases de datos cuando se ejecuta como una réplica secundaria. Para obtener más información, vea [Secundarias activas: copia de seguridad en las réplicas secundarias &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
     Con capacidades secundarias activas se mejora la eficiencia de los procesos de TI y se reducen los costos mediante la mejor utilización de los recursos del hardware secundario. Además, las aplicaciones de lectura de descarga y los trabajos de copia de seguridad de las réplicas secundarias ayudan a mejorar el rendimiento de la réplica primaria.  
  
-   Admite un agente de escucha del grupo de disponibilidad para cada grupo de disponibilidad. Un *agente de escucha de grupo de disponibilidad* es un nombre de servidor al que los clientes pueden conectarse para tener acceso a una base de datos en una réplica principal o secundaria de un grupo de disponibilidad AlwaysOn. Los agentes de escucha del grupo de disponibilidad dirigen las conexiones entrantes a la réplica principal o una réplica secundaria de solo lectura. El agente de escucha proporciona conmutación por error rápida de aplicaciones después de que se produzca la conmutación por error del grupo de disponibilidad. Para obtener más información, vea [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
-   Admite una directiva flexible de conmutación por error para un mayor control sobre una conmutación por error del grupo de disponibilidad. Para obtener más información, vea [Conmutación por error y modos de conmutación por error &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
-   Admite la reparación automática de páginas para ofrecer protección frente al daño en las páginas. Para obtener más información, vea [Reparación de página automática &#40;grupos de disponibilidad/creación de reflejo de la base de datos&#41;](../../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md).  
  
-   Admite el cifrado y compresión, que proporcionan un transporte seguro y de alto rendimiento.  
  
-   Proporciona un conjunto integrado de herramientas para simplificar la implementación y administración de los grupos de disponibilidad, como:  
  
    -   [!INCLUDE[tsql](../../../includes/tsql-md.md)] para crear y administrar grupos de disponibilidad. Para obtener más información, vea [Información general sobre instrucciones Transact-SQL para grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md).  
  
    -   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] son las siguientes:  
  
        -   [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] crea y configura un grupo de disponibilidad. En algunos entornos, este asistente puede también preparar automáticamente las bases de datos secundarias e iniciar la sincronización de datos de cada una de ellas. Para obtener más información, vea [Usar el cuadro de diálogo Nuevo grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md).  
  
        -   [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] agrega una o más bases de datos principales a un grupo de disponibilidad. En algunos entornos, este asistente puede también preparar automáticamente las bases de datos secundarias e iniciar la sincronización de datos de cada una de ellas. Para obtener más información, vea [Usar el Asistente para agregar una base de datos al grupo de disponibilidad (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md).  
  
        -   [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] agrega una o varias réplicas secundarias a un grupo de disponibilidad. En algunos entornos, este asistente puede también preparar automáticamente las bases de datos secundarias e iniciar la sincronización de datos de cada una de ellas. Para obtener más información, vea [Usar el Asistente para agregar una réplica al grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md).  
  
        -   [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)] inicia una conmutación manual por error en un grupo de disponibilidad. Dependiendo de la configuración y el estado de la réplica secundaria que se especifique como destino de la conmutación por error, el asistente puede realizar una conmutación por error manual planeada o forzada. Para obtener más información, vea [Usar el Asistente para grupo de disponibilidad de conmutación por error &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md).  
  
    -   El [!INCLUDE[ssAoDash](../../../includes/ssaodash-md.md)] supervisa los grupos de disponibilidad AlwaysOn, las réplicas de disponibilidad y las bases de datos de disponibilidad y evalúa los resultados de las directivas de AlwaysOn. Para obtener más información, vea [Usar el Panel AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md).  
  
    -   El panel Detalles del Explorador de objetos muestra información básica acerca de los grupos de disponibilidad existentes. Para obtener más información, vea [Usar los detalles del Explorador de objetos para supervisar los grupos de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Cmdlets de PowerShell. Para obtener más información, vea [Información general de los cmdlets de PowerShell para grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md).  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Términos y definiciones  
 **grupo de disponibilidad**  
 Contenedor para un conjunto de bases de datos, las *bases de datos de disponibilidad*, que conmutan por error juntas.  
  
 **base de datos de disponibilidad**  
 Base de datos que pertenece a un grupo de disponibilidad. Para cada base de datos de disponibilidad, el grupo de disponibilidad mantiene una sola copia de lectura y escritura (la *base de datos principal*) y de una a ocho copias de solo lectura (*bases de datos secundarias*).  
  
 **base de datos principal**  
 La copia de lectura y escritura de una base de datos de disponibilidad.  
  
 **base de datos secundaria**  
 Una copia de solo lectura de una base de datos de disponibilidad.  
  
 **réplica de disponibilidad**  
 Creación de instancia de un grupo de disponibilidad hospedado por una instancia específica de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y mantiene una copia local de cada base de datos de disponibilidad perteneciente al grupo de disponibilidad. Existen dos tipos de réplicas de disponibilidad: una sola *réplica principal* y de una a ocho *réplicas secundarias*.  
  
 **réplica principal**  
 La réplica de disponibilidad que hace que las bases de datos principales estén disponibles para las conexiones de lectura y escritura de clientes y, además, envía las entradas del registro de transacciones para cada base de datos principal a cada réplica secundaria.  
  
 **réplica secundaria**  
 Réplica de disponibilidad que mantiene una copia secundaria de cada base de datos de disponibilidad y actúa como posible destino de conmutación por error para el grupo de disponibilidad. Opcionalmente, una réplica secundaria puede admitir acceso de solo lectura a bases de datos secundarias y la creación de copias de seguridad de bases de datos secundarias.  
  
 **agente de escucha de grupo de disponibilidad**  
 Nombre del servidor al que los clientes pueden conectarse para tener acceso a una base de datos en una réplica principal o secundaria de un grupo de disponibilidad AlwaysOn. Los agentes de escucha del grupo de disponibilidad dirigen las conexiones entrantes a la réplica principal o una réplica secundaria de solo lectura.  
  
> [!NOTE]  
>  Para obtener más información, vea [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
##  <a name="interoperability-and-coexistence-with-other-database-engine-features"></a><a name="Interoperability"></a> Interoperabilidad y coexistencia con otras características del motor de base de datos  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] se puede utilizar con las siguientes características o componentes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   [Acerca de la captura de datos modificados &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
-   [Acerca del seguimiento de cambios &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-tracking-sql-server.md)  
  
-   [Bases de datos independientes](../../../relational-databases/databases/contained-databases.md)  
  
-   [Clave de cifrado de la base de datos](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
-   [Instantáneas de base de datos](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)  
  
-   [FILESTREAM](../../../relational-databases/blob/filestream-sql-server.md)  
  
-   [FileTable](../../../relational-databases/blob/filetables-sql-server.md)  
  
-   [Trasvase de registros](../../../database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
-   [Almacén remoto de blobs (RBS)](../../../relational-databases/blob/remote-blob-store-rbs-sql-server.md)  
  
-   [Replicación](../../../relational-databases/replication/sql-server-replication.md)  
  
-   [Service Broker](../../../database-engine/configure-windows/sql-server-service-broker.md)  
  
-   [Agente SQL Server](../../../ssms/agent/sql-server-agent.md)  
  
-   [Reporting Services](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)  
  
> [!WARNING]  
>  Para obtener más información sobre las restricciones y limitaciones para usar otras características con los [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vea [Grupos de disponibilidad AlwaysOn: interoperabilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Introducción a los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   **Blogs:**  
  
     [Blogs del equipo de AlwaysOn de SQL Server: blog oficial del equipo de AlwaysOn de SQL Server](/archive/blogs/sqlalwayson/)  
  
     [Blogs de los ingenieros de SQL Server de CSS](/archive/blogs/psssql/)  
  
-   **Vídeos:**  
  
     [Microsoft SQL Server Code-Named "Denali", Serie AlwaysOn, parte 1: Introducción a la solución de alta disponibilidad de próxima generación](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Microsoft SQL Server Code-Named "Denali", Serie AlwaysOn, parte 2: Crear una solución esencial de alta disponibilidad mediante AlwaysOn](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **Notas del producto:**  
  
     [Guía de soluciones AlwaysOn de Microsoft SQL Server para lograr alta disponibilidad y recuperación ante desastres](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Notas del producto de Microsoft para SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Notas del producto del equipo de asesoramiento al cliente de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Configuración de una instancia del servidor para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)   
 [Creación y configuración de grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Administración de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [Supervisión de los grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Información general sobre instrucciones Transact-SQL para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [Información general de los cmdlets de PowerShell para grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
