---
description: MSmerge_articlehistory (Transact-SQL)
title: MSmerge_articlehistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_articlehistory
- MSmerge_articlehistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_articlehistory system table
ms.assetid: 2870e7ea-dbec-4636-9171-c2cee96018ac
author: cawrites
ms.author: chadam
ms.openlocfilehash: 32db379ebb966673efca772f7d1c33dde29d9f2c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212262"
---
# <a name="msmerge_articlehistory-transact-sql"></a>MSmerge_articlehistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En la tabla **MSmerge_articlehistory** se realiza un seguimiento de los cambios realizados en los artículos durante una sesión de sincronización agente de mezcla, con una fila por cada artículo en el que se realizaron los cambios. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|IDENTIFICADOR de una sesión de trabajo de Agente de mezcla en la tabla del sistema [MSmerge_sessions](../../relational-databases/system-tables/msmerge-sessions-transact-sql.md) .|  
|**phase_id**|**int**|Fase de la sesión de sincronización, que puede ser una de las siguientes:<br /><br /> **1** = carga.<br /><br /> **2** = descargar.<br /><br /> **4** = limpieza.<br /><br /> **5** = apagado.<br /><br /> **6** = cambios de esquema.<br /><br /> **7** = BCP.|  
|**article_name**|**sysname**|Nombre del artículo en el que se realizaron cambios.|  
|**start_time**|**datetime**|Hora a la que el agente empezó a procesar el artículo.|  
|**duration**|**int**|Tiempo, en segundos, que el agente tardó en procesar un artículo.|  
|**inserta**|**int**|Número de inserciones aplicadas a un artículo específico durante la sincronización. Este valor aumenta durante el proceso de sincronización y el valor final representa el número total.|  
|**actualizaciones**|**int**|Número de actualizaciones aplicadas a un artículo específico durante la sincronización. Este valor aumenta durante el proceso de sincronización y el valor final representa el número total.|  
|**Eliminaciones**|**int**|Número de eliminaciones aplicadas a un artículo específico durante la sincronización. Este valor aumenta durante el proceso de sincronización y el valor final representa el número total.|  
|**los**|**int**|Número de conflictos que se han producido durante la sincronización. Este valor aumenta durante el proceso de sincronización y el valor final representa el número total.|  
|**conflicts_resolved**|**int**|Número de conflictos que se han producido durante la sincronización y se han resuelto. Este valor aumenta durante el proceso de sincronización y el valor final representa el número total.|  
|**rows_retried**|**int**|Número de filas con errores que se han reintentado durante la sincronización. Este valor aumenta durante el proceso de sincronización y el valor final representa el número total.|  
|**percent_complete**|**decimal**|Porcentaje del tiempo total de sincronización que el Agente de mezcla ha invertido en el artículo durante una sesión. Este valor es NULL hasta que finaliza la sesión.|  
|**estimated_changes**|**int**|Estimación del número de cambios de fila que deben aplicarse al artículo.|  
|**relative_cost**|**decimal**|Tiempo empleado en aplicar los cambios para este artículo respecto al tiempo total de la sesión completa.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
