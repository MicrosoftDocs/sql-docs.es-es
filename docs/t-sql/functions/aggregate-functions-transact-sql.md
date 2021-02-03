---
description: Funciones de agregado (Transact-SQL)
title: Funciones de agregado (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], aggregate
- aggregate functions [SQL Server], about aggregate functions
- summarizing functions
- aggregate functions [SQL Server]
ms.assetid: 0c06ae42-eb0a-4d77-9d74-aa1e7f344009
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b51617ece2e2de35d20a262ccd5de011a2180497
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204702"
---
# <a name="aggregate-functions-transact-sql"></a>Funciones de agregado (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Un función de agregado realiza un cálculo sobre un conjunto de valores y devuelve un solo valor. Con la excepción de `COUNT(*)`, las funciones de agregado ignoran los valores NULL. Las funciones de agregado se suelen usar con la cláusula GROUP BY de la instrucción SELECT.
  
Todas las funciones de agregado son deterministas. En otras palabras, las funciones de agregado devuelven el mismo valor cada vez que se las llama con un conjunto específico de valores de entrada. Vea [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md) para obtener más información sobre el determinismo de las funciones. La [cláusula OVER](../../t-sql/queries/select-over-clause-transact-sql.md) puede seguir todas las funciones de agregado excepto STRING_AGG, GROUPING o GROUPING_ID.
  
Las funciones de agregado solo se pueden usar como expresiones en las situaciones siguientes:
-   La lista de selección de una instrucción SELECT (una subconsulta o una consulta externa).  
-   Cláusula HAVING.  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] proporciona las siguientes funciones de agregado:

- [APPROX_COUNT_DISTINCT](../../t-sql/functions/approx-count-distinct-transact-sql.md)
- [AVG](../../t-sql/functions/avg-transact-sql.md)
- [CHECKSUM_AGG](../../t-sql/functions/checksum-agg-transact-sql.md)
- [COUNT](../../t-sql/functions/count-transact-sql.md)
- [COUNT_BIG](../../t-sql/functions/count-big-transact-sql.md)
- [GROUPING](../../t-sql/functions/grouping-transact-sql.md)
- [GROUPING_ID](../../t-sql/functions/grouping-id-transact-sql.md)
- [MAX](../../t-sql/functions/max-transact-sql.md)
- [MIN](../../t-sql/functions/min-transact-sql.md)
- [STDEV](../../t-sql/functions/stdev-transact-sql.md)
- [STDEVP](../../t-sql/functions/stdevp-transact-sql.md)
- [STRING_AGG](../../t-sql/functions/string-agg-transact-sql.md)
- [SUM](../../t-sql/functions/sum-transact-sql.md)
- [VAR](../../t-sql/functions/var-transact-sql.md)
- [VARP](../../t-sql/functions/varp-transact-sql.md)
  
## <a name="see-also"></a>Consulte también
[Funciones integradas &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md)  
[OVER &#40;cláusula de Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  
