---
title: Introducción a las características de rendimiento de SQL Server en Linux
description: En este artículo, se proporciona una introducción a las características de rendimiento de SQL Server para usuarios de Linux que empiecen a usar SQL Server. Muchos de estos ejemplos funcionan en todas las plataformas, pero el contexto de este artículo es Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 60036d26-4797-4872-9a9e-3552841c61be
ms.openlocfilehash: 24794910e53f730b16975714c514eb717b41ce49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350958"
---
# <a name="walkthrough-for-the-performance-features-of-sql-server-on-linux"></a>Descripción de las características de rendimiento de SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Si es usuario de Linux y está empezando a usar SQL Server, en las tareas siguientes se explican algunas de las características de rendimiento. No son exclusivas ni específicas de Linux, pero le ayudarán a hacerse una idea de las áreas en las que puede profundizar. En los ejemplos, se proporciona un vínculo a documentación detallada de cada área.

> [!NOTE]
> En los siguientes ejemplos, se usa la base de datos de ejemplo AdventureWorks. Para consultar las instrucciones sobre cómo obtener e instalar esta base de datos de ejemplo, vea [Restaurar una base de datos de SQL Server de Windows a Linux](sql-server-linux-migrate-restore-database.md).

## <a name="create-a-columnstore-index"></a>Crear un índice de almacén de columnas
Un índice de almacén de columnas es una tecnología para almacenar y consultar grandes almacenes de datos con un formato de datos en columnas denominados almacenes de columnas.  

1. Agregue un índice de almacén de columnas a la tabla SalesOrderDetail; para hacerlo, ejecute los siguientes comandos de Transact-SQL:

   ```sql
   CREATE NONCLUSTERED COLUMNSTORE INDEX [IX_SalesOrderDetail_ColumnStore]
      ON Sales.SalesOrderDetail
      (UnitPrice, OrderQty, ProductID)
   GO
   ```

2. Ejecute la consulta siguiente, que usa el índice del almacén de columnas para analizar la tabla:

   ```sql
   SELECT ProductID, SUM(UnitPrice) SumUnitPrice, AVG(UnitPrice) AvgUnitPrice,
      SUM(OrderQty) SumOrderQty, AVG(OrderQty) AvgOrderQty
   FROM Sales.SalesOrderDetail
      GROUP BY ProductID
      ORDER BY ProductID
   ```

3. Asegúrese de que el índice del almacén de columnas se haya usado; para hacerlo, busque el elemento object_id del índice del almacén de columnas y confirme que aparece en las estadísticas de uso de la tabla SalesOrderDetail:

   ```sql
   SELECT * FROM sys.indexes WHERE name = 'IX_SalesOrderDetail_ColumnStore'
   GO

   SELECT * 
   FROM sys.dm_db_index_usage_stats
      WHERE database_id = DB_ID('AdventureWorks')
      AND object_id = OBJECT_ID('AdventureWorks.Sales.SalesOrderDetail');
   ```
   
## <a name="use-in-memory-oltp"></a>Usar OLTP en memoria
SQL Server proporciona características de OLTP en memoria que pueden mejorar considerablemente el rendimiento de muchos sistemas de aplicaciones.  En esta sección de la guía de evaluación, se describen los pasos para crear una tabla optimizada para memoria almacenada en memoria y un procedimiento almacenado y compilado de forma nativa que puede acceder a la tabla sin necesidad de compilarse o interpretarse.

### <a name="configure-database-for-in-memory-oltp"></a>Configurar la base de datos para OLTP en memoria
1. Le recomendamos establecer la base de datos con un nivel de compatibilidad como mínimo de 130 para usar OLTP en memoria.  Use la consulta siguiente para comprobar el nivel de compatibilidad actual de AdventureWorks:  

   ```sql
   USE AdventureWorks
   GO
   SELECT d.compatibility_level
   FROM sys.databases as d
     WHERE d.name = Db_Name();
   GO
   ```
   
   Si es necesario, actualice el nivel a 130:

   ```sql
   ALTER DATABASE CURRENT
   SET COMPATIBILITY_LEVEL = 130;
   GO
   ```

2. Cuando en una transacción se usa una tabla basada en disco y una tabla optimizada para memoria, es esencial que la parte optimizada para memoria de la transacción opere en el nivel de aislamiento de la transacción denominado INSTANTÁNEA.  Para exigir de forma confiable este nivel para las tablas optimizadas para memoria en una transacción entre contenedores, ejecute el código siguiente:

   ```sql
   ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON
   GO
   ```

3. Antes de crear una tabla optimizada para memoria, primero necesita crear un GRUPO DE ARCHIVOS optimizado para memoria y un contenedor para archivos de datos:

   ```sql
   ALTER DATABASE AdventureWorks ADD FILEGROUP AdventureWorks_mod CONTAINS memory_optimized_data
   GO  
   ALTER DATABASE AdventureWorks ADD FILE (NAME='AdventureWorks_mod', FILENAME='/var/opt/mssql/data/AdventureWorks_mod') TO FILEGROUP AdventureWorks_mod
   GO
   ```

