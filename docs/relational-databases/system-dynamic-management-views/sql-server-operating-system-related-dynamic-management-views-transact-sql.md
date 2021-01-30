---
description: Vistas de administración dinámica relacionadas con el sistema operativo de SQL Server (Transact-SQL)
title: SQL Server vistas de administración dinámica relacionadas con el sistema operativo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/17/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- operating systems [SQL Server], dynamic management objects
- SQL Operating System dynamic management objects [SQL Server]
- SQL OS dynamic management objects [SQL Server]
- dynamic management objects [SQL Server], SQL OS
ms.assetid: 3030c86a-0a74-4fed-ac0f-392e244cb965
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5dcc428b696009b15578e0b1813dcbc2380a299f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202477"
---
# <a name="sql-server-operating-system-related-dynamic-management-views-transact-sql"></a>Vistas de administración dinámica relacionadas con el sistema operativo de SQL Server (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En esta sección se documentan las vistas de administración dinámica (DMV) que están asociadas con el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sistema operativo (SQLOS). SQLOS es responsable de administrar los recursos del sistema operativo específicos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .


|  |  |
|---------|---------|
|**sys.dm_os_buffer_descriptors** | **sys.dm_os_buffer_pool_extension_configuration**|
|**sys.dm_os_child_instances** | **sys.dm_os_cluster_nodes** |
|**sys.dm_os_cluster_properties** | **sys.dm_os_dispatcher_pools** |
|**sys.dm_os_enumerate_fixed_drives** | **sys.dm_os_host_info** |
|**sys.dm_os_hosts** | **sys.dm_os_latch_stats** |
|**sys.dm_os_loaded_modules** |**sys.dm_os_memory_brokers**|
|**sys.dm_os_memory_cache_clock_hands**|**sys.dm_os_memory_cache_counters** |
|**sys.dm_os_memory_cache_entries**|**sys.dm_os_memory_cache_hash_tables**|
|**sys.dm_os_memory_clerks**|**sys.dm_os_memory_nodes**|
|**sys.dm_os_nodes**|**sys.dm_os_performance_counters**|
|**sys.dm_os_process_memory**|**sys.dm_os_schedulers**|
|**sys.dm_os_server_diagnostics_log_configurations**|**sys.dm_os_spinlock_stats** |
|**sys.dm_os_stacks**|**sys.dm_os_sys_info**|
|**sys.dm_os_sys_memory**|**sys.dm_os_tasks**|
|**sys.dm_os_threads** |**sys.dm_os_virtual_address_dump**|
|**sys.dm_os_volume_stats**|**sys.dm_os_waiting_tasks**|
|**sys.dm_os_wait_stats**|**sys.dm_os_windows_info**|
|**sys.dm_os_workers** ||








 Las [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vistas de administración dinámica relacionadas con el sistema operativo son las [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] siguientes:  
  
|||  
|-|-|  
|**sys.dm_os_function_symbolic_name**|**sys.dm_os_ring_buffers**|  
|**sys.dm_os_memory_allocations**|**sys.dm_os_sublatches**|  
|**sys.dm_os_worker_local_storage**||  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

