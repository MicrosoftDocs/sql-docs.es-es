---
description: MSsubscriptions (Transact-SQL)
title: MSsubscriptions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsubscriptions_TSQL
- MSsubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscriptions system table
ms.assetid: b7e8301d-d115-41f6-8d4f-e0d25f453b25
author: cawrites
ms.author: chadam
ms.openlocfilehash: c03bbca418d75cfc3dcb2df0f476646f5f5dffcb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211589"
---
# <a name="mssubscriptions-transact-sql"></a>MSsubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSsubscriptions** contiene una fila por cada artículo publicado en una suscripción a la que presta servicio el distribuidor local. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher_database_id**|**int**|El Id. de la base de datos del publicador.|  
|**publisher_id**|**smallint**|IDENTIFICADOR del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador.|  
|**publication_id**|**int**|Id. de la publicación.|  
|**article_id**|**int**|Id. del artículo.|  
|**subscriber_id**|**smallint**|IDENTIFICADOR del suscriptor.|  
|**subscriber_db**|**sysname**|El nombre de la base de datos de suscripciones.|  
|**subscription_type**|**int**|El tipo de suscripción:<br /><br /> **0** = extracción.<br /><br /> **1** = extracción.<br /><br /> **2** = anónimo.|  
|**sync_type**|**tinyint**|Tipo de sincronización:<br /><br /> **1** = automático.<br /><br /> **2** = sin sincronización.|  
|**status**|**tinyint**|El estado de la suscripción:<br /><br /> **0** = inactivo.<br /><br /> **1** = suscrito.<br /><br /> **2** = activo.|  
|**subscription_seqno**|**varbinary(16)**|Número de secuencia de la transacción de instantáneas.|  
|**snapshot_seqno_flag**|**bit**|Indica el origen del número de secuencia de la transacción de instantáneas, donde el valor **1** significa que **subscription_seqno** es el número de secuencia de la instantánea.|  
|**independent_agent**|**bit**|Indica si hay un agente de distribución independiente para esta publicación.|  
|**subscription_time**|**datetime**|Solo para uso interno.|  
|**loopback_detection**|**bit**|Se aplica a las suscripciones que forman parte de una topología de replicación transaccional bidireccional. La detección de bucles de retorno determina si el Agente de distribución envía las transacciones originadas en el suscriptor al mismo suscriptor:<br /><br /> **1** = no devuelve.<br /><br /> **0** = devuelve.<br /><br />|  
|**agent_id**|**int**|Id. del agente.|  
|**update_mode**|**tinyint**|Tipo de actualización.|  
|**publisher_seqno**|**varbinary(16)**|Número de secuencia de la transacción en el publicador de esta suscripción.|  
|**ss_cplt_seqno**|**varbinary(16)**|El número de secuencia utilizado para indicar el término del procesamiento de instantáneas simultáneas.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helpsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
