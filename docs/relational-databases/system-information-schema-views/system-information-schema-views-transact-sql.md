---
description: Vistas de esquema de información del sistema (Transact-SQL)
title: Vistas de esquema de información del sistema (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- information schema views
- schemas [SQL Server], information schema views
- metadata [SQL Server], views
- views [SQL Server], information schema
- system views [SQL Server], information schema
ms.assetid: 7e9f1dfe-27e9-40e7-8fc7-bfc5cae6be10
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a39ec8c22b4267e1c6b6bc2f213775836b073a48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185482"
---
# <a name="system-information-schema-views-transact-sql"></a>Vistas de esquema de información del sistema (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Una vista de esquema de información es uno de los diversos métodos que proporciona [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para obtener metadatos. Las vistas de esquema de información proporcionan una vista interna e independiente de las tablas del sistema de los metadatos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Las vistas de esquema de información permiten que las aplicaciones funcionen correctamente aunque se hayan realizado cambios significativos en las tablas del sistema subyacentes. Las vistas de esquema de información incluidas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cumplen la definición del estándar ISO para INFORMATION_SCHEMA.

> [!IMPORTANT]
> Se han realizado algunos cambios en las vistas de esquema de información que anulan la compatibilidad con versiones anteriores. Dichos cambios se describen en los temas de las vistas específicas.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite la convención de nomenclatura de tres partes cuando se hace referencia al servidor actual. El estándar ISO también admite la convención de nomenclatura de tres partes. Sin embargo, los nombres utilizados en ambas convenciones de nomenclatura son diferentes. Las vistas de esquema de información se definen en un esquema especial llamado INFORMATION_SCHEMA. Este esquema se incluye en cada base de datos. Cada vista de esquema de información contiene metadatos para todos los objetos de datos almacenados en esa base de datos en concreto. La siguiente tabla muestra las relaciones existentes entre los nombres de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los nombres estándar de SQL.

|Nombre de servidor SQL|Se asigna a este nombre estándar equivalente de SQL|
|---------------------|-----------------------------------------------|
|Base de datos|Catálogo|
|Schema|Schema|
|Object|Object|
|tipo de datos definido por el usuario|Domain|

Esta asignación entre convenciones de nomenclaturas se aplica a las siguientes vistas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compatibles con ISO.

:::row:::
    :::column:::
        [CHECK_CONSTRAINTS](../../relational-databases/system-information-schema-views/check-constraints-transact-sql.md)

        [COLUMN_DOMAIN_USAGE](../../relational-databases/system-information-schema-views/column-domain-usage-transact-sql.md)

        [COLUMN_PRIVILEGES](../../relational-databases/system-information-schema-views/column-privileges-transact-sql.md)

        [COLUMNAS](../../relational-databases/system-information-schema-views/columns-transact-sql.md)

        [CONSTRAINT_COLUMN_USAGE](../../relational-databases/system-information-schema-views/constraint-column-usage-transact-sql.md)

        [CONSTRAINT_TABLE_USAGE](../../relational-databases/system-information-schema-views/constraint-table-usage-transact-sql.md)

        [DOMAIN_CONSTRAINTS](../../relational-databases/system-information-schema-views/domain-constraints-transact-sql.md)

        [DOMINIOS](../../relational-databases/system-information-schema-views/domains-transact-sql.md)

        [KEY_COLUMN_USAGE](../../relational-databases/system-information-schema-views/key-column-usage-transact-sql.md)

        [PARÁMETROS](../../relational-databases/system-information-schema-views/parameters-transact-sql.md)
    :::column-end:::
    :::column:::
        [REFERENTIAL_CONSTRAINTS](../../relational-databases/system-information-schema-views/referential-constraints-transact-sql.md)

        [RUTINAS](../../relational-databases/system-information-schema-views/routines-transact-sql.md)

        [ROUTINE_COLUMNS](../../relational-databases/system-information-schema-views/routine-columns-transact-sql.md)

        [ESQUEMAS](../../relational-databases/system-information-schema-views/schemata-transact-sql.md)

        [TABLE_CONSTRAINTS](../../relational-databases/system-information-schema-views/table-constraints-transact-sql.md)

        [TABLE_PRIVILEGES](../../relational-databases/system-information-schema-views/table-privileges-transact-sql.md)

        [TABLAS](../../relational-databases/system-information-schema-views/tables-transact-sql.md)

        [VIEW_COLUMN_USAGE](../../relational-databases/system-information-schema-views/view-column-usage-transact-sql.md)

        [VIEW_TABLE_USAGE](../../relational-databases/system-information-schema-views/view-table-usage-transact-sql.md)

        [VISTAS](../../relational-databases/system-information-schema-views/views-transact-sql.md)
    :::column-end:::
:::row-end:::

Además, algunas vistas contienen referencias a diferentes clases de datos, como los datos de caracteres o los datos binarios.

Al hacer referencia a las vistas de esquema de información, debe utilizar un nombre completo que incluya el nombre del esquema `INFORMATION_SCHEMA`. Por ejemplo:

```sql
SELECT TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, COLUMN_DEFAULT
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = N'Product';
```

## <a name="permissions"></a>Permisos  
La visibilidad de los metadatos en las vistas de esquema de información se limita a los elementos protegibles a los que pertenece un usuario o en los que el usuario tiene algún permiso. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).

> [!NOTE]  
> Las vistas de esquema de información se definen en todo el servidor y, por lo tanto, no se pueden denegar en el contexto de una base de datos de usuario. Para REVOCAr o denegar el acceso (SELECT), debe usarse la base de datos maestra. De forma predeterminada, el rol Public tiene permiso SELECT en todas las vistas de esquema de información, pero el contenido está limitado a las reglas de visibilidad de los metadatos.

## <a name="see-also"></a>Consulte también

- [Vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)
- [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
- [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md) 
