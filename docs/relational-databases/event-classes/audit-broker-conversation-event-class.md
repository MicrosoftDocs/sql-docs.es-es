---
description: Audit Broker Conversation, clase de eventos
title: Audit Broker Conversation (clase de eventos) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Audit Broker Conversation event class
ms.assetid: d58e3577-e297-42e5-b8fe-206665a75d13
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 41813ebefcdf014045b343719249009a2908584f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179796"
---
# <a name="audit-broker-conversation-event-class"></a>Audit Broker Conversation, clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea un evento **Audit Broker Conversation** para emitir mensajes de auditoría relacionados con la seguridad de diálogo de Service Broker.  
  
## <a name="audit-broker-conversation-event-class-data-columns"></a>Columnas de datos de la clase de evento Audit Broker Conversation  
  
|Columna de datos|Tipo|Descripción|Número de columna|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|10|Sí|  
|**BigintData1**|**bigint**|Número de secuencia del mensaje.|52|No|  
|**ClientProcessID**|**int**|Id. que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona su identificador de proceso.|9|Sí|  
|**DatabaseID**|**int**|Identificador de la base de datos especificada mediante la instrucción USE *baseDeDatos* o identificador de la base de datos predeterminada si no se emite la instrucción USE *baseDeDatos* para una instancia determinada. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos **ServerName** en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|**Error**|**int**|Número de error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , si este evento informa de un error.|31|No|  
|**EventClass**|**int**|Tipo de clase de eventos capturado. Siempre **158** para **Audit Broker Conversation**.|27|No|  
|**EventSubClass**|**int**|Tipo de subclase de evento que proporciona más información acerca de cada clase de evento. En la tabla siguiente se enumeran los valores de subclase de este evento.|21|Sí|  
|**FileName**|**nvarchar**|Motivo del error de inicio de sesión. Si el inicio de sesión se ha realizado correctamente, esta columna estará vacía.|36|No|  
|**GUID**|**uniqueidentifier**|Id. de conversación del diálogo. Este identificador se transmite como parte del mensaje y lo comparten ambas partes de la conversación.|54|No|  
|**HostName**|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para averiguar el nombre de host, use la función **HOST_NAME** .|8|Sí|  
|**IntegerData**|**int**|Número de fragmento del mensaje.|25|No|  
|**NTDomainName**|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|**NTUserName**|**nvarchar**|Nombre del usuario al que pertenece la conexión que generó este evento.|6|Sí|  
|**ObjectId**|**int**|Id. de usuario del servicio de destino.|22|No|  
|**RoleName**|**nvarchar**|Rol del identificador de conversación. Es **initiator** o **target**.|38|No|  
|**ServerName**|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|**Gravedad**|**int**|Gravedad del error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , si este evento informa de un error.|29|No|  
|**SPID**|**int**|Identificador de proceso del servidor que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna al proceso asociado al cliente.|12|Sí|  
|**StartTime**|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|**State**|**int**|Indica la ubicación en el código fuente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que produjo el evento. Cada lugar en el que se puede producir este evento tiene un código de estado diferente. Un ingeniero de soporte técnico de Microsoft puede utilizar este código de estado para buscar el lugar en que se produjo el evento.|30|No|  
|**TextData**|**ntext**|En el caso de errores, contiene un mensaje que describe el motivo. Uno de los siguientes valores:<br /><br /> <br /><br /> **No se encontró el certificado**. El usuario especificado para la seguridad del protocolo de diálogo no tiene ningún certificado.<br /><br /> **No se encuentra en un período de tiempo válido**. El usuario especificado para la seguridad del protocolo de diálogo tiene un certificado, pero ha expirado.<br /><br /> **Certificado demasiado grande para asignación de memoria**. El usuario especificado para la seguridad del protocolo de diálogo tiene un certificado, pero es demasiado grande. El tamaño máximo de certificado que admite Service Broker es de 32.768 bytes.<br /><br /> **No se encontró la clave privada del certificado**. El usuario especificado para la seguridad del protocolo de diálogo tiene un certificado, pero no hay ninguna clave privada asociada al mismo.<br /><br /> **El tamaño de la clave privada del certificado no es compatible con el proveedor de cifrado**. El tamaño de la clave privada del certificado no se puede procesar correctamente. Debe ser un múltiplo de 64 bytes.<br /><br /> **El tamaño de la clave pública del certificado no es compatible con el proveedor de cifrado**. El tamaño de la clave pública del certificado no se puede procesar correctamente. Debe ser un múltiplo de 64 bytes.<br /><br /> **El tamaño de la clave privada del certificado no es compatible con la clave de intercambio de claves cifradas**. El tamaño especificado en la clave de intercambio de claves no coincide con el de la clave privada del certificado. Esto suele indicar que el certificado del equipo remoto no coincide con el certificado de la base de datos.<br /><br /> **El tamaño de la clave pública del certificado no es compatible con la firma del encabezado de seguridad**. El encabezado de seguridad contiene una firma que no se puede validar con la clave pública del certificado. Esto suele indicar que el certificado del equipo remoto no coincide con el certificado de la base de datos.|1|Sí|  
  
 En la tabla siguiente se presentan los valores de subclase de esta clase de evento.  
  
|id|Subclase|Descripción|  
|--------|--------------|-----------------|  
|1|No hay encabezado de seguridad|Durante una conversación segura, Service Broker recibió un mensaje sin clave de sesión. Una vez establecida una conversación segura, el protocolo de diálogo requiere que todos los mensajes de la conversación incluyan una clave de sesión.|  
|2|No hay certificado|Service Broker no encontró un certificado utilizable para uno de los participantes en la conversación. Para poder proteger una conversación, la base de datos debe incluir un certificado tanto para el remitente como para el destinatario de la conversación.|  
|3|Firma incorrecta|Broker no pudo comprobar la firma del mensaje proporcionada por el remitente mediante la clave pública de su certificado. Esto puede indicar que el mensaje está dañado o se ha alterado, que los servicios remoto y local no están configurados con el mismo certificado de usuario o que el certificado ha caducado.|  
|4|Error de ejecución como destino|El usuario de destino no tiene permisos RECEIVE en la cola de destino. Para impedir que los usuarios no autorizados reciban mensajes, Service Broker no pone en cola los mensajes con un usuario de destino que no puede recibir mensajes de la cola, independientemente de si el usuario que ha iniciado el mensaje tiene permiso para poner mensajes en cola o no.|  
  
## <a name="see-also"></a>Vea también  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
