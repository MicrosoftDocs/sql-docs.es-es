---
title: 'Alta disponibilidad: bases de datos OLTP en memoria'
description: Las bases de datos de SQL Server que contienen tablas optimizadas para memoria, con o sin procedimientos almacenados compilados nativos, son totalmente compatibles con la característica Grupos de disponibilidad Always On.
ms.custom: seo-dt-2019
ms.date: 08/31/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2113a916-3b1e-496c-8650-7f495e492510
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 96686e379f9d10058ebcedffd44405b6c2f8a18a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237579"
---
# <a name="high-availability-support-for-in-memory-oltp-databases"></a>Compatibilidad con alta disponibilidad para bases de datos OLTP en memoria
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Las bases de datos que contienen tablas optimizadas para memoria, con o sin procedimientos almacenados compilados nativos, son totalmente compatibles con grupos de disponibilidad AlwaysOn.  No hay ninguna diferencia en la configuración y la compatibilidad de las bases de datos que contienen objetos de [!INCLUDE[hek_2](../../includes/hek-2-md.md)] en comparación con las que no tienen.  

 Los cambios en las tablas optimizadas para memoria de la réplica principal se aplican a las tablas de la réplica secundaria durante la fase de puesta al día. Esto permite una rápida conmutación por error a la réplica secundaria, puesto que los datos ya se encuentran en la memoria. Las tablas están disponibles para las consultas de lectura en las réplicas secundarias que se han configurado para el acceso de lectura.  

  
## <a name="always-on-availability-groups-and-in-memory-oltp-databases"></a>Grupos de disponibilidad AlwaysOn y bases de datos OLTP en memoria  
 La configuración de bases de datos con componentes de [!INCLUDE[hek_2](../../includes/hek-2-md.md)] proporciona los siguientes beneficios:  
  
-   **Una experiencia completamente integrada**   
    Puede configurar las bases de datos que contienen tablas optimizadas para memoria usando el mismo asistente con el mismo nivel de compatibilidad para las réplicas secundarias sincrónicas y asincrónicas. Además, el seguimiento de estado se proporciona con el panel AlwaysOn familiar en SQL Server Management Studio.  
  
-   **Tiempo de conmutación por error comparable**   
    Las réplicas secundarias mantienen el estado en memoria de las tablas duraderas optimizadas para memoria. En caso de conmutación por error automática o forzada, el tiempo de conmutación por error a la nueva principal es comparable a las tablas de bases de disco ya que no es necesaria la recuperación. En esta configuración, se admiten tablas con optimización para memoria creadas como SCHEMA_ONLY. Sin embargo, no se registran los cambios en estas tablas y, por tanto, no existirá ningún dato en estas tablas en la réplica secundaria.  
  
-   **Secundario legible**   
    Puede obtener acceso a las tablas optimizadas para memoria en la réplica secundaria y consultarlas si se ha configurado para acceso de lectura. En [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], la marca de tiempo de lectura en la réplica secundaria está en estrecha sincronía con la marca de tiempo de lectura en la réplica principal, lo que significa que los cambios en la principal se ven muy rápidamente en la secundaria. Este comportamiento de sincronización tan estrecho es diferente de OLTP en memoria de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  

### <a name="considerations"></a>Consideraciones

- SQL Server 2019 presentó una puesta al día en paralelo de las bases de datos de grupo de disponibilidad optimizadas para memoria. En SQL Server 2016 y 2017, las tablas basadas en disco no usan la puesta al día en paralelo si una base de datos de un grupo de disponibilidad también está optimizada para memoria. 
  
## <a name="failover-clustering-instance-fci-and-in-memory-oltp-databases"></a>Instancia de clústeres de conmutación por error (FCI) y bases de datos OLTP en memoria  
 Para lograr alta disponibilidad en una configuración de almacenamiento compartido, puede configurar una instancia de clúster de conmutación por error con bases de datos con tablas optimizadas para memoria. Tenga en cuenta los siguientes factores como parte de la configuración de una FCI:  
  
-   **Objetivo de tiempo de recuperación**   
    Es probable que el tiempo de conmutación por error sea mayor, ya que las tablas optimizadas para memoria deben cargarse en la memoria antes de que la base de datos esté disponible.  
  
-   **Tablas SCHEMA_ONLY**   
    Tenga en cuenta que las tablas SCHEMA_ONLY estarán vacías sin filas después de la conmutación por error. Así es como se ha diseñado y definido por la aplicación. Este es exactamente el mismo comportamiento que cuando se reinicia una base de datos [!INCLUDE[hek_2](../../includes/hek-2-md.md)] con una o más tablas SCHEMA_ONLY.  
  
## <a name="support-for-transaction-replication-in-in-memory-oltp"></a>Compatibilidad para la replicación de transacciones en OLTP en memoria  
 Las tablas que actúan como suscriptores de replicación transaccional, excluida la replicación transaccional punto a punto, pueden configurarse como tablas optimizadas para memoria. Otras configuraciones de replicación no son compatibles con las tablas optimizadas para memoria.  Para obtener más información, vea [Replicación para los suscriptores de tablas con optimización para memoria](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md).  
  
## <a name="see-also"></a>Consulte también  
 [Grupos de disponibilidad AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Secundarias activas: réplicas secundarias legibles (grupos de disponibilidad Always On)](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Replicación en suscriptores de tablas con optimización para memoria](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md)  
  
  
