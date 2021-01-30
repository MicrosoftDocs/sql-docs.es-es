---
description: sys.dm_xe_sessions (Transact-SQL)
title: sys.dm_xe_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_xe_sessions_TSQL
- dm_xe_sessions
- sys.dm_xe_sessions_TSQL
- sys.dm_xe_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_sessions dynamic management view
- extended events [SQL Server], views
ms.assetid: defd6efb-9507-4247-a91f-dc6ff5841e17
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d915555f2e8f6fc3cbc3df4b87bc482ed6983483
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192904"
---
# <a name="sysdm_xe_sessions-transact-sql"></a>sys.dm_xe_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información sobre una sesión de Extended Events activa. Esta sesión es una colección de eventos, acciones y destinos.  
    
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|address|**varbinary(8**|La dirección de memoria de la sesión. la dirección es única en todo el sistema local. No admite valores NULL.|  
|name|**nvarchar(256)**|El nombre de la sesión. el nombre es único en todo el sistema local. No admite valores NULL.|  
|pending_buffers|**int**|Número de búferes llenos pendientes de procesamiento. No admite valores NULL.|  
|total_regular_buffers|**int**|Número total de búferes normales que están asociados a la sesión. No admite valores NULL.<br /><br /> Nota: la mayor parte del tiempo se utilizan búferes normales. Estos búferes son de tamaño suficiente para contener muchos eventos. Normalmente habrá tres búferes o más por cada sesión. El servidor determina automáticamente el número de búferes normales, según las particiones de memoria que se establecen a través de la opción MEMORY_PARTITION_MODE. El tamaño de los búferes normales es igual al valor de la opción MAX_MEMORY (que es de 4 MB de forma predeterminado) dividido por el número de búferes. Para obtener más información sobre las opciones MEMORY_PARTITION_MODE y MAX_MEMORY, vea [Create Event SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md).|  
|regular_buffer_size|**bigint**|Tamaño en bytes del búfer de salida normal. No admite valores NULL.|  
|total_large_buffers|**int**|Número total de búferes grandes. No admite valores NULL.<br /><br /> Nota: los búferes grandes se usan cuando un evento es mayor que un búfer normal. Con este fin se reservan explícitamente. Los búferes grandes se asignan cuando se inicia la sesión del evento y su tamaño se determina según la opción MAX_EVENT_SIZE. Para obtener más información acerca de la opción MAX_EVENT_SIZE, consulte [Create Event SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md).|  
|large_buffer_size|**bigint**|Tamaño en bytes del búfer grande. No admite valores NULL.|  
|total_buffer_size|**bigint**|Tamaño total del búfer de memoria que se utiliza para almacenar los eventos de la sesión, en bytes. No admite valores NULL.|  
|buffer_policy_flags|**int**|Mapa de bits que indica el comportamiento de los búferes de eventos de la sesión cuando todos los búferes están llenos y se activa un nuevo evento. No admite valores NULL.|  
|buffer_policy_desc|**nvarchar(256)**|Descripción que indica el comportamiento de los búferes de eventos de la sesión cuando todos los búferes están llenos y se activa un nuevo evento.  No admite valores NULL. buffer_policy_desc puede ser una de las siguientes:<br /><br /> Quitar evento<br /><br /> No quitar eventos<br /><br /> Quitar búfer lleno<br /><br /> Asignar nuevo búfer|  
|flags|**int**|Mapa de bits que indica las marcas establecidas en la sesión. No admite valores NULL.|  
|flag_desc|**nvarchar(256)**|Descripción de las marcas activadas en la sesión.  No admite valores NULL. flag_desc puede ser cualquier combinación de lo siguiente:<br /><br /> Vaciar búferes al cerrar<br /><br /> Distribuidor dedicado<br /><br /> Permitir eventos recursivos|  
|dropped_event_count|**int**|Número de eventos eliminados cuando los búferes estaban llenos. Este valor es **0** si la Directiva de búferes es "quitar búfer lleno" o "no quitar eventos". No admite valores NULL.|  
|dropped_buffer_count|**int**|Número de búferes que se quitaron cuando los búferes estaban llenos. Este valor es **0** si la Directiva de búfer está establecida en "Quitar evento" o "no quitar eventos". No admite valores NULL.|  
|blocked_event_fire_time|**int**|El periodo de tiempo que la activación de eventos permaneció bloqueada mientras los búferes estaban llenos. Este valor es **0** si la Directiva de búferes es "quitar búfer lleno" o "Quitar evento". No admite valores NULL.|  
|create_time|**datetime**|Hora en que se creó la sesión. No admite valores NULL.|  
|largest_event_dropped_size|**int**|El tamaño del evento más grande de los que no cupieron en el búfer de la sesión. No admite valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

