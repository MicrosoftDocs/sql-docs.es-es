---
description: Trabajo con tablas temporales con control de versiones del sistema con optimización para memoria
title: Trabajo con tablas temporales con control de versiones del sistema con optimización para memoria | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 691d4f80-6754-43f5-8b43-d4facf08f6fc
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: acfdadf0216bd243f61339982615c4bf83796dfa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177982"
---
# <a name="working-with-memory-optimized-system-versioned-temporal-tables"></a>Trabajo con tablas temporales con control de versiones del sistema con optimización para memoria


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


En este tema se describen las diferencias entre trabajar con una tabla temporal con versiones del sistema optimizadas para memoria y trabajar con una tabla temporal con versiones del sistema basadas en disco.

> [!NOTE]
> El uso de tablas temporales con tablas con optimización para memoria solo se aplica a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] y no a [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

## <a name="discovering-metadata"></a>Detección de metadatos

Para detectar los metadatos sobre una tabla temporal con versiones del sistema optimizadas para memoria, tiene que combinar información de [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) y [sys.internal_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md). Una tabla temporal con versiones del sistema se presenta como parent_object_id de la tabla de historial en memoria interno.

En este ejemplo se muestra cómo consultar y combinar estas tablas.

```sql
SELECT SCHEMA_NAME (T1.schema_id) as TemporalTableSchema
    , OBJECT_NAME(IT.parent_object_id) as TemporalTableName
    , T1.object_id as TemporalTableObjectId
    , IT.Name as InternalHistoryStagingName
    , SCHEMA_NAME (T2.schema_id) as HistoryTableSchema
    , OBJECT_NAME (T1.history_table_id) as HistoryTableName
FROM sys.internal_tables IT
JOIN sys.tables T1
    ON IT.parent_object_id = T1.object_id
JOIN sys.tables T2
    ON T1.history_table_id = T2.object_id
WHERE T1.is_memory_optimized = 1 AND T1.temporal_type = 2

```

## <a name="modifying-data"></a>Modificar datos

Las tablas temporales con versiones del sistema optimizadas para memoria se pueden modificar mediante procedimientos almacenados compilados de forma nativa, que permiten convertir tablas no temporales optimizadas para memoria a las versiones del sistema y mantener los procedimientos existentes almacenados de forma nativa.

En este ejemplo se muestra cómo se puede modificar una tabla creada anteriormente en un módulo compilado de forma nativa.

```sql
CREATE PROCEDURE dbo.UpdateFXCurrencyPair
   (
      @ProviderID int
      , @CurrencyID1 int
      , @CurrencyID2 int
      , @BidRate decimal(8,4)
      , @AskRate decimal(8,4)
   )
WITH NATIVE_COMPILATION, SCHEMABINDING
   , EXECUTE AS OWNER
AS
   BEGIN ATOMIC WITH
   (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'English')
      UPDATE dbo.FXCurrencyPairs SET AskRate = @AskRate, BidRate = @BidRate
     WHERE ProviderID = @ProviderID AND CurrencyID1 = @CurrencyID1 AND CurrencyID2 = @CurrencyID2
END
GO ;

```

## <a name="see-also"></a>Consulte también

- [Tablas temporales con control de versiones del sistema con tablas con optimización para memoria](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Creación de una tabla temporal con control de versiones del sistema con optimización para memoria](../../relational-databases/tables/creating-a-memory-optimized-system-versioned-temporal-table.md)
- [Supervisión de tablas temporales con control de versiones del sistema con optimización para memoria](../../relational-databases/tables/monitoring-memory-optimized-system-versioned-temporal-tables.md)
- [Consideraciones de rendimiento con tablas temporales con control de versiones del sistema optimizadas para memoria](../../relational-databases/tables/
- [Tablas temporales](../../relational-databases/tables/temporal-tables.md)
- [Comprobaciones de coherencia del sistema de la tabla temporal](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Administración de la retención de datos históricos en las tablas temporales con control de versiones del sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Funciones y vistas de metadatos de la tabla temporal](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
