---
description: sp_replmonitorhelpsubscription (Transact-SQL)
title: sp_replmonitorhelpsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelpsubscription_TSQL
- sp_replmonitorhelpsubscription
helpviewer_keywords:
- sp_replmonitorhelpsubscription
ms.assetid: a681b2db-c82d-4624-a10c-396afb0ac42f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 898104516115207bd0cee45bf05f187922c13f6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204369"
---
# <a name="sp_replmonitorhelpsubscription-transact-sql"></a>sp_replmonitorhelpsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve información de estado actual sobre las suscripciones que pertenecen a una o más publicaciones en el publicador y devuelve una fila por cada suscripción devuelta. Este procedimiento almacenado, que se utiliza para supervisar la replicación, se ejecuta en el distribuidor en la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_replmonitorhelpsubscription [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @mode = ] mode ]  
    [ , [ @topnum = ] topnum ]   
    [ , [ @exclude_anonymous = ] exclude_anonymous ]   
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador cuyo estado se está supervisando. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL. Si es **null**, se devuelve información para todos los publicadores que utilizan el distribuidor.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos publicada. *publisher_db* es de **tipo sysname y su** valor predeterminado es NULL. Si es NULL, se devuelve información sobre todas las bases de datos publicadas en el publicador.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación que se está supervisando. *Publication* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @publication_type = ] publication_type` Si el tipo de publicación. *publication_type* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Publicación transaccional.|  
|**1**|Publicación de instantáneas.|  
|**2**|Publicación de combinación.|  
|NULL (predeterminado)|La replicación intenta determinar el tipo de publicación.|  
  
`[ @mode = ] mode` Es el modo de filtrado que se va a utilizar al devolver información de supervisión de suscripciones. *mode* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0** (valor predeterminado)|Devuelve todas las suscripciones.|  
|**1**|Solo devuelve las suscripciones con errores.|  
|**2**|Solo devuelve las suscripciones que han generado advertencias de métrica de umbral.|  
|**3**|Solo devuelve las suscripciones que o tienen errores o han generado advertencias de métrica de umbral.|  
|**4**|Devuelve las primeras 25 suscripciones con peor rendimiento.|  
|**5**|Devuelve las 50 suscripciones con un rendimiento peor.|  
|**6**|Solo devuelve las suscripciones que se están sincronizando en ese momento.|  
|**7**|Solo devuelve las suscripciones que no se están sincronizando en ese momento.|  
  
`[ @topnum = ] topnum` Restringe el conjunto de resultados solo al número especificado de suscripciones en la parte superior de los datos devueltos. *topnum* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @exclude_anonymous = ] exclude_anonymous` Es si se excluyen las suscripciones de extracción anónimas del conjunto de resultados. *exclude_anonymous* es de **bit** y su valor predeterminado es **0**; un valor de **1** significa que se excluyen las suscripciones anónimas y un valor de **0** significa que se incluyen.  
  
