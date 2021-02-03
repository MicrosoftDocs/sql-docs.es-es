---
description: Database Mirroring State Change, clase de eventos
title: Database Mirroring State Change (clase de eventos) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event notifications [SQL Server], database mirroring
- database mirroring [SQL Server], event notifications
- Database Mirroring State Change event class
ms.assetid: f936a99e-2a81-4768-8177-5c969bbe2e04
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 762b4b264b03a54873918c2a6116b46cc49f1107
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210917"
---
# <a name="database-mirroring-state-change-event-class"></a>Database Mirroring State Change, clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   La clase de eventos **Database Mirroring State Change** indica cuándo cambia el estado de una base de datos reflejada. Incluya esta clase de eventos en los seguimientos que supervisan las condiciones de bases de datos reflejadas.  
  
 Cuando la clase de eventos **Database Mirroring State Change** se incluye en un seguimiento, la sobrecarga relativa es baja. La sobrecarga puede ser mayor si el estado de las bases de datos reflejadas aumenta.  
  
## <a name="data-database-mirroring-state-change-event-class-data-columns"></a>Columnas de datos de la clase de eventos Database Mirroring State Change  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|Identificador de la base de datos especificada mediante la instrucción USE *database* o la base de datos predeterminada si no se emite la instrucción USE *database* para una determinada instancia. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos **ServerName** en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|**DatabaseName**|**nvarchar**|Nombre de la base de datos reflejada.|35|Sí|  
|**EventClass**|**int**|Tipo de evento = 167.|27|No|  
|**EventSequence**|**int**|Secuencia de la clase de eventos en el lote.|51|No|  
|**IntegerData**|**int**|Id. del estado previo.|25|Sí|  
|**IsSystem**|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|**LoginSid**|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede encontrar esta información en la vista de catálogo **sys.server_principals** . Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|**IdSolicitud**|**int**|Identificador de la solicitud que contiene la instrucción.|49|Sí|  
|**ServerName**|**nvarchar**|Nombre de la instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|**SessionLoginName**|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, **SessionLoginName** muestra inicioDeSesión1 y **LoginName** muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|**SPID**|**int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|**StartTime**|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|**State**|**int**|Id. de los nuevos estados de creación de reflejos:<br /><br /> 0 = Notificación de NULL<br /><br /> 1 = Entidad de seguridad sincronizada con testigo<br /><br /> 2 = Entidad de seguridad sincronizada sin testigo<br /><br /> 3 = Reflejo sincronizado con testigo<br /><br /> 4 = Reflejo sincronizado sin testigo<br /><br /> 5 = Conexión con entidad de seguridad perdida<br /><br /> 6 = Conexión con reflejo perdida<br /><br /> 7 = Conmutación por error manual<br /><br /> 8 = Conmutación automática por error<br /><br /> 9 = Creación de reflejos suspendida<br /><br /> 10 = Sin quórum<br /><br /> 11 = Sincronizando el reflejo<br /><br /> 12 = ·Entidad de seguridad en ejecución expuesta|30|Sí|  
|**TextData**|**ntext**|Descripción del cambio de estado.|1|Sí|  
|**TransactionID**|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
