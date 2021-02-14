---
description: sys.dm_hadr_cluster_members (Transact-SQL)
title: sys.dm_hadr_cluster_members (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/31/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_cluster_members_TSQL
- sys.dm_hadr_cluster_members
- dm_hadr_cluster_members_TSQL
- dm_hadr_cluster_members
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_cluster_members catalog view
ms.assetid: feb20b3a-8835-41d3-9a1c-91d3117bc170
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eb47ace512b9db4983a9b61ccbdb6db490545937
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354172"
---
# <a name="sysdm_hadr_cluster_members-transact-sql"></a>sys.dm_hadr_cluster_members (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Si el nodo de WSFC que hospeda una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que está habilitada para [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] tiene quórum de WSFC, devuelve una fila para cada uno de los miembros que constituyen el quórum y el estado de cada uno de ellos. Esto incluye todos los nodos del clúster (devueltos con CLUSTER_ENUM_NODE tipo por la función **Clusterenum** ) y el testigo de disco o de recurso compartido de archivos, si existe. La fila devuelta para un miembro determinado contiene información sobre el estado de ese miembro. Por ejemplo, para un clúster de cinco nodos con quórum de mayoría de nodo en el que un nodo está inactivo, cuando se consulta **Sys.dm_hadr_cluster_members** desde una instancia del servidor que está habilitada para que se encuentra [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] en un nodo con quórum, **Sys.dm_hadr_cluster_members** refleja el estado del nodo inactivo como "NODE_DOWN".  
  
 Si el nodo de WSFC no tiene el quórum, no se devuelve ninguna fila.  
  
 Use esta vista de administración dinámica para responder las preguntas siguientes:  
  
-   ¿Qué nodos se están ejecutando actualmente en el clúster de WSFC?  
  
-   ¿Cuántos errores más puede tolerar el clúster de WSFC antes de perder el quórum en un caso de mayoría de nodo?  

 > [!TIP]
 > A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , esta vista de administración dinámica admite Always on instancias de clúster de conmutación por error además de Always on grupos de disponibilidad.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**member_name**|**nvarchar(128)**|Nombre de miembro, que puede ser un nombre de equipo, una letra de unidad o una ruta de acceso de recurso compartido de archivos.|  
|**member_type**|**tinyint**|Tipo del miembro; puede ser:<br /><br /> 0 = Nodo de WSFC<br /><br /> 1 = Testigo de disco<br /><br /> 2 = Testigo de recurso compartido de archivos<br /><br /> 3 = testigo en la nube|  
|**member_type_desc**|**nvarchar(50)**|Descripción de **member_type**, uno de los siguientes:<br /><br /> CLUSTER_NODE<br /><br /> DISK_WITNESS<br /><br /> FILE_SHARE_WITNESS<br /><br /> CLOUD_WITNESS|  
|**member_state**|**tinyint**|El estado del miembro; puede ser:<br /><br /> 0 = Sin conexión<br /><br /> 1 = En línea|  
|**member_state_desc**|**nvarchar(60)**|Descripción de **member_state**, uno de los siguientes:<br /><br /> UP<br /><br /> ABAJO|  
|**number_of_quorum_votes**|**tinyint**|Número de votos de quórum propiedad de este miembro de quórum. En el caso de que no haya mayoría: quórums de Solo disco: el valor predeterminado de este valor es 0. Para otros tipos de quórum, el valor predeterminado de este valor es 1.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="examples"></a>Ejemplos  
  
## <a name="see-also"></a>Consulte también  
 [Always On vistas y funciones de administración dinámica de grupos de disponibilidad &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Vistas de catálogo de grupos de disponibilidad AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
