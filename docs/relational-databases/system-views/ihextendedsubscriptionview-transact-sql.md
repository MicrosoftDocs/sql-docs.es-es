---
description: IHextendedSubscriptionView (Transact-SQL)
title: IHextendedSubscriptionView (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHextendedSubscriptionView_TSQL
- IHextendedSubscriptionView
dev_langs:
- TSQL
helpviewer_keywords:
- IHextendedSubscriptionView view
ms.assetid: 124756a4-463a-4a81-bf5b-de7e8ffc7a62
author: stevestein
ms.author: sstein
ms.openlocfilehash: 75498189c413aa89a81d2c0e8086c0f5cc37e1a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208883"
---
# <a name="ihextendedsubscriptionview-transact-sql"></a>IHextendedSubscriptionView (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **IHextendedSubscriptionView** expone información sobre la suscripción a una publicación que no es de SQL Server. Esta vista se almacena en la base de datos de **distribución** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|El identificador único de un artículo.|  
|**dest_db**|**sysname**|El nombre de la base de datos de destino.|  
|**srvid**|**smallint**|El identificador único de un suscriptor.|  
|**login_name**|**sysname**|El inicio de sesión utilizado para establecer la conexión con un suscriptor.|  
|**distribution_jobid**|**binary**|Identifica el trabajo del Agente de distribución.|  
|**publisher_database_id**|**int**|Identifica la base de datos de publicaciones.|  
|**subscription_type**|**int**|El tipo de suscripción:<br /><br /> **0** = inserciones: el agente de distribución se ejecuta en el suscriptor.<br /><br /> **1** = extracción: el agente de distribución se ejecuta en el distribuidor.|  
|**sync_type**|**tinyint**|El tipo de sincronización inicial:<br /><br /> **1** = automática<br /><br /> **2** = ninguno|  
|**status**|**tinyint**|El estado de la suscripción:<br /><br /> **0** = inactivo<br /><br /> **1** = suscrito<br /><br /> **2** = activo|  
|**snapshot_seqno_flag**|**bit**|Indica si se utiliza el número de secuencia de instantánea.|  
|**independent_agent**|**bit**|Especifica si hay un Agente de distribución independiente para esta publicación.<br /><br /> **0** = la publicación utiliza una agente de distribución compartida y cada par de base de datos del publicador/base de datos de suscriptor tiene un único agente compartido.<br /><br /> **1** = hay un agente de distribución independiente para esta publicación.|  
|**subscription_time**|**datetime**|Solo para uso interno.|  
|**loopback_detection**|**bit**|Se aplica a las suscripciones que forman parte de una topología de replicación transaccional bidireccional. La detección de bucles de retorno determina si el Agente de distribución envía las transacciones originadas en el suscriptor al mismo suscriptor:<br /><br /> **1** = no devuelve.<br /><br /> **0** = devuelve.|  
|**agent_id**|**int**|El identificador único del Agente de distribución.|  
|**update_mode**|**tinyint**|Indica el tipo del modo de actualización, que puede ser uno de los siguientes:<br /><br /> **0** = solo lectura.<br /><br /> **1** = actualización inmediata.<br /><br /> **2** = actualización en cola mediante Message Queue Server.<br /><br /> **3** = actualización inmediata con actualización en cola como conmutación por error mediante Message Queue Server.<br /><br /> **4** = actualización en cola mediante la cola de SQL Server.<br /><br /> **5** = actualización inmediata con conmutación por error de actualización en cola mediante SQL Server cola.|  
|**publisher_seqno**|**varbinary(16)**|Número de secuencia de la transacción en el publicador de esta suscripción.|  
|**ss_cplt_seqno**|**varbinary(16)**|El número de secuencia utilizado para indicar el término del procesamiento de instantáneas simultáneas.|  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
