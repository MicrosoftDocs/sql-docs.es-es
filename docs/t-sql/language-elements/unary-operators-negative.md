---
description: '- (Negativo) (Transact-SQL)'
title: '- (Negativo) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- negative
dev_langs:
- TSQL
helpviewer_keywords:
- '- (negative)'
- negative operator (-)
- negative values
ms.assetid: d6c14d14-d379-403b-82db-c197ad58c896
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6302a8b54b4316d689fe1c4b4bbcc458f21675c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203611"
---
# <a name="unary-operators---negative"></a>Operadores unarios: negativo
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve el valor negativo de una expresión numérica (un operador unario). Los operadores unarios realizan una operación sobre una única expresión de cualquiera de los tipos de datos de la categoría del tipo de datos numérico.   
  
|Operator|Significado|  
|--------------|-------------|  
|[+ (Positivo)](../../t-sql/language-elements/unary-operators-positive.md)|El valor numérico es positivo.|  
|[- (Negativo)](../../t-sql/language-elements/unary-operators-negative.md)|El valor numérico es negativo.|  
|[~ (NOT bit a bit)](../../t-sql/language-elements/bitwise-not-transact-sql.md)|Devuelve los bits complementarios del número.|  
  
 Los operadores + (Positivo) y - (Negativo) se pueden utilizar en cualquier expresión de cualquiera de los tipos de datos de la categoría del tipo de datos numérico. El operador ~ (NOT bit a bit) solo se puede utilizar en expresiones de cualquiera de los tipos de datos de la categoría del tipo de datos entero. 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
- numeric_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *numeric_expression*  
 Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida de cualquiera de los tipos de datos de la categoría del tipo de datos numérico, a excepción de la categoría de fecha y hora.  
  
## <a name="result-types"></a>Tipos de resultado  
 Devuelve el tipo de datos de *expresión_numérica*, excepto si se trata de una expresión **tinyint** sin signo que se promueve a un resultado **smallint** con signo.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-setting-a-variable-to-a-negative-value"></a>A. Establecer una variable con un valor negativo  
 En el ejemplo siguiente se establece una variable con un valor negativo.  
  
```sql 
USE tempdb;  
GO  
DECLARE @MyNumber DECIMAL(10,2);  
SET @MyNumber = -123.45;  
SELECT @MyNumber AS NegativeValue;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
NegativeValue  
---------------------------------------  
-123.45  
  
(1 row(s) affected)  
  
```  
  
### <a name="b-changing-a-variable-to-a-negative-value"></a>B. Cambiar una variable a un valor negativo  
 En el ejemplo siguiente se cambia una variable a un valor negativo.  
  
```sql  
USE tempdb;  
GO  
DECLARE @Num1 INT;  
SET @Num1 = 5;  
SELECT @Num1 AS VariableValue, -@Num1 AS NegativeValue;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
VariableValue NegativeValue  
------------- -------------  
5             -5  
  
(1 row(s) affected)  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-returning-the-negative-of-a-positive-constant"></a>C. Devolución del valor negativo de una constante positiva  
 En el ejemplo siguiente se devuelve el valor negativo de una constante positiva.  
  
```sql  
USE ssawPDW;  
  
SELECT TOP (1) - 17 FROM DimEmployee;  
```  
  
 Devoluciones  
  
```  
-17  
```  
  
### <a name="d-returning-the-positive-of-a-negative-constant"></a>D. Devolución del valor positivo de una constante negativa  
 En el ejemplo siguiente se devuelve el valor positivo de una constante negativa.  
  
```sql  
USE ssawPDW;  
  
SELECT TOP (1) - ( - 17) FROM DimEmployee;  
```  
  
 Devoluciones  
  
```  
17  
```  
  
### <a name="e-returning-the-negative-of-a-column"></a>E. Devolución del valor negativo de una columna  
 En el ejemplo siguiente se devuelve el valor negativo del valor `BaseRate` de cada empleado de la tabla `dimEmployee`.  
  
```sql  
USE ssawPDW;  
  
SELECT - BaseRate FROM DimEmployee;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)  
  
  

