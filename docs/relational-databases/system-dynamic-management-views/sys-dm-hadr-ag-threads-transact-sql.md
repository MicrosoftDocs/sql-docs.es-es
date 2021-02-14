---
description: sys.dm_hadr_ag_threads (Transact-SQL)
title: sys.dm_hadr_ag_threads (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/08/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_ag_threads_TSQL
- sys.dm_hadr_ag_threads
- dm_hadr_ag_threads_TSQL
- dm_hadr_ag_threads
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_ag_threads catalog view
ms.assetid: ''
author: kfarlee
ms.author: kfarlee
monikerRange: ''
ms.openlocfilehash: c8487dd07e424b7edccc17b8a9587cfc91f74268
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100354129"
---
# <a name="sysdm_hadr_ag_threads-transact-sql"></a>sys.dm_hadr_ag_threads (Transact-SQL)

Las DMV de telemetría de subprocesos HADR (**Sys.dm_hadr_ag_threads** y [Sys.dm_hadr_db_threads](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-db-threads-transact-sql.md)) permiten a los usuarios obtener rápidamente información sobre el uso de subprocesos por grupo de disponibilidad y base de datos de alta disponibilidad. Entender este uso de subprocesos es una prueba comparativa importante para optimizar los grupos de disponibilidad.

Esta DMV informa del uso de subprocesos en el nivel de grupo de disponibilidad.

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificador del grupo de disponibilidad.|
|**name**|**nvarchar(128)**|Nombre del grupo de disponibilidad.|
|**num_databases**|**int**|Número de bases de datos del grupo de disponibilidad.|
|**num_capture_threads**|**int**|Número de subprocesos de captura de registro utilizados por todas las bases de datos de este grupo de disponibilidad.|
|**num_redo_threads**|**int**|Número de subprocesos de rehacer utilizados por todas las bases de datos de este grupo de disponibilidad.|
|**num_parallel_redo_threads**|**int**|Número de subprocesos de puesta al día en paralelo utilizados por todas las bases de datos de este grupo de disponibilidad.|
|**num_hadr_threads**|**int**|Número de todos los subprocesos de AlwaysOn utilizados por todas las bases de datos de este grupo de disponibilidad.|

## <a name="permissions"></a>Permisos  

 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  

 [Always On vistas y funciones de administración dinámica de grupos de disponibilidad &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Vistas de catálogo de grupos de disponibilidad AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  