---
description: IHpublishertables (Transact-SQL)
title: IHpublishertables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHpublishertables
- IHpublishertables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublishertables system table
ms.assetid: 7d16ac39-633a-4fe2-8f22-1d9afc191ee9
author: cawrites
ms.author: chadam
ms.openlocfilehash: d707fece2ded03399d410b69847593cfa0789045
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201632"
---
# <a name="ihpublishertables-transact-sql"></a>IHpublishertables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla del sistema **IHpublishertables** representa los metadatos almacenados en el publicador. La tabla contiene una fila para cada tabla de origen publicada desde un publicador que no sea de SQL Server utilizando el distribuidor actual. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**table_id**|**int**|Identifica una tabla publicada.|  
|**publisher_id**|**smallint**|Identifica el publicador que no es de SQL Server desde el que se publica la tabla.|  
|**name**|**sysname**|El nombre de la tabla publicada.|  
|**owner**|**sysname**|El propietario de la tabla.|  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
