---
description: MSdbms_datatype (Transact-SQL)
title: MSdbms_datatype (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdbms_datatype
- MSdbms_datatype_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_datatype system table
ms.assetid: 606168cc-79a8-442f-ab43-936f8f884d72
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6db79a6d76443492e4079df48c34c518db4dea0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160458"
---
# <a name="msdbms_datatype-transact-sql"></a>MSdbms_datatype (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSdbms_datatype** contiene la lista completa de tipos de datos nativos en cada sistema de administración de bases de datos (DBMS) compatible que se usa como publicador o suscriptor en la replicación de bases de datos heterogéneas. Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**datatype_id**|**int**|Identifica cada tipo de datos único.|  
|**dbms_id**|**int**|Identifica el DBMS al que pertenece el tipo.|  
|**type**|**sysname**|Nombre del tipo de datos (nativos).|  
|**CreateParams**|**int**|Mapa de bits que describe la combinación de longitud, precisión y escala aplicable a cada tipo de datos, que incluye:<br /><br /> **0x1** = precisión.<br /><br /> **0X2** = escala.<br /><br /> **0x4** = longitud.|  
  
## <a name="remarks"></a>Observaciones  
 Esta tabla contiene entradas para los tipos de datos de SQL Server, dado que una instancia de SQL Server puede suscribirse a una base de datos que no sea de SQL Server y publicarse en un suscriptor que no sea de SQL Server.  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Especificar asignaciones de tipos de datos para un publicador de Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
