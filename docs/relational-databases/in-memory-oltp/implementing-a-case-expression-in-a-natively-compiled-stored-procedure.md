---
title: Expresión CASE de un procedimiento almacenado compilado de forma nativa
description: Los módulos T-SQL compilados de forma nativa admiten expresiones CASE en algunas versiones de SQL Server. En este ejemplo se implementa la expresión CASE en una consulta.
ms.custom: seo-dt-2019
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2f82db01-da7e-4a7d-8bc0-48b245e6f768
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9711cebcaab12cd0c665591612b56aefd8f0cfbb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351259"
---
# <a name="implementing-a-case-expression-in-a-natively-compiled-stored-procedure"></a>Implementación de una expresión CASE de un procedimiento almacenado compilado de forma nativa
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

**Se aplica a:** [!INCLUDE[ssSDSFull_md](../../includes/sssdsfull-md.md)] y SQL Server a partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]

Las expresiones CASE se admiten en los módulos T-SQL compilados de forma nativa. En el ejemplo siguiente se muestra una forma de usar la expresión CASE en una consulta. 

``` 
-- Query using a CASE expression in a natively compiled stored procedure.
CREATE PROCEDURE dbo.usp_SOHOnlineOrderResult  
   WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
   AS BEGIN ATOMIC WITH  (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE=N'us_english')  
   SELECT   
      SalesOrderID,   
      CASE (OnlineOrderFlag)   
      WHEN 1 THEN N'Order placed online by customer'  
      ELSE N'Order placed by sales person'  
      END  
   FROM Sales.SalesOrderHeader_inmem
END  
GO  
  
EXEC dbo.usp_SOHOnlineOrderResult  
GO  
``` 

**Se aplica a:** [!INCLUDE[ssSQL14-md](../../includes/ssSQL14-md.md)] y SQL Server a partir de [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]

  Las expresiones CASE *no* se admiten en los módulos T-SQL compilados de forma nativa. En el ejemplo siguiente se muestra una forma de implementar la funcionalidad de una expresión CASE en un procedimiento almacenado compilado de forma nativa.  
  
 En los ejemplos de código se usa una variable de tabla para crear un conjunto de resultados único. Esto solo es adecuado al procesar un número limitado de filas, ya que implica la creación de una copia adicional de las filas de datos.  
  
 Es conveniente que pruebe el rendimiento de esta solución.  
  
```  
-- original query  
SELECT   
   SalesOrderID,   
   CASE (OnlineOrderFlag)   
   WHEN 1 THEN N'Order placed online by customer'  
   ELSE N'Order placed by sales person'  
   END  
FROM Sales.SalesOrderHeader_inmem  
  
--  workaround for CASE in natively compiled stored procedures  
--  use a table for the single resultset  
CREATE TYPE dbo.SOHOnlineOrderResult AS TABLE  
(  
   SalesOrderID uniqueidentifier not null index ix_SalesOrderID,  
     OrderFlag nvarchar(100) not null  
) with (memory_optimized=on)  
go  
  
-- natively compiled stored procedure that includes the query  
CREATE PROCEDURE dbo.usp_SOHOnlineOrderResult  
   WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
   AS BEGIN ATOMIC WITH  
      (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE=N'us_english')  
  
   -- table variable for creating the single resultset  
   DECLARE @result dbo.SOHOnlineOrderResult  
  
   -- CASE OnlineOrderFlag=1  
   INSERT @result   
   SELECT SalesOrderID, N'Order placed online by customer'  
      FROM Sales.SalesOrderHeader_inmem  
      WHERE OnlineOrderFlag=1  
  
   -- ELSE  
   INSERT @result   
   SELECT SalesOrderID, N'Order placed by sales person'  
      FROM Sales.SalesOrderHeader_inmem  
      WHERE OnlineOrderFlag!=1  
  
   -- return single resultset  
   SELECT SalesOrderID, OrderFlag FROM @result  
END  
GO  
  
EXEC dbo.usp_SOHOnlineOrderResult  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Problemas de migración para los procedimientos almacenados compilados de forma nativa](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Construcciones de Transact-SQL no admitidas en In-Memory OLTP.](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  
