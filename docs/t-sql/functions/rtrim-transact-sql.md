---
description: RTRIM (Transact-SQL)
title: RTRIM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RTRIM_TSQL
- RTRIM
dev_langs:
- TSQL
helpviewer_keywords:
- RTRIM function
- character strings [SQL Server], trailing blanks
- blank characters [SQL Server]
- trailing blanks
ms.assetid: 52fd6e8d-650c-4f66-abcf-67765aa5aa83
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b7ca4eaf86750e6d7d074329ee0f46b05bec1925
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183500"
---
# <a name="rtrim-transact-sql"></a>RTRIM (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una cadena de caracteres después de truncar todos los espacios finales.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
RTRIM ( character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *character_expression*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de datos de caracteres. *character_expression* puede ser una constante, una variable o una columna de datos binarios o de caracteres.  
  
 *character_expression* debe ser de un tipo de datos que se pueda convertir implícitamente a **varchar**. De lo contrario, use [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) para convertir *character_expression* explícitamente.  
  
## <a name="return-types"></a>Tipos de valor devueltos  
 **varchar** o **nvarchar**  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-simple-example"></a>A. Ejemplo sencillo  
 El siguiente ejemplo toma una cadena de caracteres que tiene espacios al final de la frase y devuelve el texto sin esos espacios.  
  
```sql  
SELECT RTRIM('Removes trailing spaces.   ');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
  `Removes trailing spaces.`  
  
### <a name="b-simple-example"></a>B. Ejemplo sencillo  
 En el ejemplo siguiente se muestra cómo utilizar `RTRIM` para quitar los espacios finales. En este caso hay otra cadena concatenada a la primera cadena para mostrar que los espacios han desaparecido.  
  
```sql  
SELECT RTRIM('Four spaces are after the period in this sentence.    ') + 'Next string.';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
`Four spaces are after the period in this sentence.Next string.`  

### <a name="c-using-rtrim-with-a-variable"></a>C. Usar RTRIM con una variable  
 En el ejemplo siguiente se muestra cómo utilizar `RTRIM` para quitar los espacios finales de una variable de caracteres.  
  
```sql  
DECLARE @string_to_trim VARCHAR(60);  
SET @string_to_trim = 'Four spaces are after the period in this sentence.    ';  
SELECT @string_to_trim + ' Next string.';  
SELECT RTRIM(@string_to_trim) + ' Next string.';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Four spaces are after the period in this sentence.     Next string.

Four spaces are after the period in this sentence. Next string.
```

  
## <a name="see-also"></a>Consulte también  
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)  
 [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)  
 [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)  
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  


