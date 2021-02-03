---
title: Estadísticas de consulta activa | Microsoft Docs
description: Obtenga información sobre cómo ver el plan de ejecución en directo de una consulta activa en SQL Server Management Studio. Use las estadísticas de ejecución para depurar los problemas de rendimiento de las consultas.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query statistics [SQL Server] live query stats
- live query statistics
- debugging [SQL Server], live query stats
- statistics [SQL Server], live query statistics
- query profiling
- lightweight query profiling
- lightweight profiling
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 588b51b156681750b071915decb4960f9238c407
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236464"
---
# <a name="live-query-statistics"></a>Estadísticas de consulta activa
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ofrece la posibilidad de ver el plan de ejecución de una consulta activa. Este plan de consulta activa ofrece información en tiempo real sobre el proceso de ejecución de consulta a medida que los controles fluyen de un [operador de plan de consulta](../../relational-databases/showplan-logical-and-physical-operators-reference.md) a otro. El plan de consulta activa muestra el progreso general de las consultas, así como estadísticas de tiempo de ejecución de nivel de operador como el número de filas, las filas generadas, el tiempo transcurrido, el progreso del operador, etc. Estos datos están disponibles en tiempo real sin necesidad de esperar a que la consulta se complete, de modo que estas estadísticas de ejecución son extremadamente útiles para depurar problemas de rendimiento de consultas. Esta característica está disponible a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], pero puede funcionar con [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  

> [!NOTE]
> De forma interna, las estadísticas de consultas dinámicas aprovechan la DMV [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md).
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
> [!WARNING]  
> Esta característica sirve principalmente para solucionar problemas. Al usarla, el rendimiento general de las consultas podría bajar de forma moderada, especialmente en [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Para obtener más información, vea [Infraestructura de generación de perfiles de consultas](../../relational-databases/performance/query-profiling-infrastructure.md).  
> Esta característica se puede usar con el [depurador de Transact-SQL](../../ssms/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md).  
  
## <a name="to-view-live-query-statistics-for-one-query"></a>Para ver estadísticas de consultas dinámicas para una consulta 
  
1.  Para ver el plan de ejecución de consultas dinámicas, haga clic en el icono **Incluir estadísticas de consultas dinámicas** del menú de herramientas.  
  
     ![Botón Estadísticas de consulta activa en la barra de herramientas](../../relational-databases/performance/media/livequerystatstoolbar.png "Botón Estadísticas de consulta activa en la barra de herramientas")  
  
     También puede tener acceso al plan de ejecución de consulta activa si hace clic con el botón derecho en una consulta seleccionada en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y, después, hace clic en **Incluir estadísticas de consultas dinámicas**.  
  
     ![Botón Estadísticas de consulta activa en el menú emergente](../../relational-databases/performance/media/livequerystatsmenu.png "Botón Estadísticas de consulta activa en el menú emergente")  
  
2.  Ejecute la consulta. El plan de consulta activa muestra el progreso general de la consulta y las estadísticas de ejecución en tiempo de ejecución (por ejemplo, el tiempo transcurrido, el progreso, etc.) de los operadores del plan de consulta. Las estadísticas de ejecución y la información de progreso de consulta se actualizan periódicamente mientras la ejecución de la consulta está en curso. Use esta información para entender el proceso de ejecución de consulta general, así como para depurar consultas de larga ejecución, consultas que se ejecutan indefinidamente, consultas que provocan un desbordamiento de tempdb y problemas de tiempo de espera.  
  
     ![Botón Estadísticas de consulta activa en el plan de presentación](../../relational-databases/performance/media/livequerystatsplan.png "Botón Estadísticas de consulta activa en el plan de presentación")  
  
## <a name="to-view-live-query-statistics-for-any-query"></a>Para ver estadísticas de consultas dinámicas para cualquier consulta 

También se puede acceder al plan de ejecución de consultas dinámicas desde el **[Monitor de actividad](../../relational-databases/performance-monitor/activity-monitor.md)** ; para ello, haga clic con el botón derecho en cualquier consulta de la tabla **Procesos** o **Consultas costosas activas**.  
  
 ![Botón Estadísticas de consulta activa en el Monitor de actividad](../../relational-databases/performance/media/livequerystatsactmon.png "Botón Estadísticas de consulta activa en el Monitor de actividad")  
  
## <a name="remarks"></a>Observaciones  
 La infraestructura de perfil de estadísticas debe estar habilitada para que las estadísticas de consulta activa puedan capturar información sobre el progreso de las consultas. Según la versión, la sobrecarga puede ser significativa. Para obtener más información sobre esta sobrecarga, vea [Infraestructura de generación de perfiles de consultas](../../relational-databases/performance/query-profiling-infrastructure.md).
  
## <a name="permissions"></a>Permisos  
Requiere el permiso `SHOWPLAN` de nivel de base de datos para rellenar la página de resultados **Estadísticas de consultas dinámicas** y los permisos necesarios habituales para ejecutar la consulta.
En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], requiere el permiso `VIEW SERVER STATE` de nivel de servidor para ver las estadísticas en tiempo real.  
En los niveles Premium de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] se necesita el permiso `VIEW DATABASE STATE` en la base de datos para ver las estadísticas en tiempo real. En los niveles Estándar y Básico de [!INCLUDE[ssSDS](../../includes/sssds-md.md)], se necesita el **administrador del servidor** o una cuenta de **administrador de Azure Active Directory** para ver las estadísticas en tiempo real.
  
## <a name="see-also"></a>Consulte también  
 [Planes de ejecución](../../relational-databases/performance/execution-plans.md)    
 [Query Processing Architecture Guide](../../relational-databases/query-processing-architecture-guide.md)   (Guía de arquitectura de procesamiento de consultas)  
 [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Herramientas de supervisión y optimización del rendimiento](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Abrir el Monitor de actividad &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Monitor de actividad](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
 [Marcas de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
 [Referencia de operadores lógicos y físicos del plan de presentación](../../relational-databases/showplan-logical-and-physical-operators-reference.md)     
 [Query Profiling Infrastructure](../../relational-databases/performance/query-profiling-infrastructure.md) (Infraestructura de generación de perfiles de consultas)