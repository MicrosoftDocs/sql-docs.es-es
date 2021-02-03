---
description: LEFT (Transact-SQL)
title: LEFT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LEFT
- LEFT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- character strings [SQL Server], LEFT
- characters [SQL Server], leftmost
- LEFT function
- leftmost character of expression
ms.assetid: 44a8c71b-63d8-458b-8b5d-99d570067c3c
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a30e01ddb227bafeac5913750945d7c610252e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188635"
---
# <a name="left-transact-sql"></a>LEFT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve la parte izquierda de una cadena de caracteres con el número de caracteres especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
LEFT ( character_expression , integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *character_expression*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de datos binarios o de caracteres. *character_expression* puede ser una constante, una variable o una columna. *character_expression* puede ser cualquier tipo de datos (excepto **text** o **ntext**) que se pueda convertir implícitamente a **varchar** o **nvarchar**. De lo contrario, use la función [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) para convertir *character_expression* explícitamente.  
 
> [!NOTE]  
> Si *string_expression* es de tipo **binary** o **varbinary**, LEFT realizará una conversión implícita a **varchar** y, por tanto, no conservará la entrada binaria.  
  
 *integer_expression*  
 Es un entero positivo que especifica cuántos caracteres de *character_expression* se van a devolver. Si *integer_expression* es negativo, se devuelve un error. Si *integer_expression* es de tipo **bigint** y contiene un valor grande, *character_expression* debe ser de un tipo de datos de gran tamaño, como **varchar(max)** .  
  
 El parámetro *integer_expression* cuenta un carácter suplente UTF 16 como un carácter.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve **varchar** cuando *character_expression* es de un tipo de datos de caracteres no Unicode.  
  
 Devuelve **nvarchar** cuando *character_expression* es de un tipo de datos de caracteres Unicode.  
  
## <a name="remarks"></a>Observaciones  
 Al usar intercalaciones de SC, el parámetro *integer_expression* cuenta un par suplente UTF 16 como un carácter. Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-left-with-a-column"></a>A. Utilizar LEFT con una columna  
 En el ejemplo siguiente se devuelven los cinco caracteres situados más a la izquierda de cada nombre de producto de la tabla `Product` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT LEFT(Name, 5)   
FROM Production.Product  
ORDER BY ProductID;  
GO  
```  
  
### <a name="b-using-left-with-a-character-string"></a>B. Utilizar LEFT con una cadena de caracteres  
 En el ejemplo siguiente se utiliza `LEFT` para devolver los dos caracteres situados más a la izquierda de la cadena de caracteres `abcdefg`.  
  
```sql  
SELECT LEFT('abcdefg',2);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--   
ab   
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-left-with-a-column"></a>C. Utilizar LEFT con una columna  
 En el ejemplo siguiente se devuelven los cinco caracteres situados más a la izquierda de cada nombre de producto.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LEFT(EnglishProductName, 5)   
FROM dbo.DimProduct  
ORDER BY ProductKey;  
```  
  
### <a name="d-using-left-with-a-character-string"></a>D. Utilizar LEFT con una cadena de caracteres  
 En el ejemplo siguiente se utiliza `LEFT` para devolver los dos caracteres situados más a la izquierda de la cadena de caracteres `abcdefg`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LEFT('abcdefg',2) FROM dbo.DimProduct;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--   
ab  
```  
  
## <a name="see-also"></a>Consulte también  
 [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)  
 [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)  
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

