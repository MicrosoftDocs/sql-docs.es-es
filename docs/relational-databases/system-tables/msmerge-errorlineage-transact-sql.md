---
description: MSmerge_errorlineage (Transact-SQL)
title: MSmerge_errorlineage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_errorlineage_TSQL
- MSmerge_errorlineage
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_errorlineage system table
ms.assetid: 3bcbd328-c958-4cd4-a573-3c35539fa919
author: cawrites
ms.author: chadam
ms.openlocfilehash: 95b73be9ad1f9eb2fc3ef659375b59d4e60ba21f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098206"
---
# <a name="msmerge_errorlineage-transact-sql"></a>MSmerge_errorlineage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_errorlineage** contiene filas que se han eliminado en el suscriptor, pero cuya eliminación no se propaga al publicador. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|Es el valor entero asignado a la tabla publicada para la replicación de mezcla. Corresponde al campo sobrenombre de la tabla **sysmergearticles** .|  
|**rowguid**|**uniqueidentifier**|Identificador de fila.|  
|**DMX**|**varbinary (501)**|Almacena un historial de los suscriptores y publicadores que han efectuado actualizaciones en alguna fila. Se utiliza para detectar y solucionar situaciones de conflicto.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
