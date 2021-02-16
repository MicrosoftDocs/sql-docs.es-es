---
description: Establecer opciones de índice
title: Establecer opciones de índice | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- ALLOW_ROW_LOCKS option
- SORT_IN_TEMPDB option
- DROP_EXISTING clause
- large data, indexes
- PAD_INDEX
- STATISTICS_NORECOMPUTE
- MAXDOP index option, setting
- index options [SQL Server]
- MAXDOP index option
- IGNORE_DUP_KEY option
- ALLOW_PAGE_LOCKS option
- OPTIMIZE_FOR_SEQUENTIAL_KEY option
- ONLINE
ms.assetid: 7969af33-e94c-41f7-ab89-9d9a2747cd5c
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ab6e8a4195ec78c3ae9593991d7c376464925cdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353620"
---
# <a name="set-index-options"></a>Establecer opciones de índice

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

En este tema se describe cómo modificar las propiedades de un índice en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].

 **En este artículo**

- **Antes de empezar:**

   [Limitaciones y restricciones](#Restrictions)

   [Seguridad](#Security)

- **Para modificar las propiedades de un índice, use:**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones

- Las opciones siguientes se aplican inmediatamente al índice mediante la cláusula SET de la instrucción ALTER INDEX: ALLOW_PAGE_LOCKS, ALLOW_ROW_LOCKS, OPTIMIZE_FOR_SEQUENTIAL_KEY, IGNORE_DUP_KEY y STATISTICS_NORECOMPUTE.
- Las opciones siguientes se pueden establecer cuando se vuelve a generar un índice mediante ALTER INDEX REBUILD o CREATE INDEX WITH DROP_EXISTING: PAD_INDEX, FILLFACTOR, SORT_IN_TEMPDB, IGNORE_DUP_KEY, STATISTICS_NORECOMPUTE, ONLINE, ALLOW_ROW_LOCKS, ALLOW_PAGE_LOCKS, MAXDOP y DROP_EXISTING (solo CREATE INDEX).

### <a name="security"></a><a name="Security"></a> Seguridad

#### <a name="permissions"></a><a name="Permissions"></a> Permisos

Requiere el permiso ALTER en la tabla o la vista.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio

### <a name="to-modify-the-properties-of-an-index-in-table-designer"></a>Para modificar las propiedades de un índice en el Diseñador de tablas

1. En el Explorador de objetos, haga clic en el signo más para expandir la base de datos que contiene la tabla en la que quiere modificar las propiedades de un índice.
2. Haga clic en el signo más para expandir la carpeta **Tablas** .
3. Haga clic con el botón derecho en la tabla en la que quiere modificar las propiedades de un índice y seleccione **Diseño**.
4. En el menú **Diseñador de tablas** , haga clic en **Índices o claves**.
5. Seleccione el índice que desea modificar. Sus propiedades aparecerán en la cuadrícula principal.
6. Cambie los valores de una o todas las propiedades para personalizar el índice.
7. Haga clic en **Cerrar**.
8. En el menú **Archivo** , seleccione **Guardar**_nombre_tabla_.

### <a name="to-modify-the-properties-of-an-index-in-object-explorer"></a>Para modificar las propiedades de un índice en el Explorador de objetos

1. En el Explorador de objetos, haga clic en el signo más para expandir la base de datos que contiene la tabla en la que quiere modificar las propiedades de un índice.
2. Haga clic en el signo más para expandir la carpeta **Tablas** .
3. Haga clic en el signo más para expandir la tabla en la que quiera modificar las propiedades de un índice.
4. Haga clic en el signo más para expandir la carpeta **Índices** .
5. Haga clic con el botón derecho en el índice en el que quiere modificar las propiedades y seleccione **Propiedades**.
6. Debajo de **Seleccionar una página**, seleccione **Opciones**.
7. Cambie los valores de una o todas las propiedades para personalizar el índice.
8. Para agregar, quitar o cambiar la posición de una columna de índice, seleccione la página **General** del cuadro de diálogo **Propiedades del índice:** _nombre de índice_. Para obtener más información, consulte [Index Properties F1 Help](../../relational-databases/indexes/index-properties-f1-help.md).

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL

### <a name="to-see-the-properties-of-all-the-indexes-in-a-table"></a>Para ver las propiedades de todos los índices en una tabla

En el ejemplo siguiente se muestran las propiedades de todos los índices de una tabla en la base de datos de AdventureWorks.

```sql
SELECT i.name AS index_name
   , i.type_desc
   , i.is_unique
   , ds.type_desc AS filegroup_or_partition_scheme
   , ds.name AS filegroup_or_partition_scheme_name
   , i.ignore_dup_key
   , i.is_primary_key
   , i.is_unique_constraint
   , i.fill_factor
   , i.is_padded
   , i.is_disabled
   , i.allow_row_locks
   , i.allow_page_locks
   , i.has_filter
   , i.filter_definition
FROM sys.indexes AS i
   INNER JOIN sys.data_spaces AS ds
      ON i.data_space_id = ds.data_space_id
   WHERE is_hypothetical = 0 AND i.index_id <> 0
       AND i.object_id = OBJECT_ID('HumanResources.Employee')
;
```

#### <a name="to-set-the-properties-of-an-index"></a>Para establecer las propiedades de un índice

En el ejemplo siguiente se establecen las propiedades de los índices de la base de datos de AdventureWorks.

[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/set-index-options_1.sql)]

[!code-sql[IndexDDL#AlterIndex2](../../relational-databases/indexes/codesnippet/tsql/set-index-options_2.sql)]

Para obtener más información, vea [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).
