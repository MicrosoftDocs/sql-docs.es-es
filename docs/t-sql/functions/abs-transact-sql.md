---
description: ABS (Transact-SQL)
title: ABS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ABS_TSQL
- ABS
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], positive
- values [SQL Server], absolute
- ABS function
- absolute positive value
ms.assetid: e2ea7a6d-3e2f-472c-afbc-437d3b835c03
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8bb8cf3767ee06f7d0548353327ec3c6da315732
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204747"
---
# <a name="abs-transact-sql"></a>ABS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Función matemática que devuelve el valor absoluto positivo de una expresión numérica específica. (`ABS` cambia los valores negativos por valores positivos. `ABS` no tiene ningún efecto en los valores cero o positivos).
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ABS ( numeric_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*numeric_expression*  
Una expresión de la categoría de tipo de datos numérico exacto o numérico aproximado.
  
## <a name="return-types"></a>Tipos de valor devuelto  
Devuelve el mismo tipo que *numeric_expression*.
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se muestra el resultado de usar la función `ABS` en tres números distintos.
  
```sql
SELECT ABS(-1.0), ABS(0.0), ABS(1.0);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---- ---- ----  
1.0  .0   1.0  
```  
  
La función `ABS` puede causar un error de desbordamiento cuando el valor absoluto de un número supera el número más grande que puede representar el tipo de datos especificado. Por ejemplo, el tipo de datos `int` tiene un rango de valores comprendido entre `-2,147,483,648` y `2,147,483,647`. El cálculo del valor absoluto del entero con signo `-2,147,483,648` causará un error de desbordamiento porque su valor absoluto supera el límite del intervalo positivo para el tipo de datos `int`.
  
```sql
DECLARE @i INT;  
SET @i = -2147483648;  
SELECT ABS(@i);  
GO  
```  
  
Devuelve este mensaje de error:
  
"Mensaje 8115, nivel 16, estado 2, línea 3"
  
“Error de desbordamiento aritmético al convertir expresión al tipo de datos int”.

  
## <a name="see-also"></a>Consulte también
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[Funciones matemáticas &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)  
[Funciones integradas &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md)
  
  

