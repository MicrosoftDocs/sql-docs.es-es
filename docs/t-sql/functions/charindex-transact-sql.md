---
title: CHARINDEX (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para la función CHARINDEX.
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHARINDEX
- CHARINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], pattern searching
- CHARINDEX function
- pattern searching [SQL Server]
- starting point of expression in character string
ms.assetid: 78c10341-8373-4b30-b404-3db20e1a3ac4
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 43c10d7f6f65d0171204869be1a5897ac00d22bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193444"
---
# <a name="charindex-transact-sql"></a>CHARINDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función busca una expresión de caracteres dentro de una segunda expresión de caracteres, y devuelve la posición inicial de la primera expresión si se encuentra.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CHARINDEX ( expressionToFind , expressionToSearch [ , start_location ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*expressionToFind*  
Una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de caracteres que contiene la secuencia que se va a buscar. *expressionToFind* tiene un límite de 8000 caracteres.
  
*expressionToSearch*  
Una expresión de caracteres que se va a buscar.
  
*start_location*  
Una expresión **integer** o **bigint** donde empieza la búsqueda. Si no se especifica *start_location*, tiene un valor negativo o un valor cero (0), la búsqueda empieza al principio de *expressionToSearch*.
  
## <a name="return-types"></a>Tipos de valores devueltos
**bigint** si *expressionToSearch* tiene el tipo de datos **nvarchar(max)** , **varbinary(max)** o **varchar(max)** ; **int** en caso contrario.
  
## <a name="remarks"></a>Observaciones  
Si *expressionToFind* o *expressionToSearch* tiene un tipo de datos Unicode (**nchar** o **nvarchar**) y la otra expresión no, la función CHARINDEX convierte esa otra expresión a un tipo de datos Unicode. CHARINDEX no se puede usar con los tipos de datos **image**, **ntext** o **text**.
  
Si *expressionToFind* o *expressionToSearch* tiene un valor NULL, CHARINDEX devuelve NULL.
  
Si CHARINDEX no encuentra *expressionToFind* dentro de *expressionToSearch*, devuelve 0.
  
CHARINDEX realiza comparaciones en función de intercalación de entrada. Para realizar una comparación de una intercalación especificada, use COLLATE para aplicar una intercalación explícita a la entrada.
  
La posición inicial devuelta es de base 1, no de base 0.
  
0x0000 (**char(0)** ) es un carácter no definido en las intercalaciones de Windows y no se puede incluir en CHARINDEX.
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caracteres adicionales (pares suplentes)  
Al usar intercalaciones de SC, *start_location* y el valor devuelto cuentan los pares suplentes como un carácter, no como dos. Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-the-starting-position-of-an-expression"></a>A. Devolver la posición inicial de una expresión  
Este ejemplo se busca `bicycle` en la variable de valor de cadena de búsqueda `@document`.
  
```sql
DECLARE @document VARCHAR(64);  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('bicycle', @document);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------   
48            
```  
  
### <a name="b-searching-from-a-specific-position"></a>B. Buscar desde una posición concreta  
En este ejemplo se usa el parámetro opcional *ubicación_inicial* para empezar la búsqueda de `vital` en el quinto carácter de la variable de valor de cadena de búsqueda `@document`.
  
```sql
DECLARE @document VARCHAR(64);  
  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('vital', @document, 5);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------   
16            
  
(1 row(s) affected)  
```  
  
### <a name="c-searching-for-a-nonexistent-expression"></a>C. Buscar una expresión inexistente  
En este ejemplo se muestra el conjunto de resultados cuando CHARINDEX no encuentra *expressionToFind* dentro de *expressionToSearch*.
  
```sql
DECLARE @document VARCHAR(64);  
  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('bike', @document);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
0
  
(1 row(s) affected)
```
  
### <a name="d-performing-a-case-sensitive-search"></a>D. Realizar una búsqueda con distinción de mayúsculas y minúsculas  
En este ejemplo se muestra una búsqueda con distinción de mayúsculas y minúsculas de la cadena `'TEST'` en la cadena de búsqueda `'This is a Test``'`.
  
```sql
USE tempdb;  
GO  
--perform a case sensitive search  
SELECT CHARINDEX ( 'TEST',  
       'This is a Test'  
       COLLATE Latin1_General_CS_AS);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
0
```  
  
En este ejemplo se muestra una búsqueda con distinción de mayúsculas y minúsculas de la cadena `'Test'` en `'This is a Test'`.
  
```sql
  
USE tempdb;  
GO  
SELECT CHARINDEX ( 'Test',  
       'This is a Test'  
       COLLATE Latin1_General_CS_AS);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
11
```  
  
### <a name="e-performing-a-case-insensitive-search"></a>E. Realizar una búsqueda sin distinción de mayúsculas y minúsculas  
En este ejemplo se muestra una búsqueda sin distinción de mayúsculas y minúsculas de la cadena `'TEST'` en `'This is a Test'`.
  
```sql
USE tempdb;  
GO  
SELECT CHARINDEX ( 'TEST',  
       'This is a Test'  
       COLLATE Latin1_General_CI_AS);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
11
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-searching-from-the-start-of-a-string-expression"></a>F. Buscar desde el principio de una expresión de cadena  
En este ejemplo se devuelve la primera ubicación de la cadena `is` en la cadena `This is a string`, empezando por la posición 1 (el primer carácter) de `This is a string`.
  
```sql
SELECT CHARINDEX('is', 'This is a string');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
3
```  
  
### <a name="g-searching-from-a-position-other-than-the-first-position"></a>G. Buscar desde una posición distinta de la primera posición  
En este ejemplo se devuelve la primera ubicación de la cadena `is` en la cadena `This is a string`, empezando por la posición 4 (el cuarto carácter).
  
```sql
SELECT CHARINDEX('is', 'This is a string', 4);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
 6
 ```  
  
### <a name="h-results-when-the-string-is-not-found"></a>H. Resultados cuando no se encuentra la cadena  
En este ejemplo se muestra el valor devuelto cuando CHARINDEX no encuentra la cadena *patrón_de_cadena* en la cadena buscada.
  
```sql
SELECT TOP(1) CHARINDEX('at', 'This is a string') FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
0
```  
  
## <a name="see-also"></a>Consulte también
 [LEN &#40;Transact-SQL&#41;](../../t-sql/functions/len-transact-sql.md)  
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)  
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
 [+ &#40;Concatenación de cadenas&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
 [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  


