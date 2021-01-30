---
description: sys.dm_os_cluster_nodes (Transact-SQL)
title: sys.dm_os_cluster_nodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_cluster_nodes_TSQL
- dm_os_cluster_nodes_TSQL
- dm_os_cluster_nodes
- sys.dm_os_cluster_nodes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_cluster_nodes dynamic management view
ms.assetid: 92fa804e-2d08-42c6-a36f-9791544b1d42
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7db044bba8fc69287653fe5806a02d287593176c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184968"
---
# <a name="sysdm_os_cluster_nodes-transact-sql"></a>sys.dm_os_cluster_nodes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada nodo en la configuración de la instancia en clúster de conmutación por error. Si la instancia actual es una instancia en clúster de conmutación por error, devuelve una lista de nodos en los que se ha definido esta instancia en clúster de conmutación por error (antes "servidor virtual"). Si la instancia del servidor actual no es una instancia en clúster de conmutación por error, devuelve un conjunto de filas vacío.  
  
> **Nota:** Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_cluster_nodes**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**NodeName**|**sysname**|Nombre de un nodo en la configuración de la instancia en clúster de conmutación por error (servidor virtual) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|status|**int**|Estado del nodo en una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instancia de clúster de conmutación por error: 0, 1, 2, 3,-1. Para obtener más información, consulte [función GetClusterNodeState](/windows/win32/api/clusapi/nf-clusapi-getclusternodestate).|  
|status_description|**nvarchar (20)**|Descripción del estado del nodo de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 0: activo<br /><br /> 1: inactivo<br /><br /> 2: en pausa<br /><br /> 3: uniéndose<br /><br /> -1 = desconocido|  
|is_current_owner|bit|1 significa que este nodo es el propietario actual del recurso de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="remarks"></a>Observaciones  
 Si se habilita la agrupación en clústeres de conmutación por error, la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se puede ejecutar en cualquiera de los nodos del clúster de conmutación por error designados como parte de la configuración de la instancia en clúster de conmutación por error (servidor virtual) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> **Nota:** Esta vista reemplaza a la función fn_virtualservernodes, que quedará en desuso en una versión futura.  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW SERVER STATE en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa sys. dm_os_cluster_nodes para devolver los nodos de una instancia de servidor en clúster.  
  
```  
SELECT NodeName, status, status_description, is_current_owner   
FROM sys.dm_os_cluster_nodes;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|NodeName|status|status_description|is_current_owner|  
|--------------|------------|-------------------------|------------------------|  
|node1|0|up|1|  
|node2|0|up|0|  
|Nodo3|1| Abajo|0|  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_os_cluster_properties &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-properties-transact-sql.md)   
 [sys.dm_io_cluster_shared_drives &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md)   
 [sys.fn_virtualservernodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-virtualservernodes-transact-sql.md)   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
