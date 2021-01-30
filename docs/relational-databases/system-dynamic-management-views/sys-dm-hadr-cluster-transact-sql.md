---
description: sys.dm_hadr_cluster (Transact-SQL)
title: sys.dm_hadr_cluster (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/31/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_cluster
- dm_hadr_cluster_HADR
- sys.dm_hadr_cluster_TSQL
- dm_hadr_cluster
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_cluster catalog view
- Availability Groups [SQL Server], WSFC clusters
ms.assetid: 13ce70e4-9d43-4a80-a826-099e6213bf85
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0648f9c281c59789f3ae9d4f97cc3d277303abd1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171797"
---
# <a name="sysdm_hadr_cluster-transact-sql"></a>sys.dm_hadr_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Si el nodo de clústeres de conmutación por error de Windows Server (WSFC) que hospeda una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] habilitada para [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] tiene quórum de WSFC, **Sys.dm_hadr_cluster** devuelve una fila que expone el nombre del clúster e información sobre el cuórum. Si el nodo de WSFC no tiene quórum, no se devuelve ninguna fila.  
 > [!TIP]
 > A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , esta vista de administración dinámica admite Always on instancias de clúster de conmutación por error además de Always on grupos de disponibilidad.

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**cluster_name**|**nvarchar(128)**|Nombre de clúster de WSFC que hospeda las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] habilitadas para [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].|  
|**quorum_type**|**tinyint**|Tipo de quórum utilizado por este clúster de WSFC; puede ser:<br /><br /> 0 = Mayoría de nodo. Esta configuración de quórum puede admitir errores de la mitad de los (con redondeo por exceso) menos uno. Por ejemplo, en un clúster de siete nodos, esta configuración de quórum puede admitir errores de tres nodos.<br /><br /> 1 = Nodo y mayoría de disco. Si el testigo de disco permanece en línea, esta configuración de quórum puede admitir errores de la mitad del número de nodos (con redondeo por exceso). Por ejemplo, un clúster de seis nodos en el que el testigo de disco está en línea puede admitir errores de tres nodos. Si el testigo de disco se queda sin conexión o sufre un error, esta configuración de quórum puede admitir errores de la mitad de los nodos (con redondeo por exceso) menos uno. Por ejemplo, un clúster de seis nodos con un testigo de disco con errores puede admitir errores de 3-1=2 nodos.<br /><br /> 2 = Nodo y mayoría de recurso compartido de archivos. Esta configuración de quórum funciona de forma similar a Nodo y mayoría del disco, pero utiliza un testigo de recurso compartido de archivos en lugar de un testigo de disco.<br /><br /> 3 = Sin mayoría: solo disco. Si el disco de quórum está en línea, esta configuración de quórum puede admitir errores de todos los nodos excepto uno.<br /><br /> 4 = Cuórum desconocido. Cuórum desconocido para el clúster.<br /><br /> 5 = testigo en la nube. El clúster emplea Microsoft Azure para el arbitraje del cuórum. Si el testigo en la nube está disponible, el clúster puede admitir el error de la mitad de los nodos (redondeando).|  
|**quorum_type_desc**|**varchar(50)**|Descripción de **quorum_type**, uno de los siguientes:<br /><br /> NODE_MAJORITY<br /><br /> NODE_AND_DISK_MAJORITY<br /><br /> NODE_AND_FILE_SHARE_MAJORITY<br /><br /> NO_MAJORITY: _DISK_ONLY <br /><br /> UNKNOWN_QUORUM <br /><br /> CLOUD_WITNESS|  
|**quorum_state**|**tinyint**|Estado del quórum de WSFC, uno de los siguientes:<br /><br /> 0 = estado de quórum desconocido<br /><br /> 1 = Quórum normal<br /><br /> 2 = Quórum forzado|  
|**quorum_state_desc**|**varchar(50)**|Descripción de **quorum_state**, uno de los siguientes:<br /><br /> UNKNOWN_QUORUM_STATE<br /><br /> NORMAL_QUORUM<br /><br /> FORCED_QUORUM|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Always On vistas y funciones de administración dinámica de grupos de disponibilidad &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Vistas de catálogo de grupos de disponibilidad AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [sys.dm_hadr_cluster_members &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
  
  
