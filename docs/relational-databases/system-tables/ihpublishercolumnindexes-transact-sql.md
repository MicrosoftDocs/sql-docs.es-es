---
description: IHpublishercolumnindexes (Transact-SQL)
title: IHpublishercolumnindexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHpublishercolumnindexes
- IHpublishercolumnindexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublishercolumnindexes system table
ms.assetid: 95b95a1d-b502-4838-825f-82a456487e25
author: cawrites
ms.author: chadam
ms.openlocfilehash: 13d7e4240b86950b21889f660d9e228eb09552e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201668"
---
# <a name="ihpublishercolumnindexes-transact-sql"></a>IHpublishercolumnindexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla del sistema **IHpublishercolumnindexes** asigna columnas de una publicación que no es de SQL Server en la tabla del sistema [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) a índices en la tabla del sistema [IHpublisherindexes](../../relational-databases/system-tables/ihpublisherindexes-transact-sql.md) . Esta tabla se almacena en la base de datos de distribución.  
  
## <a name="definition"></a>Definición  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publishercolumn_id**|**int**|Identifica la columna de [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) con un índice asociado.|  
|**publisherindex_id**|**int**|Identifica un índice de la tabla [IHpublisherindexes](../../relational-databases/system-tables/ihpublisherindexes-transact-sql.md) asociada a la columna.|  
|**indid**|**int**|Indica la posición de la columna en la tabla publicada.|  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
