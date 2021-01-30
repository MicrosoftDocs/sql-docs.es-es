---
title: sp_replmonitorchangepublicationthreshold (T-SQL)
description: Describe el sp_replmonitorchangepublicationthreshold procedimiento almacenado que cambia la métrica del umbral de supervisión de una publicación.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorchangepublicationthreshold_TSQL
- sp_replmonitorchangepublicationthreshold
helpviewer_keywords:
- sp_replmonitorchangepublicationthreshold
ms.assetid: 2c3615d8-4a1a-4162-b096-97aefe6ddc16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 62f360416f46eee0bbe685f6ebd1af7520526ee6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204391"
---
# <a name="sp_replmonitorchangepublicationthreshold-transact-sql"></a>sp_replmonitorchangepublicationthreshold (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cambia la métrica del umbral de supervisión de una publicación. Este procedimiento almacenado, que se utiliza para supervisar la replicación, se ejecuta en el distribuidor en la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_replmonitorchangepublicationthreshold [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @metric_id = ] metric_id ]   
    [ , [ @thresholdmetricname = ] 'thresholdmetricname'   
    [ , [ @value = ] value ]   
    [ , [ @shouldalert = ] shouldalert ]   
    [ , [ @mode = ] mode ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos publicada. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación para la que se cambian los atributos del umbral de supervisión. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication_type = ] publication_type` Si el tipo de publicación. *publication_type* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Publicación transaccional.|  
|**1**|Publicación de instantáneas.|  
|**2**|Publicación de combinación.|  
|NULL (predeterminado)|La replicación intenta determinar el tipo de publicación.|  
  
`[ @metric_id = ] metric_id` Es el identificador de la métrica de umbral de publicación que se va a cambiar. *metric_id* es de **tipo int**, su valor predeterminado es NULL y puede tener uno de estos valores.  
  
|Value|Nombre de la métrica|  
|-----------|-----------------|  
|**1**|**expiration** : supervisa la expiración inminente de suscripciones a publicaciones transaccionales.|  
|**2**|**latency** : supervisa el rendimiento de suscripciones a publicaciones transaccionales.|  
|**4**|**mergeexpiration** : supervisa la expiración inminente de suscripciones a publicaciones de combinación.|  
|**5**|**mergeslowrunduration** : supervisa la duración de las sincronizaciones de mezcla en conexiones de bajo ancho de banda (acceso telefónico).|  
|**6**|**mergefastrunduration** : supervisa la duración de las sincronizaciones de mezcla en conexiones de red de área local (LAN) de gran ancho de banda.|  
|**7**|**mergefastrunspeed** : supervisa la velocidad de sincronización de sincronizaciones de mezcla en conexiones de red de área local (LAN) de gran ancho de banda.|  
|**8**|**mergeslowrunspeed** : supervisa la velocidad de sincronización de sincronizaciones de mezcla en conexiones de bajo ancho de banda (acceso telefónico).|  
  
 Debe especificar *metric_id* o *thresholdmetricname*. Si se especifica *thresholdmetricname* , *METRIC_ID* debe ser null.  
  
`[ @thresholdmetricname = ] 'thresholdmetricname'` Es el nombre de la métrica de umbral de la publicación que se va a cambiar. *thresholdmetricname* es de **tipo sysname y su** valor predeterminado es NULL. Debe especificar *thresholdmetricname* o *metric_id*. Si se especifica *metric_id* , *THRESHOLDMETRICNAME* debe ser null.  
  
`[ @value = ] value` Es el nuevo valor de la métrica de umbral de la publicación. *Value* es de **tipo int** y su valor predeterminado es NULL. Si es **null**, no se actualiza el valor de métrica.  
  
`[ @shouldalert = ] shouldalert` Indica si se genera una alerta cuando se alcanza una métrica de umbral de la publicación. *shouldalert* es de **bit** y su valor predeterminado es NULL. Un valor de **1** significa que se genera una alerta y un valor de **0** significa que no se genera una alerta.  
  
`[ @mode = ] mode` Es si está habilitada la métrica de umbral de la publicación. el *modo* es **tinyint** y su valor predeterminado es **1**. Un valor de **1** significa que la supervisión de esta métrica está habilitada y un valor de **2** significa que la supervisión de esta métrica está deshabilitada.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_replmonitorchangepublicationthreshold** se usa con todos los tipos de replicación.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de base de datos **db_owner** o **replmonitor** de la base de datos de distribución pueden ejecutar **sp_replmonitorchangepublicationthreshold**.  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la replicación mediante programación](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
