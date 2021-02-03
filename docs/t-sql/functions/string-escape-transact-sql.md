---
description: STRING_ESCAPE (Transact-SQL)
title: STRING_ESCAPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STRING_ESCAPE
- STRING_ESCAPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRING_ESCAPE function
ms.assetid: 2163bc7a-3816-4304-9c40-8954804f5465
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: fb23ecedcaa224d66a6d23a113e696dd05caae05
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207932"
---
# <a name="string_escape-transact-sql"></a>STRING_ESCAPE (Transact-SQL)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Aplica caracteres de escape a caracteres especiales en textos y devuelve texto con caracteres de escape. **STRING_ESCAPE** es una función determinista, introducida en SQL Server 2016. 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
STRING_ESCAPE( text , type )  
```  

## <a name="arguments"></a>Argumentos

 *text*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md)**nvarchar** que representa el objeto que se debe escapar.  
  
 *type*  
 Reglas de escape que se aplicarán. Actualmente el valor admitido es `'json'`.  
  
## <a name="return-types"></a>Tipos de valor devuelto

 Texto **nvarchar(max)** con caracteres especiales y de control de escape. Actualmente **STRING_ESCAPE** solo puede aplicar caracteres de escape a los caracteres especiales de JSON que se muestran en las tablas siguientes.  
  
|Carácter especial|Secuencia codificada|  
|-----------------------|----------------------|  
|Comillas (")|\\"|  
|Barra oblicua invertida (\\)| \\\\ |  
|Barra oblicua (/)|\\/|  
|Retroceso|\b|  
|Avance de página|\f|  
|Nueva línea|\n|  
|Retorno de carro|\r|  
|Tabulación horizontal|\t|  
  
|Carácter de control|Secuencia codificada|  
|-----------------------|----------------------|  
|CHAR(0)|\u0000|  
|CHAR(1)|\u0001|  
|…|...|  
|CHAR(31)|\u001f|  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a--escape-text-according-to-the-json-formatting-rules"></a>A.  Aplicar carácter de escape a texto según las reglas de formato de JSON

 En esta consulta se aplican caracteres de escape a caracteres especiales mediante las reglas de JSON y se devuelve texto de escape.  
  
```sql
SELECT STRING_ESCAPE('\   /  
\\    "     ', 'json') AS escapedText;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
escapedText  
-------------------------------------------------------------  
\\\t\/\n\\\\\t\"\t
```  
  
### <a name="b-format-json-object"></a>B. Aplicar formato a objeto de JSON

 En esta consulta se crea texto de JSON a partir de variables de cadena y número, y se aplica un carácter de escape a cualquier carácter especial de JSON en variables.  
  
```
SET @json = FORMATMESSAGE('{ "id": %d,"name": "%s", "surname": "%s" }',
    17, STRING_ESCAPE(@name,'json'), STRING_ESCAPE(@surname,'json') );  
```  
  
## <a name="see-also"></a>Consulte también

- [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
- [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
- [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
- [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
- [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
- [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
- [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
- [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
- [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
- [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)
