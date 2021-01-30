---
description: sys.dm_broker_activated_tasks (Transact-SQL)
title: sys.dm_broker_activated_tasks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_broker_activated_tasks
- sys.dm_broker_activated_tasks_TSQL
- dm_broker_activated_tasks
- dm_broker_activated_tasks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_activated_tasks dynamic management view
ms.assetid: 17e6f87f-8f56-489d-9aed-216afc8ef310
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 22c0c0678546899131154b31ec5a04b11c62a107
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202412"
---
# <a name="sysdm_broker_activated_tasks-transact-sql"></a>sys.dm_broker_activated_tasks (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada procedimiento almacenado activado por Service Broker.  
 

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**spid**|**int**|Id. de la sesión del procedimiento almacenado activado. Acepta valores NULL.|  
|**database_id**|**smallint**|Id. de la base de datos en la que se define la cola. Acepta valores NULL.|  
|**queue_id**|**int**|Id. del objeto de la cola para el que se activó el procedimiento almacenado. Acepta valores NULL.|  
|**procedure_name**|**nvarchar (650)**|Nombre del procedimiento almacenado activado. Acepta valores NULL.|  
|**execute_as**|**int**|Id. del usuario con el que se ejecuta el procedimiento almacenado. Acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="physical-joins"></a>Combinaciones físicas  
 ![Combinaciones de sys.dm_broker_activated_tasks](../../relational-databases/system-dynamic-management-views/media/join-dm-broker-activated-tasks-1.gif "Combinaciones de sys.dm_broker_activated_tasks")  
  
## <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Relación|  
|----------|--------|------------------|  
|dm_broker_activated_tasks.spid|dm_exec_sessions.session_id|Uno a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

