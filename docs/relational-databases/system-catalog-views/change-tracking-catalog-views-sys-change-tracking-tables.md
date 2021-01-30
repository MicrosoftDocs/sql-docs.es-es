---
description: Change Tracking vistas de catálogo-sys.change_tracking_tables
title: sys.change_tracking_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- change_tracking_tables_TSQL
- sys.change_tracking_tables
- change_tracking_tables
- sys.change_tracking_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- change tracking [SQL Server], sys.change_tracking_tables
- sys.change_tracking_tables
ms.assetid: 97ec69b6-0d49-4d98-82f0-d3e77ba1ad2b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 371fa8a112239e0edd214a5c4484e3b4ce06423f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193042"
---
# <a name="change-tracking-catalog-views---syschange_tracking_tables"></a>Change Tracking vistas de catálogo-sys.change_tracking_tables
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila por cada tabla en la base de datos actual que tiene habilitado el seguimiento de cambios.  
   
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Id. de una tabla que contiene un diario de cambios. La tabla puede contener un diario de cambios aun cuando el seguimiento de cambios esté actualmente deshabilitado.<br /><br /> El Id. de tabla es único en la base de datos.|  
|is_track_columns_updated_on|**bit**|El estado actual de seguimiento de cambios en la tabla:<br /><br /> 0 = OFF<br /><br /> 1 = ON|  
|begin_version|**bigint**|Versión de la base de datos cuando se inició el seguimiento de cambios para la tabla. Normalmente, esta versión indica si el seguimiento de cambios estaba habilitado, pero se restablece este valor si se trunca la tabla.|  
|cleanup_version|**bigint**|Versión hasta la que el proceso de limpieza podría haber quitado la información de seguimiento de cambios.|  
|min_valid_version|**bigint**|Versión válida mínima de la información de seguimiento de cambios disponible para la tabla.<br /><br /> Al obtener los cambios de la tabla asociada a esta fila, el valor de last_sync_version debe ser mayor o igual que la versión indicada por la columna. Para obtener más información, vea [CHANGE_TRACKING_MIN_VALID_VERSION &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md).|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [CHANGE_TRACKING_MIN_VALID_VERSION &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md)   
 [Change Tracking vistas de catálogo &#40;Transact-SQL&#41;](./catalog-views-transact-sql.md)   
 [Seguimiento de cambios de datos &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
