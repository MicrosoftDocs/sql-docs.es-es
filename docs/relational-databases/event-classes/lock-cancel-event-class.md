---
description: Lock:Cancel (clase de eventos)
title: Clase de eventos Lock:Cancel | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Cancel event class
ms.assetid: d9203e58-40ba-4712-a918-2c34a5d396d7
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4688f2ded26e081ba4fc5fdb4386a5d87dedcb68
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210406"
---
# <a name="lockcancel-event-class"></a>Lock:Cancel (clase de eventos)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   La clase de eventos **Lock:Cancel** indica que se ha cancelado la adquisición de un bloqueo de un recurso; por ejemplo, debido a la cancelación de una consulta.  
  
## <a name="lockcancel-event-class-data-columns"></a>Columnas de datos de la clase de evento Lock:Cancel  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**ApplicationName**|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|10|Sí|  
|**BinaryData**|**image**|Identificador del recurso de bloqueo.|2|Sí|  
|**ClientProcessID**|**int**|Identificador que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona el identificador de proceso del cliente.|9|Sí|  
|**DatabaseID**|**int**|Identificador de la base de datos en la que se ha adquirido el bloqueo. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos **ServerName** en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|**DatabaseName**|**nvarchar**|Nombre de la base de datos en que se ha intentado adquirir el bloqueo.|35|Sí|  
|**Duration**|**bigint**|Tiempo (en microsegundos) entre el envío de la solicitud de bloqueo y la cancelación del mismo.|13|Sí|  
|**EndTime**|**datetime**|Hora a la que finalizó el evento.|15|Sí|  
|**EventClass**|**int**|Tipo de evento = 26.|27|No|  
|**EventSequence**|**int**|Secuencia de un evento determinado de la solicitud.|51|No|  
|**GroupID**|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
|**HostName**|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|**IntegerData2**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|55|Sí|  
|**IsSystem**|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|**LoginName**|**nvarchar**|Nombre del inicio de sesión del usuario (inicio de sesión de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenciales de inicio de sesión de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con el formato DOMINIO\nombreDeUsuario).|11|Sí|  
|**LoginSid**|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede encontrar esta información en la vista de catálogo **sys.server_principals** . Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|**Modo**|**int**|Modo resultante tras cancelar el bloqueo.<br /><br /> 0=NULL: Compatible con los demás modos de bloqueo (LCK_M_NL)<br /><br /> 1=Bloqueo Estabilidad del esquema (LCK_M_SCH_S)<br /><br /> 2=Bloqueo Modificación del esquema (LCK_M_SCH_M)<br /><br /> 3=Bloqueo Compartido (LCK_M_S)<br /><br /> 4=Bloqueo Actualizar (LCK_M_U)<br /><br /> 5=Bloqueo Exclusivo (LCK_M_X)<br /><br /> 6=Bloqueo Intención compartida (LCK_M_IS)<br /><br /> 7=Bloqueo Actualizar intención (LCK_M_IU)<br /><br /> 8=Bloqueo Intención exclusiva (LCK_M_IX)<br /><br /> 9=Actualizar intención compartida (LCK_M_SIU)<br /><br /> 10=Intención compartida exclusiva (LCK_M_SIX)<br /><br /> 11=Actualizar intención exclusiva (LCK_M_UIX)<br /><br /> 12=Bloqueo Actualización masiva (LCK_M_BU)<br /><br /> 13=Intervalo de claves compartido/compartido (LCK_M_RS_S)<br /><br /> 14=Intervalo de claves compartido/actualización (LCK_M_RS_U)<br /><br /> 15=Intervalo de claves de inserción NULL (LCK_M_RI_NL)<br /><br /> 16=Intervalo de claves de inserción compartido (LCK_M_RI_S)<br /><br /> 17=Intervalo de claves de inserción de actualización (LCK_M_RI_U)<br /><br /> 18=Intervalo de claves de inserción exclusivo (LCK_M_RI_X)<br /><br /> 19=Intervalo de claves exclusivo compartido (LCK_M_RX_S)<br /><br /> 20=Intervalo de claves exclusivo de actualización (LCK_M_RX_U)<br /><br /> 21=Intervalo de claves exclusivo exclusivo (LCK_M_RX_X)|32|Sí|  
|**NTDomainName**|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|**NTUserName**|**nvarchar**|Nombre del usuario de Windows.|6|Sí|  
|**ObjectID**|**int**|Id. del objeto en el que se ha cancelado el bloqueo, si está disponible y es aplicable.|22|Sí|  
|**ObjectID2**|**bigint**|Id. del objeto o entidad relacionado, si está disponible y es aplicable.|56|Sí|  
|**OwnerID**|**int**|1=TRANSACTION<br /><br /> 2=CURSOR<br /><br /> 3=SESSION<br /><br /> 4=SHARED_TRANSACTION_WORKSPACE<br /><br /> 5=EXCLUSIVE_TRANSACTION_WORKSPACE|58|Sí|  
|**IdSolicitud**|**int**|Identificador de la solicitud que contiene la instrucción.|49|Sí|  
|**ServerName**|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|**SessionLoginName**|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, **SessionLoginName** muestra inicioDeSesión1 y **LoginName** muestra inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|**SPID**|**int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|**StartTime**|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|**TextData**|**ntext**|Valor de texto dependiente del tipo de bloqueo que se ha adquirido. Este valor es el mismo que el de la columna **resource_description** de **sys.dm_tran_locks**.|1|Sí|  
|**TransactionID**|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
|**Tipo**|**int**|1=NULL_RESOURCE<br /><br /> 2=DATABASE<br /><br /> 3=FILE<br /><br /> 5=OBJECT<br /><br /> 6=PAGE<br /><br /> 7=KEY<br /><br /> 8=EXTENT<br /><br /> 9=RID<br /><br /> 10=APPLICATION<br /><br /> 11=METADATA<br /><br /> 12=AUTONAMEDB<br /><br /> 13=HOBT<br /><br /> 14=ALLOCATION_UNIT|57|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
  
