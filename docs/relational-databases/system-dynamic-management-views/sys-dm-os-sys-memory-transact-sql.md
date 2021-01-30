---
description: sys.dm_os_sys_memory (Transact-SQL)
title: sys.dm_os_sys_memory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_sys_memory
- sys.dm_os_sys_memory_TSQL
- sys.dm_os_sys_memory
- dm_os_sys_memory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_sys_memory dynamic management view
ms.assetid: 1ca58814-1caa-44c1-b307-ff0bdcbbef62
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9618f225fb5345a40b940470a875c25bc69f5de6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190103"
---
# <a name="sysdm_os_sys_memory-transact-sql"></a>sys.dm_os_sys_memory (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Devuelve información sobre la memoria del sistema operativo.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está limitado por, y responde ante, las condiciones externas de la memoria en el sistema operativo y los límites físicos del hardware subyacente. Determinar el estado general del sistema es una parte importante de la evaluación del uso de la memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_sys_memory**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**total_physical_memory_kb**|**bigint**|Tamaño total de la memoria física disponible para el sistema operativo, en kilobytes (KB).|  
|**available_physical_memory_kb**|**bigint**|Tamaño de la memoria física disponible, en KB.|  
|**total_page_file_kb**|**bigint**|Tamaño del límite de confirmación indicado por el sistema operativo, en KB|  
|**available_page_file_kb**|**bigint**|Cantidad total de archivo de paginación que no se usa, en KB.|  
|**system_cache_kb**|**bigint**|Cantidad total de la memoria caché del sistema, en KB.|  
|**kernel_paged_pool_kb**|**bigint**|Cantidad total del bloque de kernel paginado, en KB.|  
|**kernel_nonpaged_pool_kb**|**bigint**|Cantidad total del bloque de kernel no paginado, en KB.|  
|**system_high_memory_signal_state**|**bit**|Estado de la notificación de recursos de memoria alta del sistema. Un valor de 1 indica que Windows ha establecido la señal de memoria alta. Para obtener más información, vea [CreateMemoryResourceNotification](/windows/win32/api/memoryapi/nf-memoryapi-creatememoryresourcenotification) en MSDN Library.|  
|**system_low_memory_signal_state**|**bit**|Estado de la notificación de recursos de memoria baja del sistema. Un valor de 1 indica que Windows ha establecido la señal de memoria baja. Para obtener más información, vea [CreateMemoryResourceNotification](/windows/win32/api/memoryapi/nf-memoryapi-creatememoryresourcenotification) en MSDN Library.|  
|**system_memory_state_desc**|**nvarchar(256)**|Descripción del estado de la memoria. Vea la tabla siguiente.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
|Condición|Valor|  
|---------------|-----------|  
|system_high_memory_signal_state = 1<br /><br /> y<br /><br /> system_low_memory_signal_state = 0|La memoria física disponible es alta.|  
|system_high_memory_signal_state = 0<br /><br /> y<br /><br /> system_low_memory_signal_state = 1|La memoria física disponible es baja.|  
|system_high_memory_signal_state = 0<br /><br /> y<br /><br /> system_low_memory_signal_state = 0|El uso de la memoria física es continuo|  
|system_high_memory_signal_state = 1<br /><br /> y<br /><br /> system_low_memory_signal_state = 1|El estado de la memoria física está cambiando<br /><br /> Las señales alta y baja nunca deben estar activas de manera simultánea. Sin embargo, los cambios rápidos que se realizan en el sistema operativo pueden hacer que ambos valores parezcan estar activos en una aplicación de modo usuario. El que ambas señales aparezcan activas se interpretará como un estado de transición.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
