---
title: Memory Manager (objeto de SQL Server) | Microsoft Docs
description: Obtenga información sobre el objeto Memory Manager, que proporciona contadores para supervisar el uso general de memoria de servidor en SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Memory Manager
- Memory Manager object
ms.assetid: dbf49000-eeb0-4e9c-a361-5092363920dc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 179cd20f1006565ea4ca6924d568b6da80dbb154
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505639"
---
# <a name="sql-server-memory-manager-object"></a>Memory Manager (objeto de SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El objeto **Memory Manager** de Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona contadores para supervisar el uso de memoria en el servidor. La supervisión de la utilización total de la memoria en el servidor para medir la actividad de los usuarios y el uso de los recursos puede ayudar a identificar cuellos de botella en el rendimiento. Supervisar la memoria que utiliza una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ayuda a determinar:  
  
-   Si existen cuellos de botella debidos a la falta de memoria física disponible para almacenar en caché datos a los que se tiene acceso con frecuencia. En este caso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe recuperar los datos del disco.  
  
-   Si se puede mejorar el rendimiento de las consultas agregando memoria o aumentando la memoria disponible para la memoria caché de datos o las estructuras internas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="memory-manager-counters"></a>Contadores de Memory Manager  
 En esta tabla se describen los contadores de **Memory Manager** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Contadores de Memory Manager de SQL Server|Descripción|  
|----------------------------------------|-----------------|  
|**Memoria de conexión (KB)**|Especifica la cantidad total de memoria dinámica que utiliza el servidor para mantener las conexiones.|  
|**Memoria caché de base de datos (KB)**|Especifica la cantidad de memoria que el servidor está utilizando actualmente para la memoria caché de páginas de base de datos.|  
|**Beneficio externo de memoria**| Una estimación interna de la ventaja de rendimiento de agregar memoria a una caché específica. El motor la usa para equilibrar el uso de memoria entre la caché y es útil admitirla cuando se solucionan problemas de casos de crecimiento inesperado de la caché. El valor se muestra como un entero basado en un cálculo interno. | 
|**Memoria disponible (KB)**|Especifica la cantidad de memoria asignada no utilizada por el servidor actualmente.|  
|**Memoria de área de trabajo concedida (KB)**|Especifica la cantidad total de memoria concedida actualmente para la ejecución de procesos, como operaciones de hash, ordenación, copia masiva y creación de índices.|  
|**Bloqueos de cierre**|Especificación del número actual de bloqueos de cierre en uso en el servidor (se actualiza periódicamente). Un bloqueo de cierre representa un recurso individual bloqueado, como una tabla, página o fila.|  
|**Bloqueos de cierre asignados**|Especifica el número actual de bloqueos de cierre asignados. Al iniciar el servidor, el número de bloqueos de cierre asignados más el número de bloqueos de propietario de bloqueo asignados depende de la opción de configuración **Bloqueos** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si se necesitan más bloqueos de cierre, el valor aumenta.|  
|**Memoria de bloqueos (KB)**|Especifica la cantidad total de memoria dinámica que el servidor utiliza para bloqueos.|  
|**Bloqueos de propietario de bloqueo**|Especifica el número de bloqueos de propietario de bloqueo actualmente en uso en el servidor (se actualiza periódicamente). Un bloqueo de propietario de bloqueo representa la propiedad de un bloqueo sobre un objeto por parte de un subproceso individual. Por tanto, si cada uno de tres subprocesos tiene un bloqueo compartido (S) en una página, existirán tres bloqueos de propietario de bloqueo.|  
|**Bloqueos de propietario de bloqueo asignados**|Especifica el número actual de bloqueos de propietario de bloqueo asignados. Al iniciar el servidor, el número de bloqueos de propietario de bloqueo asignados y el número de bloqueos de cierre asignados depende de la opción de configuración **Bloqueos** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si se necesitan más bloqueos de propietario de bloqueo, el valor aumenta dinámicamente.|  
|**Memoria de grupo de registros (KB)**|Cantidad total de memoria dinámica que está usando el servidor para el grupo de registros.| 
|**Memoria máxima del área de trabajo (KB)**|Indica la cantidad máxima de memoria disponible para la ejecución de procesos, como las operaciones de hash, ordenación, copia masiva y creación de índices.|  
|**Concesiones de memoria otorgadas**|Especifica el número total de procesos que consiguieron una concesión de memoria del área de trabajo.|  
|**Concesiones de memoria pendientes**|Especifica el número total de procesos en espera de una concesión de memoria del área de trabajo.|  
|**Memoria del optimizador (KB)**|Especifica la cantidad total de memoria dinámica que el servidor usa para la optimización de consultas.|  
|**Memoria de servidor reservada (KB)**|Indica la cantidad de memoria que el servidor ha reservado para el uso futuro. Este contador muestra la cantidad actual de memoria sin usar concedida inicialmente que se muestra en **Memoria de área de trabajo concedida (KB)** .|  
|**Memoria caché de SQL (KB)**|Especifica la cantidad total de memoria dinámica que el servidor usa para la memoria caché dinámica de SQL.|  
|**Memoria de servidor descartada (KB)**|Especifica la cantidad de memoria que el servidor usa para otro fin distinto a las páginas de base de datos.|  
|**Memoria del servidor de destino (KB)**|Indica la cantidad ideal de memoria dinámica que el servidor puede usar.|  
|**Memoria total del servidor (KB)**|Especifica la cantidad de memoria que el servidor ha confirmado con el administrador de memoria.|  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar el uso de recursos &#40;Monitor de sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [Buffer Manager (objeto de SQL Server)](../../relational-databases/performance-monitor/sql-server-buffer-manager-object.md)   
[sys.dm_os_performance_counters (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
