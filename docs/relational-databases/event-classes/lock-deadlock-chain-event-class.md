---
description: Lock:Deadlock Chain (clase de eventos)
title: Clase de eventos Lock:Deadlock Chain | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Deadlock Chain event class
ms.assetid: 9883127b-aa34-4235-88cc-c161cd2112cc
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 75d130e91c7dca0591aebef54ee890f5bd042fb7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194856"
---
# <a name="lockdeadlock-chain-event-class"></a>Lock:Deadlock Chain (clase de eventos)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La clase de eventos Lock:Deadlock Chain se produce para cada participante en un interbloqueo.  
  
 Utilice la clase de eventos Lock:Deadlock Chain para supervisar si se cumplen las condiciones del interbloqueo. Esta información es útil para determinar si los interbloqueos afectan de forma significativa al rendimiento de la aplicación y qué objetos están implicados. Se puede examinar el código de la aplicación que modifica estos objetos para determinar si se pueden realizar cambios para minimizar los interbloqueos.  
  
## <a name="lockdeadlock-chain-event-class-data-columns"></a>Columnas de datos de la clase de eventos Lock:Deadlock Chain  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BinaryData|**image**|Identificador del recurso de bloqueo.|2|Sí|  
|DatabaseID|**int**|Identificador de la base de datos a la que pertenece este recurso. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos ServerName en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|DatabaseName|**nvarchar**|Nombre de la base de datos a la que pertenece el recurso.|35|Sí|  
|EventClass|**int**|Tipo de evento = 59.|27|No|  
|EventSequence|**int**|Secuencia de un evento determinado de la solicitud.|51|No|  
|EventSubClass|**int**|Tipo de la subclase de eventos.<br /><br /> 101=Tipo de recurso Bloqueo<br /><br /> 102=Tipo de recurso Intercambio|21|Sí|  
|IntegerData|**int**|Número de interbloqueo. Los números se asignan a partir de 0 cuando se inicia el servidor y se incrementan para cada interbloqueo.|25|Sí|  
|IntegerData2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|55|Sí|  
|IsSystem|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|LoginSid|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede buscar esta información en la vista de catálogo sys.server_principals. Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|Mode|**int**|0=NULL: Compatible con los demás modos de bloqueo (LCK_M_NL)<br /><br /> 1=Bloqueo Estabilidad del esquema (LCK_M_SCH_S)<br /><br /> 2=Bloqueo Modificación del esquema (LCK_M_SCH_M)<br /><br /> 3=Bloqueo Compartido (LCK_M_S)<br /><br /> 4=Bloqueo Actualizar (LCK_M_U)<br /><br /> 5=Bloqueo Exclusivo (LCK_M_X)<br /><br /> 6=Bloqueo Intención compartida (LCK_M_IS)<br /><br /> 7=Bloqueo Actualizar intención (LCK_M_IU)<br /><br /> 8=Bloqueo Intención exclusiva (LCK_M_IX)<br /><br /> 9=Actualizar intención compartida (LCK_M_SIU)<br /><br /> 10=Intención compartida exclusiva (LCK_M_SIX)<br /><br /> 11=Actualizar intención exclusiva (LCK_M_UIX)<br /><br /> 12=Bloqueo Actualización masiva (LCK_M_BU)<br /><br /> 13=Intervalo de claves compartido/compartido (LCK_M_RS_S)<br /><br /> 14=Intervalo de claves compartido/actualización (LCK_M_RS_U)<br /><br /> 15=Intervalo de claves de inserción NULL (LCK_M_RI_NL)<br /><br /> 16=Intervalo de claves de inserción compartido (LCK_M_RI_S)<br /><br /> 17=Intervalo de claves de inserción de actualización (LCK_M_RI_U)<br /><br /> 18=Intervalo de claves de inserción exclusivo (LCK_M_RI_X)<br /><br /> 19=Intervalo de claves exclusivo compartido (LCK_M_RX_S)<br /><br /> 20=Intervalo de claves exclusivo de actualización (LCK_M_RX_U)<br /><br /> 21=Intervalo de claves exclusivo exclusivo (LCK_M_RX_X)|32|Sí|  
|ObjectID|**int**|Id. del objeto bloqueado, si está disponible y es aplicable.|22|Sí|  
|ObjectID2|**bigint**|Id. del objeto o entidad relacionado, si está disponible y es aplicable.|56|Sí|  
|OwnerID|**int**|1=TRANSACTION<br /><br /> 2=CURSOR<br /><br /> 3=SESSION<br /><br /> 4=SHARED_TRANSACTION_WORKSPACE<br /><br /> 5=EXCLUSIVE_TRANSACTION_WORKSPACE|58|Sí|  
|RequestID|**int**|Identificador de la solicitud que contiene la instrucción.|49|Sí|  
|nombreDeServidor|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|SessionLoginName|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, SessionLoginName muestra inicioDeSesión1 y LoginName muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.|64|Sí|  
|SPID|**int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|StartTime|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|TextData|**ntext**|Valor de texto que depende del tipo de recurso.|1|Sí|  
|TransactionID|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
|Tipo|**int**|1=NULL_RESOURCE<br /><br /> 2=DATABASE<br /><br /> 3=FILE<br /><br /> 5=OBJECT<br /><br /> 6=PAGE<br /><br /> 7=KEY<br /><br /> 8=EXTENT<br /><br /> 9=RID<br /><br /> 10=APPLICATION<br /><br /> 11=METADATA<br /><br /> 12=AUTONAMEDB<br /><br /> 13=HOBT<br /><br /> 14=ALLOCATION_UNIT|57|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
  
