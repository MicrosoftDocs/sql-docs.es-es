---
description: Funciones de seguimiento de cambios (Transact-SQL)
title: Funciones de Change Tracking (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], change tracking
- change tracking [SQL Server], functions
ms.assetid: 04eb53c4-8b69-414e-9696-185d227fea35
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 98713d32007aecb6eec05dc172fd57d3af08aba0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196182"
---
# <a name="change-tracking-functions-transact-sql"></a>Funciones de seguimiento de cambios (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  El mecanismo de seguimiento de cambios registra las operaciones de inserción, actualización y eliminación aplicadas sobre las tablas con seguimiento, y proporciona los detalles de los cambios en un formato relacional de fácil uso. Las siguientes funciones devuelven información acerca de los cambios.  
  
|Función|Descripción|  
|--------------|-----------------|  
|[CHANGETABLE (CAMBIOS)](../../relational-databases/system-functions/changetable-transact-sql.md)|Devuelve información de seguimiento de todos los cambios en una tabla producidos desde una versión especificada.|  
|[CHANGETABLE (VERSIÓN)](../../relational-databases/system-functions/changetable-transact-sql.md)|Devuelve la última información de seguimiento de cambios para una fila especificada.|  
|[CHANGE_TRACKING_MIN_VALID_VERSION ()](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md)|Devuelve la versión mínima que es válida para su uso en la obtención de información de seguimiento de cambios de la tabla especificada cuando se usa la función [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) .|  
|[CHANGE_TRACKING_CURRENT_VERSION](../../relational-databases/system-functions/change-tracking-current-version-transact-sql.md)|Obtiene una versión que está asociada a la última transacción confirmada. Puede utilizar esta versión la próxima vez que se enumeren los cambios mediante CHANGETABLE.|  
|[CHANGE_TRACKING_IS_COLUMN_IN_MASK](../../relational-databases/system-functions/change-tracking-is-column-in-mask-transact-sql.md)|Interpreta el valor SYS_CHANGE_COLUMNS devuelto por la función CHANGETABLE (CHANGes...).|  
|[WITH CHANGE_TRACKING_CONTEXT](../../relational-databases/system-functions/with-change-tracking-context-transact-sql.md)|Habilita la especificación de un contexto de cambios, como un Id. del autor, cuando una aplicación cambia los datos.|  
  
## <a name="see-also"></a>Vea también  
 [Seguimiento de cambios de datos &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
  
