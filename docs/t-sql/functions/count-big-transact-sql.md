---
description: COUNT_BIG (Transact-SQL)
title: COUNT_BIG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COUNT_BIG_TSQL
- COUNT_BIG
dev_langs:
- TSQL
helpviewer_keywords:
- totals [SQL Server], COUNT_BIG function
- counting items in group
- groups [SQL Server], number of items in
- number of group items
- COUNT_BIG function
ms.assetid: f2e3601f-487e-4917-bb01-47b1047908cd
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 639ee47ccf91d456c4f5a34b4b53a4cfb85ed0dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184162"
---
# <a name="count_big-transact-sql"></a>COUNT_BIG (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve el número de elementos encontrados en un grupo. `COUNT_BIG` funciona como la función [COUNT](../../t-sql/functions/count-transact-sql.md). Estas funciones difieren solo en los tipos de datos de sus valores devueltos. `COUNT_BIG` siempre devuelve un valor de tipo de datos **bigint**. `COUNT` siempre devuelve un valor de tipo de datos **int**.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql

-- Aggregation Function Syntax  
COUNT_BIG ( { [ [ ALL | DISTINCT ] expression ] | * } )  
  
-- Analytic Function Syntax  
COUNT_BIG ( [ ALL ] { expression | * } ) OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
ALL  
Aplica la función de agregado a todos los valores. ALL sirve como valor predeterminado.
  
DISTINCT  
Especifica que `COUNT_BIG` devuelva el número de valores únicos no NULL.
  
*expression*  
Una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de cualquier tipo. `COUNT_BIG` no admite funciones de agregado ni subconsultas en una expresión.
  
*\**  
Especifica que `COUNT_BIG` debe contar todas las filas para determinar el total de filas de la tabla a devolver. `COUNT_BIG(*)` no toma ningún parámetro y no admite el uso de DISTINCT. `COUNT_BIG(*)` no requiere el parámetro *expression* porque, por definición, no usa información sobre ninguna columna concreta. `COUNT_BIG(*)` devuelve el número de filas de una tabla especificada y conserva las filas duplicadas. Cuenta cada fila por separado, incluidas las filas que contienen valores de tipo null.
  
OVER **(** [ *partition_by_clause* ] [ *order_by_clause* ] **)**  
*partition_by_clause* divide el conjunto de resultados generado por la cláusula `FROM` en particiones a las que se aplica la función `COUNT_BIG`. Si no se especifica, la función trata todas las filas del conjunto de resultados de la consulta como un único grupo. *order_by_clause* determina el orden lógico de la operación. Para más información, consulte [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).
  
## <a name="return-types"></a>Tipos de valores devueltos
**bigint**
  
## <a name="remarks"></a>Observaciones  
COUNT_BIG(\*) devuelve el número de elementos de un grupo. Esto incluye los valores NULL y los duplicados
  
COUNT_BIG (ALL *expresión*) evalúa *expresión* en todas las filas del grupo y devuelve el número de valores no NULL.
  
COUNT_BIG (DISTINCT *expresión*) evalúa *expresión* en todas las filas del grupo y devuelve el número de valores únicos no NULL.
  
COUNT_BIG es una función determinista cuando se usa **_sin_** las cláusulas OVER y ORDER BY. COUNT_BIG es no determinista cuando se usa **_con_** las cláusulas OVER y ORDER BY. Para más información, consulte [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Ejemplos  
Para ver algunos ejemplos, consulte [COUNT &#40;Transact-SQL&#41;](../../t-sql/functions/count-transact-sql.md).
  
## <a name="see-also"></a>Vea también
[Funciones de agregado &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT &#40;Transact-SQL&#41;](../../t-sql/functions/count-transact-sql.md)  
[int, bigint, smallint y tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)  
[OVER &#40;cláusula de Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  
