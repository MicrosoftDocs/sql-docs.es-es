---
description: Cambiar el nombre a las columnas (motor de base de datos)
title: Cambio de nombre de las columnas (motor de base de datos) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], names
- renaming columns
- column names [SQL Server]
ms.assetid: 7c71ec9f-0180-4398-b32a-4bfb7592e75d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ef2568fbb27a2139896cb18fd99fb9e65c403d8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212026"
---
# <a name="rename-columns-database-engine"></a>Cambiar el nombre a las columnas (motor de base de datos)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Puede cambiar una columna de la tabla en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].

**En este tema**

- **Antes de empezar:**

   [Limitaciones y restricciones](#Restrictions)

   [Seguridad](#Security)

- **Para cambiar el nombre de las columnas, utilizando:**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones

Cambiar el nombre de una columna automáticamente no cambiará las referencias a esa columna. Es necesario modificar de forma manual los objetos que hacen referencia a la columna cuyo nombre se ha cambiado. Por ejemplo, si se cambia el nombre de una columna de una tabla y en un desencadenador existe una referencia a esa columna, es necesario modificar el desencadenador para reflejar el nuevo nombre de la columna. Use [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) para ver las dependencias del objeto antes de cambiarle el nombre.

### <a name="security"></a><a name="Security"></a> Seguridad

#### <a name="permissions"></a><a name="Permissions"></a> Permisos

Requiere el permiso ALTER en el objeto.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio

### <a name="to-rename-a-column-using-object-explorer"></a>Para cambiar el nombre de una columna mediante el Explorador de objetos

1. En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].
2. En el **Explorador de objetos**, haga clic con el botón derecho en la tabla en la que quiere cambiar nombres de columnas y elija **Cambiar nombre**.
3. Escriba un nuevo nombre de columna.

### <a name="to-rename-a-column-using-table-designer"></a>Para cambiar el nombre de una columna mediante el Diseñador de tablas

1. En el **Explorador de objetos**, haga clic con el botón derecho en la tabla en la que quiere cambiar nombres de columnas y elija **Diseño**.
2. En **Nombre de columna**, seleccione el nombre que desea cambiar y escriba uno nuevo.
3. En el menú **Archivo**, haga clic en ***Guardar** _nombre de tabla_.

> [!NOTE]
> También puede cambiar el nombre de una columna en la pestaña **Propiedades de columna** . Seleccione la columna cuyo nombre desea cambiar y escriba un nuevo valor en **Nombre**.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL

**Para cambiar el nombre de una columna**

### <a name="to-rename-a-column"></a>Para cambiar el nombre de una columna

En el ejemplo siguiente se cambia el nombre de la columna `TerritoryID` de la tabla `Sales.SalesTerritory` a `TerrID` en la base de datos de AdventureWorks.

```sql
EXEC sp_rename 'Sales.SalesTerritory.TerritoryID', 'TerrID', 'COLUMN';
```

Para obtener más información, vea [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md).
