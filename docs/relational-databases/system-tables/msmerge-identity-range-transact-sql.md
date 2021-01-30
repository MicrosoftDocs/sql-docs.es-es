---
description: MSmerge_identity_range (Transact-SQL)
title: MSmerge_identity_range (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_identity_range_TSQL
- MSmerge_identity_range
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_identity_range system table
ms.assetid: 493a2028-88a0-4e83-ad89-ae5661d9f477
author: cawrites
ms.author: chadam
ms.openlocfilehash: 34c6c9258c4fd3a72f1c499abebefecdc65c3912
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203595"
---
# <a name="msmerge_identity_range-transact-sql"></a>MSmerge_identity_range (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_identity_range** se utiliza para realizar el seguimiento de los intervalos numéricos asignados a las columnas de identidad para la suscripción a publicaciones en las que la replicación administra automáticamente estas asignaciones de intervalos. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**Subid**|**uniqueidentifier**|Número de identificación único para una suscripción dada.|  
|**artid**|**uniqueidentifier**|El número de identificación único del artículo indicado.|  
|**range_begin**|**Numeric (38)**|Valor de identidad al principio del intervalo actual.|  
|**range_end**|**Numeric (38)**|Valor de identidad al final del intervalo actual.|  
|**next_range_begin**|**Numeric (38)**|Valor de identidad al principio del siguiente intervalo que se va a asignar.|  
|**next_range_end**|**Numeric (38)**|Valor de identidad al final del siguiente intervalo que se va a asignar.|  
|**is_pub_range**|**bit**|Un valor de **1** si el intervalo de identidad se asigna a la publicación.|  
|**max_used**|**Numeric (38)**|Valor de identidad máximo que se puede asignar.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
