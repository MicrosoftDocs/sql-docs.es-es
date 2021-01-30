---
description: MSmerge_genhistory (Transact-SQL)
title: MSmerge_genhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_genhistory_TSQL
- MSmerge_genhistory
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_genhistory system table
ms.assetid: 475d08ae-eb8b-49de-afd6-33c96ab8004d
author: cawrites
ms.author: chadam
ms.openlocfilehash: a56a77aebc8b7643a178d23b7f5b444270091106
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209615"
---
# <a name="msmerge_genhistory-transact-sql"></a>MSmerge_genhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_genhistory** contiene una fila por cada generación de la que un suscriptor conoce (dentro del período de retención). Se utiliza para evitar enviar las generaciones comunes durante los intercambios y para volver a sincronizar los suscriptores restaurados a partir de copias de seguridad. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**guidsrc**|**uniqueidentifier**|Identificador global de los cambios que ha identificado la generación en el suscriptor.|  
|**pubid**|**uniqueidentifier**|Identificador de publicación.|  
|**última**|**bigint**|Valor de generación.|  
|**art_nick**|**int**|El alias del artículo.|  
|**nicknames**|**varbinary (1001)**|Lista de alias de otros suscriptores de los que se conoce que han tenido esta generación. Se usa para evitar enviar una generación a un suscriptor que ya ha visto los cambios. Los alias de la lista se mantienen ordenados para que las búsquedas sean más eficaces. Si hay más alias de los que caben en el campo, no podrán aprovechar esta optimización.|  
|**coldate**|**datetime**|Fecha en que se agrega la generación actual a la tabla.|  
|**genstatus**|**tinyint**|La generación puede tener los estados siguientes:<br /><br /> **0** = abierto.<br /><br /> **1** = cerrado.<br /><br /> **2** = cerrada y originaria en otro suscriptor.|  
|**changecount**|**int**|Número de cambios reflejados en una generación dada.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
