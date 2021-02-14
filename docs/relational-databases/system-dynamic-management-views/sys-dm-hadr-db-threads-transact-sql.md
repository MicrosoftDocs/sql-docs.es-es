---
description: sys.dm_hadr_db_threads (Transact-SQL)
title: sys.dm_hadr_db_threads (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/08/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_db_threads_TSQL
- sys.dm_hadr_db_threads
- dm_hadr_db_threads_TSQL
- dm_hadr_db_threads
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_db_threads catalog view
ms.assetid: ''
author: kfarlee
ms.author: kfarlee
monikerRange: ''
ms.openlocfilehash: 7637c5b2acb302c4af8960161fb5a687ff7a02d0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100338842"
---
# <a name="sysdm_hadr_db_threads-transact-sql"></a>sys.dm_hadr_db_threads (Transact-SQL)

Las DMV de telemetría de subprocesos HADR (**Sys.dm_hadr_db_threads** y [Sys.dm_hadr_ag_threads](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-ag-threads-transact-sql.md)) permiten a los usuarios obtener rápidamente información sobre el uso de subprocesos por grupo de disponibilidad y base de datos de alta disponibilidad. Entender este uso de subprocesos es una prueba comparativa importante para optimizar los grupos de disponibilidad.

Esta DMV informa del uso de subprocesos en el nivel de base de datos del grupo de disponibilidad.

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificador del grupo de disponibilidad al que pertenece la base de datos.|
|**ag_db_id**|**uniqueidentifier**|Identificador de la base de datos dentro del grupo de disponibilidad. Este identificador es idéntico en cada réplica al que está unido esta base de datos.|
|**name**|**nvarchar(128)**|Nombre de la base de datos.|
|**num_capture_threads**|**int**|Número de subprocesos de captura de registro para esta base de datos.|
|**num_redo_threads**|**int**|Número de subprocesos de puesta al día para esta base de datos.|
|**num_parallel_redo_threads**|**int**|Número de subprocesos de puesta al día en paralelo para esta base de datos.|

## <a name="permissions"></a>Permisos  

 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también

 [Always On vistas y funciones de administración dinámica de grupos de disponibilidad &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Vistas de catálogo de grupos de disponibilidad AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  