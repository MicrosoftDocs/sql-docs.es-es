---
description: Precisión, escala y longitud (Transact-SQL)
title: Precisión, escala y longitud (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], length
- data types [SQL Server], scale
- precision [SQL Server], data types
- lengths [SQL Server], data types
- number of digits
- scale [SQL Server]
- scale [SQL Server], data types
- data types [SQL Server], precision
ms.assetid: fbc9ad2c-0d3b-4e98-8fdd-4d912328e40a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: da816b6e55bc1578a820a53d6e1036b685086713
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338047"
---
# <a name="precision-scale-and-length-transact-sql"></a>Precisión, escala y longitud (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La precisión es el número de dígitos de un número. La escala es el número de dígitos situados a la derecha de la coma decimal de un número. Por ejemplo, el número 123,45 tiene una precisión de 5 y una escala de 2.
  
En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la precisión máxima predeterminada de los tipos de datos **numéricos** y **decimales** es 38. En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el valor máximo predeterminado es 28.
  
La longitud de un tipo de datos numérico es el número de bytes utilizados para almacenar el número. Para varchar y char, la longitud de una cadena de caracteres es el número de bytes. Para nvarchar y nchar, la longitud de una cadena de caracteres es el número de pares de bytes. La longitud de los tipos de datos **binary**, **varbinary** e **image** es el número de bytes. Por ejemplo, un tipo de datos **int** que puede contener 10 dígitos se almacena en 4 bytes y no acepta puntos decimales. El tipo de datos **int** tiene una precisión de 10, una longitud de 4 y una escala de 0.
  
Cuando se concatenan dos expresiones **char**, **varchar**, **binary** o **varbinary**, la longitud de la expresión resultante es la suma de las longitudes de las dos expresiones de origen, hasta 8000 bytes.
  
Cuando se concatenan dos expresiones **nchar** o **nvarchar**, la longitud de la expresión resultante es la suma de las longitudes de las dos expresiones de origen, hasta 4000 pares de bytes.
  
Cuando se comparan dos expresiones del mismo tipo de datos pero de distintas longitudes mediante UNION, EXCEPT o INTERSECT, la longitud resultante es la longitud mayor de las dos expresiones.
  
La precisión y la escala de los tipos de datos numéricos, además de los **decimales**, son fijas. Si un operador aritmético tiene dos expresiones del mismo tipo, el resultado tiene el mismo tipo de datos con la precisión y la escala definidas para ese tipo. Si un operador tiene dos expresiones con tipos de datos numéricos diferentes, las reglas de prioridad de tipos de datos definen el tipo de datos del resultado. El resultado tiene la precisión y la escala definidas para el tipo de datos que le corresponde.
  
En la tabla siguiente se define la forma de calcular la precisión y la escala del resultado cuando el resultado de una operación es de tipo **decimal**. El resultado es **decimal** cuando:
-   Ambas expresiones son de tipo **decimal**.  
-   Una expresión es **decimal** y la otra es de un tipo de datos con una prioridad menor que **decimal**.  
  
Las expresiones de operando se denotan como expresión e1, con precisión p1 y escala s1, y expresión e2, con precisión p2 y escala s2. La precisión y la escala de cualquier expresión que no sea **decimal** son la precisión y la escala definidas para el tipo de datos de la expresión. La función max(a,b) indica que se tome el valor mayor entre "a" y "b". De forma similar, min(a,b) indica que se tome el valor menor entre "a" y "b".
  
|Operación|Precisión del resultado|Escala del resultado *|  
|---|---|---|
|e1 + e2|máx(s1, s2) + máx(p1-s1, p2-s2) + 1|máx(s1, s2)|  
|e1 - e2|máx(s1, s2) + máx(p1-s1, p2-s2) + 1|máx(s1, s2)|  
|e1 * e2|p1 + p2 + 1|s1 + s2|  
|e1 / e2|p1 - s1 + s2 + máx(6, s1 + p2 + 1)|máx(6, s1 + p2 + 1)|  
|e1 { UNION &#124; EXCEPT &#124; INTERSECT } e2|máx(s1, s2) + máx(p1-s1, p2-s2)|máx(s1, s2)|  
|e1 % e2|min(p1-s1, p2 -s2) + max( s1,s2 )|máx(s1, s2)|  
  
\* La precisión del resultado y la escala tienen un máximo absoluto igual a 38. Cuando la precisión de un resultado es mayor que 38, se reduce a 38, y la escala correspondiente se reduce para intentar evitar que la parte entera del resultado se trunque. En algunos casos, como en la multiplicación o la división, el factor de escala no se reduce para conservar la precisión decimal, aunque se pueda generar un error por desbordamiento.

Además de las operaciones de suma y resta, necesitamos que los sitios `max(p1 - s1, p2 - s2)` almacenen las partes enteras de los números decimales. Si no hay suficiente espacio para almacenarlas, es decir, `max(p1 - s1, p2 - s2) < min(38, precision) - scale`, la escala se reduce para proporcionar suficiente espacio para integrar la parte entera. La escala resultante es `MIN(precision, 38) - max(p1 - s1, p2 - s2)`, con lo que la parte fraccionaria se puede redondear para que entre en la escala resultante.

En las operaciones de multiplicación y división necesitamos que los sitios `precision - scale` almacenen la parte entera del resultado. La escala se puede reducir utilizando las reglas siguientes:
1.  La escala resultante se reduce a `min(scale, 38 - (precision-scale))` si la parte entera es menor que 32, porque no puede ser mayor que `38 - (precision-scale)`. En este caso, el resultado se puede redondear.
1. La escala no cambia si es inferior a 6 y si la parte entera es mayor que 32. En este caso, se puede producir un error de desbordamiento porque no cabe en el decimal (38, escala). 
1. La escala se establece en 6 si es mayor que 6 y si la parte entera es mayor que 32. En este caso, se reducen tanto la parte entera como la escala y el tipo resultante es decimal (38,6). El resultado se tiene que redondear hasta 6 puntos decimales o se producirá un error si la parte entera no cabe en 32 dígitos.

## <a name="examples"></a>Ejemplos
La siguiente expresión devuelve resultados `0.00000090000000000` sin redondear, porque el resultado puede caber en `decimal(38,17)`:
```sql
select cast(0.0000009000 as decimal(30,20)) * cast(1.0000000000 as decimal(30,20)) [decimal 38,17]
```
En este caso, la precisión es 61 y la escala es 40.
La parte entera (precision-scale = 21) es menor que 32, por lo que este es el caso (1) de las reglas de multiplicación y la escala se calcula como `min(scale, 38 - (precision-scale)) = min(40, 38 - (61-40)) = 17`. El tipo de resultado es `decimal(38,17)`.

La siguiente expresión devuelve resultados `0.000001` que caben en `decimal(38,6)`:
```sql
SELECT CAST(0.0000009000 AS DECIMAL(30,10)) * CAST(1.0000000000 AS DECIMAL(30,10)) [decimal(38, 6)]
```
En este caso, la precisión es 61 y la escala es 20.
La escala es mayor que 6 y la parte entera (`precision-scale = 41`) es mayor que 32. Este es el caso (3) de las reglas de multiplicación y el tipo de resultado es `decimal(38,6)`.

## <a name="see-also"></a>Consulte también
[Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
