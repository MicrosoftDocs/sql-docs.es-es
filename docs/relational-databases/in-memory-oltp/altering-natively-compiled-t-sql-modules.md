---
title: Modificar módulos T-SQL compilados de forma nativa | Microsoft Docs
description: Obtenga información sobre cómo realizar operaciones ALTER en procedimientos almacenados compilados de forma nativa y en módulos de Transact-SQL compilados de forma nativa en SQL Server y Azure SQL Database.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 010318a0-6807-47c3-8ecc-bb7cb60513f0
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dbbdb928a72525586ede01df5bfd7618fa66af51
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237608"
---
# <a name="altering-natively-compiled-t-sql-modules"></a>Altering Natively Compiled T-SQL Modules
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], puede realizar operaciones `ALTER` en procedimientos almacenados compilados de forma nativa y otros módulos [!INCLUDE[tsql](../../includes/tsql-md.md)] compilados de forma nativa, como UDF escalares y desencadenadores mediante la instrucción `ALTER`.  
  
Al ejecutar `ALTER` en un módulo [!INCLUDE[tsql](../../includes/tsql-md.md)] compilado de forma nativa, el módulo se vuelve a compilar con una nueva definición. Mientras la recompilación está en curso, la versión antigua del módulo sigue estando disponible para su ejecución. Una vez completada la compilación, se agotan las ejecuciones del módulo y se instala la nueva versión del módulo. Cuando se modifica un módulo [!INCLUDE[tsql](../../includes/tsql-md.md)] compilado de forma nativa, puede modificar las opciones siguientes.  
  
-   Parámetros  
-   EXECUTE AS  
-   NIVEL DE AISLAMIENTO DE TRANSACCIÓN  
-   LANGUAGE  
-   DATEFIRST  
-   DATEFORMAT  
-   DELAYED_DURABILITY  
  
> [!NOTE]  
> No es posible convertir módulos [!INCLUDE[tsql](../../includes/tsql-md.md)] compilados de forma nativa en módulos compilados de forma no nativa. No es posible convertir módulos T-SQL compilados de forma no nativa en módulos compilados de forma nativa.  
  
Para más información sobre la funcionalidad y la sintaxis de `ALTER PROCEDURE`, vea [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md).  
  
Si ejecuta [sp_recompile](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md) en módulos [!INCLUDE[tsql](../../includes/tsql-md.md)] compilados de forma nativa, hará que los módulos se vuelvan a compilar en la siguiente ejecución.  
  
## <a name="example"></a>Ejemplo  
En el ejemplo siguiente se crea una tabla optimizada para memoria (T1) y un procedimiento almacenado compilado de forma nativa (usp_1) que selecciona todas las columnas de T1. Después, se modifica usp_1 para quitar la cláusula `EXECUTE AS`, cambiar el valor de `LANGUAGE` y seleccionar una sola columna (C1) de T1.  
  
```sql  
CREATE TABLE [dbo].[T1] (  
  [c1] [int] NOT NULL,  
  [c2] [float] NOT NULL,  
  CONSTRAINT [PK_T1] PRIMARY KEY NONCLUSTERED ([c1])  
  ) WITH ( MEMORY_OPTIMIZED = ON , DURABILITY = SCHEMA_AND_DATA )  
GO  
  
CREATE PROCEDURE [dbo].[usp_1]  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english'  
)  
   SELECT c1, c2 FROM dbo.T1  
END  
GO  
  
ALTER PROCEDURE [dbo].[usp_1]  
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'Dutch'  
)  
   SELECT c1 FROM dbo.T1  
END  
GO    
```   
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados compilados de forma nativa](./a-guide-to-query-processing-for-memory-optimized-tables.md)