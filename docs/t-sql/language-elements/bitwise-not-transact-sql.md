---
description: ~ (NOT bit a bit) (Transact-SQL)
title: ~ (NOT bit a bit) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ~_TSQL
- bitwise
- NOT
- "~"
- Bitwise NOT
dev_langs:
- TSQL
helpviewer_keywords:
- NOT keyword
- bitwise NOT (~)
- ~ (bitwise NOT)
ms.assetid: 02da8016-f6c0-41ae-8d59-33eaa02bfc95
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 473ef65d1b2149268e5e3b0b8bd3e76e2c65a20b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199995"
---
# <a name="-bitwise-not-transact-sql"></a>~ (NOT bit a bit) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Lleva a cabo una operación lógica NOT bit a bit en un valor entero.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
~ expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression*  
 Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida de uno de los tipos de datos de la categoría del tipo de datos entero, o los tipos de datos **bit**, **binary** o **varbinary**. *expression* se trata como un número binario para la operación bit a bit.  
  
> [!NOTE]  
>  Solo una *expression* puede ser del tipo de datos **binary** o **varbinary** en una operación bit a bit.  
  
## <a name="result-types"></a>Tipos de resultado  
 **int** si los valores de entrada son **int**.  
  
 **smallint** si los valores de entrada son **smallint**.  
  
 **tinyint** si los valores de entrada son **tinyint**.  
  
 **bit** si los valores de entrada son **bit**.  
  
## <a name="remarks"></a>Comentarios  
 El operador bit a bit **~** realiza una operación lógica NOT bit a bit para *expression*, tomando un bit cada vez. Si *expression* tiene un valor de 0, los bits en el conjunto de resultados están establecidos en 1; de lo contrario, el bit en el resultado se borra a un valor de 0. En otras palabras, los unos pasan a los ceros y los ceros pasan a unos.  
  
> [!IMPORTANT]  
>  Cuando se realiza cualquier clase de operación bit a bit, es importante la longitud del almacenamiento de la expresión que se utiliza para la operación bit a bit. Se recomienda utilizar el mismo número de bytes al almacenar los valores. Por ejemplo, almacenar el valor decimal 5 como un **tinyint**, **smallint** o **int** produce un valor que se almacena con un número de bytes diferente: **tinyint** almacena los datos con 1 byte, **smallint** los almacena con 2 bytes e **int**, con 4 bytes. De esta forma, realizar una operación bit a bit en un valor decimal **int** puede producir resultados distintos de los que usan una traducción a binario directo o hexadecimal, especialmente cuando se usa el operador **~** (NOT bit a bit). La operación NOT bit a bit puede tener lugar en una variable con una longitud menor. En este caso, cuando se convierte a una variable de un tipo de datos más largo, los bits del conjunto de 8 bits superiores no pueden establecerse en el valor previsto. Se recomienda convertir la variable del tipo de datos más pequeño al tipo de datos mayor, y, a continuación, realizar la operación NOT en el resultado.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una tabla con el tipo de datos **int** para almacenar los valores y se insertan los dos valores en una fila.  
  
```sql  
CREATE TABLE bitwise (  
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL); 
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 La consulta siguiente realiza la operación NOT bit a bit en las columnas `a_int_value` y `b_int_value`.  
  
```sql  
SELECT ~ a_int_value, ~ b_int_value  
FROM bitwise;  
```  
  
 El conjunto de resultados es:  
  
```  
--- ---   
-171  -76   
  
(1 row(s) affected)  
```  
  
 La representación binaria de 170 (`a_int_value` o `A`) es `0000 0000 1010 1010`. Al realizar la operación NOT bit a bit con este valor, se obtiene el resultado binario `1111 1111 0101 0101`, que es el -171 en notación decimal. La representación binaria de 75 es `0000 0000 0100 1011`. Realizar la operación NOT bit a bit produce `1111 1111 1011 0100`, que es el decimal -76.  
  
```  
 (~A)     
         0000 0000 1010 1010  
         -------------------  
         1111 1111 0101 0101  
(~B)     
         0000 0000 0100 1011  
         -------------------  
         1111 1111 1011 0100  
```  
  
 
## <a name="see-also"></a>Consulte también  
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Operadores bit a bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
  
  


