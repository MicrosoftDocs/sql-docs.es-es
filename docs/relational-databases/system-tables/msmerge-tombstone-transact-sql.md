---
description: MSmerge_tombstone (Transact-SQL)
title: MSmerge_tombstone (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_tombstone_TSQL
- MSmerge_tombstone
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_tombstone system table
ms.assetid: 8b3fc7bf-729b-40f2-8a26-e7dfbe8ddb38
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6cd4fb0d58f0e400d23834090f31bf9043f547d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205902"
---
# <a name="msmerge_tombstone-transact-sql"></a>MSmerge_tombstone (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_tombstone** contiene información sobre las filas eliminadas y permite propagar las eliminaciones a otros suscriptores. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**rowguid**|**uniqueidentifier**|Identificador de fila.|  
|**tablenick**|**int**|Alias de la tabla.|  
|**type**|**tinyint**|Tipo de eliminación:<br /><br /> 1 = Eliminación de usuario.<br /><br /> 5 = La fila ya no pertenece a la partición filtrada.<br /><br /> 6 = Eliminación de sistema.|  
|**DMX**|**varbinary (249)**|Indica la versión del registro eliminada y las actualizaciones conocidas antes de su eliminación. Permite usar reglas para la resolución coherente de un conflicto cuando un suscriptor actualiza una fila mientras otro suscriptor la elimina.|  
|**última**|**int**|Se asigna cuando se elimina una fila. Si un suscriptor solicita la generación N, solo se envían los marcadores de exclusión con la generación >= N.|  
|**logical_record_parent_rowguid**|**uniqueidentifier**|Identifica el registro lógico al que pertenece una fila eliminada.|  
|**logical_record_lineage**|**Varbinary (501)**|El alias del suscriptor, pares de números de versión que se utilizan para mantener un historial de las eliminaciones del registro lógico al que pertenece esta fila.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
