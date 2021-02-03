---
description: OLEDB DataRead, clase de eventos
title: OLEDB DataRead, clase de eventos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- OLEDB DataRead event class
ms.assetid: fb6869ba-3199-4e32-a650-60a5dda2571e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ad4603f5179755a8d6acac79cec2bdb888188be0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185077"
---
# <a name="oledb-dataread-event-class"></a>OLEDB DataRead, clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La clase de eventos OLEDB DataRead se produce cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] llama a un proveedor OLE DB para consultas distribuidas y procedimientos almacenados remotos. Incluya esta clase de eventos en los seguimientos que supervisan cuándo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza una llamada de solicitud de datos al proveedor OLE DB.  
  
 Al incluir la clase OLEDB DataRead habrá una gran sobrecarga. Se recomienda limitar el uso de esta clase de eventos a los seguimientos que supervisan problemas específicos durante períodos breves.  
  
## <a name="oledb-dataread-event-class-data-columns"></a>Columnas de datos de la clase de eventos OLEDB DataRead  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|10|Sí|  
|ClientProcessID|**int**|Identificador que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona el identificador de proceso del cliente.|9|Sí|  
|DatabaseID|**int**|Identificador de la base de datos especificada mediante la instrucción USE *database* o la base de datos predeterminada si no se emite la instrucción USE *database* para una determinada instancia. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos ServerName en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|DatabaseName|**nvarchar**|Nombre de la base de datos en la que se ejecuta la instrucción del usuario.|35|Sí|  
|Duration|**bigint**|Tiempo necesario para completar el evento de llamada OLE DB.|13|No|  
|EndTime|**datetime**|Hora a la que finalizó el evento.|15|Sí|  
|Error|**int**|Número de error de un evento dado. Con frecuencia, es el número de error almacenado en la vista de catálogo sys.messages.|31|Sí|  
|EventClass|**int**|Tipo de evento = 121.|27|No|  
|EventSequence|**int**|Secuencia de la clase de eventos OLE DB en el lote.|51|No|  
|EventSubClass|**int**|Tipo de la subclase de eventos.<br /><br /> 0=Inicio<br /><br /> 1=Completado|21|No|  
|GroupID|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
|HostName|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|IsSystem|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|LinkedServerName|**nvarchar**|Nombre del servidor vinculado.|45|Sí|  
|LoginName|**nvarchar**|Nombre del inicio de sesión del usuario (inicio de sesión de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenciales de inicio de sesión de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows en formato DOMINIO\nombreDeUsuario).|11|Sí|  
|LoginSid|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede buscar esta información en la vista de catálogo sys.server_principals. Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|MethodName|**nvarchar**|Nombre del método de llamada.|47|No|  
|NTDomainName|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|NTUserName|**nvarchar**|Nombre del usuario de Windows.|6|Sí|  
|ProviderName|**nvarchar**|Nombre del proveedor OLE DB.|46|Sí|  
|RequestID|**int**|Identificador de la solicitud que contiene la instrucción.|49|Sí|  
|SessionLoginName|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, SessionLoginName muestra inicioDeSesión1 y LoginName muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|SPID|**Int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|StartTime|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|TextData|**nvarchar**|Parámetros enviados y recibidos en la llamada OLE DB.|1|No|  
|TransactionID|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Objetos de automatización OLE en Transact-SQL](../../relational-databases/stored-procedures/ole-automation-objects-in-transact-sql.md)  
  
  
