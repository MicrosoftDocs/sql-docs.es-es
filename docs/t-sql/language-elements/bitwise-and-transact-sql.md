---
description: '&amp; (AND bit a bit) (Transact-SQL)'
title: '&amp; (AND bit a bit) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- bitwise
- '&'
- '&_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- AND, bitwise AND
- '& (bitwise AND)'
- bitwise AND (&)
ms.assetid: 20275755-4fa7-47b1-a9be-ac85606d63b0
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 99d07e7e957c9447f493acee1a40c8a8a96064a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159193"
---
# <a name="amp-bitwise-and-transact-sql"></a>&amp; (AND bit a bit) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Realiza una operación lógica AND bit a bit entre dos valores enteros.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
expression & expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression*  
 Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida de uno de los tipos de datos de la categoría del tipo de datos entero, o los tipos de datos **bit**, **binary** o **varbinary**. *expression* se trata como un número binario para la operación bit a bit.  
  
> [!NOTE]  
>  En una operación bit a bit, solo un objeto *expression* puede tener el tipo de datos **binario** o **varbinary**.  
  
## <a name="result-types"></a>Tipos de resultado  
 **int** si los valores de entrada son **int**.  
  
 **smallint** si los valores de entrada son **smallint**.  
  
 **tinyint** si los valores de entrada son **tinyint** o **bit**.  
  
## <a name="remarks"></a>Observaciones  
 El operador **&** bit a bit realiza una operación lógica AND bit a bit entre las dos expresiones, y, para ello, usa el bit correspondiente de ambas. Los bits del resultado se establecen en 1 si y solo si los dos bits (para el bit actual que se resuelve) de las expresiones de entrada tienen el valor 1; de otro modo, el bit del resultado se establece en 0.  
  
 Si las expresiones de la izquierda y de la derecha tienen tipos de datos de entero diferentes (por ejemplo, *expression* de la izquierda es de tipo **smallint** y *expression* de la derecha es de tipo **int**), el argumento del tipo de datos más pequeño se convierte al tipo de datos mayor. En este caso, la _expresión_ **smallint** se convierte en un valor **int**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una tabla con el tipo de datos **int** para almacenar los valores, y se insertan dos valores en una misma fila.  
  
```sql
CREATE TABLE bitwise (   
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL);  
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 Esta consulta realiza una operación AND bit a bit entre las columnas `a_int_value` y `b_int_value`.  
  
```sql  
SELECT a_int_value & b_int_value  
FROM bitwise;  
GO  
```  
  
 El conjunto de resultados es:  
  
```  
-----------   
10            
  
(1 row(s) affected)  
```  
  
 La representación binaria de 170 (`a_int_value` o `A`) es `0000 0000 1010 1010`. La representación binaria de 75 (`b_int_value` o `B`) es `0000 0000 0100 1011`. Al realizar la operación AND bit a bit con estos dos valores, se obtiene el resultado binario `0000 0000 0000 1010`, que es el 10 en notación decimal.  
  
```  
(A & B)  
0000 0000 1010 1010  
0000 0000 0100 1011  
-------------------  
0000 0000 0000 1010  
```  
  
  
## <a name="see-also"></a>Consulte también  
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Operadores bit a bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)   
 [&= &#40;Asignación de AND bit a bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)   
 [Operadores compuestos &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


