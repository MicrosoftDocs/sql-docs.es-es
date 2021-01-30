---
description: logmarkhistory (Transact-SQL)
title: logmarkhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- logmarkhistory
- logmarkhistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- logmarkhistory system table
ms.assetid: 5c1becc5-f34e-4869-bf69-dfafab684540
author: cawrites
ms.author: chadam
ms.openlocfilehash: e9da84d86ee40e39a0ed41dfc23a728593382436
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160540"
---
# <a name="logmarkhistory-transact-sql"></a>logmarkhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada transacción marcada que se ha confirmado. Esta tabla se almacena en la base de datos **msdb** .  
  

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Base de datos local donde tiene lugar la transacción marcada.|  
|**mark_name**|**nvarchar(128)**|Nombre proporcionado por el usuario para la transacción marcada.|  
|**description**|**nvarchar(255)**|Descripción proporcionada por el usuario para la transacción marcada. Puede ser NULL.|  
|**user_name**|**nvarchar(128)**|Nombre de usuario de la base de datos que llevó a cabo la transacción marcada. Puede ser NULL.|  
|**LSN**|**numeric(25,0)**|Número de secuencia de registro de la transacción donde tuvo lugar la marca.|  
|**mark_time**|**datetime**|Hora de la confirmación de la transacción marcada (hora local).|  
  
## <a name="see-also"></a>Consulte también  
 [Restaurar una base de datos en una transacción marcada &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)   
 [Usar transacciones marcadas para recuperar bases de datos relacionadas sistemáticamente &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)   
 [Tablas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
