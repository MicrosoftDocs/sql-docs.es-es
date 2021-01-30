---
description: sys.dm_hadr_database_replica_cluster_states (Transact-SQL)
title: sys.dm_hadr_database_replica_cluster_states (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_database_replica_cluster_states
- dm_hadr_database_replica_cluster_states_TSQL
- sys.dm_hadr_database_replica_cluster_states_TSQL
- dm_hadr_database_replica_cluster_states
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_database_replica_cluster_states dynamic management view
ms.assetid: 6f719071-ebce-470d-aebd-1f55ee8cd70a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9b96476a0b9b089fa82b3291f783782359e60636
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182075"
---
# <a name="sysdm_hadr_database_replica_cluster_states-transact-sql"></a>sys.dm_hadr_database_replica_cluster_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila que contiene información destinada a proporcionar una visión general del estado de las bases de datos de disponibilidad de los Grupos de disponibilidad AlwaysOn de cada uno de los grupos de disponibilidad AlwaysOn del clúster del servicio de clústeres de conmutación por error de Windows Server (WSFC). Consulte **Sys.dm_hadr_database_replica_states** para responder a las siguientes preguntas:  
  
-   ¿Están todas las bases de datos de un grupo de disponibilidad listas para una conmutación por error?  
  
-   Después de una conmutación por error forzada, ¿se ha suspendido localmente una base de datos secundaria y se ha confirmado su estado suspendido en la nueva réplica principal?  
  
-   Si la réplica principal no está disponible actualmente, ¿qué réplica secundaria permitiría una pérdida de datos mínima si se convierte en la réplica principal?  
  
-   Cuando el valor de la columna [Sys. databases](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)   **log_reuse_wait_desc** es "AVAILABILITY_REPLICA", ¿qué réplica secundaria de un grupo de disponibilidad está manteniendo el truncamiento del registro en una base de datos principal determinada?  
   
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|Identificador de la réplica de disponibilidad dentro del grupo de disponibilidad.|  
|**group_database_id**|**uniqueidentifier**|Identificador de la base de datos dentro del grupo de disponibilidad. Este identificador es idéntico en cada réplica al que está unido esta base de datos.|  
|**database_name**|**sysname**|Nombre de la base de datos que pertenece al grupo de disponibilidad.|  
|**is_failover_ready**|**bit**|Indica si la base de datos secundaria está sincronizada con la base de datos principal correspondiente. uno de los siguientes:<br /><br /> 0 = La base de datos no está marcada como sincronizada en el clúster. La base de datos no está lista para una conmutación por error.<br /><br /> 1 = La base de datos está marcada como sincronizada en el clúster. La base de datos está lista para una conmutación por error.|  
|**is_pending_secondary_suspend**|**bit**|Indica si, después de una conmutación por error forzada, la base de datos está pendiente de suspensión pendiente; puede ser uno de los siguientes:<br /><br /> 0 = Cualquier estado a excepción de HADR_SYNCHRONIZED_ SUSPENDED.<br /><br /> 1 = HADR_SYNCHRONIZED_ SUSPENDED. Cuando una conmutación por error forzada se completa, cada una de las bases de datos secundarias se establece en HADR_SYNCHONIZED_SUSPENDED y permanece en este estado hasta que la nueva réplica principal recibe confirmación de esa base de datos secundaria en el mensaje SUSPEND.<br /><br /> NULL = Desconocido (sin quórum)|  
|**is_database_joined**|**bit**|Indica si la base de datos de esta réplica de disponibilidad se ha unido al grupo de disponibilidad; puede ser uno de los siguientes:<br /><br /> 0 = La base de datos no se ha unido al grupo de disponibilidad en esta réplica de disponibilidad.<br /><br /> 1 = La base de datos se ha unido al grupo de disponibilidad en esta réplica de disponibilidad.<br /><br /> NULL = Desconocido (la réplica de disponibilidad no tiene quórum).|  
|**recovery_lsn**|**numeric(25,0)**|En la réplica principal, el final del registro de transacciones antes de que la réplica escriba nuevas entradas de registro después de la recuperación o la conmutación por error. En la réplica principal, la fila de una base de datos secundaria dada tendrá el valor que la réplica principal necesita para la sincronización con la réplica secundaria (es decir, para la reversión y la reinicialización).<br /><br /> En las réplicas secundarias este valor es NULL. Tenga en cuenta que cada réplica secundaria tendrá el valor MAX o un valor inferior al que la réplica principal ha notificado a la réplica secundaria que se va a volver.|  
|**truncation_lsn**|**numeric(25,0)**|El valor de truncamiento de registro de [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], que puede ser más alto que el LSN de truncamiento local si el truncamiento de registro local está bloqueado (como mediante una operación de copia de seguridad).|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Always On vistas y funciones de administración dinámica de grupos de disponibilidad &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Vistas de catálogo de grupos de disponibilidad AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
  
