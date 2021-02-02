---
description: RIGHT (Transact-SQL)
title: RIGHT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RIGHT_TSQL
- RIGHT
dev_langs:
- TSQL
helpviewer_keywords:
- rightmost character of expression
- RIGHT function
- character strings [SQL Server], RIGHT
ms.assetid: 43f1fe1f-aa18-47e3-ba20-e03e32254a6d
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da0e2e1224cce5dfe13da57eb0efb7959ae8c4ec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99101814"
---
# <a name="right-transact-sql"></a>RIGHT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve la parte derecha de una cadena de caracteres con el número de caracteres especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
RIGHT ( character_expression , integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *character_expression*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de datos binarios o de caracteres. *character_expression* puede ser una constante, una variable o una columna. *character_expression* puede ser cualquier tipo de datos (excepto **text** o **ntext**) que se pueda convertir implícitamente a **varchar** o **nvarchar**. De lo contrario, use la función [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) para convertir *character_expression* explícitamente.  
   
> [!NOTE]  
> Si *string_expression* es de tipo **binary** o **varbinary**, RIGHT realizará una conversión implícita a **varchar** y, por tanto, no conservará la entrada binaria.  
  
 *integer_expression*  
 Es un entero positivo que especifica cuántos caracteres de *character_expression* se van a devolver. Si *integer_expression* es negativo, se devuelve un error. Si *integer_expression* es de tipo **bigint** y contiene un valor grande, *character_expression* debe ser de un tipo de datos de gran tamaño, como **varchar(max)** .  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve **varchar** cuando *character_expression* es de un tipo de datos de caracteres no Unicode.  
  
 Devuelve **nvarchar** cuando *character_expression* es de un tipo de datos de caracteres Unicode.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caracteres adicionales (pares suplentes)  
 Al utilizar las intercalaciones de SC, la función RIGHT cuenta un par suplente UTF 16 como un carácter individual. Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-right-with-a-column"></a>A. Usar RIGHT con una columna  
 En el ejemplo siguiente se devuelven los cinco caracteres situados más a la derecha del nombre de cada persona de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT RIGHT(FirstName, 5) AS 'First Name'  
FROM Person.Person  
WHERE BusinessEntityID < 5  
ORDER BY FirstName;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
First Name  
----------  
Ken  
Terri  
berto  
Rob  
  
(4 row(s) affected)  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-using-right-with-a-column"></a>B. Usar RIGHT con una columna  
 En el siguiente ejemplo se devuelven los cinco caracteres situados más a la derecha de cada apellido de la tabla `DimEmployee`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT RIGHT(LastName, 5) AS Name  
FROM dbo.DimEmployee  
ORDER BY EmployeeKey;  
```  
  
 A continuación se muestra un conjunto parcial de resultados.  
  
 ```
Name
-----
lbert
Brown
rello
lters
 ```  
  
### <a name="c-using-right-with-a-character-string"></a>C. Usar RIGHT con una cadena de caracteres  
 En el siguiente ejemplo se usa `RIGHT` para devolver los dos caracteres situados más a la derecha de la cadena de caracteres `abcdefg`.  
  
```sql  
SELECT RIGHT('abcdefg', 2); 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
-------  
fg
```  
  
## <a name="see-also"></a>Consulte también  
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)  
 [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)  
 [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)  
 [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)  
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

