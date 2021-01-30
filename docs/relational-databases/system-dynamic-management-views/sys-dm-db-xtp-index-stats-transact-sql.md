---
description: sys.dm_db_xtp_index_stats (Transact-SQL)
title: sys.dm_db_xtp_index_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_xtp_index_stats
- dm_db_xtp_index_stats
- sys.dm_db_xtp_index_stats_TSQL
- dm_db_xtp_index_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_index_stats dynamic management view
ms.assetid: 8d0a50b8-2015-4576-930f-e3307dfc888e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f9fdfede86fd67c4911a8ec7bb0cdd75df012283
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160149"
---
# <a name="sysdm_db_xtp_index_stats-transact-sql"></a>sys.dm_db_xtp_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Contiene las estadísticas recopiladas desde el último reinicio de la base de datos.  
  
 Para obtener más información, vea [&#40;de OLTP en memoria In-Memory&#41;de optimización ](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md) y [directrices para usar índices en tablas de Memory-Optimized](/previous-versions/sql/sql-server-2016/dn133166(v=sql.130)).  

  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_id|**bigint**|Id. del objeto al que pertenece este índice.|  
|xtp_object_id|**bigint**|IDENTIFICADOR interno correspondiente a la versión actual del objeto.<br /><br /> Nota: se aplica a [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] .|  
|index_id|**bigint**|Id. del índice. El index_id es exclusivo solo dentro del objeto.|  
|scans_started|**bigint**|Número de recorridos de índice OLTP en memoria realizados. Cada selección, inserción, actualización o eliminación requiere un recorrido de índice.|  
|scans_retried|**bigint**|Número de recorridos de índice que tuvieron que reintentarse.|  
|rows_returned|**bigint**|Número de filas devueltas acumulado desde que se creó la tabla o desde el inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|rows_touched|**bigint**|Número acumulado de filas a las que se tuvo acceso desde que se creó la tabla o desde el inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|rows_expiring|**bigint**|Solo para uso interno.|  
|rows_expired|**bigint**|Solo para uso interno.|  
|rows_expired_removed|**bigint**|Solo para uso interno.|  
|phantom_scans_started|**bigint**|Solo para uso interno.|  
|phatom_scans_retries|**bigint**|Solo para uso interno.|  
|phantom_rows_touched|**bigint**|Solo para uso interno.|  
|phantom_expiring_rows_encountered|**bigint**|Solo para uso interno.|  
|phantom_expired_rows_encountered|**bigint**|Solo para uso interno.|  
|phantom_expired_removed_rows_encountered|**bigint**|Solo para uso interno.|  
|phantom_expired_rows_removed|**bigint**|Solo para uso interno.|  
|object_address|**varbinary(8**|Solo para uso interno.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW DATABASE STATE en la base de datos actual.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de tablas optimizadas para memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
