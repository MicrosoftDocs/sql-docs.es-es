---
description: IHpublisherindexes (Transact-SQL)
title: IHpublisherindexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHpublisherindexes
- IHpublisherindexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublisherindexes system table
ms.assetid: 6008ef89-eeb9-46dc-93a2-f7623298cf0f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5e27ab9816b6b1e96a31e9869b6c41db4c93c25b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201639"
---
# <a name="ihpublisherindexes-transact-sql"></a>IHpublisherindexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla del sistema **IHpublisherindexes** contiene una fila por cada índice replicado desde publicadores que no son de SQL Server mediante el distribuidor actual. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisherindex_id**|**int**|Identifica un índice publicado.|  
|**table_id**|**int**|Identifica la tabla de [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md) a la que pertenece el índice.|  
|**publisher_id**|**smallint**|Identifica el publicador que no es de SQL Server desde el cual se está publicando el índice.|  
|**name**|**sysname**|Nombre del índice publicado.|  
|**type**|**nvarchar(255)**|Un tipo de índice admitido de la tabla del sistema [IHindextypes](../../relational-databases/system-tables/ihindextypes-transact-sql.md) .|  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
