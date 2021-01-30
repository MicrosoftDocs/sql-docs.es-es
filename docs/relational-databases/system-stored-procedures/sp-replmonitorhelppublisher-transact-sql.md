---
description: sp_replmonitorhelppublisher (Transact-SQL)
title: sp_replmonitorhelppublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelppublisher_TSQL
- sp_replmonitorhelppublisher
helpviewer_keywords:
- sp_replmonitorhelppublisher
ms.assetid: 171501fe-4b74-4647-96c3-7691c777e01b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6252047b509af4311045185197994426c1d019ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198284"
---
# <a name="sp_replmonitorhelppublisher-transact-sql"></a>sp_replmonitorhelppublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve información sobre el estado actual para uno o más publicadores asociados a un distribuidor. Este procedimiento almacenado, que se utiliza para supervisar la replicación, se ejecuta en el distribuidor en la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_replmonitorhelppublisher [ [ @publisher = ] 'publisher' ]  
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador cuyo estado se está supervisando. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL. Si el valor es NULL, se devuelve información para todos los publicadores que utilizan el distribuidor.  
  
`[ @refreshpolicy = ] refreshpolicy` Solo para uso interno.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Es el nombre de un publicador.|  
|**distribution_db**|**sysname**|Es el nombre de la base de datos de distribución utilizada por un publicador.|  
|**status**|**int**|Estado máximo de todos los agentes de replicación asociados a las publicaciones de este publicador, el cual puede ser uno de los valores siguientes.<br /><br /> **1** = iniciado<br /><br /> **2** = correcto<br /><br /> **3** = en curso<br /><br /> **4** = inactivo<br /><br /> **5** = reintentando<br /><br /> **6** = error|  
|**warning**|**int**|Advertencia de umbral máximo generada por una suscripción perteneciente a una publicación de este publicador, la cual puede ser el resultado lógico OR de uno o más de estos valores.<br /><br /> **1** = expiración: una suscripción a una publicación transaccional no se ha sincronizado en el umbral del período de retención.<br /><br /> **2** = latencia: el tiempo que se tarda en replicar los datos de un publicador transaccional en el suscriptor supera el umbral, en segundos.<br /><br /> **4** = mergeexpiration: una suscripción a una publicación de combinación no se ha sincronizado en el umbral del período de retención.<br /><br /> **8** = mergefastrunduration: el tiempo que se tarda en completar la sincronización de una suscripción de mezcla supera el umbral, en segundos, a través de una conexión de red rápida.<br /><br /> **16** = mergeslowrunduration: el tiempo que se tarda en completar la sincronización de una suscripción de mezcla supera el umbral, en segundos, a través de una conexión de red lenta o de acceso telefónico.<br /><br /> **32** = mergefastrunspeed: la tasa de entrega de filas durante la sincronización de una suscripción de mezcla no ha podido mantener la tasa de umbral, en filas por segundo, en una conexión de red rápida.<br /><br /> **64** = mergeslowrunspeed: la tasa de entrega de filas durante la sincronización de una suscripción de mezcla no ha podido mantener la tasa de umbral, en filas por segundo, en una conexión de red lenta o de acceso telefónico.|  
|**publicationcount**|**int**|Es el número de publicaciones pertenecientes al publicador.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_replmonitorhelppublisher** se usa con todos los tipos de replicación.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** en el distribuidor o los miembros de los roles fijos de base de datos **db_owner** o **replmonitor** de la base de datos de distribución pueden ejecutar **sp_replmonitorhelppublisher**.  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la replicación mediante programación](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
