---
title: Panel de rendimiento | Microsoft Docs
description: Obtenga más información sobre el panel de rendimiento de SQL Server Management Studio, que proporciona información rápida sobre SQL Server y Azure SQL Managed Instance.
ms.custom: ''
ms.date: 11/13/2020
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- performance dashboard [SQL Server]
- performance dashboard reports
- perf dashboard
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pelopes
ms.author: pelopes
manager: amitban
ms.openlocfilehash: ad05914c851b30c567f15cfa43e85299b37b81c5
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235792"
---
# <a name="performance-dashboard"></a>Panel de rendimiento
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

En la versión 17.2 de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y versiones posteriores se incluye el panel de rendimiento. Este panel se ha diseñado para proporcionar información rápida de forma visual sobre el estado de rendimiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e Instancia administrada de [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)]. 

El panel de rendimiento ayuda a identificar rápidamente si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] están experimentando un cuello de botella de rendimiento. Y si se encuentra un cuello de botella, captura fácilmente datos de diagnóstico adicionales que pueden ser necesarios para resolver el problema. Algunos problemas comunes de rendimiento que el panel de rendimiento puede ayudar a identificar son los siguientes:
-  Cuellos de botella de la CPU (y las consultas que consumen más CPU)
-  Cuellos de botella de E/S (y las consultas que realizan más operaciones de entrada y salida)
-  Recomendaciones de índice generadas por el optimizador de consultas (índices que faltan)
-  Bloqueos
-  Contención de recursos (incluida la contención de bloqueos temporales)

El panel de rendimiento también ayuda a identificar las consultas que consumen muchos recursos que posiblemente se hayan ejecutado antes, y algunas métricas están disponibles para definir el consumo elevado: CPU, Escrituras lógicas, Lecturas lógicas, Duración, Lecturas físicas y Tiempo de CLR.

El panel de rendimiento se divide en las siguientes secciones y subinformes:
-  Uso de CPU del sistema
-  Solicitudes de espera actuales
-  Actividad actual
   -  Solicitudes de usuario
   -  Sesiones de usuario
   -  Frecuencia de aciertos de caché
-  Información histórica
   -  Esperas
   -  Bloqueos temporales
   -  Estadísticas de E/S
   -  Consultas costosas
- Información variada
  -  Seguimientos activos
  -  Sesiones de xEvent activas
  -  Bases de datos
  -  Índices que faltan

> [!NOTE] 
> Internamente, el panel de rendimiento usa vistas de administración dinámica (DMV) y funciones (DMF) relacionadas con la [ejecución](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md), [índices](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md) y [E/S](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md).

## <a name="to-view-the-performance-dashboard"></a>Para ver el panel de rendimiento 
  
Para ver el panel de rendimiento, haga clic con el botón derecho en el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el Explorador de objetos y seleccione **Informes**, **Informes estándar** y **Panel de rendimiento**.  
  
![Panel de rendimiento en el menú](../../relational-databases/performance/media/perf_dashboard_ssms.png "Panel de rendimiento en el menú")  
  
El panel de rendimiento aparecerá como una nueva pestaña. A continuación se muestra un ejemplo de un cuello de botella de CPU claramente presente:  
  
![Pantalla principal del panel de rendimiento](../../relational-databases/performance/media/perf_dashboard.png "Pantalla principal del panel de rendimiento")  
  
## <a name="remarks"></a>Observaciones
En el informe **Índices que faltan** se muestran los índices que es posible que falten, identificados por el optimizador de consultas durante la compilación de la consulta. Pero estas recomendaciones no deben tomarse al pie de la letra. Microsoft recomienda que se evalúen para la creación los índices con una puntuación mayor que 100 000, ya que tienen la mejora anticipada más alta para las consultas de usuario. 

> [!TIP]
> Evalúe siempre si una nueva sugerencia de índice es comparable a un índice existente en la misma tabla, donde se pueden lograr los mismos resultados prácticos simplemente mediante el cambio de un índice existente en lugar de crear uno. Por ejemplo, dado un nuevo índice sugerido en las columnas C1, C2 y C3, evalúe primero si hay un índice existente en las columnas C1 y C2. Si lo hay, puede ser preferible simplemente agregar la columna C3 al índice existente (y conservar el orden de las columnas ya existentes) para evitar la creación de uno nuevo.
> Para más información, vea la [Guía de diseño y de arquitectura de índices](../../relational-databases/sql-server-index-design-guide.md).

En el informe **Esperas** se filtran todas las esperas inactivas y en suspensión. Para más información sobre las esperas, vea [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) y [Optimización del rendimiento de SQL Server 2005 mediante esperas y colas](https://download.microsoft.com/download/4/7/a/47a548b9-249e-484c-abd7-29f31282b04d/performance_tuning_waits_queues.doc).

Los informes **Consultas que consumen muchos recursos** se restablecen cuando se reinicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] porque se borran los datos de las DMV subyacentes. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede encontrar información detallada sobre las consultas que consumen muchos recursos en el Almacén de consultas. 


> [!NOTE]
> El panel de rendimiento se publicó inicialmente como una descarga independiente para [SQL Server 2005](https://techcommunity.microsoft.com/t5/SQL-Server-Support/SQL-Server-2005-Performance-Dashboard-Reports/ba-p/315415) y después se actualizó para [SQL Server 2012](https://www.microsoft.com/download/details.aspx?id=29063). Aunque el representador de informes SQL Server Management Studio no admite que el Portapapeles acceda al texto contenido en los informes, se puede acceder al texto mediante los informes independientes.  Descargue los informes de independiente si necesita copiar el texto de la consulta de los informes.

## <a name="permissions"></a>Permisos  
En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], requiere los permisos `VIEW SERVER STATE` y `ALTER TRACE`. En [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], requiere el permiso `VIEW DATABASE STATE` en la base de datos.

## <a name="see-also"></a>Consulte también  
 [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Herramientas de supervisión y optimización del rendimiento](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Abrir el Monitor de actividad &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Monitor de actividad](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Supervisar el rendimiento mediante el Almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
