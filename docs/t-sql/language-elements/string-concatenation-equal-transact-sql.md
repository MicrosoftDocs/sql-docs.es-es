---
title: '\+ = Concatenación de cadenas'
description: Concatene dos cadenas y establezca como valor de la cadena el resultado de la operación.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 12/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- concatenate strings
- string concatenation
- += (concatenate operator)
ms.assetid: 4aaeaab7-9b2b-48e0-8487-04ed672ebcb1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5569660d93112d91664a3177d1bdd7f5dcdbe359
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352861"
---
# <a name="-string-concatenation-assignment-transact-sql"></a>+= (Asignación de concatenación de cadenas) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Concatena dos cadenas y establece como valor de la cadena el resultado de la operación. Por ejemplo, si una variable @x es igual a "Adventure", @x += "Works" toma el valor original de @x, agrega "Works" a la cadena y establece @x en el nuevo valor ("AdventureWorks").  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
expression += expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression*  
 Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida de alguno de los tipos de datos de carácter.  
  
## <a name="result-types"></a>Tipos de resultado  
 Devuelve el tipo de datos que se define para la variable.  
  
## <a name="remarks"></a>Observaciones  
 SET @v1 += "expresión" equivale a SET @v1 = @v1 + ("expresión"). Asimismo, SET @v1 = @v2 + @v3 + @v4 equivale a SET @v1 = (@v2 + @v3) + @v4.  
  
 El operador += no se puede utilizar sin una variable. Por ejemplo, el código siguiente provocará un error:  
  
```sql  
SELECT 'Adventure' += 'Works'  
```  
  
## <a name="examples"></a>Ejemplos  
### <a name="a-concatenation-using--operator"></a>A. Concatenación con el operador +=
 En el siguiente ejemplo se concatena con el operador `+=`.  
  
```sql  
DECLARE @v1 VARCHAR(40);  
SET @v1 = 'This is the original.';  
SET @v1 += ' More text.';  
PRINT @v1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `This is the original. More text.`  
  
### <a name="b-order-of-evaluation-while-concatenating-using--operator"></a>B. Orden de evaluación al concatenar con el operador +=
En el siguiente ejemplo se concatenan varias cadenas para formar una cadena larga y, luego, se intenta calcular la longitud de la cadena final. En este ejemplo se muestran las reglas de truncamiento y orden de evaluación cuando se usa el operador de concatenación. 

```sql
DECLARE @x VARCHAR(4000) = REPLICATE('x', 4000)
DECLARE @z VARCHAR(8000) = REPLICATE('z',8000)
DECLARE @y VARCHAR(max);
 
SET @y = '';
SET @y += @x + @z;
SELECT LEN(@y) AS Y; -- 8000
 
SET @y = '';
SET @y = @y + @x + @z;
SELECT LEN(@y) AS Y; -- 12000
 
SET @y = '';
SET @y = @y +(@x + @z);
SELECT LEN(@y) AS Y; -- 8000
-- or
SET @y = '';
SET @y = @x + @z + @y;
SELECT LEN(@y) AS Y; -- 8000
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Y       
 ------- 
 8000 
  
 (1 row(s) affected) 
  
    
 Y       
 ------- 
 12000 
  
 (1 row(s) affected) 

 Y       
 ------- 
 8000 
  
 (1 row(s) affected) 
  
 Y       
 ------- 
 8000 
  
 (1 row(s) affected)
  ```   
   
## <a name="see-also"></a>Consulte también  
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [+= &#40;Asignación de suma&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/add-equals-transact-sql.md)   
 [+ &#40;Concatenación de cadenas&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
  
  
