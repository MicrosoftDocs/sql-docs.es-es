---
description: sys.dm_hadr_availability_group_states (Transact-SQL)
title: sys.dm_hadr_availability_group_states (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_availability_group_states
- sys.dm_hadr_availability_group_states_TSQL
- dm_hadr_availability_group_states_TSQL
- dm_hadr_availability_group_states
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_availability_group_states dynamic management view
ms.assetid: d18019dd-f8dc-4492-b035-b1a639369b65
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ee24a962698e2a661b4859c846a748a5b8517e7d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354167"
---
# <a name="sysdm_hadr_availability_group_states-transact-sql"></a>sys.dm_hadr_availability_group_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila para cada Always On grupo de disponibilidad que posee una réplica de disponibilidad en la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cada fila muestra los estados que definen el estado de un grupo de disponibilidad determinado.  
  
> [!NOTE]  
>  Para obtener la lista completa de, consulte la vista de catálogo [Sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificador único del grupo de disponibilidad.|  
|**primary_replica**|**varchar(128)**|Nombre de la instancia de servidor que hospeda la réplica principal actual.<br /><br /> NULL = No es la réplica principal no se puede comunicar con el clúster de conmutación por error de WSFC.|  
|**primary_recovery_health**|**tinyint**|Indica el estado de recuperación de la réplica principal; puede ser uno de los siguientes:<br /><br /> 0 = en curso<br /><br /> 1 = En línea<br /><br /> NULL<br /><br /> En las réplicas secundarias, la **primary_recovery_health** columna es NULL.|  
|**primary_recovery_health_desc**|**nvarchar(60)**|Descripción de **primary_replica_health**, uno de los siguientes:<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**secondary_recovery_health**|**tinyint**|Indica el estado de recuperación de una réplica de réplica secundaria, uno de los siguientes:<br /><br /> 0 = en curso<br /><br /> 1 = En línea<br /><br /> NULL<br /><br /> En la réplica principal, el **secondary_recovery_health** columna es NULL.|  
|**secondary_recovery_health_desc**|**nvarchar(60)**|Descripción de **secondary_recovery_health**, uno de los siguientes:<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**synchronization_health**|**tinyint**|Refleja un resumen del **synchronization_health** de todas las réplicas de disponibilidad en el grupo de disponibilidad. A continuación se muestran los valores posibles y sus descripciones.<br /><br /> 0: no es correcto. Ninguna de las réplicas de disponibilidad tiene un **synchronization_health** correcto (2 = correcto).<br /><br /> 1: parcialmente correcto. El estado de sincronización de algunas réplicas de disponibilidad, pero no de todas, es correcto.<br /><br /> 2: correcto. El estado de sincronización de todas las réplicas de disponibilidad es correcto.<br /><br /> Para obtener información sobre el estado de sincronización de réplicas, vea la columna **synchronization_health** en [Sys.dm_hadr_availability_replica_states &#40;&#41;de Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md).|  
|**synchronization_health_desc**|**nvarchar(60)**|Descripción de **synchronization_health**, uno de los siguientes:<br /><br /> NOT_HEALTHY<br /><br /> PARTIALLY_HEALTHY<br /><br /> HEALTHY|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Funciones y vistas de administración dinámica de grupos de disponibilidad AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)  
  
  
