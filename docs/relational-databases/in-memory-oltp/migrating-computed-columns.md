---
title: Migración de columnas calculadas | Microsoft Docs
description: Obtenga información sobre cómo simular columnas calculadas en tablas optimizadas para memoria. Evalúe si la funcionalidad de columna calculada es necesaria después de la migración.
ms.custom: ''
ms.date: 12/17/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 64a9eade-22c3-4a9d-ab50-956219e08df1
author: MightyPen
ms.author: genemi
monikerRange: =sql-server-2016
ms.openlocfilehash: a5038368e66524324fb0189b2f1b06e0b374e41a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100353690"
---
# <a name="migrating-computed-columns"></a>Migrar columnas calculadas

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Las tablas optimizadas para memoria no admiten columnas calculadas. Sin embargo, puede simular una columna calculada.

A partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)], se admiten columnas calculadas en tablas e índices optimizados para memoria.

Debe considerar la necesidad de conservar sus columnas calculadas al migrar las tablas basadas en disco a tablas optimizadas para memoria. Las diferentes características de rendimiento de las tablas optimizadas para memoria y los procedimientos almacenados compilados de forma nativa pueden invalidar la necesidad de persistencia.  
  
## <a name="non-persisted-computed-columns"></a>Columnas calculadas no persistentes  
 Para simular los efectos de una columna calculada no persistente, cree una vista en la tabla optimizada para memoria. En la instrucción SELECT que define la vista, agregue la definición de columna calculada en la vista. Excepto en un procedimiento almacenado compilado de forma nativa, las consultas que utilizan los valores de la columna calculada deben leerse en la vista. Dentro de los procedimientos almacenados compilados de forma nativa, debe actualizar cualquier instrucción de selección, actualización o eliminación según la definición de la columna calculada.  
  
```sql  
-- Schema for the table dbo.OrderDetails:  
-- OrderId int not null primary key,  
-- ProductId int not null,  
-- SalePrice money not null,  
-- Quantity int not null,  
-- Total money not null  
--  
-- Total is computed as SalePrice * Quantity and is not persisted.  
CREATE VIEW dbo.v_order_details AS  
   SELECT  
      OrderId,  
      ProductId,  
      SalePrice,  
      Quantity,  
      Quantity * SalePrice AS Total  
   FROM dbo.order_details  
```  
  
## <a name="persisted-computed-columns"></a>Columnas calculadas persistentes  
 Para simular los efectos de una columna calculada persistente, cree un procedimiento almacenado para insertar en la tabla y otro procedimiento almacenado para actualizar la tabla. Al insertar o actualizar la tabla, invoque estos procedimientos almacenados para realizar estas tareas. En los procedimientos almacenados, calcule el valor para el campo calculado según las entradas, igual que se define la columna calculada en la tabla basada en disco original. A continuación, inserte en la tabla o actualícela según se necesite en el procedimiento almacenado.  
  
```sql  
-- Schema for the table dbo.OrderDetails:  
-- OrderId int not null primary key,  
-- ProductId int not null,  
-- SalePrice money not null,  
-- Quantity int not null,  
-- Total money not null  
--  
-- Total is computed as SalePrice * Quantity and is persisted.  
-- we need to create insert and update procedures to calculate Total.  

CREATE PROCEDURE sp_insert_order_details   
@OrderId int, @ProductId int, @SalePrice money, @Quantity int  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH (LANGUAGE = N'english', TRANSACTION ISOLATION LEVEL = SNAPSHOT)  

-- compute the value here.   
-- this stored procedure works with single rows only.  
-- for bulk inserts, accept a table-valued parameter into the stored procedure  
-- and use an INSERT INTO SELECT statement.  

DECLARE @total money = @SalePrice * @Quantity  
INSERT INTO dbo.OrderDetails (OrderId, ProductId, SalePrice, Quantity, Total)  
VALUES (@OrderId, @ProductId, @SalePrice, @Quantity, @total)  
END  
GO  
  
CREATE PROCEDURE sp_update_order_details_by_id  
@OrderId int, @ProductId int, @SalePrice money, @Quantity int  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH (LANGUAGE = N'english', TRANSACTION ISOLATION LEVEL = SNAPSHOT)  

-- compute the value here.   
-- this stored procedure works with single rows only.  

DECLARE @total money = @SalePrice * @Quantity  
UPDATE dbo.OrderDetails   
SET ProductId = @ProductId, SalePrice = @SalePrice, Quantity = @Quantity, Total = @total  
WHERE OrderId = @OrderId  
END  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Migrar a OLTP en memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
