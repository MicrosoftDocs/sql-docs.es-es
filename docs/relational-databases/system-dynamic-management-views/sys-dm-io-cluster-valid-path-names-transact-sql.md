---
description: sys.dm_io_cluster_valid_path_names (Transact-SQL)
title: sys.dm_io_cluster_valid_path_names (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_io_cluster_valid_path_names
- dm_io_cluster_valid_path_names_TSQL
- sys.dm_io_cluster_valid_path_names_TSQL
- dm_io_cluster_valid_path_names
dev_langs:
- TSQL
helpviewer_keywords:
- dm_io_cluster_valid_path_names
- sys.dm_io_cluster_valid_path_names
- cluster valid path names
- csv name
- cluster shared volume names
ms.assetid: 5bc8a0e5-6c72-425b-8c58-f276eb9add2c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 615849d483d2883476fb2eb65ba5183581426af6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185001"
---
# <a name="sysdm_io_cluster_valid_path_names-transact-sql"></a>sys.dm_io_cluster_valid_path_names (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información sobre todos los discos compartidos válidos, incluidos los volúmenes compartidos en clúster, para una instancia de clúster de conmutación por error de SQL Server. Si la instancia no está en clúster, se devuelve un conjunto de filas vacío.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**path_name**|**Nvarchar (512)**|Punto de montaje del volumen o ruta de acceso de unidad que se puede usar como directorio raíz de los archivos de base de datos y de registro. No admite valores NULL.|  
|**cluster_owner_node**|**Nvarchar (64)**|Propietario actual de la unidad. Para los volúmenes compartidos en clúster (CSV), el propietario es el nodo que hospeda el servidor de metadatos. No admite valores NULL.|  
|**is_cluster_shared_volume**|**Poco**|Devuelve 1 si la unidad en la que se encuentra esta ruta de acceso es un volumen compartido en clúster; de lo contrario, devuelve 0.|  
  
## <a name="remarks"></a>Observaciones  
 Una instancia de clúster de conmutación por error (FCI) de SQL Server debe usar almacenamiento compartido entre todos los nodos de FCI para el almacenamiento de archivos de datos y de registro. Los discos que se muestran en esta vista son los que se encuentran en el grupo de recursos de clúster asociado a la instancia y son los únicos discos que se pueden usar para el almacenamiento de archivos de datos o de registro.  
  
> [!NOTE]  
>  Esta vista reemplazará [sys.dm_io_cluster_shared_drives &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md) en una versión futura.  
  
## <a name="permissions"></a>Permisos  
 El usuario debe tener permiso VIEW SERVER STATE para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa sys.dm_io_cluster_valid_path_names para determinar las unidades compartidas en una instancia de servidor en clúster:  
  
```  
SELECT * FROM sys.dm_io_cluster_valid_path_names;  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_os_cluster_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [sys.dm_io_cluster_shared_drives &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md)   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

