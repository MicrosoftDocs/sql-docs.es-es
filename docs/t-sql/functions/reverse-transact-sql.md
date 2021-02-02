---
description: REVERSE (Transact-SQL)
title: REVERSE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REVERSE_TSQL
- REVERSE
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], reverse
- REVERSE function
- reverse character expressions
ms.assetid: 555d8877-7cc7-4955-ae2c-6215aca313b7
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a79c58527173dba36c43e41f0f7d944f43286b81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99101440"
---
# <a name="reverse-transact-sql"></a>REVERSE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve el orden inverso de un valor de cadena.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
REVERSE ( string_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *string_expression*  
 *string_expression* es una [expression](../../t-sql/language-elements/expressions-transact-sql.md) de tipo de datos binario o de cadena. *string_expression* puede ser una constante, una variable o una columna de datos binarios o de caracteres.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **varchar** o **nvarchar**  
  
## <a name="remarks"></a>Observaciones  
 *string_expression* debe ser de un tipo de datos que se pueda convertir implícitamente a **varchar**. De lo contrario, use [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) para convertir *string_expression* explícitamente.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caracteres adicionales (pares suplentes)  
 Al utilizar las intercalaciones de SC, la función REVERSE no invertirá el orden de dos mitades de un par suplente.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelven los nombres de todos los contactos con los caracteres invertidos. En este ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT FirstName, REVERSE(FirstName) AS Reverse  
FROM Person.Person  
WHERE BusinessEntityID < 5  
ORDER BY FirstName;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
FirstName      Reverse
-------------- --------------
Ken            neK
Rob            boR
Roberto        otreboR
Terri          irreT

(4 row(s) affected)
```  
  
 En el ejemplo siguiente se invierten los caracteres de una variable.  
  
```sql
DECLARE @myvar VARCHAR(10);  
SET @myvar = 'sdrawkcaB';  
SELECT REVERSE(@myvar) AS Reversed ;  
GO  
```  
  
 En el ejemplo siguiente se realiza una conversión implícita de un tipo de datos **int** a un tipo de datos **varchar** y luego se invierte el resultado.  
  
```sql
SELECT REVERSE(1234) AS Reversed ;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En este ejemplo se devuelven los nombres de todas las bases de datos y los nombres con los caracteres invertidos.  
  
```sql
SELECT name, REVERSE(name) FROM sys.databases;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

