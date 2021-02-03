---
description: Audit Database Mirroring Login, clase de eventos
title: Audit Database Mirroring Login (clase de eventos) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event notifications [SQL Server], database mirroring
- Audit Database Mirroring Login event class
- database mirroring [SQL Server], event notifications
ms.assetid: d0bd436d-aade-4208-a7e5-75cf3b5d0ce9
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eb9145c2eb8c6d29f70a3c1426c2a35dc717154e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193106"
---
# <a name="audit-database-mirroring-login-event-class"></a>Audit Database Mirroring Login, clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea un evento **Audit Database Mirroring Login** para informar de los mensajes de auditoría relacionados con la seguridad de transporte de la creación de reflejo de bases de datos.  
  
## <a name="audit-database-mirroring-login-event-class-data-columns"></a>Columnas de datos de la clase de evento Audit Database Mirroring Login  
  
|Columna de datos|Tipo|Descripción|Número de columna|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|No se utiliza en esta clase de evento.|10|Sí|  
|**ClientProcessID**|**int**|No se utiliza en esta clase de evento.|9|Sí|  
|**DatabaseID**|**int**|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos **ServerName** en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|**EventClass**|**int**|Tipo de clase de eventos capturado. Siempre **154** para **Audit Database Mirroring Login**.|27|No|  
|**EventSequence**|**int**|Número de secuencia de este evento.|51|No|  
|**EventSubClass**|**int**|Tipo de subclase de evento que proporciona más información acerca de cada clase de evento. En la tabla siguiente se enumeran los valores de subclase de este evento.|21|Sí|  
|**FileName**|**nvarchar**|Método de autenticación admitido configurado en el extremo remoto de creación de reflejo de bases de datos. Cuando hay más de un método disponible, el extremo que acepte (el destino) determina el método que se intentará primero. Los valores posibles son:<br /><br /> <br /><br /> **No**. No hay ningún método de autenticación configurado.<br /><br /> **NTLM**. Requiere autenticación NTLM.<br /><br /> **KERBEROS**. Requiere autenticación Kerberos.<br /><br /> **NEGOTIATE**. Windows negocia el método de autenticación.<br /><br /> **CERTIFICATE**. Requiere el certificado configurado para el extremo, que se almacena en la base de datos **maestra** .<br /><br /> **NTLM, CERTIFICATE**. Acepta NTLM o el certificado del extremo para la autenticación.<br /><br /> **KERBEROS, CERTIFICATE**. Acepta Kerberos o el certificado del extremo para la autenticación.<br /><br /> **NEGOTIATE, CERTIFICATE**. Windows negocia el método de autenticación, o se puede utilizar un certificado del extremo para la autenticación.<br /><br /> **CERTIFICATE, NTLM**. Acepta un certificado del extremo o NTLM para la autenticación.<br /><br /> **CERTIFICATE, KERBEROS**. Acepta un certificado de extremo o Kerberos para la autenticación.<br /><br /> **CERTIFICATE, NEGOTIATE**. Acepta un certificado del extremo para la autenticación o Windows negocia el método de autenticación.|36|No|  
|**HostName**|**nvarchar**|No se utiliza en esta clase de evento.|8|Sí|  
|**IsSystem**|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|No|  
|**LoginSid**|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|**NTDomainName**|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|**NTUserName**|**nvarchar**|Nombre del usuario al que pertenece la conexión que generó este evento.|6|Sí|  
|**ObjectName**|**nvarchar**|Cadena de conexión utilizada para esta conexión.|34|No|  
|**OwnerName**|**nvarchar**|Método de autenticación admitido configurado en el extremo local de creación de reflejo de bases de datos. Cuando hay más de un método disponible, el extremo que acepte (el destino) determina el método que se intentará primero. Los valores posibles son:<br /><br /> <br /><br /> **No**. No hay ningún método de autenticación configurado.<br /><br /> **NTLM**. Requiere autenticación NTLM.<br /><br /> **KERBEROS**. Requiere autenticación Kerberos.<br /><br /> **NEGOTIATE**. Windows negocia el método de autenticación.<br /><br /> **CERTIFICATE**. Requiere el certificado configurado para el extremo, que se almacena en la base de datos **maestra** .<br /><br /> **NTLM, CERTIFICATE**. Acepta NTLM o el certificado del extremo para la autenticación.<br /><br /> **KERBEROS, CERTIFICATE**. Acepta Kerberos o el certificado del extremo para la autenticación.<br /><br /> **NEGOTIATE, CERTIFICATE**. Windows negocia el método de autenticación o se puede utilizar un certificado del extremo para la autenticación.<br /><br /> **CERTIFICATE, NTLM**. Acepta un certificado del extremo o NTLM para la autenticación.<br /><br /> **CERTIFICATE, KERBEROS**. Acepta un certificado de extremo o Kerberos para la autenticación.<br /><br /> **CERTIFICATE, NEGOTIATE**. Acepta un certificado del extremo para la autenticación o Windows negocia el método de autenticación.|37|No|  
|**ProviderName**|**nvarchar**|Método de autenticación utilizado para esta conexión.|46|No|  
|**RoleName**|**nvarchar**|Rol de la conexión. Es **initiator** o **target**.|38|No|  
|**ServerName**|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|**SPID**|**int**|Identificador de proceso del servidor que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna al proceso asociado al cliente.|12|Sí|  
|**StartTime**|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|**State**|**int**|Indica la ubicación en el código fuente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que produjo el evento. Cada lugar en el que se puede producir este evento tiene un código de estado diferente. Un ingeniero de soporte técnico de Microsoft puede utilizar este código de estado para buscar el lugar en que se produjo el evento.|30|No|  
|**TargetUserName**|**nvarchar**|Estado del inicio de sesión. Uno de los valores siguientes:<br /><br /> **INITIAL**<br /><br /> **WAIT LOGIN NEGOTIATE**<br /><br /> **ONE ISC**<br /><br /> **ONE ASC**<br /><br /> **TWO ISC**<br /><br /> **TWO ASC**<br /><br /> **WAIT ISC Confirm**<br /><br /> **WAIT ASC Confirm**<br /><br /> **WAIT REJECT**<br /><br /> **WAIT PRE-MASTER SECRET**<br /><br /> **WAIT VALIDATION**<br /><br /> **WAIT ARBITRATION**<br /><br /> **ONLINE**<br /><br /> **ERROR**<br /><br /> <br /><br /> Nota: ISC = Iniciar contexto de seguridad. ASC = Accept Security Context (Aceptar contexto de seguridad).|39|No|  
|**TransactionID**|**bigint**|Identificador de la transacción asignado por el sistema.|4|No|  
  
 En la tabla siguiente se presentan los valores de subclase de esta clase de evento.  
  
