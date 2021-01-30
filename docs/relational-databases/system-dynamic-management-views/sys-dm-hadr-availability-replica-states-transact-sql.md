---
description: sys.dm_hadr_availability_replica_states (Transact-SQL)
title: sys.dm_hadr_availability_replica_states (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_availability_replica_states
- sys.dm_hadr_availability_replica_states_TSQL
- sys.dm_hadr_availability_replica_states
- dm_hadr_availability_replica_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_availability_replica_states dynamic management view
ms.assetid: d2e678bb-51e8-4a61-b223-5c0b8d08b8b1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ab171b0adfe6497520ba55d94ec6a1435cc6678a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179318"
---
# <a name="sysdm_hadr_availability_replica_states-transact-sql"></a>sys.dm_hadr_availability_replica_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila para cada réplica local y una fila para cada réplica remota en el mismo grupo de disponibilidad Always On que una réplica local. Cada fila contiene información sobre el estado de una réplica determinada.  
  
> [!IMPORTANT]  
>  Para obtener información sobre cada réplica de un grupo de disponibilidad determinado, consulte **Sys.dm_hadr_availability_replica_states** en la instancia del servidor que hospeda la réplica principal. Cuando se consulta en una instancia de servidor que hospeda una réplica secundaria de un grupo de disponibilidad, esta vista de administración dinámica devuelve solo información local para el grupo de disponibilidad.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|Identificador único de la réplica.|  
|**group_id**|**uniqueidentifier**|Identificador único del grupo de disponibilidad.|  
|**is_local**|**bit**|Si la réplica es local, uno de los siguientes:<br /><br /> 0 = Indica una réplica secundaria remota en un grupo de disponibilidad cuya réplica principal está hospedada en la instancia del servidor local. Este valor solo se produce en la ubicación de la réplica principal.<br /><br /> 1 = indica una réplica local. En las réplicas secundarias, es el único valor disponible para el grupo de disponibilidad al que pertenece la réplica.|  
|**role**|**tinyint**|[!INCLUDE[ssHADR](../../includes/sshadr-md.md)]Rol actual de una réplica local o una réplica remota conectada, uno de los siguientes:<br /><br /> 0 = Resolver<br /><br /> 1 = Principal<br /><br /> 2 = Secundario<br /><br /> Para obtener más información sobre los roles de [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], vea [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).|  
|**role_desc**|**nvarchar(60)**|Descripción del **rol**, uno de los siguientes:<br /><br /> RESOLVING<br /><br /> PRIMARY<br /><br /> SECONDARY|  
|**operational_state**|**tinyint**|Estado operativo actual de la réplica, uno de los siguientes:<br /><br /> 0 = conmutación por error pendiente<br /><br /> 1 = pendiente<br /><br /> 2 = en línea<br /><br /> 3 = sin conexión<br /><br /> 4 = error<br /><br /> 5 = No se pudo establecer quórum<br /><br /> NULL = La réplica no es local.<br /><br /> Para obtener más información, vea [roles y Estados operativos](#RolesAndOperationalStates), más adelante en este tema.|  
|**\_desc. estado operativo \_**|**nvarchar(60)**|Descripción del **\_ estado operativo**, uno de los siguientes:<br /><br /> PENDING_FAILOVER<br /><br /> PENDING<br /><br /> ONLINE<br /><br /> OFFLINE<br /><br /> FAILED<br /><br /> FAILED_NO_QUORUM<br /><br /> NULL|  
|**mantenimiento de la recuperación \_**|**tinyint**|Resumen de la **columna \_ Estado de base de datos** de la vista de administración dinámica [Sys.dm_hadr_database_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) . A continuación se muestran los valores posibles y sus descripciones.<br /><br /> 0: en curso.  Al menos una base de datos combinada tiene un estado de base de datos distinto de ONLINE (el **\_ Estado** de la base de datos no es 0).<br /><br /> 1: en línea. Todas las bases de datos Unidas tienen el estado de base de datos en línea (**database_state** es 0).<br /><br /> NULL: **is_local** = 0|  
|**recovery_health_desc**|**nvarchar(60)**|Descripción de **recovery_health**, uno de los siguientes:<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**Estado de sincronización \_**|**tinyint**|Refleja un resumen del estado de sincronización de base de datos (**synchronization_state**) de todas las bases de datos de disponibilidad Unidas (también conocidas como *réplicas*) y el modo de disponibilidad de la réplica (modo de confirmación sincrónica o asincrónica). El Resumen reflejará el estado acumulado menos correcto en las bases de datos de la réplica. A continuación se muestran los valores posibles y sus descripciones.<br /><br /> 0: no es correcto.   El estado de al menos una de las bases de datos unidas es NOT SYNCHRONIZING.<br /><br /> 1: parcialmente correcto. Algunas réplicas no están en el estado de sincronización del destino: las réplicas de confirmación sincrónica deben ser sincronizadas y las réplicas de confirmación asincrónica deberían estar sincronizándose.<br /><br /> 2: correcto. Todas las réplicas están en el estado de sincronización del destino: las réplicas de confirmación sincrónica se sincronizan y las réplicas de confirmación asincrónica se están sincronizando.|  
|**synchronization_health_desc**|**nvarchar(60)**|Descripción de **synchronization_health**, uno de los siguientes:<br /><br /> NOT_HEALTHY<br /><br /> PARTIALLY_HEALTHY<br /><br /> HEALTHY|  
|**connected_state**|**tinyint**|Si una réplica secundaria está conectada actualmente a la réplica principal. Los valores posibles se muestran a continuación con sus descripciones.<br /><br /> 0: desconectado. La respuesta de una réplica de disponibilidad al estado desconectado depende de su rol: en la réplica principal, si una réplica secundaria está desconectada, sus bases de datos secundarias se marcan como no SINCRONIZAdas en la réplica principal, lo que espera a que la secundaria vuelva a conectarse. En una réplica secundaria, cuando detecta que está desconectada, la réplica secundaria intenta volver a conectarse a la réplica principal.<br /><br /> 1: conectado.<br /><br /> Cada réplica principal realiza un seguimiento del estado de conexión de cada réplica secundaria en el mismo grupo de disponibilidad. Las réplicas secundarias realizan un seguimiento del estado de solo la réplica principal.|  
|**connected_state_desc**|**nvarchar(60)**|Descripción de **connection_state**, uno de los siguientes:<br /><br /> DISCONNECTED<br /><br /> CONNECTED|  
|**last_connect_error_number**|**int**|Número del último error de conexión.|  
|**last_connect_error_description**|**nvarchar(1024)**|Texto del mensaje de **last_connect_error_number** .|  
|**last_connect_error_timestamp**|**datetime**|Marca de tiempo de fecha y hora que indica cuándo se produjo el error de **last_connect_error_number** .|  
  
##  <a name="roles-and-operational-states"></a><a name="RolesAndOperationalStates"></a> Roles y Estados operativos  
 El rol, **rol**, refleja el estado de una réplica de disponibilidad determinada y el estado operativo, **operational_state**, describe si la réplica está lista para procesar las solicitudes de cliente para toda la base de datos de la réplica de disponibilidad. A continuación se indica un resumen de los Estados operativos que son posibles para cada rol: resolver, principal y secundario.  
  
 **Resolviendo:** Cuando una réplica de disponibilidad está en el rol de resolución, los Estados operativos posibles son los que se muestran en la tabla siguiente.  
  
|Estado de funcionamiento|Descripción|  
|-----------------------|-----------------|  
|PENDING_FAILOVER|Se procesa un comando de conmutación por error para el grupo de disponibilidad.|  
|OFFLINE|Todos los datos de configuración para la réplica de disponibilidad se han actualizado en el clúster de WSFC y, además, en los metadatos locales, pero el grupo de disponibilidad no tiene actualmente una réplica principal.|  
|FAILED|Se ha producido un error de lectura al intentar recuperar información del clúster de WSFC.|  
|FAILED_NO_QUORUM|El nodo de WSFC local no tiene quórum. Es un estado deducido.|  
  
 **Principal:** Cuando una réplica de disponibilidad está realizando el rol principal, es actualmente la réplica principal. Los Estados operativos posibles son los que se muestran en la tabla siguiente.  
  
|Estado de funcionamiento|Descripción|  
|-----------------------|-----------------|  
|PENDING|Es un estado transitorio, pero una réplica principal se puede bloquear en este estado si los subprocesos de trabajo no están disponibles para procesar las solicitudes.|  
|ONLINE|El recurso de grupo de disponibilidad está en línea, y todos los subprocesos de trabajo de la base de datos se han seleccionado.|  
|FAILED|La réplica de disponibilidad no puede leer ni escribir en el clúster de WSFC.|  
  
 **Secundario:** Cuando una réplica de disponibilidad está realizando el rol secundario, es actualmente una réplica secundaria. Los Estados operativos posibles son los que se muestran en la tabla siguiente.  
  
|Estado de funcionamiento|Descripción|  
|-----------------------|-----------------|  
|ONLINE|La réplica secundaria local no está conectada a la réplica principal.|  
|FAILED|La réplica secundaria local no puede leer ni escribir en el clúster de WSFC.|  
|NULL|En una réplica principal, se devuelve este valor cuando la fila está relacionada con una réplica secundaria.|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
