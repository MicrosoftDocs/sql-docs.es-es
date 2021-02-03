---
description: + (Suma unaria) (Transact-SQL)
title: + (Signo unario más) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- + (positive)
- +
- positive
dev_langs:
- TSQL
helpviewer_keywords:
- + (positive operator)
- positive operator (+)
- positive values [SQL Server]
ms.assetid: 0f31c5cc-3078-4f6a-9870-7eb1a98053fb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4e72fc7ec26e44298111b5d431fede0f4ef6e6fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184709"
---
# <a name="unary-operators---positive"></a>Operadores unarios: positivo
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Devuelve el valor de una expresión numérica (un operador unario). Los operadores unarios realizan una operación sobre una única expresión de cualquiera de los tipos de datos de la categoría del tipo de datos numérico.   
  
|Operator|Significado|  
|--------------|-------------|  
|[+ (Positivo)](../../t-sql/language-elements/unary-operators-positive.md)|El valor numérico es positivo.|  
|[- (Negativo)](../../t-sql/language-elements/unary-operators-negative.md)|El valor numérico es negativo.|  
|[~ (NOT bit a bit)](../../t-sql/language-elements/bitwise-not-transact-sql.md)|Devuelve los bits complementarios del número.|  
  
 Los operadores + (Positivo) y - (Negativo) se pueden utilizar en cualquier expresión de cualquiera de los tipos de datos de la categoría del tipo de datos numérico. El operador ~ (NOT bit a bit) solo se puede utilizar en expresiones de cualquiera de los tipos de datos de la categoría del tipo de datos entero.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
+ numeric_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *numeric_expression*  
 Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida de cualquiera de los tipos de datos de la categoría de tipos de datos numéricos, excepto los tipos de datos **datetime** y **smalldatetime**.  
  
## <a name="result-types"></a>Tipos de resultado  
 Devuelve el tipo de datos de *numeric_expression*.  
  
## <a name="remarks"></a>Comentarios  
 Aunque una suma unaria puede aparecer antes de cualquier expresión numérica, no realiza ninguna operación en el valor devuelto de la expresión. En concreto, no devolvería el valor positivo de una expresión negativa. Para devolver el valor positivo de una expresión negativa, use la función [ABS](../../t-sql/functions/abs-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-setting-a-variable-to-a-positive-value"></a>A. Establecer una variable en un valor positivo  
 En el siguiente ejemplo se establece una variable en un valor positivo.  
  
```sql  
DECLARE @MyNumber DECIMAL(10,2);  
SET @MyNumber = +123.45;  
SELECT @MyNumber;  
GO  
```  
  
 El conjunto de resultados es:  
  
```  
-----------   
123.45            
  
(1 row(s) affected)  
```  
  
### <a name="b-using-the-unary-plus-operator-with-a-negative-value"></a>B. Usar el operador de suma unaria con un valor negativo  
 En el siguiente ejemplo se muestra el uso de la suma unaria con una expresión negativa y la función ABS() en la misma expresión negativa. La suma unaria no afecta a la expresión, pero la función ABS devuelve el valor positivo de la expresión.  
  
```sql  
USE tempdb;  
GO  
DECLARE @Num1 INT;  
SET @Num1 = -5;  
SELECT +@Num1, ABS(@Num1);  
GO  
```  
  
 El conjunto de resultados es:  
  
```  
----------- -----------  
-5          5  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también  
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [ABS &#40;Transact-SQL&#41;](../../t-sql/functions/abs-transact-sql.md)  
  
  
