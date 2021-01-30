---
title: Vistas de administración dinámica de tablas con optimización para memoria (Transact-SQL)
description: Obtenga información sobre las vistas de administración dinámica de SQL Server y las vistas de catálogo de objetos que se usan con In-Memory OLTP en SQL Server.
ms.custom: seo-dt-2019
ms.date: 02/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: ccd82fed-1a3f-47de-85c4-1c9bdd80b027
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 50b110b71cdbfc1d9f6775c0b046ff4cbdf69db6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196305"
---
# <a name="memory-optimized-table-dynamic-management-views-transact-sql"></a>Vistas de administración dinámica de tablas con optimización para memoria (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Las siguientes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vistas de administración dinámica (DMV) se usan con In-Memory OLTP:  
  
 Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  

:::row:::
    :::column:::
        [sys.dm_db_xtp_checkpoint_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-stats-transact-sql.md)

        [sys.dm_db_xtp_gc_cycle_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-gc-cycle-stats-transact-sql.md)

        [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)

        [sys.dm_db_xtp_merge_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-merge-requests-transact-sql.md)

        [sys.dm_db_xtp_nonclustered_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-nonclustered-index-stats-transact-sql.md)

        [sys.dm_db_xtp_transactions &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-transactions-transact-sql.md)

        [sys.dm_xtp_gc_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-gc-stats-transact-sql.md)

        [sys.dm_xtp_transaction_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-transaction-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_checkpoint_files &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md)

        [sys.dm_db_xtp_hash_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-hash-index-stats-transact-sql.md)

        [sys.dm_db_xtp_memory_consumers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md)

        [sys.dm_db_xtp_object_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-object-stats-transact-sql.md)

        [sys.dm_db_xtp_table_memory_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md)

        [sys.dm_xtp_gc_queue_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-gc-queue-stats-transact-sql.md)

        [sys.dm_xtp_system_memory_consumers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-system-memory-consumers-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="object-catalog-views"></a>Vistas de catálogo de objetos

Las vistas de catálogo de objetos siguientes se usan específicamente con In-Memory OLTP.

:::row:::
    :::column:::
        [sys.hash_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-hash-indexes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.memory_optimized_tables_internal_attributes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-memory-optimized-tables-internal-attributes-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="internal-dmvs"></a>DMV internas

Hay DMV adicionales que están pensados únicamente para uso interno y para los que no se proporciona documentación directa. En el área de las tablas optimizadas para memoria, las DMV no documentadas incluyen lo siguiente:

- sys.dm_xtp_threads
- sys.dm_xtp_transaction_recent_rows

