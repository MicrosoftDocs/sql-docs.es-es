---
description: MSmerge_dynamic_snapshots (Transact-SQL)
title: MSmerge_dynamic_snapshots (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_dynamic_snapshots_TSQL
- MSmerge_dynamic_snapshots
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_dynamic_snapshots system table
ms.assetid: a5592b3c-731b-4fc9-ae4b-2602ed78248e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0bea9259e6218d32ae0e278046493aaeeafeaa41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181268"
---
# <a name="msmerge_dynamic_snapshots-transact-sql"></a>MSmerge_dynamic_snapshots (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En la tabla **MSmerge_dynamic_snapshots** se realiza un seguimiento de la ubicación de la instantánea de datos filtrados para cada partición definida para una publicación de combinación con filtros de fila con parámetros. Esta tabla se almacena en la base de datos de **publicación** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**partition_id**|**int**|El Id. de la partición de mezcla.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|La ubicación de la instantánea de datos filtrados de la partición.|  
|**last_updated**|**datetime**|La fecha en que se ha actualizado la instantánea de datos filtrados.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
