---
description: Broker:Conversation, clase de eventos
title: Clase de eventos Broker:Conversation | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Broker:Conversation event class
ms.assetid: 784707b5-cc67-46a3-8ae6-8f8ecf4b27c0
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 32e74cf95915559a1d8d5e751b79f096d4bb59ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187466"
---
# <a name="brokerconversation-event-class"></a>Broker:Conversation, clase de eventos

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Broker:Conversation** para informar del progreso de una conversación de Service Broker.  
  
## <a name="brokerconversation-event-class-data-columns"></a>Columnas de datos de la clase de eventos Broker:Conversation  
  
|Columna de datos|Tipo|Descripción|Número de columna|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación en lugar de con el nombre mostrado del programa.|10|Sí|  
|**ClientProcessID**|**int**|Id. que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona su identificador de proceso.|9|Sí|  
|**DatabaseID**|**int**|Id. de la base de datos especificada por la instrucción USE *database* . Si no se ha emitido ninguna instrucción USE *database*, el identificador de la base de datos predeterminada. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos **ServerName** en el seguimiento y el servidor está disponible. Determine el valor de una base de datos con la función **DB_ID** .|3|Sí|  
|**EventClass**|**int**|Tipo de clase de eventos capturado. Es siempre **124** para **Broker:Conversation**.|27|No|  
|**EventSequence**|**int**|Número de secuencia de este evento.|51|No|  
|**EventSubClass**|**nvarchar**|Tipo de subclase de evento. Proporciona más información sobre cada clase de eventos.|21|Sí|  
|**GUID**|**uniqueidentifier**|Id. de conversación del diálogo. Este identificador se transmite como parte del mensaje y lo comparten ambas partes de la conversación.|54|No|  
|**HostName**|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para averiguar el nombre de host, use la función **HOST_NAME** .|8|Sí|  
|**IsSystem**|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario.<br /><br /> 0 = usuario<br /><br /> 1 = sistema|60|No|  
|**LoginSid**|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|**MethodName**|**nvarchar**|Grupo de conversación al que pertenece la conversación.|47|No|  
|**NTDomainName**|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|**NTUserName**|**nvarchar**|Nombre del usuario al que pertenece la conexión que generó este evento.|6|Sí|  
|**ObjectName**|**nvarchar**|Identificador de conversación del diálogo.|34|No|  
|**Prioridad**|**int**|Nivel de prioridad de la conversación.|5|Sí|  
|**RoleName**|**nvarchar**|Rol del identificador de conversación. Es **initiator** o **target**.|38|No|  
|**ServerName**|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuyo seguimiento se realiza.|26|No|  
|**Gravedad**|**int**|Gravedad del error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , si este evento informa de un error.|29|No|  
|**SPID**|**int**|Identificador de proceso del servidor que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna al proceso relacionado con el cliente.|12|Sí|  
|**StartTime**|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|**TextData**|**ntext**|El estado actual de la conversación. Puede tener uno de los siguientes valores:|1|Sí|  
|||**SO**. Salida iniciada. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procesó una instrucción BEGIN CONVERSATION para esta conversación, pero no se ha enviado ningún mensaje.|||  
|||**SI**. Entrada iniciada. Otra instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] inició una nueva conversación con la instancia actual, pero la instancia actual no ha terminado de recibir el primer mensaje. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría crear la conversación en este estado si se fragmenta el primer mensaje o si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recibe los mensajes sin orden. No obstante, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría crear la conversación en el estado CO si la primera transmisión recibida de la conversación contiene el primer mensaje completo.|||  
|||**CO**. Conversando. La conversación está establecida y los dos lados de la conversación pueden enviar mensajes. La mayor parte de la comunicación de un servicio típico tiene lugar cuando la conversación está en este estado.|||  
|||**DI**. Entrada desconectada. El lado remoto de la conversación ha emitido un END CONVERSATION. La conversación permanece en este estado hasta que el lado local de la conversación emite un END CONVERSATION. Una aplicación puede seguir recibiendo mensajes de la conversación. Puesto que el lado remoto de la conversación ha finalizado la conversación, una aplicación no puede enviar mensajes en esta conversación. Cuando una aplicación emite un END CONVERSATION, la conversación pasa al estado CD (Cerrada).|||  
|||**HACER**. Salida desconectada. El lado local de la conversación ha emitido un END CONVERSATION. La conversación permanece en este estado hasta que el lado remoto de la conversación confirma un END CONVERSATION. Una aplicación no puede seguir enviando ni recibiendo mensajes de la conversación. Cuando el lado remoto de la conversación confirma el END CONVERSATION, la conversación pasa al estado CD (Cerrada).|||  
|||**ER**. Error. Se ha producido un error en este extremo. Las columnas Error, Severity y State contienen información sobre el error específico que se ha producido.|||  
|||**CD**. Cerrada. El extremo de la conversación ya no se utiliza.|||  
|**Transaction ID**|**bigint**|Identificador de la transacción asignado por el sistema.|4|No|  
  
 En la tabla siguiente se indican los valores de la subclase de esta clase de eventos.  
  