|id|Subclase|Descripción|  
|--------|--------------|-----------------|  
|1|Login Success|Un evento Login Success informa de que el proceso de inicio de sesión de creación de reflejo de bases de datos adyacente ha finalizado correctamente.|  
|2|Login Protocol Error|Un evento Login Protocol Error informa de que el inicio de sesión de creación de reflejo de bases de datos recibe un mensaje que está bien estructurado pero que no es válido para el estado actual del proceso de inicio de sesión. Es posible que el mensaje se haya perdido o se haya enviado fuera de secuencia.|  
|3|Message Format Error|Un evento Message Format Error informa de que el inicio de sesión de creación de reflejo de bases de datos recibe un mensaje que no coincide con el formato esperado. Es posible que el mensaje se haya dañado, o que un programa diferente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está enviando mensajes al puerto que utiliza la creación de reflejo de bases de datos.|  
|4|Negotiate Failure|Un evento Negotiate Failure informa de que el extremo local y el extremo remoto de creación de reflejo de bases de datos son compatibles mutuamente con niveles exclusivos de autenticación.|  
|5|Authentication Failure|Un evento Authentication Failure informa de que un extremo de creación de reflejo de bases de datos no puede realizar la autenticación para la conexión debido a un error. Para la autenticación de Windows, este evento informa de que el extremo de creación de reflejo de bases de datos no puede utilizar la autenticación de Windows. Para la autenticación basada en certificados, este evento informa de que el extremo de creación de reflejo de bases de datos no puede obtener acceso al certificado.|  
|6|Error de autorización|Un evento Authorization Failure informa de que un extremo de creación de reflejo de bases de datos denegó la autorización para la conexión. Para la autenticación de Windows, este evento informa de que el identificador de seguridad de la conexión no coincide con uno de los usuarios de la base de datos. Para la autenticación basada en certificados, este evento informa de que la clave pública entregada en el mensaje no se corresponde con ningún certificado de la base de datos **maestra** .|  
  
## <a name="see-also"></a>Consulte también  
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
