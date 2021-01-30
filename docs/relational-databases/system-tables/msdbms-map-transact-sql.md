---
description: MSdbms_map (Transact-SQL)
title: MSdbms_map (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdbms_map
- MSdbms_map_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_map system table
ms.assetid: df67e691-3a50-450a-99c5-8c4a041749ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: 41d0283235329d286f3ed3d6e92403c0fa20ae66
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160470"
---
# <a name="msdbms_map-transact-sql"></a>MSdbms_map (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSdbms_map** contiene información del tipo de datos de origen, así como un vínculo a la información del tipo de datos de destino predeterminado para los pares de DBMS de origen y de destino. Esta tabla se almacena en la base de datos **msdb** y se utiliza para la publicación heterogénea.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**map_id**|**int**|Identifica de manera única una asignación de tipo de datos.|  
|**src_dbms_id**|**int**|Identifica el DBMS de origen especificando su **dbms_id** en la tabla [MSdbms](../../relational-databases/system-tables/msdbms-transact-sql.md) .|  
|**dest_dbms_id**|**int**|Identifica el DBMS de destino especificando su **dbms_id** en la tabla [MSdbms](../../relational-databases/system-tables/msdbms-transact-sql.md) .|  
|**src_datatype_id**|**int**|Identifica el **datatype_id** de la tabla de [MSdbms_datatype](../../relational-databases/system-tables/msdbms-datatype-transact-sql.md) para el tipo de datos de origen.|  
|**src_len_min**|**bigint**|La longitud mínima del tipo de datos en el DBMS de origen, en la que un valor NULL indica que no se utiliza una longitud.|  
|**src_len_max**|**bigint**|La longitud máxima del tipo de datos en el DBMS de origen, en la que un valor NULL indica que no se utiliza una longitud.|  
|**src_prec_min**|**bigint**|La precisión mínima del tipo de datos en el DBMS de origen, en la que un valor NULL indica que no se utiliza una precisión.|  
|**src_prec_max**|**bigint**|La precisión máxima del tipo de datos en el DBMS de origen, en la que un valor NULL indica que no se utiliza una precisión.|  
|**src_scale_min**|**int**|La escala mínima del tipo de datos en el DBMS de origen, en la que un valor NULL indica que no se utiliza una escala.|  
|**src_scale_max**|**int**|La escala máxima del tipo de datos en el DBMS de origen, en la que un valor NULL indica que no se utiliza una escala.|  
|**src_nullable**|**bit**|Indica si la columna de destino en la asignación admite valores NULL, donde un valor NULL significa que no se requiere esta definición.|  
|**default_datatype_mapping_id**|**int**|Identifica la asignación de tipo de datos predeterminada especificando su **map_id** en la tabla [MSdbms_datatype_mapping](../../relational-databases/system-tables/msdbms-datatype-mapping-transact-sql.md).|  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Especificar asignaciones de tipos de datos para un publicador de Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
