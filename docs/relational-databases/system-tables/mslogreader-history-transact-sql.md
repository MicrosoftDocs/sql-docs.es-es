---
description: MSlogreader_history (Transact-SQL)
title: MSlogreader_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSlogreader_history_TSQL
- MSlogreader_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSlogreader_history system table
ms.assetid: 2e399fa1-3591-4c1c-96b7-7964fe82c7c4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 71963149d6ad8be972acd6a2da3380212eb8548a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212309"
---
# <a name="mslogreader_history-transact-sql"></a>MSlogreader_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSlogreader_history** contiene filas de historial para los agentes de registro del log asociados al distribuidor local. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|Id. del Agente de registro del LOG.|  
|**runstatus**|**int**|Estado de ejecución:<br /><br /> 1 = Iniciada.<br /><br /> 2 = Correcto.<br /><br /> 3 = En curso.<br /><br /> 4 = Inactiva.<br /><br /> 5 = Reintentar.<br /><br /> 6 = Error.|  
|**start_time**|**datetime**|Hora a la que comienza la ejecución del trabajo.|  
|**time**|**datetime**|Hora a la que se registra el mensaje.|  
|**duration**|**int**|Duración, en segundos, de la sesión del mensaje.|  
|**comentarios**|**nvarchar(255)**|Texto del mensaje.|  
|**xact_seqno**|**varbinary(16)**|Número de secuencia de la última transacción procesada.|  
|**delivery_time**|**int**|La hora en que se entrega la primera transacción.|  
|**delivered_transactions**|**int**|Número total de transacciones entregadas en la sesión.|  
|**delivered_commands**|**int**|Número total de comandos entregados en la sesión.|  
|**average_commands**|**int**|Número promedio de comandos entregados en la sesión.|  
|**delivery_rate**|**float**|Promedio de comandos entregados por segundo.|  
|**delivery_latency**|**int**|Latencia entre la entrada del comando en la base de datos publicada y su entrada en la base de datos de distribución. En milisegundos.|  
|**error_id**|**int**|IDENTIFICADOR del error en la **MSrepl_error** tabla del sistema.|  
|**timestamp**|**timestamp**|La columna de marca de tiempo de esta tabla.|  
|**updateable_row**|**bit**|Se establece en **1** si se puede sobrescribir la fila de historial.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
