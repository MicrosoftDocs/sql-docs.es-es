---
description: MSpeer_lsns (Transact-SQL)
title: MSpeer_lsns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSpeer_lsns
- MSpeer_lsns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_lsns system table
ms.assetid: 0ba33907-601b-4c3d-8099-2663f680a161
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8316d023cb030e2eba09aff82d4e941cf551d1a5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205859"
---
# <a name="mspeer_lsns-transact-sql"></a>MSpeer_lsns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **Mspeer_lsns** se usa para asignar cada transacción a una suscripción en una topología de replicación punto a punto. Esta tabla se almacena en cada base de datos de publicación en una topología de replicación punto a punto y en la base de datos de suscripciones de todos los suscriptores en una publicación punto a punto. Para obtener más información sobre este tipo de topología de replicación transaccional, vea [replicación transaccional punto a punto](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md). Esta tabla se almacena en la base de datos de publicación.  
  
## <a name="definition"></a>Definición  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identifica un LSN punto a punto.|  
|**last_updated**|**datetime**|**Fecha y hora** en que se realizó la última actualización de la fila.|  
|**llegar**|**sysname**|Nombre del publicador que originó la transacción.|  
|**originator_db**|**sysname**|Nombre de la base de datos en que se originó la transacción.|  
|**originator_publication**|**sysname**|Nombre de la publicación en que se originó la transacción.|  
|**originator_publication_id**|**int**|Identificador de la publicación en que se originó la transacción.|  
|**originator_db_version**|**int**|Identifica el número de versión de la base de datos de origen.|  
|**originator_lsn**|**int**|Identifica el LSN en la publicación de origen.|  
|**originator_version**|**int**|Identifica el número de versión del publicador.|  
|**originator_id**|**smallint**|Identifica cada nodo de la topología para detectar conflictos. Para más información, consulte [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
