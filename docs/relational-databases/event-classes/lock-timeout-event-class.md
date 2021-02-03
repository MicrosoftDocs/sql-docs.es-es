---
title: Clase de eventos Lock:Timeout | Microsoft Docs
description: La clase de eventos Lock:Timeout indica que se ha agotado el tiempo de espera de una solicitud de bloqueo en un recurso debido a un bloqueo de cierre en el recurso en SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Timeout event class
ms.assetid: 8492f4be-4ea9-4059-80e0-9e7b71597da9
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ec230556d518a6f84b7e0ad6266a68c383921a5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211999"
---
# <a name="locktimeout-event-class"></a>Lock:Timeout (clase de eventos)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La clase de eventos Lock:Timeout indica que una solicitud de bloqueo de un recurso, como una página, ha agotado el tiempo de espera debido a que existía otra transacción que mantenía un bloqueo de cierre en el recurso necesario. El tiempo de espera está determinado por la función del sistema @@LOCK_TIMEOUT y se puede establecer con la instrucción SET LOCK_TIMEOUT.  
  
 Utilice la clase de eventos Lock:Timeout para supervisar cuándo se producen las condiciones de tiempo de espera. Esta información es útil para determinar si los tiempos de espera afectan al rendimiento de la aplicación de manera significativa y qué objetos están implicados. Puede examinar el código de aplicación que modifica estos objetos para determinar si se pueden realizar cambios para minimizar los tiempos de espera.  
  
 Los eventos Lock:Timeout con una duración 0 son habitualmente resultado de sondeos de bloqueo internos y no indican necesariamente que exista un problema. El evento Lock:Timeout (timeout > 0) se puede usar para omitir los tiempos de espera con una duración 0.  
  
