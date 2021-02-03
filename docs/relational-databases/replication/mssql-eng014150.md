---
description: MSSQL_ENG014150
title: MSSQL_ENG014150 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014150 error
ms.assetid: c3dd3109-abf3-4b38-a4e9-ef48d0235656
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 6b6ad0060a3d039db1e46fdaab13251e28f5a3f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198775"
---
# <a name="mssql_eng014150"></a>MSSQL_ENG014150
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|14150|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|Replicación-%s: el agente %s se ejecutó correctamente. %s|  
  
## <a name="explanation"></a>Explicación  
 Este mensaje indica que un agente de replicación ha finalizado la ejecución correctamente. La replicación usa los siguientes agentes:  
  
-   Agente de instantáneas. Todas las publicaciones usan este agente.  
  
-   Agente de registro del LOG. Todas las publicaciones transaccionales usan este agente.  
  
-   Agente de lectura de cola. Todas las publicaciones transaccionales habilitadas para las suscripciones de actualización en cola usan este agente.  
  
-   Agente de distribución. Este agente sincroniza las suscripciones a publicaciones transaccionales y publicaciones de instantáneas.  
  
-   Agente de mezcla. Este agente sincroniza las suscripciones a publicaciones de combinación.  
  
-   Trabajos de mantenimiento de la replicación.  
  
## <a name="user-action"></a>Acción del usuario  
 El Agente de registro del LOG, el Agente de lectura de cola y el Agente de distribución se ejecutan normalmente de forma continua, mientras que el resto de agentes se ejecutan normalmente a petición o en función de una programación. Si no espera que un agente haya completado una ejecución, compruebe el estado del agente. Para más información, consulte [Monitor Replication Agents](../../relational-databases/replication/monitor/monitor-replication-agents.md).  
  
## <a name="see-also"></a>Consulte también  
 [Administración del Agente de replicación](../../relational-databases/replication/agents/replication-agent-administration.md)   
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md)   
 [Replication Log Reader Agent](../../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md)   
 [Agente de lectura de cola de replicación](../../relational-databases/replication/agents/replication-queue-reader-agent.md)   
 [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
  
