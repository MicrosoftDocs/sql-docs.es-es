---
description: MStracer_tokens (Transact-SQL)
title: MStracer_tokens (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MStracer_tokens_TSQL
- MStracer_tokens
dev_langs:
- TSQL
helpviewer_keywords:
- MStracer_tokens system table
ms.assetid: b273aa48-8188-4213-8e2c-311543c3236f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2bbd85ea937aee3445d216c35cefe3f61fb494fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211553"
---
# <a name="mstracer_tokens-transact-sql"></a>MStracer_tokens (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En la tabla **MStracer_tokens** se mantiene un registro de los registros de testigos de seguimiento insertados en una publicación. Esta tabla se almacena en la base de datos de distribución y es utilizada por la replicación para la supervisión del rendimiento.   
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**tracer_id**|**int**|Identifica un registro de un token de seguimiento.|  
|**publication_id**|**int**|Identifica la publicación en la que se ha insertado el registro del token de seguimiento.|  
|**publisher_commit**|**datetime**|La fecha y la hora en que se ha confirmado el registro del token de seguimiento en el publicador.|  
|**distributor_commit**|**datetime**|La fecha y la hora en que se ha confirmado el registro del token de seguimiento en el distribuidor.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
