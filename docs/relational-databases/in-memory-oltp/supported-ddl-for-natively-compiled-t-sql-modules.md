---
title: Construcciones DDL admitidas para módulos T-SQL compilados de forma nativa | Microsoft Docs
description: Obtenga información sobre las construcciones DDL admitidas para módulos de T-SQL compilados de forma nativa, como procedimientos almacenados, UDF escalares, funciones TVF insertadas y desencadenadores.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 6b21f47e-bceb-4054-8b3c-9d39bb9583c0
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7dbce86c0b46e1d256934b79765372978221dd41
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485147"
---
# <a name="supported-ddl-for-natively-compiled-t-sql-modules"></a>Construcciones DDL admitidas para módulos T-SQL compilados de forma nativa
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se enumeran las construcciones DDL admitidas para módulos T-SQL compilados de forma nativa, como procedimientos almacenados, UDF escalares, funciones TVF en línea y desencadenadores.  
  
 Para obtener información sobre las características y el área expuesta de T-SQL que se pueden usar como parte de los módulos T-SQL compilados de forma nativa, vea [Características admitidas en los módulos T-SQL compilados de forma nativa](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
 Para obtener información sobre las construcciones no compatibles, vea [Construcciones Transact-SQL no admitidas por OLTP en memoria](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md).  
  
 Se admite lo siguiente:  
  
-   [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
  
-   [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)  
  
-   [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)  
  
-   Instrucciones [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md) e INSERT SELECT  
  
-   SCHEMABINDING y BEGIN ATOMIC (se requiere para los procedimientos almacenados compilados de forma nativa)  
  
     Para más información, vea [Creating Natively Compiled Stored Procedures](../../relational-databases/in-memory-oltp/creating-natively-compiled-stored-procedures.md).  
  
-   NATIVE_COMPILATION  
  
     Para más información, vea [Native Compilation of Tables and Stored Procedures](../../relational-databases/in-memory-oltp/native-compilation-of-tables-and-stored-procedures.md).  
  
-   Los parámetros y las variables se pueden declarar como NOT NULL (disponibles solo para módulos compilados de forma nativa: procedimientos almacenados compilados de forma nativa y funciones definidas por el usuario escalares y compiladas de forma nativa).  
  
-   Parámetros con valores de tabla.  
  
     Para obtener más información, vea[Usar parámetros con valores de tabla &#40;motor de base de datos&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
-   EXECUTE AS OWNER, SELF, CALLER y usuario.  
  
-   GRANT (conceda) y DENY (deniegue) permisos a las tablas y los procedimientos.  
  
     Para obtener más información, vea [GRANT &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md) y [DENY &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados compilados de forma nativa](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
