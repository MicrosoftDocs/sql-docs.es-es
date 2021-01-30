---
description: MSrepl_originators (Transact-SQL)
title: MSrepl_originators (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSrepl_originators_TSQL
- MSrepl_originators
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_originators system table
ms.assetid: a3ac20a6-73f6-4fdc-ad5f-5f72746c9871
author: cawrites
ms.author: chadam
ms.openlocfilehash: b9c5a5690f6349975a2edafd7715d81c769ce31c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210707"
---
# <a name="msrepl_originators-transact-sql"></a>MSrepl_originators (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSrepl_originators** contiene una fila por cada suscriptor actualizable del que se originó la transacción. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identifica al suscriptor que se actualiza.|  
|**publisher_database_id**|**int**|Identifica la base de datos de publicaciones.|  
|**srvname**|**sysname**|El nombre del servidor que se actualiza.|  
|**nombrebd**|**sysname**|El nombre de la base de datos que se actualiza.|  
|**publication_id**|**int**|Identifica la publicación.|  
|**dbversion**|**int**|Identifica la versión de la base de datos.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
