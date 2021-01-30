---
description: MSdistribution_history (Transact-SQL)
title: MSdistribution_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdistribution_history
- MSdistribution_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistribution_history system table
ms.assetid: 55665bd2-9e1d-4efc-8f60-c63a24f66b28
author: cawrites
ms.author: chadam
ms.openlocfilehash: c3dc79e93c8d7225a2dcabec47814ad870bdaeca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160393"
---
# <a name="msdistribution_history-transact-sql"></a>MSdistribution_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSdistribution_history** contiene filas de historial para los agentes de distribución asociados al distribuidor local. Esta tabla se almacena en la base de datos de distribución.  
  
## <a name="definition"></a>Definición  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|Id. del Agente de distribución.|  
|**runstatus**|**int**|El estado de ejecución:<br /><br /> **1** = Inicio.<br /><br /> **2** = correcto.<br /><br /> **3** = en curso.<br /><br /> **4** = inactivo.<br /><br /> **5** = Reintentar.<br /><br /> **6** = error.|  
|**start_time**|**datetime**|Hora a la que comienza la ejecución del trabajo.|  
|**time**|**datetime**|Hora a la que se registra el mensaje.|  
|**duration**|**int**|Duración, en segundos, de la sesión del mensaje.|  
|**comentarios**|**nvarchar(4000)**|Texto del mensaje.|  
|**xact_seqno**|**varbinary(16)**|Número de secuencia de la última transacción procesada.|  
|**current_delivery_rate**|**float**|Número promedio de comandos entregados por segundo desde la última entrada del historial.|  
|**current_delivery_latency**|**int**|Latencia entre la entrada del comando en la base de datos de distribución y su aplicación al suscriptor desde la última entrada del historial. En milisegundos.|  
|**delivered_transactions**|**int**|Número total de transacciones entregadas en la sesión.|  
|**delivered_commands**|**int**|Número total de comandos entregados en la sesión.|  
|**average_commands**|**int**|Número promedio de comandos entregados en la sesión.|  
|**delivery_rate**|**float**|Promedio de comandos entregados por segundo.|  
|**delivery_latency**|**int**|Latencia entre la entrada del comando en la base de datos de distribución y su aplicación al suscriptor. En milisegundos.|  
|**total_delivered_commands**|**bigint**|Número total de comandos entregados desde la creación de la suscripción.|  
|**error_id**|**int**|IDENTIFICADOR del error en la **MSrepl_error** tabla del sistema.|  
|**updateable_row**|**bit**|Se establece en **1** si se puede sobrescribir la fila de historial.|  
|**timestamp**|**timestamp**|La columna de marca de tiempo de esta tabla.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
