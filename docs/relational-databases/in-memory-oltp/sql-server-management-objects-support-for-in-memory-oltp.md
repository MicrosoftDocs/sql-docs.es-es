---
title: 'Compatibilidad de Objetos de administración de SQL Server: OLTP en memoria'
description: Conozca qué elementos de Objetos de administración de SQL Server (SMO) admiten OLTP en memoria. Revise los tipos y miembros en el espacio de nombres Microsoft.SqlServer.Management.Smo.
ms.custom: seo-dt-2019
ms.date: 08/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2b67292d-6d8e-4016-9063-a97461ffe57a
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fcacc0f64b33bedaeda87ef7a7a7b6363d1359a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353653"
---
# <a name="sql-server-management-objects-support-for-in-memory-oltp"></a>Compatibilidad de Objetos de administración de SQL Server con OLTP en memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
En este tema se describen los elementos de Objetos de administración de SQL Server (SMO) que admiten el OLTP en memoria.  

## <a name="smo-types-and-members"></a>Tipos y miembros de SMO

Los tipos y miembros siguientes forman parte del espacio de nombres **Microsoft.SqlServer.Management.Smo** y admiten el OLTP en memoria:

- **<xref:Microsoft.SqlServer.Management.Smo.DurabilityType>** (enumeración)
- FileGroup. **<xref:Microsoft.SqlServer.Management.Smo.FileGroup.FileGroupType%2A>** (propiedad)
- FileGroup. **<xref:Microsoft.SqlServer.Management.Smo.FileGroup.%23ctor%2A>** (constructor)
- **<xref:Microsoft.SqlServer.Management.Smo.FileGroupType>** (enumeración)
- Index. **<xref:Microsoft.SqlServer.Management.Smo.Index.BucketCount%2A>** (propiedad)
- IndexType. **<xref:Microsoft.SqlServer.Management.Smo.IndexType.NonClusteredHashIndex>** (miembro de la enumeración)
- Index. **<xref:Microsoft.SqlServer.Management.Smo.Index.IsMemoryOptimized%2A>** (propiedad)
- Server. **<xref:Microsoft.SqlServer.Management.Smo.Server.IsXTPSupported%2A>** (propiedad)
- StoredProcedure. **<xref:Microsoft.SqlServer.Management.Smo.StoredProcedure.IsNativelyCompiled%2A>** (propiedad)
- StoredProcedure. **<xref:Microsoft.SqlServer.Management.Smo.StoredProcedure.IsSchemaBound%2A>** (propiedad)
- Table. **<xref:Microsoft.SqlServer.Management.Smo.Table.Durability%2A>** (propiedad)
- Table. **<xref:Microsoft.SqlServer.Management.Smo.Table.IsMemoryOptimized%2A>** (propiedad)
- UserDefinedTableType. **<xref:Microsoft.SqlServer.Management.Smo.UserDefinedTableType.IsMemoryOptimized%2A>** (propiedad)

## <a name="c-code-example"></a>Ejemplo de código de C#

#### <a name="assemblies-referenced-by-the-compiled-code-example"></a>Ensamblados a los que hace referencia el ejemplo de código compilado

- Microsoft.SqlServer.ConnectionInfo.dll
- Microsoft.SqlServer.Management.Sdk.Sfc.dll
- Microsoft.SqlServer.Smo.dll
- Microsoft.SqlServer.SqlEnum.dll

#### <a name="actions-taken-in-the-code-example"></a>Acciones realizadas en el ejemplo de código

1. Crear una base de datos con un grupo de archivos optimizados para memoria y un archivo optimizado para memoria.  
2. Crear una tabla optimizada para memoria perdurable con una clave principal, un índice no agrupado y un índice de hash no agrupado.  
3. Crear columnas e índices.  
4. Crear un tipo de tabla optimizada para memoria definido por el usuario.  
5. Crear un procedimiento almacenado compilado de forma nativa.

#### <a name="source-code"></a>Código fuente
  