`[ @refreshpolicy = ] refreshpolicy` Solo para uso interno.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**status**|**int**|Examina el estado de todos los agentes de replicación asociados a la publicación, y devuelve el estado más alto encontrado en el orden siguiente:<br /><br /> **6** = error<br /><br /> **5** = reintentando<br /><br /> **2** = detenido<br /><br /> **4** = inactivo<br /><br /> **3** = en curso<br /><br /> **1** = iniciado|  
|**warning**|**int**|Advertencia de umbral máximo generada por una suscripción que pertenece a la publicación, que puede ser el resultado de OR lógico de uno o más de estos valores.<br /><br /> **1** = expiración: una suscripción a una publicación transaccional no se ha sincronizado en el umbral del período de retención.<br /><br /> **2** = latencia: el tiempo que se tarda en replicar los datos de un publicador transaccional en el suscriptor supera el umbral, en segundos.<br /><br /> **4** = mergeexpiration: una suscripción a una publicación de combinación no se ha sincronizado en el umbral del período de retención.<br /><br /> **8** = mergefastrunduration: el tiempo que se tarda en completar la sincronización de una suscripción de mezcla supera el umbral, en segundos, a través de una conexión de red rápida.<br /><br /> **16** = mergeslowrunduration: el tiempo que se tarda en completar la sincronización de una suscripción de mezcla supera el umbral, en segundos, a través de una conexión de red lenta o de acceso telefónico.<br /><br /> **32** = mergefastrunspeed: la tasa de entrega de filas durante la sincronización de una suscripción de mezcla no ha podido mantener la tasa de umbral, en filas por segundo, en una conexión de red rápida.<br /><br /> **64** = mergeslowrunspeed: la tasa de entrega de filas durante la sincronización de una suscripción de mezcla no ha podido mantener la tasa de umbral, en filas por segundo, en una conexión de red lenta o de acceso telefónico.|  
|**suscriptor**|**sysname**|Es el nombre del suscriptor.|  
|**subscriber_db**|**sysname**|Es el nombre de la base de datos utilizada para la suscripción.|  
|**publisher_db**|**sysname**|Es el nombre de la base de datos de publicación.|  
|**publicaciones**|**sysname**|Es el nombre de una publicación.|  
|**publication_type**|**int**|Es el tipo de publicación, que puede ser uno de estos valores:<br /><br /> **0** = publicación transaccional<br /><br /> **1** = publicación de instantáneas<br /><br /> **2** = publicación de combinación|  
|**subtipo**|**int**|Es el tipo de suscripción, que puede ser uno de los siguientes valores:<br /><br /> **0** = inserciones<br /><br /> **1** = extracción<br /><br /> **2** = anónimo|  
|**duplica**|**int**|La mayor latencia, en segundos, para los cambios de datos propagados por los agentes de distribución o de registro del LOG para una publicación transaccional.|  
|**latencythreshold**|**int**|La latencia máxima para la publicación transaccional por encima de la cual se genera una advertencia.|  
|**agentnotrunning**|**int**|Es la cantidad de tiempo, en horas, durante la que el agente no se ha ejecutado.|  
|**agentnotrunningthreshold**|**int**|Es el tiempo, en horas, que el agente no se ha ejecutado antes de que se genere una advertencia.|  
|**timetoexpiration**|**int**|Es el tiempo, en horas, antes de que expire la suscripción si no se sincroniza.|  
|**expirationthreshold**|**int**|Es el tiempo, en horas, en que se genera una advertencia antes de que expire la suscripción.|  
|**last_distsync**|**datetime**|Es la fecha y hora en que el Agente de distribución se ejecutó por última vez.|  
|**distribution_agentname**|**sysname**|Es el nombre del trabajo del Agente de distribución para la suscripción a una publicación transaccional.|  
|**mergeagentname**|**sysname**|Es el nombre del trabajo de Agente de mezcla para la suscripción a una publicación de combinación.|  
|**mergesubscriptionfriendlyname**|**sysname**|Es el nombre descriptivo dado a la suscripción.|  
|**mergeagentlocation**|**sysname**|Es el nombre del servidor en el que se ejecuta el Agente de mezcla.|  
|**mergeconnectiontype**|**int**|Conexión que se utiliza al sincronizar una suscripción a una publicación de combinación. Puede ser uno de los siguientes valores:<br /><br /> **1** = red de área local (LAN)<br /><br /> **2** = conexión de red de acceso telefónico<br /><br /> **3** = sincronización Web.|  
|**mergePerformance**|**int**|Rendimiento de la última sincronización con respecto a todas las sincronizaciones de la suscripción. Se basa en la tasa de entrega de la última sincronización dividida entre la media de todas las tasas de entrega anteriores.|  
|**mergerunspeed**|**float**|Tasa de entrega de la última sincronización de la suscripción.|  
|**mergerunduration**|**int**|Es el tiempo necesario para completar la última sincronización de la suscripción.|  
|**monitorranking**|**int**|Es el valor de categoría utilizado para ordenar las suscripciones en el conjunto de resultados. Puede ser uno de estos valores:<br /><br /> Para una publicación transaccional:<br /><br /> **60** = error<br /><br /> **56** = ADVERTENCIA: rendimiento crítico<br /><br /> **52** = ADVERTENCIA: con expiración en breve o expirada<br /><br /> **50** = ADVERTENCIA: suscripción no inicializada<br /><br /> **40** = reintentando comando con errores<br /><br /> **30** = no se está ejecutando (correcto)<br /><br /> **20** = en ejecución (iniciando, en ejecución o inactivo)<br /><br /> Para una publicación de combinación:<br /><br /> **60** = error<br /><br /> **56** = ADVERTENCIA: rendimiento crítico<br /><br /> **54** = ADVERTENCIA: mezcla de ejecución prolongada<br /><br /> **52** = ADVERTENCIA: expirando pronto<br /><br /> **50** = ADVERTENCIA: suscripción no inicializada<br /><br /> **40** = reintentando comando con errores<br /><br /> **30** = en ejecución (Inicio, ejecución o inactivo)<br /><br /> **20** = no se está ejecutando (correcto)|  
|**distributionagentjobid**|**binario (16)**|Id. del trabajo de Agente de distribución para las suscripciones a una publicación transaccional.|  
|**mergeagentjobid**|**binario (16)**|Id. del trabajo de Agente de mezcla para las suscripciones a una publicación de combinación.|  
|**distributionagentid**|**int**|Id. del trabajo de Agente de distribución para la suscripción.|  
|**distributionagentprofileid**|**int**|Identificador del perfil del agente utilizado por el Agente de distribución.|  
|**mergeagentid**|**int**|Identificador del trabajo del Agente de combinación para la suscripción.|  
|**mergeagentprofileid**|**int**|Id. del perfil de agente utilizado por el Agente de mezcla.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_replmonitorhelpsubscription** se usa con todos los tipos de replicación.  
  
 **sp_replmonitorhelpsubscription** ordena el conjunto de resultados en función de la gravedad del estado de la suscripción, que viene determinado por el valor de *monitorranking*. Por ejemplo, las filas de todas las suscripciones con un estado de error se colocan por encima de las filas de suscripciones con un estado de advertencia.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de base de datos **db_owner** o **replmonitor** en la base de datos de distribución pueden ejecutar **sp_replmonitorhelpsubscription**.  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la replicación mediante programación](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
