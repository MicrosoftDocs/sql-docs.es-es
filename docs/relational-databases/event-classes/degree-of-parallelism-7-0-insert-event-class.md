---
description: Degree of Parallelism (7.0 Insert), clase de eventos
title: Degree of Parallelism (7.0 Insert), clase de eventos (7.0 Insert) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Degree of Parallelism event class
ms.assetid: 6753ef30-890f-47a3-b0b6-8abb184e1d83
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 197dc975e6ca6e8119644188fac33aec77a541f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182929"
---
# <a name="degree-of-parallelism-70-insert-event-class"></a>Degree of Parallelism (7.0 Insert), clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
   La clase de eventos **Degree of Parallelism (7.0 Insert)** se produce cada vez que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ejecuta una instrucción SELECT, INSERT, UPDATE o DELETE.  
  
 Cuando se incluye esta clase de eventos en un seguimiento, la sobrecarga que conlleva puede reducir mucho el rendimiento si estos eventos se producen frecuentemente. Para minimizar la sobrecarga provocada, limite el uso de esta clase de eventos a los seguimientos que supervisan brevemente problemas específicos.  
  
## <a name="degree-of-parallelism-70-insert-event-class-data-columns"></a>Columnas de datos de la clase de eventos Degree of Parallelism (7.0 Insert)  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|10|Sí|  
|**BinaryData**|**image**|Número de CPU utilizadas para completar el proceso según los valores siguientes:<br /><br /> 0x00000000, indica un plan en serie que se ejecuta en serie.<br /><br /> 0x01000000, indica un plan paralelo que se ejecuta en serie.<br /><br /> >= 0x02000000, indica un plan paralelo que se ejecuta en paralelo.|2|No|  
|**ClientProcessID**|**int**|Identificador que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona su identificador de proceso.|9|Sí|  
|**DatabaseID**|**int**|Identificador de la base de datos especificada mediante la instrucción USE database o la base de datos predeterminada si no se emite la instrucción USE database para una determinada instancia. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos **ServerName** en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|**DatabaseName**|**nvarchar**|Nombre de la base de datos en la que se ejecuta la instrucción del usuario.|35|Sí|  
|**Clase de eventos**|**int**|Tipo de evento = 28.|27|No|  
|**EventSequence**|**int**|Secuencia de un evento determinado de la solicitud.|51|No|  
|**EventSubClass**|**int**|Indica la instrucción ejecutada, según los valores siguientes:<br /><br /> 1=Seleccionar<br /><br /> 2=Insertar<br /><br /> 3=Actualizar<br /><br /> 4=Eliminar|21|No|  
|**GroupID**|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
|**HostName**|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|**Integer Data**|**int**|La cantidad de "memoria de área de trabajo" en kilobytes que se ha concedido a la consulta para realizar operaciones de hash, ordenar o crear índices. La memoria se adquirirá durante la ejecución, según sea necesario.|25|Sí|  
|**IsSystem**|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|**LoginName**|**nvarchar**|Nombre del inicio de sesión del usuario (inicio de sesión de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenciales de inicio de sesión de Windows con el formato DOMAIN\\*username*).|11|Sí|  
|**LoginSid**|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede buscar esta información en la vista de catálogo sys.server_principals. Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|**NTDomainName**|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|**NTUserName**|**nvarchar**|Nombre del usuario de Windows.|6|Sí|  
|**IdSolicitud**|**int**|Solicitar identificación que inició la consulta de texto completo.|49|Sí|  
|**ServerName**|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|**SessionLoginName**|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, **SessionLoginName** muestra inicioDeSesión1 y **LoginName** muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|**SPID**|**int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|**StartTime**|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|**TransactionID**|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)  
  
  
