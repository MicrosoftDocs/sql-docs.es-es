---
description: sys.dm_tcp_listener_states (Transact-SQL)
title: sys.dm_tcp_listener_states (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tcp_listener_states
- dm_tcp_listener_states
- sys.dm_tcp_listener_states_TSQL
- dm_tcp_listener_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], listeners
- sys.dm_tcp_listener_states dynamic management view
ms.assetid: 9997ffed-a4c1-428f-8bac-3b9e4b16d7cf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: af9f50c3314386fbac22ea35abc26d4ab9be6d75
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203322"
---
# <a name="sysdm_tcp_listener_states-transact-sql"></a>sys.dm_tcp_listener_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila que contiene la información de estado dinámico para cada agente de escucha TCP.  
  
> [!NOTE]
> El agente de escucha del grupo de disponibilidad podría escuchar en el mismo puerto que el agente de escucha de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En este caso, los agentes de escucha se enumeran por separado, como en el caso de un agente de escucha de Service Broker.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**listener_id**|**int**|IDENTIFICADOR interno del agente de escucha. No admite valores NULL.<br /><br /> Clave principal.|  
|**ip_address**|**nvarchar (48)**|La dirección IP del agente de escucha que está en línea y que está siendo objeto de escucha actualmente. Se permite IPv4 e IPv6. Si un agente de escucha posee ambos tipos de direcciones, se enumeran por separado. Un carácter comodín IPv4 se muestra como "0.0.0.0". Un carácter comodín IPv6 se muestra como "::".<br /><br /> No admite valores NULL.|  
|**is_ipv4**|**bit**|Tipo de dirección IP<br /><br /> 1 = IPv4<br /><br /> 0 = IPv6|  
|**port**|**int**|Número de puerto en el que escucha el agente de escucha. No admite valores NULL.|  
|**type**|**tinyint**|Tipo de agente de escucha, uno de los siguientes:<br /><br /> 0 = [!INCLUDE[tsql](../../includes/tsql-md.md)]<br /><br /> 1 = Service Broker<br /><br /> 2 = Creación de reflejo de base de datos<br /><br /> No admite valores NULL.|  
|**type_desc**|**nvarchar (20)**|Descripción del **tipo**, uno de los siguientes:<br /><br /> TSQL<br /><br /> SERVICE_BROKER<br /><br /> DATABASE_MIRRORING<br /><br /> No admite valores NULL.|  
|**state**|**tinyint**|Estado del agente de escucha del grupo de disponibilidad, uno de los siguientes:<br /><br /> 1 = En línea. El agente de escucha está escuchando y procesando solicitudes.<br /><br /> 2 = Pendiente de reiniciarse. El agente de escucha está sin conexión, pendiente de un reinicio.<br /><br /> Si el agente de escucha del grupo de disponibilidad está escuchando en el mismo puerto que la instancia de servidor, estos dos agentes de escucha tienen siempre el mismo estado.<br /><br /> No admite valores NULL.<br /><br /> Nota: los valores de esta columna proceden del objeto TSD_listener. La columna no admite un estado sin conexión porque cuando TDS_listener está sin conexión, no se le puede consultar el estado.|  
|**state_desc**|**nvarchar (16)**|Descripción del **Estado**, uno de los siguientes:<br /><br /> ONLINE<br /><br /> PENDING_RESTART<br /><br /> No admite valores NULL.|  
|**start_time**|**datetime**|Marca de tiempo que indica cuándo se inició el agente de escucha. No admite valores NULL.|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Vistas de catálogo de grupos de disponibilidad AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Funciones y vistas de administración dinámica de grupos de disponibilidad AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)  
  
  
