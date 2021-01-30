---
description: MSsubscription_articles (Transact-SQL)
title: MSsubscription_articles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsubscription_articles
- MSsubscription_articles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_articles system table
ms.assetid: dbc1737f-261e-4017-b9cd-703b9fc4ac78
author: cawrites
ms.author: chadam
ms.openlocfilehash: 16d9d91b7c3948f18d90b235c4f853d1220c5380
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211595"
---
# <a name="mssubscription_articles-transact-sql"></a>MSsubscription_articles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSsubscription_articles** contiene información sobre los artículos de una suscripción en cola. Esta tabla solo se llena con los tipos de replicación de actualización en cola y actualización inmediata con la actualización en cola como una conmutación por error.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|El Id. del agente que sirve a este artículo.|  
|**artid**|**int**|El identificador de artículo de la tabla **sysarticles** .|  
|**artículo**|**sysname**|Nombre del artículo de la tabla **sysarticles** .|  
|**dest_table**|**sysname**|El nombre de la tabla de destino de la tabla **sysarticles** .|  
|**owner**|**sysname**|El propietario de la suscripción.|  
|**cft_table**|**sysname**|El nombre de la tabla de conflictos de este artículo, para el tipo de replicación de actualización en cola.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
