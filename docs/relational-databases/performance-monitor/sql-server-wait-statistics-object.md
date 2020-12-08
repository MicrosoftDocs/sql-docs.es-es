---
title: Wait Statistics (objeto de SQL Server) | Microsoft Docs
description: Obtenga información sobre el objeto de rendimiento SQLServer:Wait Statistics, que contiene contadores de rendimiento que ofrecen información sobre el estado de la espera.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Wait Statistics object
- SQLServer:Wait Statistics
ms.assetid: cb7f917d-4291-4115-9b78-ee7692ebbb2d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: df97bc68487d6113857fa0d9c6a56bdc7a2871b9
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505564"
---
# <a name="sql-server-wait-statistics-object"></a>Wait Statistics (objeto de SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El objeto de rendimiento de **SQLServer:Wait Statistics** contiene contadores de rendimiento que ofrecen información sobre el estado de la espera.  
  
 La tabla incluida a continuación enumera los contadores que contiene el objeto de Wait Statistics.  
  
|Contadores de Wait Statistics de SQL Server|Descripción|  
|-----------------------------------------|-----------------|  
|**Esperas de bloqueo**|Estadísticas para procesos que esperan en un bloqueo.|  
|**Esperas de búfer de registro**|Estadísticas para procesos que esperan que el búfer de registro esté disponible.|  
|**Esperas de escritura en registro**|Estadísticas para procesos que esperan que se escriba el búfer de registro.|  
|**Esperas de cola de concesión de memoria**|Estadísticas para procesos que esperan que una concesión de memoria esté disponible.|  
|**Esperas de E/S de red**|Estadísticas relacionadas con la espera en E/S de red.|  
|**Esperas de bloqueos temporales distintos de páginas**|Estadísticas relacionadas con bloqueos temporales distintos de páginas.|  
|**Esperas de bloqueos temporales de E/S de páginas**|Estadísticas relacionadas con bloqueos temporales de E/S de páginas.|  
|**Esperas de bloqueos temporales de páginas**|Estadísticas relacionadas con bloqueos temporales de páginas, no incluidos los bloqueos temporales de E/S.|  
|**Esperas de objetos de memoria seguros para subprocesos**|Estadísticas para procesos que esperan en asignadores de memoria seguros para subprocesos.|  
|**Esperas de propiedad de transacción**|Estadísticas relacionadas con procesos que sincronizan el acceso a la transacción.|  
|**Espera del objeto de trabajo**|Estadísticas relacionadas con procesos que esperan que el objeto de trabajo esté disponible.|  
|**Esperas de sincronización de área de trabajo**|Estadísticas relacionadas con procesos que sincronizan el acceso al área de trabajo.|  
  
 Cada contador del objeto contiene las instancias siguientes:  
  
|Elemento|Descripción|  
|----------|-----------------|  
|**Tiempo promedio de espera (ms)**|Tiempo promedio del tipo de espera seleccionado.|  
|**Tiempo de espera acumulado (ms) por segundo**|Tiempo de espera agregado por segundo para el tipo de espera seleccionado.|  
|**Espera en curso**|Número de procesos actualmente en espera en el tipo siguiente.|  
|**Esperas iniciadas por segundo**|Número de esperas iniciadas por segundo del tipo de espera seleccionado.|  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar el uso de recursos&#40;Monitor de sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
