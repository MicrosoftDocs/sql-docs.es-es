---
description: Crear claves principales
title: Creación de claves principales | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- primary keys [SQL Server], creating
ms.assetid: 85c623ca-4656-4d70-a9db-ee4d897cd214
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: debd09257826c625e6eee4d71a81cdb455683f3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344277"
---
# <a name="create-primary-keys"></a>Crear claves principales


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Puede definir una clave principal en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Al crear una clave principal, se crea automáticamente un índice agrupado único o bien un índice no agrupado si así se especifica.

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones

- Una tabla solo puede incluir una restricción PRIMARY KEY.

- Todas las columnas definidas en una restricción PRIMARY KEY se deben definir como NOT NULL. Si no se especifica nulabilidad, la nulabilidad de todas las columnas que participan en una restricción PRIMARY KEY se establece en NOT NULL.

### <a name="security"></a><a name="Security"></a> Seguridad

#### <a name="permissions"></a><a name="Permissions"></a> Permisos

La creación de una tabla nueva con una clave principal requiere el permiso CREATE TABLE en la base de datos y el permiso ALTER en el esquema en el que se crea la tabla.

La creación de una clave principal de una tabla existente requiere el permiso ALTER en la tabla.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio

### <a name="to-create-a-primary-key"></a>Para crear una clave principal

1. En el Explorador de objetos, haga clic con el botón derecho en la tabla a la que quiere agregar una restricción UNIQUE y haga clic en **Diseño**.
2. En el **Diseñador de tablas**, haga clic en el selector de filas de la columna de base de datos que desea definir como clave principal. Si desea seleccionar varias columnas, mantenga presionada la tecla CTRL mientras hace clic en los selectores de fila de las otras columnas.
3. Haga clic con el botón derecho en el selector de fila para la columna y seleccione **Establecer clave principal**.

> [!CAUTION]
> Si desea volver a definir la clave principal, debe eliminar las relaciones establecidas con la clave principal existente antes de crear una nueva. Un mensaje le advertirá de que las relaciones existentes se eliminarán automáticamente durante este proceso.

Las columnas de clave principal se indican mediante un símbolo de clave principal en el selector de fila.

Si una clave principal consta de más de una columna, se admitirán valores duplicados en una columna, pero cada combinación de valores de todas las columnas de la clave principal debe ser única.

Si define una clave compuesta, el orden de las columnas de la clave principal coincide con el de las columnas mostradas en la tabla. Sin embargo, puede cambiar el orden de las columnas después de crear la clave principal. Para obtener más información, vea [Modificar claves principales](../../relational-databases/tables/modify-primary-keys.md).

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL

### <a name="to-create-a-primary-key-in-an-existing-table"></a>Para crear una clave principal en una tabla existente

En el ejemplo siguiente se crea una clave principal en la columna `TransactionID` de la base de datos de AdventureWorks.

```sql
ALTER TABLE Production.TransactionHistoryArchive
   ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID);
```

### <a name="to-create-a-primary-key-in-a-new-table"></a>Para crear una clave principal en una nueva tabla

En el ejemplo siguiente se crea una tabla y se define una clave principal en la columna `TransactionID` de la base de datos de AdventureWorks.

```sql
CREATE TABLE Production.TransactionHistoryArchive1
   (
      TransactionID int IDENTITY (1,1) NOT NULL
      , CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
   )
;
```

### <a name="to-create-a-primary-key-with-clustered-index-in-a-new-table"></a>Para crear una clave principal con un índice agrupado en una tabla nueva

En el ejemplo siguiente se crea una tabla y se define una clave principal en la columna `CustomerID` y un índice agrupado en `TransactionID` en la base de datos de AdventureWorks.

```sql
-- Create table to add the clustered index
CREATE TABLE Production.TransactionHistoryArchive1
   (
      CustomerID uniqueidentifier DEFAULT NEWSEQUENTIALID()
      , TransactionID int IDENTITY (1,1) NOT NULL
      , CONSTRAINT PK_TransactionHistoryArchive1_CustomerID PRIMARY KEY NONCLUSTERED (CustomerID)
   )
;

-- Now add the clustered index
CREATE CLUSTERED INDEX CIX_TransactionID ON Production.TransactionHistoryArchive1 (TransactionID);
```

## <a name="see-also"></a>Consulte también

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 
- [table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)
