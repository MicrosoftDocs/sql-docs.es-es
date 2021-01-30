---
description: sys.dm_broker_connections (Transact-SQL)
title: sys.dm_broker_connections (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/08/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_broker_connections
- dm_broker_connections
- sys.dm_broker_connections_TSQL
- dm_broker_connections_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_connections dynamic management view
ms.assetid: d9e20433-67fe-4fcc-80e3-b94335b2daef
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9da190e779d70581ea7b5b6d5fc8552b7bda9991
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174558"
---
# <a name="sysdm_broker_connections-transact-sql"></a>sys.dm_broker_connections (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada conexión de red de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. En la tabla siguiente se proporciona más información:  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**connection_id**|**uniqueidentifier**|Identificador de la conexión. Acepta valores NULL.|  
|**transport_stream_id**|**uniqueidentifier**|Identificador de la conexión de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interfaz de red (SNI) utilizada por esta conexión para las comunicaciones TCP/IP. Acepta valores NULL.|  
|**state**|**smallint**|Estado actual de la conexión. Acepta valores NULL. Valores posibles:<br /><br /> 1 = NEW<br /><br /> 2 = CONNECTING<br /><br /> 3 = CONNECTED<br /><br /> 4 = LOGGED_IN<br /><br /> 5 = CERRADO|  
|**state_desc**|**nvarchar(60)**|Estado actual de la conexión. Acepta valores NULL. Valores posibles:<br /><br /> NEW<br /><br /> CONNECTING<br /><br /> CONNECTED<br /><br /> LOGGED_IN<br /><br /> CLOSED|  
|**connect_time**|**datetime**|Fecha y hora a la que se inició la conexión. Acepta valores NULL.|  
|**login_time**|**datetime**|Fecha y hora a la que se inició una sesión correctamente para la conexión. Acepta valores NULL.|  
|**authentication_method**|**nvarchar(128)**|Nombre del método de autenticación de Windows, como por ejemplo NTLM o KERBEROS. Este valor proviene de Windows. Acepta valores NULL.|  
|**principal_name**|**nvarchar(128)**|Nombre de inicio de sesión que fue validado para obtener permiso de conexión. En el caso de la autenticación de Windows, este valor es el nombre del usuario remoto. En el caso de autenticación basada en certificados, este valor es el propietario del certificado. Acepta valores NULL.|  
|**remote_user_name**|**nvarchar(128)**|Nombre del usuario del mismo nivel en la otra base de datos utilizado por la Autenticación de Windows. Acepta valores NULL.|  
|**last_activity_time**|**datetime**|Fecha y hora a la que se utilizó la conexión por última vez para enviar o recibir información. Acepta valores NULL.|  
|**is_accept**|**bit**|Indica si la conexión se originó en el lado remoto. Acepta valores NULL.<br /><br /> 1 = La conexión es una solicitud aceptada por la instancia remota.<br /><br /> 0 = La conexión fue iniciada por la instancia local.|  
|**login_state**|**smallint**|Estado del proceso de inicio de sesión de esta conexión. Valores posibles:<br /><br /> 0 = INITIAL<br /><br /> 1 = WAIT LOGIN NEGOTIATE<br /><br /> 2 = ONE ISC<br /><br /> 3 = ONE ASC<br /><br /> 4 = TWO ISC<br /><br /> 5 = TWO ASC<br /><br /> 6 = WAIT ISC Confirm<br /><br /> 7 = WAIT ASC Confirm<br /><br /> 8 = WAIT REJECT<br /><br /> 9 = WAIT PRE-MASTER SECRET<br /><br /> 10 = WAIT VALIDATION<br /><br /> 11 = WAIT ARBITRATION<br /><br /> 12 = EN LÍNEA<br /><br /> 13 = ERROR|  
|**login_state_desc**|**nvarchar(60)**|Estado actual del inicio de sesión en el equipo remoto. Valores posibles:<br /><br /> Se está inicializando el protocolo de enlace de la conexión.<br /><br /> El protocolo de enlace de la conexión está esperando el mensaje de negociación de inicio de sesión.<br /><br /> El protocolo de enlace de la conexión se ha inicializado y ha enviado el contexto de seguridad para la autenticación.<br /><br /> El protocolo de enlace de la conexión ha recibido y aceptado el contexto de seguridad para la autenticación.<br /><br /> El protocolo de enlace de la conexión se ha inicializado y ha enviado el contexto de seguridad para la autenticación. Existe un mecanismo opcional disponible para la autenticación de los elementos del mismo nivel.<br /><br /> El protocolo de enlace de la conexión ha recibido y enviado el contexto de seguridad aceptado para la autenticación. Existe un mecanismo opcional disponible para la autenticación de los elementos del mismo nivel.<br /><br /> El protocolo de enlace de la conexión está esperando el mensaje de confirmación de inicialización del contexto de seguridad.<br /><br /> El protocolo de enlace de la conexión está esperando el mensaje de confirmación de aceptación del contexto de seguridad.<br /><br /> El protocolo de enlace de la conexión está esperando el mensaje de rechazo de SSPI para un error de autenticación.<br /><br /> El protocolo de enlace de la conexión está esperando el mensaje de secreto maestro preliminar.<br /><br /> El protocolo de enlace de la conexión está esperando el mensaje de validación.<br /><br /> El protocolo de enlace de la conexión está esperando el mensaje de arbitraje.<br /><br /> El protocolo en enlace de la conexión está completado y en línea (listo) para el intercambio de mensajes.<br /><br /> La conexión tiene errores.|  
|**peer_certificate_id**|**int**|El identificador del objeto local del certificado que utiliza la instancia remota para la autenticación. El propietario de este certificado debe tener permisos CONNECT para el extremo de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Acepta valores NULL.|  
|**encryption_algorithm**|**smallint**|Algoritmo de cifrado utilizado para esta conexión. Acepta valores NULL. Valores posibles:<br /><br /> **Valor &#124; Descripción &#124; opción DDL correspondiente**<br /><br /> 0 &#124; ninguna &#124; deshabilitada<br /><br /> 1 &#124; SOLO FIRMA<br /><br /> 2 &#124; AES, RC4 &#124; necesario &#124; algoritmo RC4 obligatorio}<br /><br /> 3 &#124; AES &#124;algoritmo necesario AES<br /><br /> **Nota:** El algoritmo RC4 solo se admite por razones de compatibilidad con versiones anteriores. El material nuevo solo se puede cifrar con RC4 o RC4_128 cuando la base de datos tenga el nivel de compatibilidad 90 o 100. (No se recomienda). Use un algoritmo más reciente como uno de los algoritmos AES en su lugar. En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, el material cifrado con RC4 o RC4_128 se puede descifrar en cualquier nivel de compatibilidad.|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Representación de texto del algoritmo de cifrado. Acepta valores NULL. Valores posibles:<br /><br /> **Descripción &#124; opción DDL correspondiente**<br /><br /> NINGUNO &#124; deshabilitado<br /><br /> RC4 &#124; {&#124; obligatorio de algoritmo RC4}<br /><br /> AES &#124; algoritmo obligatorio AES<br /><br /> NINGUNO, RC4 &#124; {compatible &#124; algoritmo RC4}<br /><br /> NINGUNO, AES &#124; algoritmo RC4 compatible<br /><br /> RC4, AES &#124; algoritmo obligatorio RC4 AES<br /><br /> AES, RC4 &#124; algoritmo necesario AES RC4<br /><br /> NINGUNO, RC4, AES &#124; se admite el algoritmo RC4 AES<br /><br /> NINGUNO, AES, RC4 &#124; algoritmo admitido AES RC4|  
|**receives_posted**|**smallint**|Número de recepciones asincrónicas de red que aún no se han completado para esta conexión. Acepta valores NULL.|  
|**is_receive_flow_controlled**|**bit**|Determina si se han postergado las recepciones de red debido al control de flujo de la red porque la red está ocupada. Acepta valores NULL.<br /><br /> 1 = True|  
|**sends_posted**|**smallint**|El número de envíos asincrónicos de red que aún no se han completado para esta conexión. Acepta valores NULL.|  
|**is_send_flow_controlled**|**bit**|Determina si se han postergado los envíos de red debido al control de flujo de la red porque la red está ocupada. Acepta valores NULL.<br /><br /> 1 = True|  
|**total_bytes_sent**|**bigint**|Número total de bytes enviados por esta conexión. Acepta valores NULL.|  
|**total_bytes_received**|**bigint**|Número total de bytes recibidos por esta conexión. Acepta valores NULL.|  
|**total_fragments_sent**|**bigint**|Número total de fragmentos de mensaje de [!INCLUDE[ssSB](../../includes/sssb-md.md)] enviados por esta conexión. Acepta valores NULL.|  
|**total_fragments_received**|**bigint**|Número total de fragmentos de mensaje de [!INCLUDE[ssSB](../../includes/sssb-md.md)] recibidos por esta conexión. Acepta valores NULL.|  
|**total_sends**|**bigint**|Número total de solicitudes de envío de red emitidas por esta conexión. Acepta valores NULL.|  
|**total_receives**|**bigint**|Número total de solicitudes de recepción de red emitidas por esta conexión. Acepta valores NULL.|  
|**peer_arbitration_id**|**uniqueidentifier**|Identificador interno para el extremo. Acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="physical-joins"></a>Combinaciones físicas  
 ![Combinaciones de sys.dm_broker_connections](../../relational-databases/system-dynamic-management-views/media/join-dm-broker-connections-1.gif "Combinaciones de sys.dm_broker_connections")  
  
## <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Relación|  
|----------|--------|------------------|  
|**dm_broker_connections.connection_id**|**dm_exec_connections.connection_id**|Uno a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

