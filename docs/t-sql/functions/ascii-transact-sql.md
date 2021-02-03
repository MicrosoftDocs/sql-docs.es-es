---
description: ASCII (Transact-SQL)
title: ASCII (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASCII_TSQL
- ASCII
dev_langs:
- TSQL
helpviewer_keywords:
- ASCII function
- characters [SQL Server], ASCII
- code [SQL Server], ASCII
- leftmost character of expression
ms.assetid: 45c2044a-0593-4805-8bae-0fad4bde2e6b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 57d1d99951784c4126accc3862bc86b5bc9dce7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193481"
---
# <a name="ascii-transact-sql"></a>ASCII (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Devuelve el valor del código ASCII del carácter más a la izquierda de una expresión de caracteres.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ASCII ( character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*character_expression*  
Una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de tipo **char** o **varchar**.
  
## <a name="return-types"></a>Tipos de valores devueltos
 **int**  
  
## <a name="remarks"></a>Observaciones
ASCII significa **A** merican **S** tandard **C** ode for **I** nformation **I** nterchange. Actúa como un estándar de codificación de caracteres para los equipos modernos. Para obtener una lista de caracteres ASCII, vea la sección **Caracteres imprimibles** de [ASCII](https://www.wikipedia.org/wiki/ASCII).

ASCII es un juego de caracteres de 7 bits. ASCII extendido o ASCII alto es un juego de caracteres de 8 bits no controlado por la función `ASCII`. 

## <a name="examples"></a>Ejemplos 

### <a name="a-this-example-assumes-an-ascii-character-set-and-returns-the-ascii-value-for-6-characters"></a>A. En este ejemplo se da por supuesto que es un juego de caracteres ASCII y se devuelve el valor `ASCII` para seis caracteres.
  
```sql
SELECT ASCII('A') AS A, ASCII('B') AS B,   
ASCII('a') AS a, ASCII('b') AS b,  
ASCII(1) AS [1], ASCII(2) AS [2];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
A           B           a           b           1           2  
----------- ----------- ----------- ----------- ----------- -----------  
65          66          97          98          49          50  
```  
  
### <a name="b-this-examples-shows-how-a-7-bit-ascii-value-is-returned-correctly-but-an-8-bit-extended-ascii-value-is-not-handled"></a>B. En este ejemplo se muestra cómo se devuelve correctamente un valor ASCII de 7 bits, pero no se controla un valor ASCII extendido de 8 bits.

```sql
SELECT ASCII('P') AS [ASCII], ASCII('æ') AS [Extended_ASCII];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
ASCII       Extended_ASCII
----------- --------------
80          195
```

Para comprobar si los resultados anteriores se asignan al punto de código de carácter correcto, use los valores de salida con la función `CHAR` o `NCHAR`:

```sql
SELECT NCHAR(80) AS [CHARACTER], NCHAR(195) AS [CHARACTER];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
CHARACTER CHARACTER
--------- ---------
P         Ã
```

En el resultado anterior, observe que el carácter del punto de código 195 es **Ã** y no **æ**. Esto se debe a que la función `ASCII` es capaz de leer la primera secuencia de 7 bits, pero no el bit adicional. El punto de código correcto para el carácter `æ` se puede encontrar mediante la función `UNICODE`, que es capaz de devolver el punto de código de carácter correcto:

```sql
SELECT UNICODE('æ') AS [Extended_ASCII], NCHAR(230) AS [CHARACTER];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
Extended_ASCII CHARACTER
-------------- ---------
230            æ
```

## <a name="see-also"></a>Consulte también
 [CHAR &#40;Transact-SQL&#41;](../../t-sql/functions/char-transact-sql.md)  
 [NCHAR &#40;Transact-SQL&#41;](../../t-sql/functions/nchar-transact-sql.md)  
 [UNICODE &#40;Transact-SQL&#41;](../../t-sql/functions/unicode-transact-sql.md)  
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)
  
