---
description: sys.dm_fts_memory_buffers (Transact-SQL)
title: sys.dm_fts_memory_buffers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_memory_buffers
- dm_fts_memory_buffers_TSQL
- dm_fts_memory_buffers
- sys.dm_fts_memory_buffers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_memory_buffers dynamic management view
ms.assetid: 56895fe5-e8df-4d75-9adc-c1f7757cdef8
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5f8310c6ed930ab5afaa7ecfde7a1fffb7c997d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211172"
---
# <a name="sysdm_fts_memory_buffers-transact-sql"></a>sys.dm_fts_memory_buffers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información acerca de los búferes de memoria que pertenecen a un bloque de memoria específico que se utiliza como parte de un rastreo de texto completo o un intervalo de rastreo de texto completo.  
  
> [!NOTE]
> La siguiente columna se quitará en una versión futura de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **ROW_COUNT**. Evite el uso de esta columna en los nuevos trabajos de desarrollo y piense en modificar las aplicaciones que la usan actualmente.  

  
|Columna|Tipo de datos|Descripción|  
|------------|---------------|-----------------|  
|**pool_id**|**int**|Id. del grupo de memoria asignado.<br /><br /> 0 = Búferes pequeños<br /><br /> 1 = Búferes grandes|  
|**memory_address**|**varbinary(8**|Dirección del búfer de memoria asignado.|  
|**name**|**nvarchar(4000)**|Nombre del búfer de memoria compartida para el que se ha realizado esta asignación.|  
|**is_free**|**bit**|Estado actual del búfer de memoria.<br /><br /> 0 = Disponible<br /><br /> 1 = Ocupado|  
|**row_count**|**int**|Número de filas que controla actualmente este búfer.|  
|**bytes_used**|**int**|Cantidad, en bytes, de memoria en uso en este búfer.|  
|**percent_used**|**int**|Porcentaje de memoria asignada usada.|  
  
## <a name="permissions"></a>Permisos  

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="physical-joins"></a>Combinaciones físicas  
 ![Combinaciones significativas de esta vista de administración dinámica](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-memory-buffers-1.gif "Combinaciones significativas de esta vista de administración dinámica")  
  
## <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Relación|  
|----------|--------|------------------|  
|dm_fts_memory_buffers.pool_id|dm_fts_memory_pools.pool_id|Varios a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica de la búsqueda de texto completo y la búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  

