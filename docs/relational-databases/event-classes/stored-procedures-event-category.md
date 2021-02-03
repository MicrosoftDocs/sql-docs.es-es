---
description: Procedimientos almacenados (categoría de eventos)
title: Categoría de eventos Procedimientos almacenados | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Stored Procedures event category [SQL Server]
- SQL Server event classes, Stored Procedures event category
- event classes [SQL Server], Stored Procedures event category
ms.assetid: 71bebaa3-a05a-4695-b349-078cecd0949a
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 000fab845e06d28d2671032ae032bdae10881b24
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161436"
---
# <a name="stored-procedures-event-category"></a>Procedimientos almacenados (categoría de eventos)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La categoría de eventos **Procedimientos almacenados** contiene eventos de procedimientos almacenados generales.  
  
## <a name="in-this-section"></a>En esta sección  
  
|Tema|Descripción|  
|-----------|-----------------|  
|[RPC:Completed (clase de eventos)](../../relational-databases/event-classes/rpc-completed-event-class.md)|Indica que se ha completado una llamada a procedimiento remoto (RPC).|  
|[PreConnect:Completed (clase de eventos)](../../relational-databases/event-classes/preconnect-completed-event-class.md)|Indica el momento en que la función clasificadora del regulador de recursos finaliza la ejecución.|  
|[PreConnect:Starting, clase de eventos](../../relational-databases/event-classes/preconnect-starting-event-class.md)|Indica el momento en que la función clasificadora del regulador de recursos inicia la ejecución.|  
|[RPC Output Parameter (clase de eventos)](../../relational-databases/event-classes/rpc-output-parameter-event-class.md)|Realiza un seguimiento de los valores de parámetros de salida de las llamadas a procedimiento remoto tras su ejecución.|  
|[RPC:Starting (clase de eventos)](../../relational-databases/event-classes/rpc-starting-event-class.md)|Indica que se está iniciando una llamada a procedimiento remoto.|  
|[SP:CacheHit (clase de eventos)](../../relational-databases/event-classes/sp-cachehit-event-class.md)|Indica que el procedimiento almacenado se encuentra en la caché.|  
|[SP:CacheInsert (clase de eventos)](../../relational-databases/event-classes/sp-cacheinsert-event-class.md)|Indica que el procedimiento almacenado se ha incluido en la caché.|  
|[SP:CacheMiss (clase de eventos)](../../relational-databases/event-classes/sp-cachemiss-event-class.md)|Indica que el procedimiento almacenado no se encontró en la caché.|  
|[SP:CacheRemove (clase de eventos)](../../relational-databases/event-classes/sp-cacheremove-event-class.md)|Indica que el procedimiento almacenado se ha eliminado de la caché.|  
|[SP:Completed (clase de eventos)](../../relational-databases/event-classes/sp-completed-event-class.md)|Indica que ha terminado la ejecución del procedimiento almacenado.|  
|[SP:Recompile (clase de eventos)](../../relational-databases/event-classes/sp-recompile-event-class.md)|Indica que se ha vuelto a compilar el procedimiento almacenado.|  
|[SP:Starting (clase de eventos)](../../relational-databases/event-classes/sp-starting-event-class.md)|Indica que se está iniciando la ejecución del procedimiento almacenado.|  
|[SP:StmtCompleted (clase de eventos)](../../relational-databases/event-classes/sp-stmtcompleted-event-class.md)|Indica que ha finalizado una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] de un procedimiento almacenado.|  
|[SP:StmtStarting (clase de eventos)](../../relational-databases/event-classes/sp-stmtstarting-event-class.md)|Indica que se ha iniciado una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] de un procedimiento almacenado.|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
