---
description: '&lt;esquema conflict_ &gt; _ &lt; TABLE &gt; (Transact-SQL)'
title: '&lt;esquema conflict_ &gt; _ &lt; TABLE &gt; (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 01/15/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- conflict_
- conflict_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- conflict_<schema>_<table>
ms.assetid: 15ddd536-db03-454e-b9b5-36efe1f756d7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2b578a5b95dfae78b79216343f32bd7e5ea97d5c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204717"
---
# <a name="conflict_ltschemagt_lttablegt-transact-sql"></a>&lt;esquema conflict_ &gt; _ &lt; TABLE &gt; (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La \<schema> tabla conflict_ _ \<table> contiene información sobre las filas en conflicto en la replicación punto a punto. En una publicación, cada tabla replicada posee una tabla de conflictos; el nombre de esta tabla de conflictos se anexa al nombre del artículo y esquema. Estas tablas de conflictos específicas del artículo existen en cada base de datos de publicación.  
  
 En el caso de la replicación punto a punto, el Agente de distribución genera un error de forma predeterminada cuando detecta un conflicto. Se registra un error de conflicto en el registro de errores, pero no se registra ningún dato de conflicto en la tabla de conflictos, por lo que no está disponible para verse. Si el Agente de distribución puede continuar, se registra un conflicto localmente en cada nodo donde se detectó. Para obtener más información, vea "Controlar los conflictos" en [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|__$originator_id|**int**|Identificador del nodo en el que se originó el cambio en conflicto. Para obtener una lista de identificadores, ejecute [sp_help_peerconflictdetection](../../relational-databases/system-stored-procedures/sp-help-peerconflictdetection-transact-sql.md).|  
|__$origin_datasource|**int**|Nodo en el que se originó el cambio en conflicto.|  
|__$tranid|**nvarchar (40)**|Número de flujo de registro (LSN) del cambio en conflicto cuando se aplicó en __$origin_datasource.|  
|__$conflict_type|**int**|Tipo de conflicto, que puede ser uno de los valores siguientes:<br /><br /> 1: no se pudo realizar una actualización porque otra actualización cambió la fila local o se eliminó y, a continuación, se reinsertó.<br /><br /> 2: no se pudo realizar una actualización porque ya se había eliminado la fila local.<br /><br /> 3: no se pudo realizar una eliminación porque otra actualización cambió la fila local o se eliminó y, a continuación, se reinsertó.<br /><br /> 4: no se pudo realizar una eliminación porque ya se había eliminado la fila local.<br /><br /> 5: no se pudo realizar una inserción porque ya se había insertado la fila local o se insertó y, a continuación, se actualizó.|  
|__$is_winner|**bit**|Indica si la fila de esta tabla fue la ganadora del conflicto, lo que significa que se aplicó al nodo local.|  
|__$pre_version|**varbinary (32)**|Versión de la base de datos en la que se originó el cambio en conflicto.|  
|__$reason_code|**int**|Código de la resolución del conflicto. Puede ser uno de los siguientes valores:<br /><br /> 0<br /><br /> 1<br /><br /> 2<br /><br /> <br /><br /> Para obtener más información, vea **_ _ $ reason_text**.|  
|__$reason_text|**nvarchar (720)**|Resolución del conflicto. Puede ser uno de los siguientes valores:<br /><br /> Resuelto (1)<br /><br /> No resuelto (2)<br /><br /> Desconocido (0)|  
|__$update_bitmap|**varbinary (** *n* **)**. El tamaño varía en función del contenido.|Mapa de bits que indica qué columnas se actualizaron en el caso de un conflicto de actualizaciones.|  
|__$inserted_date|**datetime**|Fecha y hora en que la fila en conflicto se insertó en esta tabla.|  
|__$row_id|**timestamp**|Versión de fila que está asociada a la fila que ocasionó el conflicto.|  
|__$change_id|**binario (8)**|En una fila local, este valor es igual al valor __$row_id de la fila entrante que sufrió un conflicto con la fila local. Este valor es NULL para una fila entrante.|  
|\<base table column names>|\<base table column types>|La tabla de conflictos contiene una columna para cada columna de la tabla base.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
