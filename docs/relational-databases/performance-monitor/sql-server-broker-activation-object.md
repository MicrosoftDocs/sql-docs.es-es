---
title: Broker Activation (objeto de SQL Server) | Microsoft Docs
description: Obtenga información sobre el objeto de rendimiento SQLServer:BrokerActivation, que incluye contadores de rendimiento que aportan información acerca de la activación de procedimientos almacenados.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Broker Activation
- Broker Activation object
ms.assetid: cd9b6880-c924-42c7-b333-09c303317c0b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8c257bee159905042ddece42102ef0a55a3512f1
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505876"
---
# <a name="sql-server-broker-activation-object"></a>Broker Activation (objeto de SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El objeto de rendimiento **SQLServer:BrokerActivation** incluye contadores de rendimiento que aportan información acerca de la activación de procedimientos almacenados. En la siguiente tabla se enumeran los contadores incluidos en este objeto.  
  
|Contadores de Broker Activation de SQL Server|Descripción|  
|-------------------------------------------|-----------------|  
|**Procedimientos almacenados invocados/seg.**|Este contador informa del número total de procedimientos de activación almacenados que han invocado todos los monitores de cola en la instancia por segundo.|  
|**Límite de tareas alcanzado**|Este contador informa del número total de veces que un monitor de cola habría iniciado una tarea y no lo hizo porque ya se estaba ejecutando el máximo de tareas de la cola.|  
|**Límite de tareas alcanzado/seg.**|Este contador informa del número de veces por segundo que un monitor de cola habría iniciado una tarea y no lo hizo porque ya se estaba ejecutando el máximo de tareas de la cola.|  
|**Tareas anuladas/seg.**|Este contador informa del número de tareas de procedimiento almacenado de activación que finalizan con un error o que un monitor de cola anula porque no recibe mensajes.|  
|**Tareas en ejecución**|Este contador informa del número de procedimientos almacenados de activación que se están ejecutando.|  
|**Tareas iniciadas/seg.**|Este contador informa del número de procedimientos de activación almacenados que han iniciado por segundo todos los monitores de cola en la instancia.|  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_broker_activated_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-broker-activated-tasks-transact-sql.md)   
 [sys.dm_broker_queue_monitors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-broker-queue-monitors-transact-sql.md)   
 [Supervisar el uso de recursos&#40;Monitor de sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
