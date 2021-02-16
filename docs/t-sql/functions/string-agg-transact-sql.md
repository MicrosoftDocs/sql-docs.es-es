---
description: STRING_AGG (Transact-SQL)
title: STRING_AGG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- STRING_AGG
- STRING_AGG_TSQL
helpviewer_keywords:
- STRING_AGG function
ms.assetid: 8860ef3f-142f-4cca-aa64-87a123e91206
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6c0eaef945ae4234987e6849f305d7b26cbb9374
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346603"
---
# <a name="string_agg-transact-sql"></a>STRING_AGG (Transact-SQL)

<!--[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]-->

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Concatena los valores de expresiones de cadena y coloca valores de separador entre ellos. El separador no se agrega al final de la cadena. 
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
STRING_AGG ( expression, separator ) [ <order_clause> ]

<order_clause> ::=   
    WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )   
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*expression*  
Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de cualquier tipo. Las expresiones se convierten en tipos `NVARCHAR` o `VARCHAR` durante la concatenación. Los tipos que no son de cadena se convierten en `NVARCHAR`.

*separator*  
Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de tipo `NVARCHAR` o `VARCHAR` que se usa como separador de cadenas concatenadas. Puede ser un literal o una variable. 

<order_clause>   
Si quiere, especifique el orden de los resultados concatenados con la cláusula `WITHIN GROUP`:

```syntaxsql
WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )
```   
<order_by_expression_list>   
 
  Lista de [expresiones](../../t-sql/language-elements/expressions-transact-sql.md) no constantes que se pueden usar para ordenar los resultados. Solo se permite un parámetro `order_by_expression` por consulta. El criterio de ordenación predeterminado es ascendente.   
  
## <a name="return-types"></a>Tipos de valor devuelto

El tipo de valor devuelto depende del primer argumento (expression). Si el argumento de entrada es de tipo string (`NVARCHAR`, `VARCHAR`), el tipo del resultado será igual que el tipo de la entrada. En la siguiente tabla se enumeran las conversiones automáticas:  

|Tipo de expresión de entrada |Resultado | 
|-------|-------|
|NVARCHAR(MAX) |NVARCHAR(MAX) |
|VARCHAR(MAX) |VARCHAR(MAX) |
|NVARCHAR(1...4000) |NVARCHAR(4000) |
|VARCHAR(1...8000) |VARCHAR(8000) |
|int, bigint, smallint, tinyint, numeric, float, real, bit, decimal, smallmoney, money, datetime, datetime2 |NVARCHAR(4000) |

## <a name="remarks"></a>Comentarios

`STRING_AGG` es una función de agregado que toma todas las expresiones de las filas y las concatena en una sola cadena. Los valores de la expresión se convierten implícitamente a tipos string y, después, se concatenan. La conversión implícita de cadenas sigue las reglas existentes para las conversiones de tipos de datos. Para más información sobre las conversiones de tipo de datos, vea [CAST y CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md). 

Si la expresión de entrada es de tipo `VARCHAR`, el separador no puede ser de tipo `NVARCHAR`. 

Los valores NULL se omiten y no se agrega el separador correspondiente. Para devolver un marcador de posición para valores NULL, use la función `ISNULL` como se muestra en el ejemplo B.

`STRING_AGG` está disponible en cualquier nivel de compatibilidad.

## <a name="examples"></a>Ejemplos

### <a name="a-generate-list-of-names-separated-in-new-lines"></a>A. Generar una lista de nombres separados en líneas nuevas

En el siguiente ejemplo se genera una lista de nombres en una única celda de resultados separados por retornos de carro.
```sql
USE AdventureWorks2016
GO
SELECT STRING_AGG (CONVERT(NVARCHAR(max),FirstName), CHAR(13)) AS csv 
FROM Person.Person;  
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|csv | 
|--- |
|Syed <br />Catherine <br />Kim <br />Kim <br />Kim <br />Hazem <br />... | 

Loa valores `NULL` detectados en las celdas `name` no se incluyen en el resultado.   

> [!NOTE]  
> Si usa el Editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], la opción **Resultados a cuadrícula** no puede implementar el retorno de carro. Cambie a **Resultados a texto** para ver el resultado configurado correctamente.       
> De forma predeterminada, los resultados a texto se truncan a 256 caracteres. Para aumentar este límite, cambie la opción **Número máximo de caracteres mostrados en cada columna**.

### <a name="b-generate-list-of-names-separated-with-comma-without-null-values"></a>B. Generar una lista de nombres separados por coma sin valores NULL

En el siguiente ejemplo los valores NULL se reemplazan por "N/A" y se devuelven los nombres separados por comas en una única celda de resultados.  
```sql
USE AdventureWorks2016
GO
SELECT STRING_AGG(CONVERT(NVARCHAR(max), ISNULL(FirstName,'N/A')), ',') AS csv 
FROM Person.Person; 
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> Los resultados se muestran recortados.

|csv | 
|--- |
|Syed,Catherine,Kim,Kim,Kim,Hazem,Sam,Humberto,Gustavo,Pilar,Pilar, ...|  

### <a name="c-generate-comma-separated-values"></a>C. Generar nombres separados por comas

