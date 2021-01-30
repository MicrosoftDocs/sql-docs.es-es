---
description: MSrepl_backup_lsns (Transact-SQL)
title: MSrepl_backup_lsns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSrepl_backup_lsns_TSQL
- MSrepl_backup_lsns
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_backup_Isns system table
ms.assetid: de06c349-82a8-48c6-b602-b5d6938514f6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4d7b13788fd2230dd5252c837864dfb49e8de915
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182591"
---
# <a name="msrepl_backup_lsns-transact-sql"></a>MSrepl_backup_lsns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSrepl_backup_lsns** contiene números de secuencia de registro de transacciones (LSN) para admitir la opción ' Sync with backup ' de la base de datos de distribución. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher_database_id**|**int**|El Id. de la base de datos del publicador.|  
|**valid_xact_id**|**varbinary(16)**|El Id. de la transacción que se va a enviar al publicador para marcar el punto de truncamiento del registro. Solo se usa si la base de datos de distribución está en modo de "sincronización con copia de seguridad". Contiene el Id. de la última transacción replicada en la base de datos de distribución de la que se ha hecho una copia de seguridad. Se enviará al publicador para que el lector del registro marque el punto de truncamiento del registro.|  
|**valid_xact_seqno**|**varbinary(16)**|El número de secuencia de la transacción que se va a enviar al publicador para marcar el punto de truncamiento del registro. Se utiliza solamente si la base de datos de distribución está en el modo 'sync with backup'. Es el número de secuencia de registro de la última transacción replicada en la base de datos de distribución de la que se hecho una copia de seguridad. Se enviará al publicador para que el lector del registro marque el punto de truncamiento del registro.|  
|**next_xact_id**|**varbinary(16)**|El número de secuencia de registro temporal que utilizan las operaciones de copia de seguridad.|  
|**nextx_xact_seqno**|**varbinary(16)**|El número de secuencia de registro temporal que utilizan las operaciones de copia de seguridad.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