## <a name="locktimeout-event-class-data-columns"></a>Columnas de datos de la clase de eventos Lock:Timeout  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Nombre de la aplicación cliente que ha creado la conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta columna se rellena con los valores que pasa la aplicación, en lugar de con el nombre que se muestra para el programa.|10|Sí|  
|BinaryData|**image**|Identificador del recurso de bloqueo.|2|Sí|  
|ClientProcessID|**int**|Identificador que el equipo host asigna al proceso en el que se ejecuta la aplicación cliente. Esta columna de datos se rellena si el cliente proporciona el identificador de proceso del cliente.|9|Sí|  
|DatabaseID|**int**|Identificador de la base de datos en la que ha producido el tiempo de espera de bloqueo. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el nombre de la base de datos si se captura la columna de datos ServerName en el seguimiento y el servidor está disponible. Determina el valor de una base de datos mediante la función DB_ID.|3|Sí|  
|DatabaseName|**nvarchar**|Nombre de la base de datos en la que ha agotado el tiempo de espera.|35|Sí|  
|Duration|**bigint**|Tiempo (en microsegundos) entre el envío de la solicitud de bloqueo y la superación del tiempo de espera del mismo.|13|Sí|  
|EndTime|**datetime**|Hora a la que finalizó el evento.|15|Sí|  
|EventClass|**int**|Tipo de evento = 27.|27|No|  
|EventSequence|**int**|Secuencia de un evento determinado dentro de la solicitud.|51|No|  
|GroupID|**int**|Id. del grupo de carga de trabajo donde se activa el evento de Seguimiento de SQL.|66|Sí|  
|HostName|**nvarchar**|Nombre del equipo en el que se está ejecutando el cliente. Esta columna de datos se rellena si el cliente proporciona el nombre del host. Para determinar el nombre del host, utilice la función HOST_NAME.|8|Sí|  
|IntegerData2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|55|Sí|  
|IsSystem|**int**|Indica si el evento ha ocurrido en un proceso del sistema o en un proceso de usuario. 1 = sistema, 0 = usuario.|60|Sí|  
|LoginName|**nvarchar**|Nombre del inicio de sesión del usuario (inicio de sesión de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o credenciales de inicio de sesión de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con el formato DOMINIO\nombreDeUsuario).|11|Sí|  
|LoginSid|**image**|SID (número de identificación de seguridad) del usuario que ha iniciado la sesión. Puede buscar esta información en la vista de catálogo sys.server_principals. Cada SID es único para cada inicio de sesión en el servidor.|41|Sí|  
|Mode|**int**|Modo resultante después del tiempo de espera.<br /><br /> 0=NULL: Compatible con los demás modos de bloqueo (LCK_M_NL)<br /><br /> 1=Bloqueo Estabilidad del esquema (LCK_M_SCH_S)<br /><br /> 2=Bloqueo Modificación del esquema (LCK_M_SCH_M)<br /><br /> 3=Bloqueo Compartido (LCK_M_S)<br /><br /> 4=Bloqueo Actualizar (LCK_M_U)<br /><br /> 5=Bloqueo Exclusivo (LCK_M_X)<br /><br /> 6=Bloqueo Intención compartida (LCK_M_IS)<br /><br /> 7=Bloqueo Actualizar intención (LCK_M_IU)<br /><br /> 8=Bloqueo Intención exclusiva (LCK_M_IX)<br /><br /> 9=Actualizar intención compartida (LCK_M_SIU)<br /><br /> 10=Intención compartida exclusiva (LCK_M_SIX)<br /><br /> 11=Actualizar intención exclusiva (LCK_M_UIX)<br /><br /> 12=Bloqueo Actualización masiva (LCK_M_BU)<br /><br /> 13=Intervalo de claves compartido/compartido (LCK_M_RS_S)<br /><br /> 14=Intervalo de claves compartido/actualización (LCK_M_RS_U)<br /><br /> 15=Intervalo de claves de inserción NULL (LCK_M_RI_NL)<br /><br /> 16=Intervalo de claves de inserción compartido (LCK_M_RI_S)<br /><br /> 17=Intervalo de claves de inserción de actualización (LCK_M_RI_U)<br /><br /> 18=Intervalo de claves de inserción exclusivo (LCK_M_RI_X)<br /><br /> 19=Intervalo de claves exclusivo compartido (LCK_M_RX_S)<br /><br /> 20=Intervalo de claves exclusivo de actualización (LCK_M_RX_U)<br /><br /> 21=Intervalo de claves exclusivo exclusivo (LCK_M_RX_X)|32|Sí|  
|NTDomainName|**nvarchar**|Dominio de Windows al que pertenece el usuario.|7|Sí|  
|NTUserName|**nvarchar**|Nombre del usuario de Windows.|6|Sí|  
|ObjectID|**int**|Id. del objeto que ha superado el tiempo de espera, si está disponible y es aplicable.|22|Sí|  
|ObjectID2|**bigint**|Id. del objeto o entidad relacionado, si está disponible y es aplicable.|56|Sí|  
|OwnerID|**int**|1=TRANSACTION<br /><br /> 2=CURSOR<br /><br /> 3=SESSION<br /><br /> 4=SHARED_TRANSACTION_WORKSPACE<br /><br /> 5=EXCLUSIVE_TRANSACTION_WORKSPACE|58|Sí|  
|RequestID|**int**|Identificador de la solicitud que contiene la instrucción.|49|Sí|  
|nombreDeServidor|**nvarchar**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realiza un seguimiento.|26|No|  
|SessionLoginName|**nvarchar**|Nombre de inicio de sesión del usuario que originó la sesión. Por ejemplo, si se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante inicioDeSesión1 y ejecuta una instrucción como inicioDeSesión2, SessionLoginName mostrará inicioDeSesión1 y LoginName mostrará inicioDeSesión2. En esta columna se muestran los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de Windows.|64|Sí|  
|SPID|**int**|Identificador de la sesión en la que se produjo el evento.|12|Sí|  
|StartTime|**datetime**|Hora a la que se inició el evento, si está disponible.|14|Sí|  
|TextData|**ntext**|Valor de texto que depende del tipo de bloqueo que se adquiría cuando se agotó el tiempo de espera.|1|Sí|  
|TransactionID|**bigint**|Id. de la transacción asignado por el sistema.|4|Sí|  
|Tipo|**int**|1=NULL_RESOURCE<br /><br /> 2=DATABASE<br /><br /> 3=FILE<br /><br /> 5=OBJECT<br /><br /> 6=PAGE<br /><br /> 7=KEY<br /><br /> 8=EXTENT<br /><br /> 9=RID<br /><br /> 10=APPLICATION<br /><br /> 11=METADATA<br /><br /> 12=AUTONAMEDB<br /><br /> 13=HOBT<br /><br /> 14=ALLOCATION_UNIT|57|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Lock:Timeout &#40;timeout &#62; 0&#41;, clase de eventos](../../relational-databases/event-classes/lock-timeout-timeout-0-event-class.md)   
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
  
