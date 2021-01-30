---
description: sys.dm_db_xtp_object_stats (Transact-SQL)
title: sys.dm_db_xtp_object_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_xtp_object_stats_TSQL
- sys.dm_db_xtp_object_stats
- dm_db_xtp_object_stats
- sys.dm_db_xtp_object_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_object_stats dynamic management view
ms.assetid: 07300b59-3cab-4d3e-8138-5ea8f584f88f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 08dd84ba3c832ac60cd0a1a8a7ca15c4377ff361
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99108652"
---
# <a name="sysdm_db_xtp_object_stats-transact-sql"></a>sys.dm_db_xtp_object_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Indica el número de filas afectadas por operaciones en cada uno de los objetos de [!INCLUDE[hek_2](../../includes/hek-2-md.md)] desde el último reinicio de la base de datos. Las estadísticas se actualizan cuando se ejecuta la operación, independientemente de si la transacción se confirma o se revirtió.  
  
 sys.dm_db_xtp_object_stats puede ayudarle a identificar las tablas optimizadas para memoria que más cambian. Puede decidir quitar de la tabla los índices que no se usan o que usan con poca frecuencia, ya que cada índice afecta al rendimiento. Si hay índices hash, debe volver a evaluar periódicamente el número de depósitos. Para obtener más información, vea [Determining the Correct Bucket Count for Hash Indexes](/previous-versions/sql/).  
  
 sys.dm_db_xtp_object_stats puede ayudarle a identificar qué tablas optimizadas para memoria incurren en conflictos de escritura contra escritura, lo que puede afectar al rendimiento de la aplicación. Por ejemplo, si tiene lógica de reintento de transacciones, puede que sea necesario ejecutar más de una vez la misma instrucción. También puede usar esta información para identificar las tablas (y por tanto la lógica de negocios) que necesitan un control de errores de escritura contra escritura.  
  
 La vista contiene una fila por cada tabla optimizada en memoria en la base de datos.  
  
 Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_id|**bigint**|Id. del objeto.|  
|row_insert_attempts|**bigint**|Número de filas insertadas en la tabla desde el último reinicio de la base de datos por las transacciones confirmadas y anuladas.|  
|row_update_attempts|**bigint**|Número de filas actualizadas en la tabla desde el último reinicio de la base de datos por las transacciones confirmadas y anuladas.|  
|row_delete_attempts|**bigint**|Número de filas eliminadas de la tabla desde el último reinicio de la base de datos por las transacciones confirmadas y anuladas.|  
|write_conflicts|**bigint**|Número de conflictos de escritura que se produjeron desde el último reinicio de la base de datos.|  
|unique_constraint_violations|**bigint**|Número de infracciones de la restricción UNIQUE que se han producido desde el último reinicio de la base de datos.|  
|object_address|**varbinary(8**|Solo para uso interno.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW DATABASE STATE en la base de datos actual.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de tablas optimizadas para memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
