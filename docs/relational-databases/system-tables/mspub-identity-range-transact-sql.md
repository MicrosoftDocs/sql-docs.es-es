---
description: MSpub_identity_range (Transact-SQL)
title: MSpub_identity_range (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSpub_identity_range_TSQL
- MSpub_identity_range
dev_langs:
- TSQL
helpviewer_keywords:
- MSpub_identity_range system table
ms.assetid: 68746eef-32e1-42bc-aff0-9798cd0e88b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7a67c3f0bcfc4ac5f5a35990aefe5aa02e79e50d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200260"
---
# <a name="mspub_identity_range-transact-sql"></a>MSpub_identity_range (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En la tabla **MSpub_identity_range** se proporciona compatibilidad con la administración del intervalo de identidad. Esta tabla se almacena en la base de datos de publicaciones y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**objid**|**int**|Identificador de la tabla cuya columna de identidad se administra mediante la replicación.|  
|**range**|**bigint**|Controla el tamaño del intervalo de los valores de identidad consecutivos que podrían asignarse a la suscripción en un ajuste.|  
|**pub_range**|**bigint**|Controla el tamaño del intervalo de los valores de identidad consecutivos que podrían asignarse a la publicación en un ajuste.|  
|**current_pub_range**|**bigint**|Intervalo actual que utiliza la publicación. Puede ser diferente de *pub_range* si se ve después de haber sido modificado por **sp_changearticle** y antes del siguiente ajuste del intervalo.|  
|**threshold**|**int**|Valor de porcentaje que controla cuándo el Agente de distribución asigna un nuevo intervalo de identidad. Cuando se utiliza el porcentaje de valores especificado en *umbral* , el agente de distribución crea un nuevo intervalo de identidad.|  
|**last_seed**|**bigint**|Límite inferior del intervalo actual.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
