---
description: sysmergesubsetfilters (Transact-SQL)
title: sysmergesubsetfilters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergesubsetfilters
- sysmergesubsetfilters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergesubsetfilters system table
ms.assetid: f91d1c6c-3132-47f6-926c-88f56848cafe
author: cawrites
ms.author: chadam
ms.openlocfilehash: bd623b860391768ef8a691feb0a5f651847778a2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209202"
---
# <a name="sysmergesubsetfilters-transact-sql"></a>sysmergesubsetfilters (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información de filtro de combinación para los artículos con particiones. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**FilterName**|**sysname**|Nombre del filtro utilizado para crear el artículo.|  
|**join_filterid**|**int**|Id. del objeto que representa el filtro de combinación.|  
|**pubid**|**uniqueidentifier**|Id. de la publicación.|  
|**artid**|**uniqueidentifier**|Id. del artículo.|  
|**art_nickname**|**int**|Alias del artículo.|  
|**join_articlename**|**sysname**|Nombre de la tabla que se va a combinar para determinar si la fila le pertenece.|  
|**join_nickname**|**int**|Alias de la tabla que se va a combinar para determinar si la fila le pertenece.|  
|**join_unique_key**|**int**|Indica una combinación en una clave única de **join_tablename**:<br /><br /> 0 = No es una clave única.<br /><br /> 1 = Es una clave única.|  
|**expand_proc**|**sysname**|Nombre del procedimiento almacenado que utiliza el Agente de mezcla para identificar filas que se deben enviar o quitar de un suscriptor.|  
|**join_filterclause**|**nvarchar(1000)**|Cláusula de filtro utilizada en la combinación.|  
|**filter_type**|**tinyint**|Especifica el tipo de filtro, que puede ser uno de los siguientes:<br /><br /> 1 = Filtro de combinación.<br /><br /> 2 = Vínculo de registro lógico.<br /><br /> 3 = Filtro de combinación y vínculo de registro lógico.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
