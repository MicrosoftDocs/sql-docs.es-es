---
description: sys.dm_os_child_instances (Transact-SQL)
title: sys.dm_os_child_instances (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_child_instances
- sys.dm_os_child_instances_TSQL
- dm_os_child_instances
- dm_os_child_instances_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- server state information [SQL Server]
- sys.dm_os_child_instances dynamic management view
- monitoring server health
ms.assetid: 1bef3074-0ccc-48fa-8f3d-14f3d99df86b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 67e13b08aa1feddf49d91933942f93140c43dc44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184929"
---
# <a name="sysdm_os_child_instances-transact-sql"></a>sys.dm_os_child_instances (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada instancia de usuario creada a partir de la instancia del servidor primario.  
  
> **IMPORTANTE:** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 La información devuelta desde **Sys.dm_os_child_instances** se puede utilizar para determinar el estado de cada instancia de usuario (heart_beat) y para obtener el nombre de canalización (instance_pipe_name) que se puede utilizar para crear una conexión a la instancia de usuario mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o SQLCmd. Solo se puede conectar con una instancia de usuario después de que la inicie un proceso externo, como por ejemplo una aplicación cliente. Las herramientas de administración de SQL no pueden iniciar instancias de usuario.  
  
> **Nota:** Las instancias de usuario son solo una característica de [!INCLUDE[ssExpressEd11](../../includes/ssexpressed11-md.md)] .  
> 
> **Nota:** Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_child_instances**.  
  
|Columna|Tipo de datos|Descripción|  
|------------|---------------|-----------------|  
|**owning_principal_name**|**nvarchar(256)**|Nombre del usuario para el que se creó esta instancia.|  
|owning_principal_sid|nvarchar(256)|SID (identificador de seguridad) de la entidad de seguridad que posee esta instancia de usuario. Coincide con el identificador SID de Windows.|  
|owning_principal_sid_binary|varbinary(85)|Versión binaria del SID para el usuario propietario de la instancia de usuario.|  
|**instance_name**|**nvarchar(128)**|Nombre de esta instancia de usuario.|  
|**instance_pipe_name**|**nvarchar(260)**|Cuando se crea una instancia de usuario, se crea una canalización con nombre para que las aplicaciones se conecten a ella. Este nombre se puede utilizar en una cadena de conexión para conectarse a esta instancia de usuario.|  
|**os_process_id**|**Int**|Número del proceso de Windows para esta instancia de usuario.|  
|**os_process_creation_date**|**Datetime**|Fecha y hora de la última vez que se inició el proceso de esta instancia de usuario.|  
|**heart_beat**|**tipo**|Estado actual de esta instancia de usuario; puede ser ALIVE o DEAD.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="remarks"></a>Observaciones  
 Para obtener más información acerca de la vista de administración dinámica, vea [funciones y vistas de administración dinámica &#40;&#41;de Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
## <a name="see-also"></a>Consulte también  
 [Instancias de usuario para usuarios que no son administradores](/previous-versions/sql/)  
  
