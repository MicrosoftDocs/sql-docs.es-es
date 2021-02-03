---
description: STUFF (Transact-SQL)
title: STUFF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STUFF
- STUFF_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting characters
- STUFF function
- length characters
- removing characters
- replacing characters
- characters [SQL Server], replacing
- inserting data
ms.assetid: abb0afa9-44f6-42a2-a871-5f471dfb222b
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a91badcdfc378bc072c95952e40904778b879ddd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207934"
---
# <a name="stuff-transact-sql"></a>STUFF (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  La función STUFF inserta una cadena en otra. Elimina una longitud determinada de caracteres de la primera cadena a partir de la posición de inicio y, a continuación, inserta la segunda cadena en la primera, en la posición de inicio.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
STUFF ( character_expression , start , length , replaceWith_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *character_expression*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de datos de caracteres. *character_expression* puede ser una constante, una variable o una columna de datos binarios o de caracteres.  
  
 *start*  
 Es un valor entero que especifica la ubicación donde comienzan la eliminación y la inserción. Si *start* es negativo o cero, se devuelve una cadena de tipo NULL. Si *start* es mayor que el primer valor de *character_expression*, se devuelve una cadena de tipo NULL. *start* puede ser de tipo **bigint**.  
  
 *length*  
 Es un entero que especifica el número de caracteres que se elimina. Si *length* es negativo, se devuelve una cadena de tipo NULL. Si *length* es mayor que el primer parámetro *character_expression*, se produce una eliminación hasta el último carácter del último *character_expression*.  Si *length* es cero, la inserción se produce en la ubicación *start* y no se elimina ningún carácter. *length* puede ser de tipo **bigint**.

 *replaceWith_expression*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de datos de caracteres. *character_expression* puede ser una constante, una variable o una columna de datos binarios o de caracteres. Esta expresión reemplaza los caracteres *legth* de *character_expression*, empezando por *start*. Si se proporciona `NULL` como *replaceWith_expression*, los caracteres se quitan sin insertar nada.   
  
## <a name="return-types"></a>Tipos de valor devueltos  
 Devuelve datos de caracteres si *character_expression* es de alguno de los tipos de datos de caracteres admitidos. Devuelve datos binarios si *character_expression* es de alguno de los tipos de datos binarios admitidos.  
  
## <a name="remarks"></a>Observaciones  
 Si la posición de inicio o la longitud es negativa, o si la posición de inicio es mayor que la longitud de la primera cadena, se devuelve una cadena NULL. Si la posición inicial es 0, se devuelve un valor NULL. Si la longitud que se va a eliminar es mayor que la primera cadena, se elimina hasta el primer carácter de la primera cadena.  

Si el valor resultante es mayor que el máximo admitido por el tipo devuelto, se genera un error.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caracteres adicionales (pares suplentes)  
 Cuando se usan intercalaciones de caracteres adicionales, *character_expression* y *replaceWith_expression* pueden incluir pares suplentes. El parámetro de longitud cuenta cada suplente en *character_expression* como un único carácter.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve una cadena de caracteres creada eliminando tres caracteres de la primera cadena, `abcdef`, a partir de la posición `2` de `b`, e insertando la segunda cadena en el punto de eliminación.  
  
```sql  
SELECT STUFF('abcdef', 2, 3, 'ijklmn');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------   
aijklmnef   
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también  
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
