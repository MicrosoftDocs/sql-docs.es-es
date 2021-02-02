---
description: Showplan All for Query Compile [clase de eventos]
title: Clase de eventos Showplan All for Query Compile | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Showplan All for Query Compile event class
ms.assetid: bb1dc446-5e6c-43d6-9db8-78c76cc2e01f
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9f6e6d5eb3f0c26d664acb5af8edd783792faeeb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99151119"
---
# <a name="showplan-all-for-query-compile-event-class"></a>Showplan All for Query Compile [clase de eventos]
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La clase de eventos Showplan All for Query Compile se produce cuando [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compila una instrucción SQL. Incluya esta clase de eventos para identificar los operadores Showplan. La información contenida es un subconjunto de la disponible en la clase de eventos Showplan XML For Query Compile.  
  
 La clase de eventos Showplan All for Query Compile muestra datos completos de tiempo de compilación, por lo que los seguimientos que incluyen Showplan All for Query Compile pueden afectar de forma considerable el rendimiento. Para minimizar este riesgo, limite el uso de esta clase de evento a los seguimientos que supervisan problemas específicos durante periodos breves.  
  
 Cuando se incluye la clase de eventos Showplan All for Query Compile en un seguimiento es preciso seleccionar la columna de datos BinaryData. Si no se incluye, la información de esta clase de eventos no se mostrará en el seguimiento.  
  
## <a name="showplan-all-for-query-compile-event-class-data-columns"></a>Columnas de datos de la clase de evento Showplan All For Query Compile  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|10|Sí|  
|BinaryData|**image**|Costo estimado de la consulta.|2|No|  
|ClientProcessID|**int**|Identificador que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona su identificador de proceso.|9|Sí|  
|DatabaseID|**int**|Identificador de la base de datos especificada mediante la instrucción USE *database* o la base de datos predeterminada si no se emite la instrucción USE *database* para una determinada instancia. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos ServerName en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|DatabaseName|**nvarchar**|Nombre de la base de datos en la que se ejecuta la instrucción del usuario.|35|Sí|  
|EventClass|**int**|Tipo de evento = 169.|27|No|  
|EventSequence|**int**|Secuencia de un evento determinado dentro de la solicitud.|51|No|  
|GroupID|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
|HostName|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|IntegerData|**int**|Número estimado de filas devueltas.|25|Sí|  
|IsSystem|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|LineNumber|**int**|Muestra el número de la línea que contiene el error.|5|Sí|  
|LoginName|**nvarchar**|Nombre del inicio de sesión del usuario (inicio de sesión de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenciales de inicio de sesión de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con el formato DOMINIO\nombreDeUsuario).|11|Sí|  
|LoginSID|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede buscar esta información en la vista de catálogo sys.server_principals. Cada SID es único para cada inicio de sesión en el servidor.|41|No|  
|NestLevel|**int**|Valor entero que representa los datos devueltos por @@NESTLEVEL.|29|Sí|  
|NTDomainName|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|NTUserName|**nvarchar**|Nombre del usuario de Windows.|6|Sí|  
|ObjectID|**int**|Identificador del objeto asignado por el sistema.|22|Sí|  
|ObjectName|**nvarchar**|Nombre del objeto al que se hace referencia.|34|Sí|  
|ObjectType|**int**|Valor que representa el tipo del objeto implicado en el evento. Este valor corresponde al de la columna Type de la tabla sys.objects. Para ver los valores, vea [Columna de evento de seguimiento ObjectType](../../relational-databases/event-classes/objecttype-trace-event-column.md).|28|Sí|  
|RequestID|**int**|Id. de la solicitud que contiene la instrucción.|49|Sí|  
|nombreDeServidor|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|SessionLoginName|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, SessionLoginName muestra inicioDeSesión1 y LoginName muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|SPID|**int**|Identificador de proceso del servidor que asigna [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al procesado relacionado con el cliente.|12|Sí|  
|StartTime|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|TransactionID|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
|XactSequence|**bigint**|Token que se utiliza para describir la transacción actual.|50|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Referencia de operadores lógicos y físicos del plan de presentación](../../relational-databases/showplan-logical-and-physical-operators-reference.md)  
  
  
