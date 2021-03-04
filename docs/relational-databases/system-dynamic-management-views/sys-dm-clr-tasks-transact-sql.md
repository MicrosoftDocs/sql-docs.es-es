---
description: sys.dm_clr_tasks (Transact-SQL)
title: sys.dm_clr_tasks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_clr_tasks
- sys.dm_clr_tasks_TSQL
- dm_clr_tasks
- dm_clr_tasks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_tasks dynamic management view
ms.assetid: 462b9061-09fa-4858-9707-03d6cc19c769
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c16eea33c1c0211b37823ecff1e2ae1131d2a335
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837622"
---
# <a name="sysdm_clr_tasks-transact-sql"></a>sys.dm_clr_tasks (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila de todas las tareas CLR (Common Language Runtime) que se están ejecutando actualmente. Un lote de [!INCLUDE[tsql](../../includes/tsql-md.md)] que contiene una referencia a una rutina CLR crea una tarea independiente para la ejecución de todo el código administrado en ese lote. Muchas instrucciones del lote que requieren la ejecución del código administrado utilizan la misma tarea CLR. La tarea CLR es responsable del mantenimiento de objetos y el estado relativo a la ejecución de código administrado y también de las transacciones entre la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y CLR.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**task_address**|**varbinary(8**|Dirección de la tarea CLR.|  
|**sos_task_address**|**varbinary(8**|Dirección de la tarea del lote [!INCLUDE[tsql](../../includes/tsql-md.md)] subyacente.|  
|**appdomain_address**|**varbinary(8**|Dirección del dominio de la aplicación en la que se ejecuta esta tarea.|  
|**state**|**nvarchar(128)**|Estado actual de la tarea.|  
|**abort_state**|**nvarchar(128)**|Estado en el que está actualmente la cancelación (si la tarea se ha cancelado). Hay varios estados implicados en la anulación de tareas.|  
|**type**|**nvarchar(128)**|Tipo de tarea.|  
|**affinity_count**|**int**|Afinidad de la tarea.|  
|**forced_yield_count**|**int**|Número de veces que se forzó a la tarea a producir.|  
  
## <a name="permissions"></a>Permisos  

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
