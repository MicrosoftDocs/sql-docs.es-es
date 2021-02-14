---
description: sys.dm_os_memory_cache_counters (Transact-SQL)
title: sys.dm_os_memory_cache_counters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_cache_counters_TSQL
- dm_os_memory_cache_counters_TSQL
- sys.dm_os_memory_cache_counters
- dm_os_memory_cache_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_counters dynamic management view
ms.assetid: ca7bd036-d661-4c17-b00a-e1a975bd8932
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d81dae0e4092a9f928d31123d07ddbd5cb48a64b
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342771"
---
# <a name="sysdm_os_memory_cache_counters-transact-sql"></a>sys.dm_os_memory_cache_counters (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una instantánea del estado de una memoria caché en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **Sys.dm_os_memory_cache_counters** proporciona información en tiempo de ejecución acerca de las entradas de caché asignadas, su uso y el origen de la memoria de las entradas de la memoria caché.  
  
> **Nota:** Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_memory_cache_counters**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8**|Indica la dirección (clave principal) de los recuentos asociados con una memoria caché específica. No admite valores NULL.|  
|**name**|**nvarchar(256)**|Especifica el nombre de la memoria caché. No admite valores NULL.|  
|**type**|**nvarchar(60)**|Indica el tipo de memoria caché asociado con esta entrada. No admite valores NULL.|  
|**single_pages_kb**|**bigint**|**Se aplica a**: desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Cantidad, en kilobytes, de memoria de página única asignada. Se trata de la cantidad de memoria asignada mediante el asignador de página única. Se refiere a las páginas de 8 KB tomadas directamente del grupo de búferes para esta caché. No admite valores NULL.|  
|**pages_kb**|**bigint**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Especifica la cantidad, en kilobytes, de la memoria asignada a la memoria caché. No admite valores NULL.|  
|**multi_pages_kb**|**bigint**|**Se aplica a**: desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Cantidad, en kilobytes, de memoria de varias páginas asignada. Es la cantidad de memoria asignada mediante el asignador de varias páginas del nodo de memoria. Esta memoria se asigna fuera del grupo de búferes y aprovecha las ventajas del asignador virtual de los nodos de memoria. No admite valores NULL.|  
|**pages_in_use_kb**|**bigint**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Especifica la cantidad, en kilobytes, de la memoria asignada que está en uso en la memoria caché. Acepta valores NULL.  No se realiza el seguimiento de los valores de objetos de tipo `USERSTORE_<*>`.  Para esos valores se notifica NULL.|  
|**single_pages_in_use_kb**|**bigint**|**Se aplica a**: desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Cantidad, en kilobytes, de memoria de página única utilizada. Acepta valores NULL. No se realiza un seguimiento de esta información para objetos de tipo USERSTORE_ \<*> y estos valores serán null.|  
|**multi_pages_in_use_kb**|**bigint**|**Se aplica a**: desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Cantidad, en kilobytes, de la memoria de varias páginas utilizada. Acepta valores NULL. No se realiza un seguimiento de esta información para objetos de tipo USERSTORE_ \<*> , y estos valores serán null.|  
|**entries_count**|**bigint**|Indica el número de entradas que hay en la memoria caché. No admite valores NULL.|  
|**entries_in_use_count**|**bigint**|Indica el número de entradas que hay en la memoria caché que se está usando. No admite valores NULL.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos 

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="see-also"></a>Consulte también  
  [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


