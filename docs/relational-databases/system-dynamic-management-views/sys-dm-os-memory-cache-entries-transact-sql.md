---
description: sys.dm_os_memory_cache_entries (Transact-SQL)
title: sys.dm_os_memory_cache_entries (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_cache_entries
- sys.dm_os_memory_cache_entries
- dm_os_memory_cache_entries_TSQL
- sys.dm_os_memory_cache_entries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_entries dynamic management view
ms.assetid: dd32be6b-10d1-4059-b4fd-0bf817f40d54
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4a274e7bc724cd838373b465029a3a02c4dfc645
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184832"
---
# <a name="sysdm_os_memory_cache_entries-transact-sql"></a>sys.dm_os_memory_cache_entries (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de todas las entradas en las memorias caché de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilice esta vista para realizar el seguimiento de las entradas de memoria caché con sus objetos asociados. También puede utilizarla para obtener estadísticas sobre entradas de caché.  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_memory_cache_entries**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8**|Dirección de la caché. No admite valores NULL.|  
|**name**|**nvarchar(256)**|Nombre de la caché. No admite valores NULL.|  
|**type**|**varchar(60)**|Tipo de caché. No admite valores NULL.|  
|**entry_address**|**varbinary(8**|Dirección del descriptor de la entrada de caché. No admite valores NULL.|  
|**entry_data_address**|**varbinary(8**|Dirección de los datos de usuario en la entrada de caché.<br /><br /> 0x00000000 = No está disponible la dirección de datos de entrada.<br /><br /> No admite valores NULL.|  
|**in_use_count**|**int**|Número de usuarios simultáneos de esta entrada de caché. No admite valores NULL.|  
|**is_dirty**|**bit**|Indica si esta entrada de caché está marcada para su eliminación. 1 = marcado para eliminación. No admite valores NULL.|  
|**disk_ios_count**|**int**|Número de operaciones de E/S producidas al crearse esta entrada. No admite valores NULL.|  
|**context_switches_count**|**int**|Número de cambios de contexto producidos al crearse esta entrada. No admite valores NULL.|  
|**original_cost**|**int**|Costo original de la entrada. Este valor es una aproximación del número de operaciones de E/S producidas, el costo de instrucciones de CPU y la cantidad de memoria consumida por entrada. Cuanto mayor sea el costo, menor será la probabilidad de que se quite el elemento de la memoria caché. No admite valores NULL.|  
|**current_cost**|**int**|Costo actual de la entrada de caché. Este valor se actualiza durante el purgado de entradas. El costo actual se restablece a su valor original cuando se vuelve a utilizar la entrada. No admite valores NULL.|  
|**memory_object_address**|**varbinary(8**|Dirección del objeto de memoria asociado. Acepta valores NULL.|  
|**pages_allocated_count**|**bigint**|**Se aplica a**: desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Número de páginas de 8 KB que almacenan esta entrada de caché. No admite valores NULL.|  
|**pages_kb**|**bigint**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Cantidad de memoria en kilobytes (KB) que esta entrada de caché usa.  No admite valores NULL.|  
|**entry_data**|**nvarchar(2048)**|Representación en serie de la entrada de caché. Esta información es dependiente del almacén de caché. Acepta valores NULL.|  
|**pool_id**|**int**|**Válido para** : [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores.<br /><br /> Identificador del grupo de recursos de servidor asociado a la entrada. Acepta valores NULL.<br /><br /> no katmai|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos 

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="see-also"></a>Consulte también  
 
  [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


