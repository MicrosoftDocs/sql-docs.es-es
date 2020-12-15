---
description: sys.dm_os_process_memory (Transact-SQL)
title: sys.dm_os_process_memory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_process_memory_TSQL
- dm_os_process_memory_TSQL
- dm_os_process_memory
- sys.dm_os_process_memory
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_process_memory dynamic management view
ms.assetid: e838130c-95d4-4605-9e3b-eb0ab71cd250
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a1a15d39d155e82523adb837810dc5dd276ff91c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97411766"
---
# <a name="sysdm_os_process_memory-transact-sql"></a>sys.dm_os_process_memory (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  La mayoría de las asignaciones de memoria que se atribuyen al espacio de proceso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se controlan a través de interfaces que permiten realizar el seguimiento y las estimaciones de esas asignaciones. Sin embargo, las asignaciones de memoria se puede realizar en el espacio de direcciones [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que omite las rutinas de administración de memoria interna. Los valores se obtienen a través de las llamadas al sistema operativo base. No se manipulan mediante métodos internos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , excepto cuando se ajusta a las asignaciones de páginas bloqueadas o grandes.  
  
 Todos los valores devueltos que indican tamaños de memoria se muestran en kilobytes (KB). La columna **total_virtual_address_space_reserved_kb** es un duplicado de **virtual_memory_in_bytes** de **Sys.dm_os_sys_info**.  
  
 La tabla siguiente proporciona una imagen completa del espacio de direcciones del proceso.  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_process_memory**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**physical_memory_in_use_kb**|**bigint**|Indica el conjunto de trabajo de procesos, en KB, tal y como informa el sistema operativo, así como las asignaciones realizadas utilizando las API de página grande. No acepta valores NULL.|  
|**large_page_allocations_kb**|**bigint**|Especifica la memoria física asignada por medio de las API de página grande. No acepta valores NULL.|  
|**locked_page_allocations_kb**|**bigint**|Especifica las páginas de memoria bloqueadas en memoria. No acepta valores NULL.|  
|**total_virtual_address_space_kb**|**bigint**|Indica el tamaño total de la parte del modo usuario del espacio de direcciones virtuales. No acepta valores NULL.|  
|**virtual_address_space_reserved_kb**|**bigint**|Indica la cantidad total de espacio de direcciones virtuales reservada por el proceso. No acepta valores NULL.|  
|**virtual_address_space_committed_kb**|**bigint**|Indica la cantidad de espacio de direcciones virtuales reservada que se ha confirmado o asignado a las páginas físicas. No acepta valores NULL.|  
|**virtual_address_space_available_kb**|**bigint**|Indica la cantidad de espacio de direcciones virtuales que está actualmente libre. No acepta valores NULL.<br /><br /> **Nota:** Pueden existir regiones libres más pequeñas que la granularidad de la asignación. Estas regiones no están disponibles para las asignaciones.|  
|**page_fault_count**|**bigint**|Indica el número de errores de página en los que incurre el proceso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No acepta valores NULL.|  
|**memory_utilization_percentage**|**int**|Especifica el porcentaje de memoria confirmada que se encuentra en el conjunto de trabajo. No acepta valores NULL.|  
|**available_commit_limit_kb**|**bigint**|Indica la cantidad de memoria que está disponible para la confirmación por parte del proceso. No acepta valores NULL.|  
|**process_physical_memory_low**|**bit**|Indica que el proceso responde a una notificación de memoria física baja. No acepta valores NULL.|  
|**process_virtual_memory_low**|**bit**|Indica que se ha detectado una condición de memoria virtual baja. No acepta valores NULL.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] necesita el permiso VER ESTADO DEL SERVIDOR en el servidor.  
  
En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


