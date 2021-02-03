---
description: + (Concatenación de cadenas) (Transact-SQL)
title: + (Concatenación de cadenas) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- concatenation
- +
- string
dev_langs:
- TSQL
helpviewer_keywords:
- concatenation [SQL Server]
- string concatenation operators
- + (string concatenation)
ms.assetid: 35cb3d7a-48f5-4b13-926c-a9d369e20ed7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 47a74bd7c9626dda0d10b27b0890420f6c9734fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199520"
---
# <a name="-string-concatenation-transact-sql"></a>+ (Concatenación de cadenas) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Es un operador de una expresión de cadenas que concatena dos o más cadenas de caracteres o binarias, columnas o una combinación de nombres de columna y cadenas en una expresión (un operador de cadenas).  Por ejemplo, `SELECT 'book'+'case';` devuelve `bookcase`.
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
expression + expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression*  
 Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida de uno de los tipos de datos de la categoría de tipos de datos de caracteres o binarios excepto los tipos de datos **image**, **ntext** o **text**. Ambas expresiones deben ser del mismo tipo de datos o una se debe poder convertir implícitamente en el tipo de datos de la otra.  
  
 Debe utilizarse una conversión explícita a datos caracteres cuando se concatenan cadenas binarias y cualesquiera caracteres entre las cadenas binarias. En el ejemplo siguiente se muestra cuándo se debe usar `CONVERT` o `CAST` con la concatenación binaria y cuándo no es necesario usar `CONVERT` o `CAST`.  
  
```sql
DECLARE @mybin1 VARBINARY(5), @mybin2 VARBINARY(5)  
SET @mybin1 = 0xFF  
SET @mybin2 = 0xA5  
-- No CONVERT or CAST function is required because this example   
-- concatenates two binary strings.  
SELECT @mybin1 + @mybin2  
-- A CONVERT or CAST function is required because this example  
-- concatenates two binary strings plus a space.  
SELECT CONVERT(VARCHAR(5), @mybin1) + ' '   
   + CONVERT(VARCHAR(5), @mybin2)  
-- Here is the same conversion using CAST.  
SELECT CAST(@mybin1 AS VARCHAR(5)) + ' '   
   + CAST(@mybin2 AS VARCHAR(5))  
```  
  
## <a name="result-types"></a>Tipos de resultado  
 Devuelve el tipo de datos del argumento con la mayor prioridad. Para obtener más información, vea [Prioridad de tipo de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Observaciones  
 El operador + (Concatenación de cadenas) se comporta de modo distinto cuando trabaja con una cadena vacía de longitud cero que cuando trabaja con valores NULL o desconocidos. Se puede especificar una cadena de caracteres de longitud cero como dos comillas simples sin caracteres incluidos entre las comillas. Se puede especificar una cadena binaria de longitud cero como 0x sin ningún valor de byte especificado en la constante hexadecimal. La concatenación de una cadena de longitud cero siempre concatena las dos cadenas especificadas. Si trabaja con cadenas de valor NULL, el resultado de la concatenación depende de la configuración de la sesión. Al igual que en las operaciones aritméticas realizadas con valores NULL, si se agrega un valor NULL a un valor conocido, el resultado suele ser un valor desconocido y la operación de concatenación de cadenas que se realiza con un valor NULL también debe generar un resultado NULL. No obstante, puede modificar este comportamiento si cambia la configuración de `CONCAT_NULL_YIELDS_NULL` para la sesión actual. Para obtener más información, vea [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).  
  
 Si el resultado de la concatenación de cadenas es superior al límite de 8.000 bytes, el resultado se trunca. Sin embargo, si una de las cadenas concatenadas como mínimo es un tipo de valor grande, no se produce el truncamiento.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-string-concatenation"></a>A. Usar la concatenación de cadenas  
 En el siguiente ejemplo se crea una sola columna en el encabezado de columna `Name` de varias columnas de caracteres, con el apellido de la persona seguido de una coma, un solo espacio y, a continuación, el nombre de la persona. El conjunto de resultados está en orden alfabético ascendente por el apellido y, a continuación, por el nombre.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT (LastName + ', ' + FirstName) AS Name  
FROM Person.Person  
ORDER BY LastName ASC, FirstName ASC;  
```  
  
### <a name="b-combining-numeric-and-date-data-types"></a>B. Combinar los tipos de datos numeric y date  
 En el siguiente ejemplo se usa la función `CONVERT` para concatenar los tipos de datos **numeric** y **date**.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT 'The order is due on ' + CONVERT(VARCHAR(12), DueDate, 101)  
FROM Sales.SalesOrderHeader  
WHERE SalesOrderID = 50001;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ------------------------------------------------  
 The order is due on 04/23/2007  
 (1 row(s) affected)
 ```  
  
### <a name="c-using-multiple-string-concatenation"></a>C. Usar la concatenación de varias cadenas  
 En el siguiente ejemplo se concatenan varias cadenas para formar una cadena larga que muestra el apellido y la primera inicial de los vicepresidentes de [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)]. Se agrega una coma después del apellido y un punto después de la primera inicial.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT (LastName + ',' + SPACE(1) + SUBSTRING(FirstName, 1, 1) + '.') AS Name, e.JobTitle  
FROM Person.Person AS p  
    JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle LIKE 'Vice%'  
ORDER BY LastName ASC;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Name               Title  
 -------------      ---------------`  
 Duffy, T.          Vice President of Engineering  
 Hamilton, J.       Vice President of Production  
 Welcker, B.        Vice President of Sales  

 (3 row(s) affected)
 ```  
 
### <a name="d-using-large-strings-in-concatenation"></a>D. Usar cadenas grandes en una concatenación
En el siguiente ejemplo se concatenan varias cadenas para formar una cadena larga y, luego, se intenta calcular la longitud de la cadena final. La longitud final del conjunto de resultados es 16 000, porque la evaluación de la expresión comienza por la izquierda, esto es, @x + @z + @y = > (@x + @z) + @y. En este caso, el resultado de (@x + @z) se trunca a 8000 bytes y, luego, @y se suma al conjunto de resultados, lo que hace que la longitud final de la cadena sea 16 000. Como @y es una cadena de tipo de valor grande, no se produce ningún truncamiento.

```sql
DECLARE @x VARCHAR(8000) = REPLICATE('x', 8000)
DECLARE @y VARCHAR(max) = REPLICATE('y', 8000)
DECLARE @z VARCHAR(8000) = REPLICATE('z',8000)
SET @y = @x + @z + @y
-- The result of following select is 16000
SELECT LEN(@y) AS y
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 y        
 -------  
 16000  
  
 (1 row(s) affected)
 ```  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-multiple-string-concatenation"></a>E. Usar la concatenación de varias cadenas  
 En el siguiente ejemplo se concatenan varias cadenas para formar una cadena larga que muestra el apellido y la primera inicial de los vicepresidentes en una base de datos de ejemplo. Se agrega una coma después del apellido y un punto después de la primera inicial.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT (LastName + ', ' + SUBSTRING(FirstName, 1, 1) + '.') AS Name, Title  
FROM DimEmployee  
WHERE Title LIKE '%Vice Pres%'  
ORDER BY LastName ASC;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Name               Title                                           
-------------      ---------------  
Duffy, T.          Vice President of Engineering  
Hamilton, J.       Vice President of Production  
Welcker, B.        Vice President of Sales  
```  
  
## <a name="see-also"></a>Consulte también  
 [+= &#40;Asignación de concatenación de cadenas&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/string-concatenation-equal-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Conversión de tipos de datos &#40;motor de base de datos&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
  
  



