---
description: sys.dm_resource_governor_external_resource_pool_affinity (Transact-SQL)
title: sys.dm_resource_governor_external_resource_pool_affinity (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_resource_governor_external_resource_pool_affinity
- sys.dm_resource_governor_external_resource_pool_affinity_TSQL
- dm_resource_governor_external_resource_pool_affinity
- dm_resource_governor_external_resource_pool_affinity_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_external_resource_pool_affinity
- dm_resource_governor_external_resource_pool_affinity
ms.assetid: e32fac49-5161-47c0-8540-af3fe730c00c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1dbbbbfacabb47fc1c18bfce9930ae93bc0f266b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203382"
---
# <a name="sysdm_resource_governor_external_resource_pool_affinity-transact-sql"></a>sys.dm_resource_governor_external_resource_pool_affinity (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]
**Se aplica a:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)]y [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)][!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)]

Devuelve información de afinidad de CPU acerca de la configuración actual del grupo de recursos externos.
  
|Nombre de la columna|Tipo de datos|Descripción|
|----------------|---------------|-----------------|
|{1}pool_id{2}|**int**|IDENTIFICADOR del grupo de recursos externos. No admite valores NULL.|
|processor_group|**smallint**|Identificador del grupo de procesadores lógicos de Windows. No admite valores NULL.|
|cpu_mask|**bigint**|Máscara binaria que representa las CPU asociadas a este grupo. No admite valores NULL.|
  
## <a name="remarks"></a>Observaciones

Los grupos creados con una afinidad de no `AUTO` aparecen en esta vista porque no tienen afinidad. Para obtener más información, vea [crear un grupo de recursos externos &#40;Transact-sql&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md) y [modificar el grupo de recursos externos &#40;instrucciones de transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md) .

## <a name="permissions"></a>Permisos

Requiere el permiso `VIEW SERVER STATE`.

## <a name="see-also"></a>Consulte también

[Resource governance for machine learning in SQL Server](../../machine-learning/administration/resource-governor.md) (Gobernanza de recursos para aprendizaje automático en SQL Server)

[sys.dm_resource_governor_resource_pool_affinity &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pool-affinity-transact-sql.md)

[Opción de configuración de servidor Scripts externos habilitados](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)

[ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
