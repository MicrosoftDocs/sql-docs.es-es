---
description: sysmergepartitioninfo (Transact-SQL)
title: sysmergepartitioninfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergepartitioninfo_TSQL
- sysmergepartitioninfo
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepartitioninfo system table
ms.assetid: 7429ad2c-dd33-4f7d-89cc-700e083af518
author: cawrites
ms.author: chadam
ms.openlocfilehash: a2bf6023bff729ad468d68762639b59a81dd87de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211527"
---
# <a name="sysmergepartitioninfo-transact-sql"></a>sysmergepartitioninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información sobre las particiones de cada artículo. Contiene una fila por cada artículo de mezcla definido en la base de datos local. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**artid**|**uniqueidentifier**|El número de identificación único del artículo indicado.|  
|**pubid**|**uniqueidentifier**|Número de identificación único para esta publicación; se genera al agregarla.|  
|**partition_view_id**|**int**|El Id de la vista de partición de esta tabla. La vista muestra una asignación de cada fila del artículo a los diferentes Id. de partición a los que pertenece.|  
|**repl_view_id**|**int**|Se agregará.|  
|**partition_deleted_view_rule**|**nvarchar(4000)**|La instrucción SQL utilizada en un desencadenador de replicación de mezcla para recuperar el Id de partición de cada fila eliminada o actualizada según sus valores de columna antiguos.|  
|**partition_inserted_view_rule**|**nvarchar(4000)**|La instrucción SQL utilizada en un desencadenador de replicación de mezcla para recuperar el Id de partición de cada fila insertada o actualizada según sus valores de columna nuevos.|  
|**membership_eval_proc_name**|**sysname**|Nombre del procedimiento que evalúa los identificadores de las particiones actuales en **MSmerge_contents**.|  
|**lista_de_columnas**|**nvarchar(4000)**|La lista delimitada por comas de las columnas replicadas en un artículo.|  
|**column_list_blob**|**nvarchar(4000)**|La lista delimitada por comas de las columnas replicadas en un artículo, incluyendo las columnas de objetos binarios grandes.|  
|**expand_proc**|**sysname**|El nombre del procedimiento que vuelve a evaluar los Id. de partición de todas las filas secundarias de una fila principal recién insertada y de las filas principales que han experimentado un cambio de partición o han sido eliminadas.|  
|**logical_record_parent_nickname**|**int**|El alias del primario de nivel superior de un artículo dado de un registro lógico.|  
|**logical_record_view**|**int**|Una vista que produce el rowguid del artículo primario de nivel superior correspondiente a cada rowguid secundario.|  
|**logical_record_deleted_view_rule**|**nvarchar(4000)**|Similar a **logical_record_view**, excepto en que muestra las filas secundarias de la tabla "eliminada" en los desencadenadores Update y DELETE.|  
|**logical_record_level_conflict_detection**|**bit**|Indica si los conflictos se deben detectar en el nivel de registro lógico o en el nivel de fila o de columna.<br /><br /> **0** = se utiliza la detección de conflictos de nivel de fila o de columna.<br /><br /> **1** = se utiliza la detección de conflictos de registros lógicos, donde un cambio en una fila en el publicador y el cambio en una fila independiente, el mismo registro lógico en el suscriptor se trata como un conflicto.<br /><br /> Cuando este valor es **1**, solo se puede utilizar la resolución de conflictos de nivel de registro lógico.|  
|**logical_record_level_conflict_resolution**|**bit**|Indica si los conflictos se deben solucionar en el nivel de registros lógicos o en el nivel de fila o de columna.<br /><br /> **0** = se utiliza la resolución de nivel de columna o fila.<br /><br /> **1** = en caso de conflicto, todo el registro lógico del ganador sobrescribe todo el registro lógico en el lado perdedor.<br /><br /> Un valor de **1** se puede usar con la detección de nivel de registro lógico y con la detección de nivel de fila o de columna.|  
|**partition_options**|**tinyint**|Define el modo en el que se realiza la partición de los datos en el artículo, lo que permite optimizaciones de rendimiento cuando todas las filas pertenecen solamente a una partición o solamente a una suscripción. *partition_options* puede ser uno de los valores siguientes.<br /><br /> **0** = el filtro para el artículo es estático o no produce un subconjunto de datos único para cada partición, es decir, una partición "superpuesta".<br /><br /> **1** = las particiones se superponen y las actualizaciones DML realizadas en el suscriptor no pueden cambiar la partición a la que pertenece una fila.<br /><br /> **2** = el filtro del artículo produce particiones no superpuestas, pero varios suscriptores pueden recibir la misma partición.<br /><br /> **3** = el filtro del artículo produce particiones no superpuestas que son únicas para cada suscripción.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
