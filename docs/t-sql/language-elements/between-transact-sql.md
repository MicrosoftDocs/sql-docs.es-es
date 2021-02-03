---
description: BETWEEN (Transact-SQL)
title: BETWEEN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BETWEEN
- BETWEEN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- inclusive ranges
- testing range
- exclusive ranges
- NOT BETWEEN operator
- BETWEEN operator
- range to test [SQL Server]
ms.assetid: a5d5b050-203e-4355-ac85-e08ef5ca7823
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cd28530d0c32a3f514410aba12d68560989e598b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159167"
---
# <a name="between-transact-sql"></a>BETWEEN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Especifica un intervalo que se va a probar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
test_expression [ NOT ] BETWEEN begin_expression AND end_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *test_expression*  
 Es la [expresión](../../t-sql/language-elements/expressions-transact-sql.md) que se va a probar en el intervalo definido por *begin_expression* y *end_expression*. *test_expression* debe tener el mismo tipo de datos que *begin_expression* y *end_expression*.  
  
 NOT  
 Especifica que se niega el resultado del predicado.  
  
 *begin_expression*  
 Es cualquier expresión válida. *begin_expression* debe tener el mismo tipo de datos que *test_expression* y *end_expression*.  
  
 *end_expression*  
 Es cualquier expresión válida. *end_expression* debe tener el mismo tipo de datos que *test_expression* y *begin_expression*.  
  
 y  
 Actúa como un marcador de posición que indica que *test_expression* debe encontrarse dentro del intervalo indicado por *begin_expression* y *end_expression*.  
  
## <a name="result-types"></a>Tipos de resultado  
 **Boolean**  
  
## <a name="result-value"></a>Valor del resultado  
 BETWEEN devuelve **TRUE** si el valor de *test_expression* es mayor o igual que el valor de *begin_expression* y menor o igual que el valor de *end_expression*.  
  
 NOT BETWEEN devuelve **TRUE** si el valor de *test_expression* es menor que el valor de *begin_expression* y mayor que el valor de *end_expression*.  
  
## <a name="remarks"></a>Comentarios  
 Para especificar un intervalo exclusivo, use los operadores "mayor que" (>) y "menor que" ( <). Si alguna entrada del predicado BETWEEN o NOT BETWEEN es NULL, el resultado es UNKNOWN.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-between"></a>A. Utilizar BETWEEN  
 En el ejemplo siguiente se devuelve información sobre los roles de base de datos de una de base de datos. La primera consulta devuelve todos los roles. En el segundo ejemplo se usa la cláusula `BETWEEN` para limitar los roles a los valores `database_id` especificados.  
  
```sql  
SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R';

SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R'
AND principal_id BETWEEN 16385 AND 16390;
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]   
```  
principal_id    name
------------  ---- 
0               public
16384           db_owner
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
16391           db_datawriter
16392           db_denydatareader
16393           db_denydatawriter
```  
```  
principal_id    name
------------  ---- 
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
```  
  
### <a name="b-using--and--instead-of-between"></a>B. Utilizar > y < en lugar de BETWEEN  
 En el siguiente ejemplo se utilizan los operadores mayor que (`>`) y menor que (`<`) y, puesto que dichos operadores no son inclusivos, se devuelven nueve filas en lugar de las diez devueltas en el ejemplo anterior.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate > 27 AND ep.Rate < 30  
ORDER BY ep.Rate;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 FirstName   LastName             Rate  
 ---------   -------------------  ---------  
 Paula       Barreto de Mattos    27.1394  
 Janaina     Bueno                27.4038  
 Dan         Bacon                27.4038  
 Ramesh      Meyyappan            27.4038  
 Karen       Berg                 27.4038  
 David       Bradley              28.7500  
 Hazem       Abolrous             28.8462  
 Ovidiu      Cracium              28.8462  
 Rob         Walters              29.8462  
 ```    
  
### <a name="c-using-not-between"></a>C. Utilizar NOT BETWEEN  
 En el siguiente ejemplo se buscan todas las filas que no están incluidas en un intervalo especificado de `27` a `30`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate NOT BETWEEN 27 AND 30  
ORDER BY ep.Rate;  
GO  
```  
  
### <a name="d-using-between-with-datetime-values"></a>D. Utilizar BETWEEN con valores datetime  
 En el siguiente ejemplo se recuperan filas en las que los valores **datetime** están entre `'20011212'` y `'20020105'`, ambos incluidos.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT BusinessEntityID, RateChangeDate  
FROM HumanResources.EmployeePayHistory  
WHERE RateChangeDate BETWEEN '20011212' AND '20020105';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 BusinessEntityID RateChangeDate  
 ----------- -----------------------  
 3           2001-12-12 00:00:00.000  
 4           2002-01-05 00:00:00.000  
 ```  
 
 La consulta recupera las filas previstas porque los valores de fecha de la consulta y los valores de **datetime** almacenados en la columna `RateChangeDate` se han especificado sin la parte de hora de la fecha. Si no se especifica la parte de hora, toma el valor predeterminado 12:00 a.m. Tenga en cuenta que esta consulta no devolverá una fila que contenga una parte de hora posterior a 12:00 a.m. del 05-01-2002, ya que está fuera del rango.  
  
  
## <a name="see-also"></a>Vea también  
 [&#62; &#40;Mayor que&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/greater-than-transact-sql.md)   
 [&#60; &#40;Menor que&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/less-than-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  


