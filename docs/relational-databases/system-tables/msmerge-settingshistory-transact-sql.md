---
description: MSmerge_settingshistory (Transact-SQL)
title: MSmerge_settingshistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_settingshistory
- MSmerge_settingshistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_settingshistory system table
ms.assetid: 0bdf2d5f-5502-44cd-aa9d-2d5006ad20ce
author: cawrites
ms.author: chadam
ms.openlocfilehash: 26ea21293ffa0ba1f4db014afdf2cd8adad14e29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205914"
---
# <a name="msmerge_settingshistory-transact-sql"></a>MSmerge_settingshistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_settingshistory** se utiliza para mantener un historial de los cambios realizados en el artículo y las propiedades de la publicación para la replicación de mezcla, con una fila por cada cambio realizado en una topología de replicación de mezcla. Esta tabla también almacena información acerca de cuándo se establecieron los valores de propiedad iniciales. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**EventTime**|**datetime**|Fecha y hora del evento.|  
|**pubid**|**uniqueidentifier**|Número de identificación único de una determinada publicación.|  
|**artid**|**uniqueidentifier**|El número de identificación único del artículo indicado.|  
|**eventType**|**tinyint**|Especifica el tipo de evento que se va a registrar, que puede ser uno de los siguientes:<br /><br /> **1** : configuración de la propiedad nivel de publicación inicial.<br /><br /> **2** -cambio en una propiedad de publicación.<br /><br /> **101** : configuración inicial de la propiedad del artículo.<br /><br /> **102** : cambiar en una propiedad de artículo.|  
|**propertyname**|**sysname**|Nombre de la propiedad establecida o cambiada.|  
|**previousvalue**|**sysname**|Valor de la propiedad anterior si se ha cambiado la propiedad.|  
|**nuevovalor**|**sysname**|Valor cambiado o creado en la propiedad.|  
|**eventtext**|**nvarchar (2000)**|Cadena de caracteres que describe el evento.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
