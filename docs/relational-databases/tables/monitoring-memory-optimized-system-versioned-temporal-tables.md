---
description: Supervisión de tablas temporales con control de versiones del sistema con optimización para memoria
title: Supervisión de tablas temporales con control de versiones del sistema con optimización para memoria | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 7a06785d-dbcb-44de-b95c-26b131471bee
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e593417e5b88037ad3fddbfba4a8cb99ddb50e94
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484467"
---
# <a name="monitoring-memory-optimized-system-versioned-temporal-tables"></a>Supervisión de tablas temporales con control de versiones del sistema con optimización para memoria


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Puede utilizar las vistas existentes para realizar un seguimiento del consumo de memoria resumido y detallado para cada tabla optimizada para memoria con control de versiones del sistema.

Consumo de memoria detallado (dividido por tabla de almacenamiento provisional principal del historial interno y con control de versiones del sistema):

```sql
--Details of memory consumption
WITH InMemoryTemporalTables
AS
   (
      SELECT SCHEMA_NAME ( T1.schema_id ) AS TemporalTableSchema
         , T1.object_id AS TemporalTableObjectId
         , IT.object_id AS InternalTableObjectId
         , OBJECT_NAME ( IT.parent_object_id ) AS TemporalTableName
         , IT.Name AS InternalHistoryStagingName
      FROM sys.internal_tables IT
      JOIN sys.tables T1 ON IT.parent_object_id = T1.object_id
      WHERE T1.is_memory_optimized = 1 AND T1.temporal_type = 2
   )
SELECT
   TemporalTableSchema
   , T.TemporalTableName
   , T.InternalHistoryStagingName,
      CASE
         WHEN C.object_id = T.TemporalTableObjectId
         THEN 'Temporal Table Consumption'
         ELSE 'Internal Table Consumption'
         END ConsumedBy
   , C.*
FROM sys.dm_db_xtp_memory_consumers C
JOIN InMemoryTemporalTables T
   ON C.object_id = T.TemporalTableObjectId OR C.object_id = T.InternalTableObjectId
   WHERE T.TemporalTableSchema = 'dbo' AND T.TemporalTableName = 'FXCurrencyPairs'
;
```

Resumen del consumo de memoria (total para una tabla optimizada para memoria con control de versiones del sistema):

```sql
--Summary of memory consumption
WITH InMemoryTemporalTables
AS
   (
      SELECT SCHEMA_NAME ( T1.schema_id ) AS TemporalTableSchema
         , T1.object_id AS TemporalTableObjectId
         , IT.object_id AS InternalTableObjectId
         , OBJECT_NAME ( IT.parent_object_id ) AS TemporalTableName
         , IT.Name AS InternalHistoryStagingName
      FROM sys.internal_tables IT
      JOIN sys.tables T1 ON IT.parent_object_id = T1.object_id
      WHERE T1.is_memory_optimized = 1 AND T1.temporal_type = 2
   )
, DetailedConsumption
AS
(
   SELECT TemporalTableSchema
      , T.TemporalTableName
      , T.InternalHistoryStagingName
      , CASE
         WHEN C.object_id = T.TemporalTableObjectId
         THEN 'Temporal Table Consumption'
         ELSE 'Internal Table Consumption'
         END ConsumedBy
      , C.*
   FROM sys.dm_db_xtp_memory_consumers C
   JOIN InMemoryTemporalTables T
   ON C.object_id = T.TemporalTableObjectId OR C.object_id = T.InternalTableObjectId
)
SELECT TemporalTableSchema
   TemporalTableName
   , sum ( allocated_bytes ) AS allocated_bytes
   , sum ( used_bytes ) AS used_bytes
FROM DetailedConsumption
WHERE TemporalTableSchema = 'dbo' ANDTemporalTableName = 'FXCurrencyPairs'
GROUP BY TemporalTableSchema, TemporalTableName
;
```

## <a name="see-also"></a>Consulte también

- [Tablas temporales con control de versiones del sistema con tablas con optimización para memoria](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Creación de una tabla temporal con control de versiones del sistema con optimización para memoria](../../relational-databases/tables/creating-a-memory-optimized-system-versioned-temporal-table.md)
- [Trabajo con tablas temporales con control de versiones del sistema con optimización para memoria](../../relational-databases/tables/working-with-memory-optimized-system-versioned-temporal-tables.md)
- [Consideraciones de rendimiento con tablas temporales con control de versiones con optimización para memoria](../../relational-databases/tables/memory-optimized-system-versioned-temporal-tables-performance.md)
- [Tablas temporales](../../relational-databases/tables/temporal-tables.md)
- [Comprobaciones de coherencia del sistema de la tabla temporal](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Administración de la retención de datos históricos en las tablas temporales con control de versiones del sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Funciones y vistas de metadatos de la tabla temporal](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