```sql
USE AdventureWorks2016
GO
SELECT STRING_AGG(CONVERT(NVARCHAR(max), CONCAT(FirstName, ' ', LastName, '(', ModifiedDate, ')')), CHAR(13)) AS names 
FROM Person.Person; 
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> Los resultados se muestran recortados.

|nombres |
|--- |
|Ken Sánchez (8 de febrero de 2003 12:00 A.M.) <br />Terri Duffy (24 de febrero de 2002 12:00 A.M.) <br />Roberto Tamburello (5 de diciembre de 2001 12:00 A.M.) <br />Rob Walters (29 de diciembre de 2001 12:00 A.M.) <br />... |

> [!NOTE]  
> Si usa el Editor de consultas de Management Studio, la opción **Resultados a cuadrícula** no puede implementar el retorno de carro. Cambie a **Resultados a texto** para ver el resultado configurado correctamente.

### <a name="d-return-news-articles-with-related-tags"></a>D. Devolver artículos de noticias con etiquetas relacionadas

Los artículos y sus etiquetas están divididos en tablas diferentes. El desarrollador quiere que se devuelva una fila por cada artículo con todas las etiquetas asociadas. Si se usa la siguiente consulta:

```sql
SELECT a.articleId, title, STRING_AGG (tag, ',') as tags
FROM dbo.Article AS a
LEFT JOIN dbo.ArticleTag AS t
    ON a.ArticleId = t.ArticleId
GROUP BY a.articleId, title;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|articleId |title |etiquetas |
|--- |--- |--- |
|172 |Los sondeos vaticinan unos resultados electorales ajustados |política,sondeos,ayuntamiento |
|176 |Se espera que la nueva autopista reduzca la congestión |NULL |
|177 |Los perros siguen siendo más populares que los gatos |sondeos,animales|

> [!NOTE]
> La cláusula `GROUP BY` es necesaria si la función `STRING_AGG` no es el único elemento de la lista `SELECT`.

### <a name="e-generate-list-of-emails-per-towns"></a>E. Generar una lista de correos electrónicos por ciudades

Con la siguiente consulta se buscan las direcciones de correo electrónico de los empleados y se agrupan por ciudad:

```sql
USE AdventureWorks2016
GO

SELECT TOP 10 City, STRING_AGG(CONVERT(NVARCHAR(max), EmailAddress), ';') AS emails 
FROM Person.BusinessEntityAddress AS BEA  
INNER JOIN Person.Address AS A ON BEA.AddressID = A.AddressID
INNER JOIN Person.EmailAddress AS EA ON BEA.BusinessEntityID = EA.BusinessEntityID 
GROUP BY City;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> Los resultados se muestran recortados.

|City |emails |
|--- |--- |
|Ballard|paige28@adventure-works.com;joshua24@adventure-works.com;javier12@adventure-works.com;...|
|Baltimore|gilbert9@adventure-works.com|
|Barstow|kristen4@adventure-works.com|
|Basingstoke Hants|dale10@adventure-works.com;heidi9@adventure-works.com|
|Baytown|kelvin15@adventure-works.com|
|Beaverton|billy6@adventure-works.com;dalton35@adventure-works.com;lawrence1@adventure-works.com;...|
|Bell Gardens|christy8@adventure-works.com
|Bellevue|min0@adventure-works.com;gigi0@adventure-works.com;terry18@adventure-works.com;...|
|Bellflower|philip0@adventure-works.com;emma34@adventure-works.com;jorge8@adventure-works.com;...|
|Bellingham|christopher23@adventure-works.com;frederick7@adventure-works.com;omar0@adventure-works.com;...|

Los correos electrónicos devueltos en la columna emails se pueden usar para enviar mensajes de correo electrónico al grupo de personas que trabajan en determinadas ciudades. 

### <a name="f-generate-a-sorted-list-of-emails-per-towns"></a>F. Generar una lista ordenada de correos electrónicos por ciudades   
Al igual que el ejemplo anterior, con la siguiente consulta se buscan las direcciones de correo electrónico de los empleados, se agrupan por ciudad y se ordenan alfabéticamente:   

```sql
USE AdventureWorks2016
GO

SELECT TOP 10 City, STRING_AGG(CONVERT(NVARCHAR(max), EmailAddress), ';') WITHIN GROUP (ORDER BY EmailAddress ASC) AS emails 
FROM Person.BusinessEntityAddress AS BEA  
INNER JOIN Person.Address AS A ON BEA.AddressID = A.AddressID
INNER JOIN Person.EmailAddress AS EA ON BEA.BusinessEntityID = EA.BusinessEntityID 
GROUP BY City;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

> [!NOTE]
> Los resultados se muestran recortados.

|City |emails |
|--- |--- |
|Barstow|kristen4@adventure-works.com
|Basingstoke Hants|dale10@adventure-works.com;heidi9@adventure-works.com
|Braintree|mindy20@adventure-works.com
|Bell Gardens|christy8@adventure-works.com
|Byron|louis37@adventure-works.com
|Bordeaux|ranjit0@adventure-works.com
|Carnation|don0@adventure-works.com;douglas0@adventure-works.com;george0@adventure-works.com;...|
|Boulogne-Billancourt|allen12@adventure-works.com;bethany15@adventure-works.com;carl5@adventure-works.com;...|
|Berkshire|barbara41@adventure-works.com;brenda4@adventure-works.com;carrie14@adventure-works.com;...|
|Berks|adriana6@adventure-works.com;alisha13@adventure-works.com;arthur19@adventure-works.com;...|

## <a name="see-also"></a>Consulte también
 
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Funciones de agregado &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  