### <a name="create-a-memory-optimized-table"></a>Crear una tabla optimizada para memoria
El almacén primario de las tablas optimizadas para memoria es la memoria principal y, por lo tanto, al contrario que con las tablas basadas en discos, no es necesario leer los datos del disco en los búferes de memoria.  Para crear una tabla optimizada para memoria, use la cláusula MEMORY_OPTIMIZED = ON.

1. Ejecute la consulta siguiente para crear la tabla optimizada para memoria dbo.ShoppingCart.  De forma predeterminada, los datos persistirán en el disco con fines de durabilidad (la DURABILIDAD también se puede establecer para persistir solo en el esquema). 

   ```sql
   CREATE TABLE dbo.ShoppingCart ( 
   ShoppingCartId INT IDENTITY(1,1) PRIMARY KEY NONCLUSTERED,
   UserId INT NOT NULL INDEX ix_UserId NONCLUSTERED HASH WITH (BUCKET_COUNT=1000000), 
   CreatedDate DATETIME2 NOT NULL, 
   TotalPrice MONEY
   ) WITH (MEMORY_OPTIMIZED=ON) 
   GO
   ```

2. Inserte algunos registros en la tabla:

   ```sql
   INSERT dbo.ShoppingCart VALUES (8798, SYSDATETIME(), NULL) 
   INSERT dbo.ShoppingCart VALUES (23, SYSDATETIME(), 45.4) 
   INSERT dbo.ShoppingCart VALUES (80, SYSDATETIME(), NULL) 
   INSERT dbo.ShoppingCart VALUES (342, SYSDATETIME(), 65.4) 
   ```

### <a name="natively-compiled-stored-procedure"></a>Procedimiento almacenado compilado de forma nativa
SQL Server admite de forma nativa los procedimientos almacenados y compilados que acceden las tablas optimizadas para memoria. Las instrucciones T-SQL se compilan en código máquina y se almacenan como DLL nativas, lo que permite un acceso a los datos más rápido y una ejecución de consultas más eficiente que el uso de T-SQL tradicional.   Los procedimientos almacenados marcados con NATIVE_COMPILATION se compilan de forma nativa. 

1. Ejecute el script siguiente para crear un procedimiento almacenado y compilado de forma nativa que inserta un gran número de registros en la tabla ShoppingCart:


   ```sql
   CREATE PROCEDURE dbo.usp_InsertSampleCarts @InsertCount int 
    WITH NATIVE_COMPILATION, SCHEMABINDING AS 
   BEGIN ATOMIC 
   WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')

   DECLARE @i int = 0

   WHILE @i < @InsertCount 
    BEGIN 
     INSERT INTO dbo.ShoppingCart VALUES (1, SYSDATETIME() , NULL) 
     SET @i += 1 
    END
   END 
   ```
2. Inserte 1 000 000 de filas:

   ```sql
   EXEC usp_InsertSampleCarts 1000000 
   ```

3. Verifique que las filas se hayan insertado correctamente:

   ```sql
   SELECT COUNT(*) FROM dbo.ShoppingCart 
   ```

### <a name="learn-more-about-in-memory-oltp"></a>Más información sobre OLTP en memoria
Para obtener más información sobre OLTP en memoria, vea los temas siguientes:

- [Inicio rápido 1: Tecnologías de OLTP en memoria para acelerar el rendimiento de Transact-SQL](../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)
- [Migrar a OLTP en memoria](../relational-databases/in-memory-oltp/plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)
- [Tabla temporal y variable de tabla más rápidas con optimización para memoria](../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md)
- [Supervisar y solucionar problemas de uso de memoria](../relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage.md)
- [In-Memory OLTP (optimización In-Memory)](../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)

## <a name="use-query-store"></a>Usar el Almacén de consultas
El Almacén de consultas recopila información de rendimiento detallada sobre consultas, planes de ejecución y estadísticas del entorno de ejecución.

El Almacén de consultas no está activo de forma predeterminada y se puede habilitar con ALTER DATABASE:

   ```sql
   ALTER DATABASE AdventureWorks SET QUERY_STORE = ON;
   ```

Ejecute la consulta siguiente para mostrar información sobre consultas y planes del Almacén de consultas: 

   ```sql
   SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*
   FROM sys.query_store_plan AS Pl
      JOIN sys.query_store_query AS Qry
         ON Pl.query_id = Qry.query_id
      JOIN sys.query_store_query_text AS Txt
         ON Qry.query_text_id = Txt.query_text_id ;
   ```

## <a name="query-dynamic-management-views"></a>Vistas de administración dinámica de consultas
Las funciones y vistas de administración dinámica devuelven información sobre el estado de una instancia de servidor que se puede usar para supervisar el estado de una instancia del servidor, para diagnosticar problemas y para optimizar el rendimiento.

Para consultar la vista de administración dinámica de estadísticas de dm_os_wait:

   ```sql
   SELECT wait_type, wait_time_ms
   FROM sys.dm_os_wait_stats;
   ```