|id|Subclase|Descripción|  
|--------|--------------|-----------------|  
|1|SEND Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **SEND Message** cuando el [!INCLUDE[ssDE](../../includes/ssde-md.md)] ejecuta una instrucción SEND.|  
|2|FINALIZAR CONVERSACIÓN|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **END CONVERSATION** cuando el [!INCLUDE[ssDE](../../includes/ssde-md.md)] ejecuta una instrucción END CONVERSATION que no incluye la cláusula WITH ERROR.|  
|3|END CONVERSATION WITH ERROR|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **END CONVERSATION WITH ERROR** cuando el [!INCLUDE[ssDE](../../includes/ssde-md.md)] ejecuta una instrucción END CONVERSATION que incluye la cláusula WITH ERROR.|  
|4|Broker Initiated Error|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Broker Initiated Error** siempre que [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un mensaje de error. Por ejemplo, cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] no puede enrutar correctamente un mensaje para un diálogo, el agente crea un mensaje de error para el diálogo y genera este evento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no genera este evento cuando una aplicación finaliza una conversación con un error.|  
|5|Terminate Dialog|[!INCLUDE[ssSB](../../includes/sssb-md.md)] finalizó el diálogo. [!INCLUDE[ssSB](../../includes/sssb-md.md)] finaliza los diálogos como respuesta a condiciones que impiden que el diálogo continúe, pero que no son errores ni la finalización normal de una conversación. Por ejemplo, si se quita un servicio, [!INCLUDE[ssSB](../../includes/sssb-md.md)] finaliza todos los diálogos de ese servicio.|  
|6|Received Sequenced Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera una clase de eventos **Received Sequenced Message** cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recibe un mensaje que contiene un número de secuencia de mensaje. Todos los tipos de mensaje definidos por el usuario son mensajes en secuencia. [!INCLUDE[ssSB](../../includes/sssb-md.md)] genera un mensaje sin secuencia en dos casos:<br /><br /> Los mensajes de error generados por [!INCLUDE[ssSB](../../includes/sssb-md.md)] son sin secuencia.<br /><br /> Los reconocimientos de mensajes pueden ser sin secuencia. Para lograr una mayor eficacia, [!INCLUDE[ssSB](../../includes/sssb-md.md)] incluye reconocimientos de mensaje como parte de un mensaje en secuencia. Sin embargo, si una aplicación no envía un mensaje en secuencia al extremo remoto en un período determinado de tiempo, [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un mensaje sin secuencia para el reconocimiento del mensaje.|  
|7|Received END CONVERSATION|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento Received END CONVERSATION cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recibe un mensaje de finalización de diálogo del otro lado de la conversación.|  
|8|Received END CONVERSATION WITH ERROR|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Received END CONVERSATION WITH ERROR** cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recibe un error definido por el usuario procedente del otro lado de la conversación. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no genera este evento cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recibe un error definido por el agente.|  
|9|Received Broker Error Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Received Broker Error Message** cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] recibe un mensaje de error definido por el agente procedente del otro lado de la conversación. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no genera este evento cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] recibe un mensaje de error generado por una aplicación.<br /><br /> Por ejemplo, si la base de datos actual contiene una ruta predeterminada a una base de datos de reenvío, [!INCLUDE[ssSB](../../includes/sssb-md.md)] enruta un mensaje con un nombre de servicio desconocido a la base de datos de reenvío. Si esa base de datos no puede enrutar el mensaje, el agente de la base de datos crea un mensaje de error y lo devuelve a la base de datos actual. Cuando la base de datos actual recibe el error generado por el agente de la base de datos de reenvío, la base de datos actual genera un evento **Received Broker Error Message** .|  
|10|Received END CONVERSATION Ack|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera una clase de eventos **Received END CONVERSATION Ack** cuando el otro lado de una conversación confirma un mensaje de error o de finalización del diálogo enviado por este lado de la conversación.|  
|11|BEGIN DIALOG|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **BEGIN DIALOG** cuando el motor de base de datos ejecuta un comando BEGIN DIALOG.|  
|12|Dialog Created|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento **Dialog Created** cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un extremo para un diálogo. [!INCLUDE[ssSB](../../includes/sssb-md.md)] crea un extremo cada vez que se establece un nuevo diálogo, independientemente de si la base de datos actual es el iniciador o el destino del diálogo.|  
|13|END CONVERSATION WITH CLEANUP|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un evento END CONVERSATION WITH CLEANUP cuando el [!INCLUDE[ssDE](../../includes/ssde-md.md)] ejecuta una instrucción END CONVERSATION que incluye la cláusula WITH CLEANUP.|  
  
## <a name="see-also"></a>Vea también  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
