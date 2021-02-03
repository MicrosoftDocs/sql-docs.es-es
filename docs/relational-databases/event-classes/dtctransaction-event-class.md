---
description: DTCTransaction [clase de eventos]
title: Clase de eventos DTCTransaction | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- DTCTransaction event class
ms.assetid: 9a2d358e-5b8f-4d0b-8b93-6705c009ad57
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 12ab86f1c07c8f4522d7190a1ed3471182ec8145
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182880"
---
# <a name="dtctransaction-event-class"></a>DTCTransaction [clase de eventos]
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   Use la clase de eventos **DTCTransaction** para supervisar el estado de las transacciones del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] coordinadas mediante Coordinador de transacciones distribuidas (DTC) de [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Entre estas transacciones se incluyen las que implican dos o más bases de datos en la misma instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)], así como las transacciones distribuidas que implican dos o más instancias del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="dtctransaction-event-class-data-columns"></a>Columnas de datos de la clase de eventos DTCTransaction  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|10|Sí|  
|**BinaryData**|**image**|Representación binaria del Id. de unidad de trabajo (UOW) que identifica de forma exclusiva a esta transacción en DTC.|2|Sí|  
|**ClientProcessID**|**int**|Identificador que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona el identificador de proceso del cliente.|9|Sí|  
|**DatabaseID**|**int**|Identificador de la base de datos especificada mediante la instrucción USE *database* o la base de datos predeterminada si no se emite la instrucción USE *database* para una determinada instancia. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos **ServerName** en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|**DatabaseName**|**nvarchar**|Nombre de la base de datos en la que se ejecuta la instrucción del usuario.|35|Sí|  
|**EventClass**|**int**|Tipo de evento = 19.|27|No|  
|**EventSequence**|**int**|Secuencia de un evento determinado de la solicitud.|51|No|  
|**EventSubClass**|**int**|Tipo de la subclase de eventos.<br /><br /> 0=Obtener dirección<br /><br /> 1=Propagar transacción<br /><br /> 3=Cerrar conexión<br /><br /> 6=Crear una nueva transacción DTC<br /><br /> 7=Dar de alta en una transacción DTC<br /><br /> 9=Confirmación interna<br /><br /> 10=Anulación interna<br /><br /> 14=Preparar transacción<br /><br /> 15=La transacción está preparada<br /><br /> 16=La transacción se está anulando<br /><br /> 17=La transacción se está confirmando<br /><br /> 22=Error de TM en el estado preparado<br /><br /> 23=Desconocido|21|Sí|  
|**GroupID**|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
|**HostName**|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|**IntegerData**|**int**|Nivel de aislamiento de la transacción.|25|Sí|  
|**IsSystem**|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|**LoginName**|**nvarchar**|Nombre del inicio de sesión del usuario (inicio de sesión de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenciales de inicio de sesión de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con el formato DOMINIO\nombreDeUsuario).|11|Sí|  
|**LoginSid**|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede encontrar esta información en la vista de catálogo **sys.server_principals** . Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|**NTDomainName**|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|**NTUserName**|**nvarchar**|Nombre del usuario de Windows.|6|Sí|  
|**IdSolicitud**|**int**|Identificador de la solicitud que contiene la instrucción.|49|Sí|  
|**ServerName**|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|**SessionLoginName**|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, **SessionLoginName** muestra inicioDeSesión1 y **LoginName** muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|**SPID**|**int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|**StartTime**|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|**TextData**|**ntext**|Representación textual del UOW que identifica de forma exclusiva a esta transacción en DTC.|1|Sí|  
|**TransactionID**|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
|**XactSequence**|**bigint**|Token que se utiliza para describir la transacción actual.|50|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
