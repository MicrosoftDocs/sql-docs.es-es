---
title: Clase de evento Blocked Process Report | Microsoft Docs
description: La clase de eventos Blocked Process Report indica que una tarea ha estado bloqueada durante un tiempo superior al período de tiempo especificado en SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Blocked Process Report event class
ms.assetid: e8acb408-938d-4b36-81dd-04f087410cc5
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bfefbd1c87ed3e6da5432a3c45bd1bd99fc28a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187509"
---
# <a name="blocked-process-report-event-class"></a>Blocked Process Report, clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La clase de evento **Blocked Process Report** indica que una tarea ha estado bloqueada durante más de un período de tiempo especificado. Esta clase de evento no incluye tareas del sistema ni tareas que estén esperando recursos no detectables por interbloqueo.  
  
 Para configurar el umbral y la frecuencia con la que se genera el informe, use el comando **sp_configure** para configurar la opción **blocked process threshold** , que puede establecerse en segundos. De manera predeterminada, se producen informes de procesos no bloqueados. Para obtener más información sobre cómo configurar la opción **blocked process threshold** , vea [blocked process threshold (opción de configuración del servidor)](../../database-engine/configure-windows/blocked-process-threshold-server-configuration-option.md).  
  
 Para obtener más información sobre cómo filtrar los datos devueltos por la clase de eventos **Blocked Process Report**, vea [Filtrar eventos en un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-events-in-a-trace-sql-server-profiler.md), [Establecer un filtro de seguimiento &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/set-a-trace-filter-transact-sql.md) o [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md).  
  
## <a name="blocked-process-report-event-class-data-columns"></a>Columnas de datos de la clase de evento Blocked Process Report  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|Identificador de la base de datos en la que se ha adquirido el bloqueo. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos **ServerName** en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|**Duration**|**bigint**|Período de tiempo (en milisegundos) que el proceso ha estado bloqueado.|13|Sí|  
|**EndTime**|**datetime**|Hora a la que finalizó el evento. Esta columna no se llena para las clases de eventos de inicio, como **SQL:BatchStarting** o **SP:Starting**.|15|Sí|  
|**EventClass**|**int**|Tipo de evento = 137.|27|No|  
|**EventSequence**|**int**|Secuencia de un evento determinado dentro de la solicitud.|51|No|  
|**IndexID**|**int**|Id. del índice del objeto afectado por el evento. Para determinar el Id. de índice de un objeto, utilice la columna **indid** de la tabla del sistema **sysindexes** .|24|Sí|  
|**IsSystem**|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|**LoginSid**|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Este evento se notifica siempre desde el subproceso del sistema. IsSystem = 1; SID = sa.|41|Sí|  
|**Modo**|**int**|Estado que el evento ha recibido o ha solicitado.<br /><br /> 0=NULL<br /><br /> 1=Sch-S<br /><br /> 2=Sch-M<br /><br /> 3=S<br /><br /> 4=U<br /><br /> 5=X<br /><br /> 6=IS<br /><br /> 7=IU<br /><br /> 8=IX<br /><br /> 9=SIU<br /><br /> 10=SIX<br /><br /> 11=UIX<br /><br /> 12=BU<br /><br /> 13=RangeS-S<br /><br /> 14=RangeS-U<br /><br /> 15=RangeI-N<br /><br /> 16=RangeI-S<br /><br /> 17=RangeI-U<br /><br /> 18=RangeI-X<br /><br /> 19=RangeX-S<br /><br /> 20=RangeX-U<br /><br /> 21=RangeX-X|32|Sí|  
|**ObjectID**|**int**|Id. asignado por el sistema del objeto en el que se ha adquirido el bloqueo, si está disponible y es aplicable.|22|Sí|  
|**ServerName**|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26||  
|**SessionLoginName**|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a SQL Server mediante inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, **SessionLoginName** muestra inicioDeSesión1 y **LoginName** muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|**TextData**|**ntext**|Valor de texto que depende de la clase de eventos capturada en el seguimiento.|1|Sí|  
|**TransactionID**|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
