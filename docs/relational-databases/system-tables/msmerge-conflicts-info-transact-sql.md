---
description: MSmerge_conflicts_info (Transact-SQL)
title: MSmerge_conflicts_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_conflicts_info_TSQL
- MSmerge_conflicts_info
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_conflicts_info system table
ms.assetid: 6b76ae96-737a-4000-a6b6-fcc8772c2af4
author: cawrites
ms.author: chadam
ms.openlocfilehash: beb504c7743dd8e9a2cb93e375e3a79cf9eb09a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205066"
---
# <a name="msmerge_conflicts_info-transact-sql"></a>MSmerge_conflicts_info (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_conflicts_info** realiza un seguimiento de los conflictos que se producen al sincronizar una suscripción a una publicación de combinación. Los datos de la fila perdedora para conflictos se almacenan en la tabla [MSmerge_conflict_publication_article](../../relational-databases/system-tables/msmerge-conflict-publication-article-transact-sql.md) para el artículo en el que se produjo el conflicto. Esta tabla se almacena en la base de datos de publicación del publicador y en la base de datos de suscripciones del suscriptor.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|El alias de la tabla publicada.|  
|**rowguid**|**uniqueidentifier**|Identificador de la fila en conflicto.|  
|**origin_datasource**|**nvarchar(255)**|Nombre de la base de datos donde se originó el cambio conflictivo.|  
|**conflict_type**|**int**|Tipo de conflicto que ocurrió y que puede ser uno de los siguientes:<br /><br /> **1** = conflicto de actualización: el conflicto se detecta en el nivel de fila.<br /><br /> **2** = conflicto de actualización de columna: el conflicto detectado en el nivel de columna.<br /><br /> **3** = conflicto de actualización de eliminación de WINS: la eliminación gana el conflicto.<br /><br /> **4** = conflicto de actualización WINS delete: el ROWGUID eliminado que pierde el conflicto se registra en esta tabla.<br /><br /> **5** = error en la inserción de carga: no se pudo aplicar la inserción del suscriptor en el publicador.<br /><br /> **6** = error en la inserción de descarga: no se pudo aplicar la inserción del publicador en el suscriptor.<br /><br /> **7** = error al eliminar la carga: no se pudo cargar la eliminación en el suscriptor en el publicador.<br /><br /> **8** = error al eliminar la descarga: no se pudo descargar la eliminación en el publicador en el suscriptor.<br /><br /> **9** = error al cargar la actualización: no se pudo aplicar la actualización en el suscriptor en el publicador.<br /><br /> **10** = error de actualización de descarga: no se pudo aplicar la actualización en el publicador al suscriptor.<br /><br /> **11** = resolución<br /><br /> **12** = actualización de registro lógico WINS eliminar: el registro lógico eliminado que pierde el conflicto se registra en esta tabla.<br /><br /> **13** = conflicto del registro lógico insertar actualización: la inserción en un registro lógico entra en conflicto con una actualización.<br /><br /> **14** = conflicto de actualización de WINS de registro lógico: el registro lógico actualizado que pierde el conflicto se registra en esta tabla.|  
|**reason_code**|**int**|Código de error que puede depender del contexto. En el caso de conflictos de actualización-actualización y actualización-eliminación, el valor utilizado para esta columna es el mismo que el **conflict_type**. No obstante, para los conflictos de cambio con error, el código de motivo es el error que evitó que el Agente de mezcla aplicara el cambio. Por ejemplo, si el Agente de mezcla no puede aplicar una inserción en el suscriptor debido a una infracción de la clave principal, registra un **conflict_type** de 6 ("error de inserción de descarga") y un **reason_code** de 2627, que es el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mensaje de error interno de una infracción de clave principal: "infracción de la restricción% ls '%. * ls '. No se puede insertar una clave duplicada en el objeto '%. \* LS '. "|  
|**reason_text**|**nvarchar (720)**|Descripción del error que puede depender del contexto.|  
|**pubid**|**uniqueidentifier**|Identificador de la publicación.|  
|**MSrepl_create_time**|**datetime**|Momento en el que se produjo el conflicto.|  
|**origin_datasource_id**|**uniqueidentifier**|Identificador de la base de datos donde se originó el cambio conflictivo.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
