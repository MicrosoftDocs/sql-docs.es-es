---
description: sys.database_event_sessions (Azure SQL Database)
title: sys.database_event_sessions (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
ms.assetid: 02c2cd71-d35e-4d4c-b844-92b240f768f4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: 0d7bfe1e91dd011863ae6e867aba48dbe4491c2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210306"
---
# <a name="sysdatabase_event_sessions-azure-sql-database"></a>sys.database_event_sessions (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Muestra todas las definiciones de sesión de eventos que existen en la base de datos actual, en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .  
  
> [!NOTE]
>  La vista de catálogo similar denominada `sys.server_event_sessions` se aplica solo a [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
||  
|-|  
|**Se aplica a**: [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] y a cualquier versión posterior.|  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|Id. único de la sesión de eventos. No admite valores NULL.|  
|name|**sysname**|Nombre definido por el usuario para identificar la sesión de eventos. el nombre es único. No admite valores NULL.|  
|event_retention_mode|**NCHAR (1)**|Determina cómo se controla la pérdida de eventos. El valor predeterminado es S. No admite valores NULL. Es uno de los siguientes valores:<br /><br /> S. Se asigna a event_retention_mode_desc = ALLOW_SINGLE_EVENT_LOSS<br /><br /> M. Se asigna a event_retention_mode_desc = ALLOW_MULTIPLE_EVENT_LOSS<br /><br /> Hora Se asigna a event_retention_mode_desc = NO_EVENT_LOSS|  
|event_retention_mode_desc|**sysname**|Describe cómo se controla la pérdida de eventos. El valor predeterminado es ALLOW_SINGLE_EVENT_LOSS. No admite valores NULL. Es uno de los siguientes valores:<br /><br /> ALLOW_SINGLE_EVENT_LOSS. Pueden perderse los eventos de la sesión. Se quitan eventos individuales únicamente cuando todos los búferes de eventos están llenos. La pérdida de eventos individuales cuando los búferes están llenos permite un rendimiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aceptable, al mismo tiempo que minimiza las pérdidas en el flujo de eventos procesado.<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS. Pueden perderse búferes de eventos completos de la sesión. El número de eventos perdidos depende del tamaño de la memoria asignada a la sesión, el particionamiento de la memoria y el tamaño de los eventos del búfer. Esta opción minimiza el impacto en el rendimiento del servidor si los búferes de eventos se llenan rápidamente. Sin embargo, pueden perderse un gran número de eventos de la sesión.<br /><br /> NO_EVENT_LOSS. No se permite ninguna pérdida de eventos. Esta opción asegura que se retienen todos los eventos que aparecen. Al utilizar esta opción, se fuerza a todas las tareas que activan eventos a que esperen que haya espacio disponible en un búfer de eventos. Esto puede conducir a una degradación detectable en el rendimiento mientras la sesión de eventos está activa.|  
|max_dispatch_latency|**int**|La cantidad de tiempo, en milisegundos, durante el que los eventos se almacenarán en memoria antes de servirse a los destinos de la sesión. Los valores válidos son de 1 a 2147483648, y -1. Un valor de -1 indica que la latencia de la expedición es infinita. Acepta valores NULL.|  
|max_memory|**int**|La cantidad de memoria asignada a la sesión para el almacenado en búfer de los eventos. El valor predeterminado es 4 MB. Acepta valores NULL.|  
|max_event_size|**int**|La cantidad de memoria reservada para los eventos que no pueden almacenarse en los búferes de sesión de eventos. Si max_event_size supera el tamaño de búfer calculado, se asignarán dos búferes adicionales de max_event_size a la sesión de evento. Acepta valores NULL.|  
|memory_partition_mode|**NCHAR (1)**|Ubicación en memoria donde se crean los búferes de eventos. El modo de la partición predeterminado es G. No admite valores NULL. memory_partition_mode es uno de los siguientes:<br /><br /> G - NONE<br /><br /> C - PER_CPU<br /><br /> N - PER_NODE|  
|memory_partition_mode_desc|**sysname**|El valor predeterminado es NONE. No admite valores NULL. Es uno de los siguientes valores:<br /><br /> NONE. Se crea un único conjunto de búferes dentro de una instancia de SQL Server.<br /><br /> PER_CPU. Se crea un conjunto de búferes para cada CPU.<br /><br /> PER_NODE. Se crea un conjunto de búferes para cada nodo de acceso no uniforme a memoria (NUMA).|  
|track_causality|**bit**|Habilite o deshabilite el seguimiento de causalidad. Si está establecido en 1 (ON), se habilita el seguimiento y se pueden correlacionar los eventos relacionados en diferentes conexiones con el servidor. El valor predeterminado es 0 (OFF). No admite valores NULL.|  
|startup_state|**bit**|Valor que determina si se inicia automáticamente la sesión cuando el servidor se inicia. El valor predeterminado es 0. No admite valores NULL. Puede ser uno de los siguientes:<br /><br /> 0 (OFF). La sesión no se inicia cuando el servidor se inicia.<br /><br /> 1 (ON). La sesión de eventos se inicia cuando el servidor se inicia.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
  
