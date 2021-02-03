---
title: Clase de eventos Audit Logout | Microsoft Docs
description: La clase de eventos Audit Logout indica que un usuario ha cerrado la sesión de SQL Server. En este artículo se describen las columnas de datos de la clase de eventos Audit Logout.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Audit Logout event class
ms.assetid: 16a0178c-ca03-4078-bbdd-f481385fa2f1
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6e242ddc901453c2bcdff414dc6f8bef2ebb64ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186397"
---
# <a name="audit-logout-event-class"></a>Audit Logout, clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La clase de eventos **Audit Logout** indica que un usuario ha cerrado la sesión de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los eventos de esta clase se activan mediante nuevas conexiones o mediante conexiones reutilizadas de un grupo de conexiones.  
  
## <a name="audit-logout-event-class-data-columns"></a>Columnas de datos de la clase de eventos Audit Logout  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|10|Sí|  
|**ClientProcessID**|**int**|Identificador que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona su identificador de proceso.|9|Sí|  
|**CPU**|**int**|Cantidad de tiempo de CPU (en milisegundos) que utiliza el usuario durante la conexión.|18|Sí|  
|**DatabaseID**|**int**|Identificador de la base de datos especificada mediante la instrucción USE *database* o la base de datos predeterminada si no se emite la instrucción USE *database* para una determinada instancia. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos **ServerName** en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|**DatabaseName**|**nvarchar**|Nombre de la base de datos en la que se ejecuta la instrucción del usuario.|35|Sí|  
|**Duration**|**bigint**|Cantidad aproximada de tiempo desde el inicio de sesión del usuario.|13|Sí|  
|**EndTime**|**datetime**|Hora de cierre de la sesión.|15|Sí|  
|**EventClass**|**int**|Tipo de evento = 15.|27|No|  
|**EventSequence**|**int**|Secuencia de un evento determinado dentro de la solicitud.|51|No|  
|**EventSubClass**|**int**|Tipo de conexión usada por el inicio de sesión. 1 = No agrupada, 2 = Agrupada.|21|Sí|  
|**HostName**|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|**IsSystem**|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|**LoginName**|**nvarchar**|Nombre del inicio de sesión del usuario (inicio de sesión de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenciales de inicio de sesión de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con el formato DOMINIO\nombreDeUsuario).|11|Sí|  
|**LoginSid**|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede encontrar esta información en la vista de catálogo **sys.server_principals** . Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|**NTDomainName**|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|**NTUserName**|**nvarchar**|Nombre del usuario de Windows.|6|Sí|  
|**Reads**|**bigint**|Cantidad de operaciones de E/S de lectura lógica emitidas por el usuario durante la conexión.|16|Sí|  
|**IdSolicitud**|**int**|Identificador de la solicitud que contiene la instrucción.|49|Sí|  
|**ServerName**|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26||  
|**SessionLoginName**|**Nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, **SessionLoginName** muestra inicioDeSesión1 y **LoginName** muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|**SPID**|**int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|**StartTime**|**datetime**|Hora de inicio del evento Audit Login relacionado, si está disponible.|14|Sí|  
|**Success**|**int**|1 = correcto. 0 = error Por ejemplo, el valor 1 significa que se ha comprobado un permiso correctamente y el valor 0 indica que se ha producido un error en la comprobación.|23|Sí|  
|**Writes**|**bigint**|Cantidad de operaciones de E/S de escritura lógica emitidas por el usuario durante la conexión.|17|Sí|  
|**GroupID**|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
