---
description: MSsubscription_agents (Transact-SQL)
title: MSsubscription_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsubscription_agents
- MSsubscription_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_agents system table
ms.assetid: 86ad5891-0bef-4963-9381-7d5b45245a0c
author: cawrites
ms.author: chadam
ms.openlocfilehash: cd2189307d78bc686f179a1b5b441e4e1f9825da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211616"
---
# <a name="mssubscription_agents-transact-sql"></a>MSsubscription_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSsubscription_agents** se usa en agente de distribución y desencadenadores de suscripciones actualizables para realizar el seguimiento de las propiedades de la suscripción. Esta tabla se almacena en la base de datos de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. de la fila.|  
|**publisher**|**sysname**|El nombre del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos de publicación.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**subscription_type**|**int**|El tipo de suscripción:<br /><br /> 0 = Inserción.<br /><br /> 1 = Extracción<br /><br /> 2 = Extracción anónima|  
|**queue_id**|**sysname**|IDENTIFICADOR de la [!INCLUDE[msCoName](../../includes/msconame-md.md)] cola de mensajes en el publicador. *queue_id* está establecido en **SQL** para la actualización en cola basada en SQL.|  
|**update_mode**|**tinyint**|El tipo de actualización:<br /><br /> **0** = solo lectura.<br /><br /> **1** = actualización inmediata.<br /><br /> **2** = actualización en cola mediante Message Queue Server.<br /><br /> **3** = actualización inmediata con actualización en cola como conmutación por error mediante Message Queue Server.<br /><br /> **4** = actualización en cola mediante la cola de SQL Server.<br /><br /> **5** = actualización inmediata con conmutación por error de actualización en cola mediante SQL Server cola.|  
|**failover_mode**|**bit**|Si se selecciona un tipo de actualización por conmutación por error, se elige este tipo de conmutación por error:<br /><br /> **0** = se utiliza la actualización inmediata. No se habilita la conmutación por error.<br /><br /> **1** = se está utilizando la actualización en cola. Se habilita la conmutación por error. La cola que se utiliza para la conmutación por error se especifica en el valor *update_mode* .|  
|**spid**|**int**|Id. del proceso de sistema para la conexión que utiliza el Agente de distribución actualmente en ejecución o recientemente ejecutado.|  
|**login_time**|**datetime**|La fecha y la hora de la conexión del Agente de distribución actualmente en ejecución o recientemente ejecutado.|  
|**allow_subscription_copy**|**bit**|Especifica si se permite o no la capacidad de copiar de la base de datos de suscripciones.|  
|**attach_state**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**attach_version**|**binario (16)**|Identificador único que representa la versión de una suscripción adjunta.|  
|**last_sync_status**|**int**|El último estado del Agente de distribución actualmente en ejecución o recientemente ejecutado. El estado puede ser:<br /><br /> **1** = iniciado.<br /><br /> **2** = correcto.<br /><br /> **3** = en curso.<br /><br /> **4** = inactivo.<br /><br /> **5** = Reintentar.<br /><br /> **6** = error.|  
|**last_sync_summary**|**sysname**|El último mensaje del Agente de distribución actualmente en ejecución o recientemente ejecutado. El estado puede ser:<br /><br /> **Introducción.**<br /><br /> **Completa.**<br /><br /> **En curso.**<br /><br /> **Activos.**<br /><br /> **Realizar.**<br /><br /> **Puedan.**|  
|**last_sync_time**|**datetime**|Fecha y hora en que se actualizaron las columnas *last_sync_summary* y *last_sync_status* . Los Agentes de distribución anónimos o de extracción que se ejecutan como trabajos del servicio del Agente SQL Server no actualizan estas columnas. En su lugar, la información del historial se registra en la tabla del historial de trabajos en ese caso.|  
|**queue_server**|**sysname**|Solo para uso interno.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)  
  
  
