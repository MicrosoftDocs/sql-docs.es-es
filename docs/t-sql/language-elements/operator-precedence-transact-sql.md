---
description: Prioridad de operador (Transact-SQL)
title: Prioridad de operador (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], precedence
- operator precedence [Transact-SQL]
- order of operator execution [Transact-SQL]
- precedence [SQL Server], operators
ms.assetid: f04d2439-6fff-4e4c-801f-cc62faef510a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7ef9b05c0ecdbb0054c9ad0ef3e1a88b123fb87b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191907"
---
# <a name="operator-precedence-transact-sql"></a>Prioridad de operador (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cuando una expresión compleja tiene múltiples operadores, la prioridad de operador determina la secuencia en que se realizan las operaciones. El orden de ejecución puede afectar de manera significativa al valor resultante.  
  
 Los operadores tienen los niveles de prioridad que se muestran en la siguiente tabla. Un operador de los niveles más altos se evalúa antes que un operador de un nivel más bajo. En la tabla siguiente, 1 es el nivel más alto y 8 es el nivel más bajo.
  
|Nivel|Operadores|  
|-----------|---------------|  
|1|~ (operador bit a bit NOT)|  
|2|* (multiplicación), / (división), % (módulo)|  
|3|+ (positivo), - (negativo), + (suma), + (concatenación), - (resta), & (AND bit a bit), ^ (OR exclusivo bit a bit), &#124; (OR bit a bit)|  
|4|=, >, \<, >=, <=, <>, !=, !>, !< (operadores de comparación)|  
|5|NOT|  
|6|y|  
|7|ALL, ANY, BETWEEN, IN, LIKE, OR, SOME|  
|8|= (asignación)|  
  
 Cuando en una expresión dos operadores tienen el mismo nivel de prioridad, se evalúan de izquierda a derecha en función de su posición dentro de la expresión. Por ejemplo, en la expresión utilizada en la siguiente instrucción `SET`, el operador de resta se evalúa antes que el operador de suma.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 4 - 2 + 27;  
-- Evaluates to 2 + 27 which yields an expression result of 29.  
SELECT @MyNumber;  
```  
  
 Utilice paréntesis para suplantar la prioridad definida de los operadores en una expresión. Todo el contenido dentro del paréntesis se evalúa para obtener un valor único. Ese valor puede ser utilizado por cualquier otro operador fuera de los paréntesis.  
  
 Por ejemplo, en la expresión utilizada en la siguiente instrucción `SET`, el operador de multiplicación tiene una prioridad mayor que el operador de suma. La operación de multiplicación se evalúa antes; el resultado de la expresión es `13`.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * 4 + 5;  
-- Evaluates to 8 + 5 which yields an expression result of 13.  
SELECT @MyNumber;  
```  
  
 En la expresión utilizada en la siguiente instrucción `SET`, el paréntesis hace que primero se lleve a cabo la suma. El resultado de la expresión es `18`.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * (4 + 5);  
-- Evaluates to 2 * 9 which yields an expression result of 18.  
SELECT @MyNumber;  
```  
  
 Si una expresión tiene paréntesis anidados, se evalúa primero la expresión más anidada. El siguiente ejemplo contiene paréntesis anidados, con la expresión `5 - 3` en el conjunto de paréntesis más anidado. Esta expresión produce un valor de `2`. Entonces, el operador de suma (`+`) suma este resultado a `4`, dando como resultado `6`. Finalmente, `6` se multiplica por `2` para producir un resultado de expresión de `12`.  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * (4 + (5 - 3) );  
-- Evaluates to 2 * (4 + 2) which then evaluates to 2 * 6, and   
-- yields an expression result of 12.  
SELECT @MyNumber;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Operadores lógicos &#40;Transact-SQL&#41;](../../t-sql/language-elements/logical-operators-transact-sql.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)  
  
