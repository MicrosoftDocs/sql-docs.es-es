---
description: MSdbms_datatype_mapping (Transact-SQL)
title: MSdbms_datatype_mapping (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdbms_datatype_mapping_TSQL
- MSdbms_datatype_mapping
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_datatype_mapping system table
ms.assetid: 13289a0b-dfb0-4771-ad80-4c5f83cded99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8ea7ffaa872030b1a1d0d6ec20e17c69d64266b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160473"
---
# <a name="msdbms_datatype_mapping-transact-sql"></a>MSdbms_datatype_mapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSdbms_datatype_mapping** contiene las asignaciones de tipos de datos permitidos del tipo de datos del sistema de administración de bases de datos (DBMS) de origen a uno o varios tipos de datos específicos en el DBMS de destino. Esta tabla se almacena en la base de datos **msdb** y se utiliza para la replicación de bases de datos heterogéneas.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**datatype_mapping_id**|**int**|Identifica cada asignación de tipo de datos única.|  
|**map_id**|**int**|Identifica el tipo de datos de origen.|  
|**dest_datatype_id**|**int**|Identifica el tipo de datos de destino.|  
|**dest_precision**|**bigint**|Define la precisión del tipo de datos de destino, donde un valor NULL significa que no se utiliza la precisión y un valor de **-1** significa que se utiliza la precisión del tipo de datos de origen.|  
|**dest_scale**|**int**|Define la escala del tipo de datos de destino, donde un valor NULL significa que no se utiliza la escala, y un valor de **-1** significa que se usa la escala del tipo de datos de origen.|  
|**dest_length**|**bigint**|Define la longitud del tipo de datos de destino, donde un valor NULL significa que no se utiliza la longitud, y un valor de **-1** significa que se utiliza la longitud del tipo de datos de origen.|  
|**dest_nullable**|**bit**|Indica si la columna de destino en la asignación admite valores NULL, donde un valor NULL significa que no se requiere esta definición.|  
|**dest_createparams**|**int**|Es el mapa de bits que describe qué combinación de longitud, precisión y escala es aplicable a cada tipo de datos, donde se incluye:<br /><br /> **0x1** = precisión.<br /><br /> **0X2** = escala.<br /><br /> **0x4** = longitud.|  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Especificar asignaciones de tipos de datos para un publicador de Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
