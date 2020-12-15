---
description: sys.dm_os_nodes (Transact-SQL)
title: sys.dm_os_nodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/13/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_nodes
- dm_os_nodes_TSQL
- dm_os_nodes
- sys.dm_os_nodes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_nodes dynamic management view
ms.assetid: c768b67c-82a4-47f5-850b-0ea282358d50
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 709ef05df1b770cc43201df974a907a49e19f38b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97411896"
---
# <a name="sysdm_os_nodes-transact-sql"></a>sys.dm_os_nodes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Un componente interno denominado SQLOS crea las estructuras de nodo que imitan el procesador de hardware. Estas estructuras se pueden cambiar mediante [el uso de Soft-Numa](../../database-engine/configure-windows/soft-numa-sql-server.md) para crear diseños de nodo personalizados.  

> [!NOTE]
> A partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] , el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] usará automáticamente Soft-Numa para ciertas configuraciones de hardware. Para obtener más información, consulte [Automatic Soft-Numa](../../database-engine/configure-windows/soft-numa-sql-server.md#automatic-soft-numa).
  
En la tabla siguiente se proporciona información acerca de estos nodos.  
  
> [!NOTE]
> Para llamar a esta DMV desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_nodes**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|node_id|**smallint**|Identificador del nodo.|  
|node_state_desc|**nvarchar(256)**|Descripción del estado del nodo. Los valores se muestran primero con los valores mutuamente exclusivos, seguidos de los valores combinables. Por ejemplo:<br /> Online, Thread Resources Low, Lazy Preemptive<br /><br />Hay cuatro valores node_state_desc mutuamente excluyentes. Se enumeran a continuación con sus descripciones.<br /><ul><li>EN línea: el nodo está en línea<li>SIN conexión: el nodo está sin conexión<li>IDLE: el nodo no tiene ninguna solicitud de trabajo pendiente y ha entrado en un estado de inactividad.<li>IDLE_READY: el nodo no tiene ninguna solicitud de trabajo pendiente y está listo para entrar en un estado de inactividad.</li></ul><br />Hay tres valores node_state_desc combinables, que se enumeran a continuación con sus descripciones.<br /><ul><li>DAC: este nodo está reservado para la [conexión administrativa dedicada](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md).<li>THREAD_RESOURCES_LOW: no se pueden crear subprocesos nuevos en este nodo debido a una condición de memoria insuficiente.<li>AGREGADO en caliente: indica que los nodos se agregaron como respuesta a un evento de CPU de adición activa.</li></ul>|  
|memory_object_address|**varbinary(8**|Dirección del objeto de memoria asociada con este nodo. Relación uno a uno con [sys.dm_os_memory_objects](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).memory_object_address.|  
|memory_clerk_address|**varbinary(8**|Dirección de distribuidor de memoria asociada con este nodo. Relación uno a uno con [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).memory_clerk_address.|  
|io_completion_worker_address|**varbinary(8**|La dirección del trabajador asignada a la realización de E/S para este nodo. Relación uno a uno con [sys.dm_os_workers](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).worker_address.|  
|memory_node_id|**smallint**|Identificador del nodo de memoria al que pertenece este nodo. Relación de varios a uno con [sys.dm_os_memory_nodes](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql.md).memory_node_id.|  
|cpu_affinity_mask|**bigint**|Mapa de bits que identifica las CPU con las que este nodo está asociado.|  
|online_scheduler_count|**smallint**|Número de programadores en línea administrados por este nodo.|  
|idle_scheduler_count|**smallint**|Número de programadores en línea que no tienen ningún trabajador activo.|  
|active_worker_count|**int**|Número de trabajadores que están activos en todos los programadores administrados por este nodo.|  
|avg_load_balance|**int**|Promedio de tareas por programador en este nodo.|  
|timer_task_affinity_mask|**bigint**|Mapa de bits que identifica los programadores que pueden tener asignadas tareas de temporizador.|  
|permanent_task_affinity_mask|**bigint**|Mapa de bits que identifica los programadores que pueden tener asignadas tareas permanentes.|  
|resource_monitor_state|**bit**|Cada nodo tiene asignado un monitor de recursos. El monitor de recursos puede estar en ejecución o inactivo. El valor 1 indica en ejecución y 0 inactividad.|  
|online_scheduler_mask|**bigint**|Identifica la máscara de afinidad de proceso para este nodo.|  
|processor_group|**smallint**|Identifica el grupo de procesadores para este nodo.|  
|cpu_count |**int** |Número de CPU disponibles para este nodo. |
|pdw_node_id|**int**|Identificador del nodo en el que se encuentra esta distribución.<br /><br /> **Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="see-also"></a>Consulte también    
 [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)  
  
