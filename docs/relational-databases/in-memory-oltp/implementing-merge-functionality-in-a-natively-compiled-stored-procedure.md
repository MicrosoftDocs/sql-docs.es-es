---
title: 'Función MERGE: procedimiento almacenado compilado de forma nativa'
description: Use este ejemplo para obtener información sobre cómo simular la instrucción MERGE de Transact-SQL en un módulo compilado de forma nativa.
ms.custom: seo-dt-2019
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: d4bcdc36-3302-4abc-9b35-64ec2b920986
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c3513b0839c4370e1815e56683fc8f49f82962a4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460438"
---
# <a name="implementing-merge-functionality-in-a-natively-compiled-stored-procedure"></a>Implementación de la funcionalidad MERGE en un procedimiento almacenado compilado de forma nativa
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  
Con el ejemplo de código Transact-SQL de esta sección se muestra cómo simular la instrucción T-SQL MERGE en un módulo compilado de forma nativa. En el ejemplo se usa una variable de tabla con una columna de identidad, se itera por las filas en la variable de tabla y, en cada fila, se lleva a cabo una actualización si se cumple la condición o una inserción si la condición no se cumple.
  
Esta es la instrucción T-SQL MERGE que quisiéramos que se admitiera dentro de un proceso nativo y que el ejemplo de código simula.  

```sql
MERGE INTO dbo.Table1 t  
    USING @tvp v  
    ON t.Column1 = v.c1  
    WHEN MATCHED THEN   
        UPDATE SET Column2 = v.c2  
    WHEN NOT MATCHED THEN  
        INSERT (Column1, Column2) VALUES (v.c1, v.c2);  
```

Esta es la instrucción T-SQL MERGE para lograr una solución alternativa y simular la instrucción MERGE.  

```sql
DROP PROCEDURE IF EXISTS dbo.usp_merge1;  
go  
DROP TYPE IF EXISTS dbo.Type1;  
go  
DROP TABLE IF EXISTS dbo.Table1;  
go  
-----------------------------  
-- target table and table type used for the workaround
-----------------------------  

CREATE TABLE dbo.Table1  
(  
    Column1  INT  NOT NULL  PRIMARY KEY NONCLUSTERED,  
    Column2  INT  NOT NULL  
)   
    WITH (MEMORY_OPTIMIZED = ON);  
go  

CREATE TYPE dbo.Type1 AS TABLE  
(  
    c1  INT  NOT NULL,  
    c2  INT  NOT NULL,  

    RowID    INT  NOT NULL  IDENTITY(1,1),  
    INDEX ix_RowID HASH (RowID) WITH (BUCKET_COUNT=1024)  
)   
    WITH (MEMORY_OPTIMIZED = ON);  
go  
-----------------------------  
-- stored procedure implementing the workaround
-----------------------------  

CREATE PROCEDURE dbo.usp_merge1   
    @tvp1 dbo.Type1 READONLY  
    WITH  
    NATIVE_COMPILATION, SCHEMABINDING  
AS   
BEGIN ATOMIC  
    WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT,  
            LANGUAGE = N'us_english')  

    DECLARE  @i INT = 1,  @c1 INT,  @c2 INT;  

    WHILE @i > 0  
    BEGIN  
        SELECT @c1 = c1, @c2 = c2  
            FROM @tvp1  
            WHERE RowID = @i;  

        --test whether the row exists in the TVP; if not, we end the loop
        IF @@ROWCOUNT=0  
            SET @i = 0
        ELSE
        BEGIN
            -- try the update
            UPDATE dbo.Table1  
                SET   Column2 = @c2  
                WHERE Column1 = @c1;  

            -- if there was no row to update, we insert
            IF @@ROWCOUNT=0  
                INSERT INTO dbo.Table1 (Column1, Column2)  
                    VALUES (@c1, @c2);  

            SET @i += 1
        END
    END  
END  
go  
-----------------------------  
-- test to validate the functionality
-----------------------------  

INSERT dbo.Table1 VALUES (1,2);  
go  

SELECT N'Before-MERGE' AS [Before-MERGE], Column1, Column2  
    FROM dbo.Table1;  
go  

DECLARE @tvp1 dbo.Type1;  

INSERT @tvp1 (c1, c2) VALUES (1,33), (2,4);  
EXECUTE dbo.usp_merge1 @tvp1;  
go  

SELECT N'After--MERGE' AS [After--MERGE], Column1, Column2  
    FROM dbo.Table1;  
go  
```

```console
    /****  Actual output:  
  
    Before-MERGE   Column1   Column2  
    Before-MERGE      1         2  
  
    After--MERGE   Column1   Column2  
    After--MERGE      1        33  
    After--MERGE      2         4  
    ****/  
```

## <a name="see-also"></a>Consulte también  
 [Problemas de migración para los procedimientos almacenados compilados de forma nativa](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Construcciones de Transact-SQL no admitidas en In-Memory OLTP.](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  
