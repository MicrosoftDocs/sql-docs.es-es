---
description: Background Job Error [clase de eventos]
title: Clase de eventos Background Job Error | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Background Job Error event class
ms.assetid: 9e6d2a0e-919d-4fe2-a306-b20b8d41c197
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1d16479ce4a8a4218cb704ac670c30401b1f55c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187493"
---
# <a name="background-job-error-event-class"></a>Background Job Error [clase de eventos]
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   La clase de eventos **Background Job Error** se produce cuando un trabajo en segundo plano finaliza de forma anómala. Esta condición podría requerir la atención de un administrador del sistema.  
  
## <a name="background-job-error-event-class-data-columns"></a>Columnas de datos de la clase de eventos Background Job Error  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|Id. de la base de datos especificada por el trabajo. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|**DatabaseName**|**nvarchar**|Nombre de la base de datos en la que se ejecuta la instrucción del usuario.|35|Sí|  
|**Error**|**int**|Número de error del último intento (solo **EventSubClass** 1).|31|Sí|  
|**EventClass**|**int**|Tipo de evento = 193.|27|No|  
|**EventSequence**|**int**|Secuencia de un evento determinado dentro de la solicitud.|51|No|  
|**EventSubClass**|**int**|Tipo de la subclase de eventos.<br /><br /> 1 = Abandono del trabajo en segundo plano después de un error.<br /><br /> 2 = Trabajo en segundo plano quitado; la cola está llena.<br /><br /> 3 = El trabajo en segundo plano devolvió un error.|21|Sí|  
|**IndexID**|**int**|Id. del índice del objeto afectado por el evento. Para determinar el Id. de índice de un objeto, utilice la columna **indid** de la tabla del sistema **sysindexes** .|24|Sí|  
|**IntegerData**|**int**|Número de intentos del trabajo (solo **EventSubClass** 1).|25|Sí|  
|**IntegerData2**|**int**|Número de secuencia de trabajo.|55|Sí|  
|**ObjectID**|**int**|Identificador del objeto asignado por el sistema.|22|Sí|  
|**SessionLoginName**|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, **SessionLoginName** muestra inicioDeSesión1 y **LoginName** muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.|64|Sí|  
|**Gravedad**|**int**|Nivel de gravedad del error del último intento (solo **EventSubClass** 1).|20|Sí|  
|**StartTime**|**datetime**|Hora en que se creó el trabajo.|14|Sí|  
|**State**|**int**|Estado del error del último intento (solo **EventSubClass** 1).|30|Sí|  
|**TextData**|**ntext**|Descripción del valor de subclase de evento.|1|Sí|  
|**Tipo**|**int**|Tipo de trabajo.|57|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Auto Stats (clase de eventos)](../../relational-databases/event-classes/auto-stats-event-class.md)  
  
  
