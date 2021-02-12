---
description: sys.dm_exec_compute_pools (Transact-SQL)
title: sys.dm_exec_compute_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, big-data-clusters
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_compute_pools
- dm_exec_compute_pools_TSQL
- dm_exec_compute_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_compute_pools dynamic management view
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-ver15||>=sql-server-linux-2017'
ms.openlocfilehash: 7e9459c31529b8446b49b776ed009c429f904b18
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071589"
---
# <a name="sysdm_exec_compute_pools-transact-sql"></a>sys.dm_exec_compute_pools (Transact-SQL)
[!INCLUDE[sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|name|`sysname`|Nombre del grupo de proceso. No admite valores NULL. Devuelve `default` para el grupo de proceso predeterminado. |
|compute_pool_id|`int`|Identificador único para el grupo. Clave para esta vista.|  
|ubicación|`sysname`|Punto de conexión al controlador en un clúster de Big Data de SQL. No admite valores NULL. |

## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.

## <a name="see-also"></a>Consulte también

[¿Qué [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ss-nover.md)] son ](../../big-data-cluster/big-data-cluster-overview.md)?
