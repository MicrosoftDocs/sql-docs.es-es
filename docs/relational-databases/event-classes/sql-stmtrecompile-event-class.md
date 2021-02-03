---
description: SQL:StmtRecompile, clase de eventos
title: Clase de eventos SQL:StmtRecompile | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SQL:StmtRecompile event class
ms.assetid: 3a134751-3e93-4fe8-bf22-1e0561189293
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cf9e300b77c76de9e6679144568a1556a041c194
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161468"
---
# <a name="sqlstmtrecompile-event-class"></a>SQL:StmtRecompile, clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La clase de eventos SQL:StmtRecompile indica recopilaciones de nivel de instrucción producidas por lotes de todos los tipos: procedimientos almacenados, desencadenadores, lotes ad hoc y consultas. Las consultas pueden enviarse mediante sp_executesql, SQL dinámico, métodos Prepare, métodos Execute u otras interfaces similares. Debería usarse la clase de eventos SQL:StmtRecompile en lugar de la clase de eventos SP:Recompile.  
  
## <a name="sqlstmtrecompile-event-class-data-columns"></a>Columnas de datos de la clase de eventosSQL:StmtRecompile  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se llena con los valores que pasa la aplicación en lugar de llenarse con el nombre que se muestra del programa.|10|Sí|  
|ClientProcessID|**int**|Identificador que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se llena si el cliente proporciona el Id. de proceso.|9|Sí|  
|DatabaseID|**int**|Id. de la base de datos en que se ejecuta el procedimiento almacenado. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|DatabaseName|**nvarchar**|Nombre de la base de datos en la que se ejecuta el procedimiento almacenado.|35|Sí|  
|EventSequence|**int**|Secuencia de un evento determinado en la solicitud.|51|No|  
|EventSubClass|**int**|Describe la causa de la recompilación:<br /><br /> 1 = Esquema cambiado<br /><br /> 2 = Estadísticas cambiadas<br /><br /> 3 = Compilación diferida<br /><br /> 4 = Opción establecida cambiada<br /><br /> 5 = Tabla temporal cambiada<br /><br /> 6 = Conjunto de filas remoto cambiado<br /><br /> 7 = Permisos For Browse cambiados<br /><br /> 8 = Entorno de notificación de consultas cambiado<br /><br /> 9 = Vista de partición cambiada<br /><br /> 10 = Opciones de cursor cambiadas<br /><br /> 11 = Opción (volver a compilar) solicitada|21|Sí|  
|GroupID|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
|HostName|**nvarchar**|Nombre del equipo en el que se ejecuta el cliente y que ha enviado esta instrucción. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|IntegerData2|**int**|Desplazamiento final de la instrucción en el procedimiento almacenado o proceso por lotes que causó la recompilación. El desplazamiento final es -1 si la instrucción es la última de su lote.|55|Sí|  
|IsSystem|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario.<br /><br /> 1 = sistema<br /><br /> 0 = usuario|60|Sí|  
|LineNumber|**int**|Número de secuencia de esta instrucción dentro del lote, si procede.|5|Sí|  
|LoginName|**nvarchar**|Nombre del inicio de sesión que ha enviado este lote.|11|Sí|  
|LoginSid|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede buscar esta información en la vista de catálogo sys.server_principals. Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|NestLevel|**int**|El nivel de anidamiento de la llamada del procedimiento almacenado. Por ejemplo, el procedimiento almacenado my_proc_a llama a my_proc_b. En este caso, my_proc_a tiene un NestLevel de 1 y my_proc_b tiene un NestLevel de 2.|29|Sí|  
|NTDomainName|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|NTUserName|**nvarchar**|Nombre de usuario de Windows perteneciente al usuario conectado.|6|Sí|  
|ObjectID|**int**|Identificador asignado por el sistema del objeto que contiene la instrucción que produce la recompilación. Este objeto puede ser un procedimiento almacenado, un desencadenador o una función definida por el usuario. Para lotes ad hoc o instrucciones SQL preparadas, ObjectID y ObjectName devuelven un valor NULL.|22|Sí|  
|ObjectName|**nvarchar**|Nombre del objeto identificado por ObjectID.|34|Sí|  
|ObjectType|**int**|Valor que representa el tipo de objeto implicado en el evento. Para más información, consulte [ObjectType Trace Event Column](../../relational-databases/event-classes/objecttype-trace-event-column.md).|28|Sí|  
|Offset|**int**|Desplazamiento inicial de la instrucción en el procedimiento almacenado o proceso por lotes que causó la recompilación.|61|Sí|  
|RequestID|**int**|Identificador de la solicitud que contiene la instrucción.|49|Sí|  
|nombreDeServidor|**nvarchar**|Nombre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del que se realiza un seguimiento.|26|No|  
|SessionLoginName|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, SessionLoginName muestra inicioDeSesión1 y LoginName muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|SPID|**int**|Id. de proceso de servidor de la conexión.|12|Sí|  
|SqlHandle|**varbinary**|Hash de 64 bits basado en el texto de una consulta ad hoc o en el Id. de base de datos y de objeto de un objeto SQL. Este valor puede pasarse a sys.dm_exec_sql_text para recuperar el texto SQL asociado.|63|No|  
|StartTime|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|TextData|**ntext**|Texto de la instrucción Transact-SQL que se ha recompilado.|1|Sí|  
|TransactionID|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
|XactSequence|**bigint**|Token que describe la transacción actual.|50|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [SP:Recompile (clase de eventos)](../../relational-databases/event-classes/sp-recompile-event-class.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
