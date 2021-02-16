---
title: Identificación de las esperas asociadas a grupos de disponibilidad
description: Identifique las esperas asociadas a los grupos de disponibilidad Always On mediante Transact-SQL (T-SQL) y eventos extendidos.
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: afa8caff-f325-48d9-a8ef-a30beab60389
author: rothja
ms.author: jroth
ms.openlocfilehash: 4be170149181263e7d904f5f820a9dd50f6d3801
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347488"
---
# <a name="identify-waits-associated-with-availability-groups"></a>Identificación de las esperas asociadas a grupos de disponibilidad
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cuando solucione problemas de latencia de grupos de disponibilidad Always On, puede supervisar las estadísticas de espera de acumulación utilizando los tipos de espera específicos de grupos de disponibilidad en la vista de administración dinámica (DMV) [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).  
  
 Para obtener información general sobre el uso de las estadísticas de espera, vea [SQL Server 2005 Waits and Queues](/previous-versions/sql/sql-server-2005/administrator/cc966413(v=technet.10)) (Esperas y colas de SQL Server 2005). Dicho documento se escribió para SQL Server 2005, pero su información se puede aplicar a versiones posteriores de SQL Server.  
  
## <a name="query-for-availability-groups-wait-types"></a>Consulta para tipos de espera de grupos de disponibilidad  
 Use la siguiente consulta de T-SQL para recuperar todas las estadísticas de espera con los tipos de espera de grupos de disponibilidad:  
  
```sql  
SELECT * FROM sys.dm_os_wait_stats   
WHERE wait_type LIKE '%hadr%'  
ORDER BY wait_time_ms DESC  
```  
  
 Para supervisar las estadísticas de espera capturando los eventos extendidos, utilice el siguiente comando T-SQL.  
  
```sql
CREATE EVENT SESSION [alwayson] ON SERVER   
ADD EVENT sqlos.wait_info(  
    WHERE ([wait_type]=(758) OR [wait_type]=(776) OR [wait_type]=(853) OR [wait_type]=(833)))  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,  
MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)  
GO  
```  
  
 Puede ver la asignación de clave-valor del tipo de espera mediante la ejecución de la consulta siguiente:  
  
```sql
SELECT * FROM sys.dm_xe_map_values   
WHERE name='wait_types' AND map_value LIKE '%hadr%'   
ORDER BY map_key ASC  
```  
  
## <a name="next-steps"></a>Pasos siguientes  
 [Tipos de esperas](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md#WaitTypes)  
  
