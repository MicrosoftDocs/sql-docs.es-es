---
description: MSmerge_contents (Transact-SQL)
title: MSmerge_contents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_contents
- MSmerge_contents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_contents system table
ms.assetid: 8d68a61a-683f-4b20-92f9-c0a8d9ba0ad1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 78d7fb9732013970956554e932b3d4804d461df9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205059"
---
# <a name="msmerge_contents-transact-sql"></a>MSmerge_contents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_contents** contiene una fila por cada fila modificada en la base de datos actual desde que se publicó. El proceso de mezcla utiliza esta tabla para determinar las filas que han cambiado. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|El alias de la tabla publicada.|  
|**rowguid**|**uniqueidentifier**|Identificador de la fila especificada.|  
|**última**|**bigint**|La generación de la fila identificada por **tablenick** y **ROWGUID**.|  
|**partchangegen**|**bigint**|Generación asociada al último cambio en los datos que podría haber determinado si la fila pertenece a una publicación filtrada.|  
|**DMX**|**varbinary (501)**|Alias del suscriptor y pares de números de versión para mantener un historial de los cambios en esta fila.|  
|**colvl**|**varbinary (7489)**|Información de versión de columna.|  
|**dor**|**uniqueidentifier**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**logical_record_parent_rowguid**|**uniqueidentifier**|Identifica la fila primaria de nivel superior en **MSmerge_contents** (por **ROWGUID**) para cada fila secundaria correspondiente en un registro lógico.|  
|**logical_record_lineage**|**varbinary (501)**|Alias del suscriptor y pares de números de versión utilizados para mantener un historial de los cambios en la fila primaria de nivel superior en un registro lógico. Este valor es NULL para todas las filas secundarias en un registro lógico.|  
|**logical_relation_change_gen**|**bigint**|Valor de generación asociado con el último cambio que ocasionó la realineación del registro lógico en la que se insertó o se quitó una fila existente de un registro lógico.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
