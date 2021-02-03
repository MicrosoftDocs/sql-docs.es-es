---
title: SUBSTRING (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para la función SUBSTRING. Esta función devuelve parte de una expresión especificada de caracteres, binaria, de texto o de imagen.
ms.date: 10/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUBSTRING
- SUBSTRING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- portion of expression returned [SQL Server]
- part of expression returned [SQL Server]
- SUBSTRING function
- offsets [SQL Server]
- binary [SQL Server], returning part of
- expressions [SQL Server], part returned
- characters [SQL Server], returning part of
ms.assetid: a19c808f-aaf9-4a69-af59-b1a5fc3e5c4c
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2a220d976b1e85dc8559b34d74e8280847afccce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207958"
---
# <a name="substring-transact-sql"></a>SUBSTRING (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Devuelve parte de una expresión de caracteres, binaria, de texto o de imagen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SUBSTRING ( expression ,start , length )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de tipo **character**, **binary**, **text**, **ntext** o **image**.  
  
 *start*  
 Es un entero o una expresión **bigint** que especifica dónde empiezan los caracteres devueltos. (La numeración se basa en 1, lo que significa que el primer carácter de la expresión es 1). Si *start* es menor que 1, la expresión devuelta empezará en el primer carácter especificado en *expression*. En este caso, el número de caracteres que se devuelve es el valor de la suma de *start* + *length*- 1 o 0. Si *start* es mayor que el número de caracteres de la expresión de valor, se devuelve una expresión de longitud cero.  
  
 *length*  
 Es un entero positivo o una expresión **bigint** que especifica cuántos caracteres de *expression* se van a devolver. Si *length* es negativo, se genera un error y finaliza la instrucción. Si la suma de *start* y *length* es mayor que el número de caracteres de *expression*, se devuelve la expresión de valor completa que empieza en *start*.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve datos de caracteres si *expression* es de alguno de los tipos de datos de caracteres admitidos. Devuelve datos binarios si *expression* es de alguno de los tipos de datos **binary** admitidos. La cadena devuelta es del mismo tipo que la expresión indicada, con las excepciones mostradas en la tabla:  
  
|Expresión especificada|Tipo de valor devuelto|  
|--------------------------|-----------------|  
|**char**/**varchar**/**text**|**varchar**|  
|**nchar**/**nvarchar**/**ntext**|**nvarchar**|  
|**binary**/**varbinary**/**image**|**varbinary**|  
  
## <a name="remarks"></a>Observaciones  
 Los valores de *start* y *length* deben especificarse en número de caracteres para los tipos de datos **ntext**, **char** o **varchar** y bytes para los tipos de datos **text**, **image**, **binary** o **varbinary**.  
  
 *expresión* debe ser de tipo **varchar(max)** o **varbinary(max)** cuando *start* o *length* contienen un valor mayor que 2 147 483 647.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caracteres adicionales (pares suplentes)  
 Al usar intercalaciones de caracteres suplementarios (SC), *start* y *length* cuentan cada par suplente de *expression* como un único carácter. Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-substring-with-a-character-string"></a>A. Usar SUBSTRING con una cadena de caracteres  
 En el siguiente ejemplo se muestra cómo devolver únicamente una parte de una cadena de caracteres. Desde la tabla `sys.databases`, esta consulta devuelve los nombres de base de datos del sistema en la primera columna, la primera letra de la base de datos en la segunda columna y el tercer y cuarto carácter en la última columna.  
  
```sql
SELECT name, SUBSTRING(name, 1, 1) AS Initial ,
SUBSTRING(name, 3, 2) AS ThirdAndFourthCharacters
FROM sys.databases  
WHERE database_id < 5;   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

|name |Initial |ThirdAndFourthCharacters|
|---|--|--|
|maestro    |m    |st |
|tempdb    |t    |mp |
|model    |m    |de |
|msdb    |m    |db |


  
 He aquí cómo mostrar el segundo, tercer y cuarto caracteres de la constante de cadena `abcdef`.  
  
```sql
SELECT x = SUBSTRING('abcdef', 2, 3);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
x  
----------  
bcd  
  
(1 row(s) affected)
```  
  
### <a name="b-using-substring-with-text-ntext-and-image-data"></a>B. Usar SUBSTRING con datos de tipo text, ntext e image  
  
> [!NOTE]  
>  Para ejecutar estos ejemplos, es necesario instalar la base de datos **pubs**.  
  
 En este ejemplo se muestra cómo se devuelven los diez primeros caracteres de cada columna de datos de tipo **text** e **image** de la tabla `pub_info` de la base de datos `pubs`. Los datos de **text** se devuelven como **varchar**, mientras que los datos de **image** se devuelven como **varbinary**.  
  
```sql
USE pubs;  
SELECT pub_id, SUBSTRING(logo, 1, 10) AS logo,   
   SUBSTRING(pr_info, 1, 10) AS pr_info  
FROM pub_info  
WHERE pub_id = '1756';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 pub_id logo    pr_info
------ ---------------------- ----------
1756   0x474946383961E3002500 This is sa

(1 row(s) affected)
```  
  
 En este ejemplo se muestra el efecto de SUBSTRING en los datos de tipo **text** y **ntext**. En primer lugar, este ejemplo crea una nueva tabla en la base de datos `pubs` denominada `npub_info`. En segundo lugar, crea la columna `pr_info` en la tabla `npub_info` a partir de los primeros 80 caracteres de la columna `pub_info.pr_info` y agrega una `ü` como primer carácter. Por último, `INNER JOIN` recupera todos los números de identificación del publicador y una función `SUBSTRING` de las columnas de información del publicador, de tipo **text** y **ntext**.  
  
```sql
IF EXISTS (SELECT table_name FROM INFORMATION_SCHEMA.TABLES   
      WHERE table_name = 'npub_info')  
   DROP TABLE npub_info;  
GO  
-- Create npub_info table in pubs database. Borrowed from instpubs.sql.  
USE pubs;  
GO  
CREATE TABLE npub_info  
(  
 pub_id CHAR(4) NOT NULL  
    REFERENCES publishers(pub_id)  
    CONSTRAINT UPKCL_npubinfo PRIMARY KEY CLUSTERED,  
pr_info ntext NULL  
);  
  
GO  
  
-- Fill the pr_info column in npub_info with international data.  
RAISERROR('Now at the inserts to pub_info...',0,1);  
  
GO  
  
INSERT npub_info VALUES('0736', N'üThis is sample text data for New Moon Books, publisher 0736 in the pubs database')  
,('0877', N'üThis is sample text data for Binnet & Hardley, publisher 0877 in the pubs databa')  
,('1389', N'üThis is sample text data for Algodata Infosystems, publisher 1389 in the pubs da')  
,('9952', N'üThis is sample text data for Scootney Books, publisher 9952 in the pubs database')  
,('1622', N'üThis is sample text data for Five Lakes Publishing, publisher 1622 in the pubs d')  
,('1756', N'üThis is sample text data for Ramona Publishers, publisher 1756 in the pubs datab')  
,('9901', N'üThis is sample text data for GGG&G, publisher 9901 in the pubs database. GGG&G i')  
,('9999', N'üThis is sample text data for Lucerne Publishing, publisher 9999 in the pubs data');  
GO  
-- Join between npub_info and pub_info on pub_id.  
SELECT pr.pub_id, SUBSTRING(pr.pr_info, 1, 35) AS pr_info,  
   SUBSTRING(npr.pr_info, 1, 35) AS npr_info  
FROM pub_info pr INNER JOIN npub_info npr  
   ON pr.pub_id = npr.pub_id  
ORDER BY pr.pub_id ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-substring-with-a-character-string"></a>C. Usar SUBSTRING con una cadena de caracteres  
 En el siguiente ejemplo se muestra cómo devolver únicamente una parte de una cadena de caracteres. En la tabla `dbo.DimEmployee`, esta consulta devuelve el apellido en una columna y solo la primera inicial en la segunda columna.  
  
```sql
-- Uses AdventureWorks  
  
SELECT LastName, SUBSTRING(FirstName, 1, 1) AS Initial  
FROM dbo.DimEmployee  
WHERE LastName LIKE 'Bar%'  
ORDER BY LastName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName             Initial
-------------------- -------
Barbariol            A
Barber               D
Barreto de Mattos    P
```  
  
 En este ejemplo se muestra el segundo, tercer y cuarto carácter de la constante de cadena `abcdef`.  
  
```sql
USE ssawPDW;  
  
SELECT TOP 1 SUBSTRING('abcdef', 2, 3) AS x FROM dbo.DimCustomer;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
x
-----
bcd
```  
  
## <a name="see-also"></a>Consulte también  
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)  
 [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)  
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  


