---
title: Modificar tablas con optimización para memoria | Microsoft Docs
description: Obtenga información sobre cómo la instrucción ALTER TABLE realiza los cambios de esquema y de índice en las tablas optimizadas para memoria. Combine las operaciones ADD, DROP y ALTER en una única instrucción.
ms.custom: ''
ms.date: 10/22/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 690b70b7-5be1-4014-af97-54e531997839
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: da12d83efd43c7ca6113348adee8e3bde14d9472
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465476"
---
# <a name="altering-memory-optimized-tables"></a>Modificar tablas con optimización para memoria

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Los cambios de esquema y de índice en las tablas optimizadas para memoria pueden realizarse mediante la instrucción ALTER TABLE. En SQL Server 2016 y Azure SQL Database, las operaciones ALTER TABLE en tablas optimizadas para memoria son sin conexión, lo que significa que la tabla no está disponible para realizar consultas mientras la operación está en curso. La aplicación de base de datos puede seguir ejecutándose, y cualquier operación que tenga acceso a la tabla se bloqueará hasta que se complete el proceso de modificación. Es posible combinar varias operaciones ADD, DROP o ALTER en una sola instrucción ALTER TABLE.

> [!IMPORTANT]
> Azure SQL Managed Instance no admite tablas optimizadas para memoria en el nivel de servicio de uso general.
  
## <a name="alter-table"></a>ALTER TABLE  

La sintaxis ALTER TABLE se usa para realizar cambios en el esquema de la tabla, así como para agregar, eliminar y volver a generar índices. Los índices se consideran parte de la definición de tabla:  
  
- La sintaxis ALTER TABLE ... ADD/DROP/ALTER INDEX solo se admite para tablas optimizadas para memoria.  
  
- Si no se usa una instrucción ALTER TABLE, las instrucciones [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) y [PAD_INDEX](../../t-sql/statements/alter-table-index-option-transact-sql.md) no son compatibles con los índices de las tablas optimizadas para memoria.  
  
Se admiten los siguientes tipos de modificaciones:  
  
- Cambiar el número de cubos  
  
- Agregar y quitar un índice  
  
- Cambiar, agregar y quitar una columna  
  
- Agregar y quitar una restricción  
  
 Para obtener más información sobre la funcionalidad ALTER TABLE y la sintaxis completa, vea [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="schema-bound-dependency"></a>Dependencia enlazada a esquema

 Es necesario que los procedimientos almacenados compilados de forma nativa estén enlazados a un esquema, es decir, que tengan una dependencia enlazada a esquema en las tablas con optimización para memoria a las que acceden y las columnas a las que hacen referencia. Una dependencia enlazada a esquema es una relación entre dos entidades que evita que la entidad a la que se hace referencia se elimine o se modifique de manera incompatible mientras exista la entidad de referencia.  
  
 Por ejemplo, si un procedimiento almacenado compilado de forma nativa enlazado a esquema hace referencia a una columna *c1* de la tabla *mytable*, la columna *c1* no se puede quitar. Igualmente, si hay un procedimiento así con una instrucción INSERT sin lista de columnas (por ejemplo, `INSERT INTO dbo.mytable VALUES (...)`), no se puede quitar ninguna columna de la tabla.  

## <a name="logging-of-alter-table-on-memory-optimized-tables"></a>Registro de ALTER TABLE en tablas optimizadas para memoria

En una tabla optimizada para memoria, la mayoría de los escenarios ALTER TABLE ahora se ejecutan en paralelo y generan una optimización de las escrituras en el registro de transacciones. La optimización se consigue únicamente al registrar los cambios de metadatos en el registro de transacciones. Sin embargo, las siguientes operaciones de ALTER TABLE se ejecutan en un solo subproceso y no tienen optimización para registro.

En este caso, la operación de un solo subproceso registraría todo el contenido de la tabla modificada en el registro de transacciones. La siguiente es una lista de las operaciones de un solo subproceso:

- Modificar o agregar una columna para usar un objeto de gran tamaño (LOB): nvarchar(max), varchar(max) o varbinary(max).

- Agregar o quitar un índice COLUMNSTORE.

- Casi todo lo que afecta a una [columna no consecutiva](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).

  - Hacer que una columna consecutiva pase a ser no consecutiva.
  - Hacer que una columna no consecutiva pase a ser consecutiva.
  - Crear una columna no consecutiva nueva.
  - *Excepción:* el alargamiento de una columna que ya no es consecutiva se registra de la forma optimizada.
  
## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se modifica el número de cubos de un índice de hash existente. Esto vuelve a generar el índice de hash con el nuevo número de cubos, mientras que las demás propiedades del índice de hash permanecen iguales.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem
       ALTER INDEX imPK_SalesOrderDetail_SalesOrderID_SalesOrderDetailID  
              REBUILD WITH (BUCKET_COUNT=67108864);  
GO
```

En el ejemplo siguiente se agrega una columna con una restricción NOT NULL y una definición DEFAULT, y se usa WITH VALUES para proporcionar valores para cada fila existente en la tabla. Si no se utiliza WITH VALUES, cada fila tiene el valor NULL en la nueva columna.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD Comment NVARCHAR(100) NOT NULL DEFAULT N'' WITH VALUES;  
GO
```

En el ejemplo siguiente se agrega una restricción de clave principal a una columna existente.  

```sql
CREATE TABLE dbo.UserSession (
   SessionId int not null,
   UserId int not null,
   CreatedDate datetime2 not null,
   ShoppingCartId int,
   index ix_UserId nonclustered hash (UserId) with (bucket_count=400000)
)
WITH (MEMORY_OPTIMIZED=ON, DURABILITY=SCHEMA_ONLY) ;  
GO  
  
ALTER TABLE dbo.UserSession  
       ADD CONSTRAINT PK_UserSession PRIMARY KEY NONCLUSTERED (SessionId);  
GO
```

En el siguiente ejemplo se quita un índice.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       DROP INDEX ix_ModifiedDate;  
GO
```  

En el siguiente ejemplo se agrega un índice.  

```sql  
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD INDEX ix_ModifiedDate (ModifiedDate);  
GO  
```  

En el siguiente ejemplo se agregan varias columnas, con un índice y restricciones.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD    CustomerID int NOT NULL DEFAULT -1 WITH VALUES,  
              ShipMethodID int NOT NULL DEFAULT -1 WITH VALUES,  
              INDEX ix_Customer (CustomerID);  
GO  
```

<a name="logging-of-alter-table-on-memory-optimized-tables-124"></a>

## <a name="see-also"></a>Consulte también  

[Tablas optimizadas para la memoria](./sample-database-for-in-memory-oltp.md)