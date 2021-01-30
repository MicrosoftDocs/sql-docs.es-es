---
description: sys.dm_io_cluster_shared_drives (Transact-SQL)
title: sys.dm_io_cluster_shared_drives (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_io_cluster_shared_drives_TSQL
- sys.dm_io_cluster_shared_drives
- dm_io_cluster_shared_drives_TSQL
- dm_io_cluster_shared_drives
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_cluster_shared_drives dynamic management view
ms.assetid: c8fcced8-c780-49dc-99bd-6beb3ca532c4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1cb3a37eb8bd0a0fea491ee6aa11e74b4c092739
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185044"
---
# <a name="sysdm_io_cluster_shared_drives-transact-sql"></a>sys.dm_io_cluster_shared_drives (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Esta vista devuelve el nombre de cada unidad compartida si la instancia de servidor actual es un servidor en clúster. Si la instancia del servidor actual no es una instancia en clúster, devuelve un conjunto de filas vacío.  
  
> [!NOTE]  
>  Para llamarlo desde [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilice el nombre **Sys.dm_pdw_nodes_io_cluster_shared_drives**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DriveName**|**nchar(2)**|Nombre de la unidad (la letra de unidad) que representa un disco individual que forma parte de la matriz de discos compartida del clúster. La columna no acepta valores NULL.|  
|**pdw_node_id**|**int**|**Se aplica a**: ssPDW<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="remarks"></a>Observaciones  
 Cuando se habilita la agrupación en clústeres, la instancia en clúster de conmutación por error requiere que los archivos de datos y de registro estén en discos compartidos para que puedan estar accesibles después de que la instancia realice una conmutación por error a otro nodo. Cada una de las filas de esta vista representa un único disco compartido que utiliza esta instancia en clúster de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para almacenar archivos de registros o datos de esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo se pueden utilizar los discos enumerados en esta vista. Los discos enumerados en esta vista son los que están en el grupo de recursos del clúster asociado a la instancia.  
  
> [!NOTE]  
>  Esta vista quedará desusada en una versión futura. En su lugar, se recomienda usar [sys.dm_io_cluster_valid_path_names &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-valid-path-names-transact-sql.md) .  
  
## <a name="permissions"></a>Permisos  
 El usuario debe tener permiso VIEW SERVER STATE para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se utiliza sys.dm_io_cluster_shared_drives para determinar las unidades compartidas en una instancia de servidor en clúster:  
  
```  
SELECT * FROM sys.dm_io_cluster_shared_drives;  
```  
  
 Éste es el conjunto de resultados:  
  
 DriveName  
  
 --------\-  
  
 m  
  
 n  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_io_cluster_valid_path_names &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-valid-path-names-transact-sql.md)   
 [sys.dm_os_cluster_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [sys.fn_servershareddrives &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-servershareddrives-transact-sql.md)   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  