```csharp
using Microsoft.SqlServer.Management.Smo;  
using System;  
  
public class A {  
   static void Main(string[] args) {  
      Server server = new Server("(local)");  
  
      // Create a database with memory-optimized filegroup and memory-optimized file.
      Database db = new Database(server, "MemoryOptimizedDatabase");  
      db.Create();  
      FileGroup fg = new FileGroup(
         db,
         "memOptFilegroup",
         FileGroupType.MemoryOptimizedDataFileGroup);  
      db.FileGroups.Add(fg);  
      fg.Create();  
      // Change this path if needed.
      DataFile file = new DataFile(
         fg,
         "memOptFile",
         @"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\MSSQLmemOptFileName");  
      file.Create();  
  
      // Create a durable memory-optimized table with primary key, nonclustered index and nonclustered hash index.
      // Define the table as memory optimized and set the durability.
      Table table = new Table(db, "memOptTable");  
      table.IsMemoryOptimized = true;  
      table.Durability = DurabilityType.SchemaAndData;  
  
      // Create columns.
      Column col1 = new Column(table, "col1", DataType.Int);  
      col1.Nullable = false;  
      table.Columns.Add(col1);  
      Column col2 = new Column(table, "col2", DataType.Float);  
      col2.Nullable = false;  
      table.Columns.Add(col2);  
      Column col3 = new Column(table, "col3", DataType.Decimal(2, 10));  
      col3.Nullable = false;  
      table.Columns.Add(col3);  
  
      // Create indexes.
      Index pk = new Index(table, "PK_memOptTable");  
      pk.IndexType = IndexType.NonClusteredIndex;  
      pk.IndexKeyType = IndexKeyType.DriPrimaryKey;  
      pk.IndexedColumns.Add(new IndexedColumn(pk, col1.Name));  
      table.Indexes.Add(pk);  
  
      Index ixNonClustered = new Index(table, "ix_nonClustered");  
      ixNonClustered.IndexType = IndexType.NonClusteredIndex;  
      ixNonClustered.IndexKeyType = IndexKeyType.None;  
      ixNonClustered.IndexedColumns.Add(
         new IndexedColumn(ixNonClustered, col2.Name));  
      table.Indexes.Add(ixNonClustered);  
  
      Index ixNonClusteredHash = new Index(table, "ix_nonClustered_Hash");  
      ixNonClusteredHash.IndexType = IndexType.NonClusteredHashIndex;  
      ixNonClusteredHash.IndexKeyType = IndexKeyType.None;  
      ixNonClusteredHash.BucketCount = 1024;  
      ixNonClusteredHash.IndexedColumns.Add(
         new IndexedColumn(ixNonClusteredHash, col3.Name));  
      table.Indexes.Add(ixNonClusteredHash);  
  
      table.Create();  
  
      // Create a user-defined memory-optimized table type.
      UserDefinedTableType uDTT = new UserDefinedTableType(db, "memOptUDTT");  
      uDTT.IsMemoryOptimized = true;  
  
      // Add columns.
      Column udTTCol1 = new Column(uDTT, "udtCol1", DataType.Int);  
      udTTCol1.Nullable = false;  
      uDTT.Columns.Add(udTTCol1);  
      Column udTTCol2 = new Column(uDTT, "udtCol2", DataType.Float);  
      udTTCol2.Nullable = false;  
      uDTT.Columns.Add(udTTCol2);  
      Column udTTCol3 = new Column(uDTT, "udtCol3", DataType.Decimal(2, 10));  
      udTTCol3.Nullable = false;  
      uDTT.Columns.Add(udTTCol3);  
  
      // Add index.
      Index ix = new Index(uDTT, "IX_UDT");  
      ix.IndexType = IndexType.NonClusteredHashIndex;  
      ix.BucketCount = 1024;  
      ix.IndexKeyType = IndexKeyType.DriPrimaryKey;  
      ix.IndexedColumns.Add(new IndexedColumn(ix, udTTCol1.Name));  
      uDTT.Indexes.Add(ix);  
  
      uDTT.Create();  
  
      // Create a natively compiled stored procedure.
      StoredProcedure sProc = new StoredProcedure(db, "nCSProc");  
      sProc.TextMode = false;  
      sProc.TextBody = "--Type body here";  
      sProc.IsNativelyCompiled = true;  
      sProc.IsSchemaBound = true;  
      sProc.ExecutionContext = ExecutionContext.Owner;  
      sProc.Create();  
   }  
}  
```  
  
## <a name="see-also"></a>Consulte también  

- [Compatibilidad de SQL Server con OLTP en memoria](./transact-sql-support-for-in-memory-oltp.md)
- [Introducción a SMO](../server-management-objects-smo/overview-smo.md)