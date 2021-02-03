---
description: NULLIF (Transact-SQL)
title: NULLIF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NULLIF
- NULLIF_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- null values [SQL Server], equivalent expressions
- expressions [SQL Server], null values
- NULLIF function
- equivalent expressions [SQL Server]
ms.assetid: 44c7b67e-74c7-4bb9-93a4-7a3016bd2feb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 105aca59bbcd707e644a19dcb44aaff0c4784aa3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191942"
---
# <a name="nullif-transact-sql"></a>NULLIF (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve un valor NULL si las dos expresiones especificadas son iguales. Por ejemplo, `SELECT NULLIF(4,4) AS Same, NULLIF(5,7) AS Different;` devuelve NULL para la primera columna (4 y 4) porque los dos valores de entrada son iguales. La segunda columna devuelve el primer valor (5) porque los dos valores de entrada son diferentes. 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
NULLIF ( expression , expression )  
```  
  
## <a name="arguments"></a>Argumentos  
 *expression*  
 Cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) escalar válida.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Devuelve el mismo tipo que el primer parámetro *expression*.  
  
 NULLIF devuelve el primer parámetro *expression* si las dos expresiones no son iguales. Si las expresiones son iguales, NULLIF devuelve un valor NULL del tipo del primer parámetro *expression*.  
  
## <a name="remarks"></a>Comentarios  
 NULLIF equivale a una expresión CASE buscada en la que las dos expresiones son iguales y la expresión resultante es NULL.  
  
 Se recomienda no usar funciones dependientes del tiempo, como RAND(), dentro de una función NULLIF. La función podría evaluarse dos veces y las dos invocaciones podrían devolver resultados diferentes.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-budget-amounts-that-have-not-changed"></a>A. Devolver importes del presupuesto que no han cambiado  
 En el ejemplo siguiente se crea una tabla denominada `budgets` para mostrar un departamento (`dept`), su presupuesto actual (`current_year`) y su presupuesto anterior (`previous_year`). En el año actual se utiliza `NULL` para los departamentos cuyos presupuestos no han cambiado desde el año anterior y `0` para los presupuestos que aún no se han determinado. Para averiguar el promedio de los departamentos que reciben un presupuesto e incluir el valor del presupuesto del año anterior (utilice el valor `previous_year` cuando el de `current_year` es `NULL`), combine las funciones `NULLIF` y `COALESCE`.  
  
```sql  
CREATE TABLE dbo.budgets  
(  
   dept            TINYINT   IDENTITY,  
   current_year    DECIMAL   NULL,  
   previous_year   DECIMAL   NULL  
);  
INSERT budgets VALUES(100000, 150000);  
INSERT budgets VALUES(NULL, 300000);  
INSERT budgets VALUES(0, 100000);  
INSERT budgets VALUES(NULL, 150000);  
INSERT budgets VALUES(300000, 250000);  
GO    
SET NOCOUNT OFF;  
SELECT AVG(NULLIF(COALESCE(current_year,  
   previous_year), 0.00)) AS 'Average Budget'  
FROM budgets;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Average Budget  
 --------------  
 212500.000000  
 (1 row(s) affected)
 ```  
  
### <a name="b-comparing-nullif-and-case"></a>B. Comparar NULLIF y CASE  
 Para mostrar la similitud entre `NULLIF` y `CASE`, las siguientes consultas determinan si los valores de las columnas `MakeFlag` y `FinishedGoodsFlag` coinciden. En la primera consulta se utiliza `NULLIF`. La segunda consulta utiliza la expresión `CASE`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductID, MakeFlag, FinishedGoodsFlag,   
   NULLIF(MakeFlag,FinishedGoodsFlag) AS 'Null if Equal'  
FROM Production.Product  
WHERE ProductID < 10;  
GO  
  
SELECT ProductID, MakeFlag, FinishedGoodsFlag,'Null if Equal' =  
   CASE  
       WHEN MakeFlag = FinishedGoodsFlag THEN NULL  
       ELSE MakeFlag  
   END  
FROM Production.Product  
WHERE ProductID < 10;  
GO  
```  

### <a name="c-returning-budget-amounts-that-contain-no-data"></a>C. Devolver importes del presupuesto que no contienen datos  
 En el siguiente ejemplo se crea una tabla `budgets`, se cargan datos y se usa `NULLIF` para devolver un valor null si `current_year` ni `previous_year` contienen datos.  
  
```sql  
CREATE TABLE budgets (  
   dept           TINYINT,  
   current_year   DECIMAL(10,2),  
   previous_year  DECIMAL(10,2)  
);  
  
INSERT INTO budgets VALUES(1, 100000, 150000);  
INSERT INTO budgets VALUES(2, NULL, 300000);  
INSERT INTO budgets VALUES(3, 0, 100000);  
INSERT INTO budgets VALUES(4, NULL, 150000);  
INSERT INTO budgets VALUES(5, 300000, 300000);  
  
SELECT dept, NULLIF(current_year,  
   previous_year) AS LastBudget  
FROM budgets;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 dept   LastBudget  
 ----   -----------  
 1      100000.00  
 2      null 
 3      0.00  
 4      null  
 5      null
 ```  
  
## <a name="see-also"></a>Consulte también  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [decimal y numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)   
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  

