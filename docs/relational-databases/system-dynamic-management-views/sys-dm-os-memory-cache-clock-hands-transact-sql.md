---
description: sys.dm_os_memory_cache_clock_hands (Transact-SQL)
title: sys.dm_os_memory_cache_clock_hands (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/21/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_cache_clock_hands_TSQL
- dm_os_memory_cache_clock_hands
- dm_os_memory_cache_clock_hands_TSQL
- sys.dm_os_memory_cache_clock_hands
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_clock_hands dynamic management view
ms.assetid: 0660eddc-691c-425f-9d43-71151d644de7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 200ec798e30679f69acb00b1ea66135fc7f3cb6e
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342840"
---
# <a name="sysdm_os_memory_cache_clock_hands-transact-sql"></a>sys.dm_os_memory_cache_clock_hands (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el estado de cada manecilla de un reloj de caché específico.  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_memory_cache_clock_hands**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8**|Dirección de la caché asociada al reloj. No admite valores NULL.|  
|**name**|**nvarchar(256)**|Nombre de la caché. No admite valores NULL.|  
|**type**|**nvarchar(60)**|Tipo de almacén de la caché. Pueden existir varias cachés del mismo tipo. No admite valores NULL.|  
|**clock_hand**|**nvarchar(60)**|Tipo de manecilla. Es uno de los siguientes:<br /><br /> Externo<br /><br /> Interno<br /><br /> No admite valores NULL.|  
|**clock_status**|**nvarchar(60)**|Estado del reloj. Es uno de los siguientes:<br /><br /> Suspended<br /><br /> En ejecución<br /><br /> No admite valores NULL.|  
|**rounds_count**|**bigint**|Número de rastreos realizados en toda la caché para eliminar entradas. No admite valores NULL.|  
|**removed_all_rounds_count**|**bigint**|Número de entradas quitadas por todos los rastreos. No admite valores NULL.|  
|**updated_last_round_count**|**bigint**|Número de entradas actualizadas durante el último rastreo. No admite valores NULL.|  
|**removed_last_round_count**|**bigint**|Número de entradas quitadas durante el último rastreo. No admite valores NULL.|  
|**last_tick_time**|**bigint**|Última vez, en milisegundos, que se movió la manecilla del reloj. No admite valores NULL.|  
|**round_start_time**|**bigint**|Tiempo, en milisegundos, del rastreo anterior. No admite valores NULL.|  
|**last_round_start_time**|**bigint**|Tiempo total, en milisegundos, que ha tardado el reloj en completar el ciclo anterior. No admite valores NULL.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos  

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="remarks"></a>Comentarios  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] almacena información en memoria en una estructura denominada caché en memoria. La información en la caché pueden ser datos, entradas de índices, planes de procedimientos compilados y diversos tipos de información de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para evitar tener que volver a crear la información, ésta se mantiene en la caché de memoria mientras sea posible y, normalmente, se quita de la caché cuando es demasiado antigua para ser útil o cuando se necesita espacio en la memoria para nueva información. El proceso que quita la información antigua se denomina rastreo de memoria. El rastreo de memoria es una actividad frecuente, pero no continua. Un algoritmo de reloj controla el rastreo de la caché de memoria. Cada reloj puede controlar varios rastreos de memoria, que se denominan manecillas. La manecilla del reloj de la caché de memoria es la ubicación actual de una de las manecillas de un rastreo de memoria.  

## <a name="see-also"></a>Consulte también  
 [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)    
 [sys.dm_os_memory_cache_counters &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-counters-transact-sql.md)
  

