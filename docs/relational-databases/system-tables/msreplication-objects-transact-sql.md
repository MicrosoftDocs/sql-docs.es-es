---
description: MSreplication_objects (Transact-SQL)
title: MSreplication_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSreplication_objects
- MSreplication_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_objects system table
ms.assetid: 08f9710d-976d-448e-bead-ac9835e87bc5
author: cawrites
ms.author: chadam
ms.openlocfilehash: cac5ebe0eda9527035ecb73b442f60958548592e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183026"
---
# <a name="msreplication_objects-transact-sql"></a>MSreplication_objects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSreplication_objects** contiene una fila por cada objeto asociado a la replicación en la base de datos del suscriptor. Esta tabla se almacena en la base de datos de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|El nombre del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**object_name**|**sysname**|El nombre del objeto.|  
|**object_type**|**char(2)**|El tipo de objeto:<br /><br /> **u** = tabla.<br /><br /> **t** = desencadenador.<br /><br /> **p** = procedimiento almacenado.|  
|**artículo**|**sysname**|Nombre del artículo con el que está asociado el objeto.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
