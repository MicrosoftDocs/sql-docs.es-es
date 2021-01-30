---
description: dbo.syssubsystems (Transact-SQL)
title: Subsistemas de dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.syssubsystems
- syssubsystems
- syssubsystems_TSQL
- dbo.syssubsystems_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syssubsystems system table
ms.assetid: 114b3d55-1ad6-4777-b868-8ef0c86ba596
author: cawrites
ms.author: chadam
ms.openlocfilehash: 71de173aa467cd3365df8154f63bc7e469fa83b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201204"
---
# <a name="dbosyssubsystems-transact-sql"></a>dbo.syssubsystems (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información sobre todos los subsistemas proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibles. La tabla **syssubsystems** se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|Id. del subsistema.|  
|**subsistema**|**nvarchar(40)**|Nombre del subsistema.|  
|**description_id**|**int**|ID. de mensaje de la fila de la vista de catálogo **Sys. Messages** que contiene la descripción del subsistema.|  
|**subsystem_dll**|**nvarchar(255)**|Ubicación de la DLL del subsistema.|  
|**agent_exe**|**nvarchar(255)**|Ruta de acceso completa al archivo ejecutable que utiliza el subsistema.|  
|**start_entry_point**|**nvarchar(30)**|Función a la que se llama cuando el subsistema se inicializa.|  
|**event_entry_point**|**nvarchar(30)**|Función a la que se llama cuando se ejecuta un paso del subsistema.|  
|**stop_entry_point**|**nvarchar(30)**|Función a la que se llama cuando un subsistema deja de ejecutarse.|  
|**max_worker_threads**|**int**|Número máximo de pasos simultáneos para un subsistema dado.|  
  
## <a name="remarks"></a>Observaciones  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden tener acceso a esta tabla.  
  
## <a name="see-also"></a>Consulte también  
 [dbo.sysproxysubsystem &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)   
 [dbo.sysproxies &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)  
  
  
