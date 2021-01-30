---
description: sp_replmonitorhelppublication (Transact-SQL)
title: sp_replmonitorhelppublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelppublication_TSQL
- sp_replmonitorhelppublication
helpviewer_keywords:
- sp_replmonitorhelppublication
ms.assetid: 7928c50c-617f-41c5-9e0f-4e42e8be55dc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 70f401f52926fc389232b82167a94f9a4ded1e0e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193746"
---
# <a name="sp_replmonitorhelppublication-transact-sql"></a>sp_replmonitorhelppublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve la información de estado actual para una o varias publicaciones del publicador. Este procedimiento almacenado, que se utiliza para supervisar la replicación, se ejecuta en el distribuidor en la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_replmonitorhelppublication [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db'   
    [ , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador cuyo estado se está supervisando. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL. Si **es null**, se devolverá información para todos los publicadores que usen el distribuidor.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos publicada. *publisher_db* es de **tipo sysname y su** valor predeterminado es NULL. Si es NULL, se devuelve información sobre todas las bases de datos publicadas en el publicador.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación que se está supervisando. *Publication* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @publication_type = ] publication_type` Si el tipo de publicación. *publication_type* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Publicación transaccional.|  
|**1**|Publicación de instantáneas.|  
|**2**|Publicación de combinación.|  
|NULL (predeterminado)|La replicación intenta determinar el tipo de publicación.|  
  
`[ @refreshpolicy = ] refreshpolicy` Solo para uso interno.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher_db**|**sysname**|Es el nombre del publicador.|  
|**publicaciones**|**sysname**|Es el nombre de una publicación.|  
|**publication_type**|**int**|Es el tipo de publicación y puede tener uno de los valores siguientes.<br /><br /> **0** = publicación transaccional<br /><br /> **1** = publicación de instantáneas<br /><br /> **2** = publicación de combinación|  
|**status**|**int**|Estado máximo de todos los agentes de replicación asociados a la publicación. Puede ser uno de estos valores.<br /><br /> **1** = iniciado<br /><br /> **2** = correcto<br /><br /> **3** = en curso<br /><br /> **4** = inactivo<br /><br /> **5** = reintentando<br /><br /> **6** = error|  
|**warning**|**int**|Advertencia de umbral máximo generada por una suscripción que pertenece a la publicación, que puede ser el resultado de OR lógico de uno o más de estos valores.<br /><br /> **1** = expiración: una suscripción a una publicación transaccional no se ha sincronizado en el umbral del período de retención.<br /><br /> **2** = latencia: el tiempo que se tarda en replicar los datos de un publicador transaccional en el suscriptor supera el umbral, en segundos.<br /><br /> **4** = mergeexpiration: una suscripción a una publicación de combinación no se ha sincronizado en el umbral del período de retención.<br /><br /> **8** = mergefastrunduration: el tiempo que se tarda en completar la sincronización de una suscripción de mezcla supera el umbral, en segundos, a través de una conexión de red rápida.<br /><br /> **16** = mergeslowrunduration: el tiempo que se tarda en completar la sincronización de una suscripción de mezcla supera el umbral, en segundos, a través de una conexión de red lenta o de acceso telefónico.<br /><br /> **32** = mergefastrunspeed: la tasa de entrega de filas durante la sincronización de una suscripción de mezcla no ha podido mantener la tasa de umbral, en filas por segundo, en una conexión de red rápida.<br /><br /> **64** = mergeslowrunspeed: la tasa de entrega de filas durante la sincronización de una suscripción de mezcla no ha podido mantener la tasa de umbral, en filas por segundo, en una conexión de red lenta o de acceso telefónico.|  
|**worst_latency**|**int**|La mayor latencia, en segundos, para los cambios de datos propagados por los agentes de distribución o de registro del LOG para una publicación transaccional.|  
|**best_latency**|**int**|La menor latencia, en segundos, para los cambios de datos propagados por los agentes de distribución o de registro del LOG para una publicación transaccional.|  
|**average_latency**|**int**|La latencia promedio, en segundos, para los cambios de datos propagados por los agentes de distribución o de registro del LOG para una publicación transaccional.|  
|**last_distsync**|**datetime**|Es el valor de datetime correspondiente a la última ejecución del Agente de distribución.|  
|**políticas**|**int**|Es el período de retención de la publicación.|  
|**latencythreshold**|**int**|Es el umbral de latencia definido para la publicación transaccional.|  
|**expirationthreshold**|**int**|Es el umbral de expiración definido para la publicación si se trata de una publicación de combinación.|  
|**agentnotrunningthreshold**|**int**|Es el umbral definido para el período de tiempo más largo transcurrido sin que se haya ejecutado un agente.|  
|**subscriptioncount**|**int**|Es el número de suscripciones de una publicación.|  
|**runningdistagentcount**|**int**|Es el número de agentes de distribución que se están ejecutando para la publicación.|  
|**snapshot_agentname**|**sysname**|Nombre del trabajo del Agente de instantáneas para la publicación.|  
|**logreader_agentname**|**sysname**|Nombre del trabajo del Agente de registro del LOG para la publicación transaccional.|  
|**qreader_agentname**|**sysname**|Nombre del trabajo del Agente de lectura de cola para una publicación transaccional que admite la actualización en cola.|  
|**worst_runspeedPerf**|**int**|Es el mayor tiempo de sincronización de la publicación de combinación.|  
|**best_runspeedPerf**|**int**|Es el menor tiempo de sincronización de la publicación de combinación.|  
|**average_runspeedPerf**|**int**|Es el tiempo medio de sincronización de la publicación de combinación.|  
|**retention_period_unit**|**int**|Es la unidad que se usa para expresar la *retención*.|  
|**publisher**|**sysname**|Nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que publica la publicación.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_replmonitorhelppublication** se usa con todos los tipos de replicación.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de base de datos **db_owner** o **replmonitor** en la base de datos de distribución pueden ejecutar **sp_replmonitorhelppublication**.  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la replicación mediante programación](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
