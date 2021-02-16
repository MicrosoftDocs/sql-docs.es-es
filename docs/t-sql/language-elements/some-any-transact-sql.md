---
description: SOME | ANY (Transact-SQL)
title: SOME | ANY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SOME
- SOME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- scalar values
- comparing scalar with single-column set
- ANY operator
- SOME | ANY keyword
- single-column set of values [SQL Server]
ms.assetid: 1f717ad6-f67b-4980-9397-577ecb0e5789
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f25709500f370b6083584b1321937b0484310bf
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342581"
---
# <a name="some--any-transact-sql"></a>SOME | ANY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Compara un valor escalar con un conjunto de valores de una sola columna. SOME y ANY son equivalentes.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
scalar_expression { = | < > | ! = | > | > = | ! > | < | < = | ! < }   
     { SOME | ANY } ( subquery )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *scalar_expression*  
 Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida.  
  
 { = \| <> \| != \| > \| >= \| !> \| < \| <= \| !< }  
 Es cualquier operador de comparación válido.  
  
 SOME | ANY  
 Especifica que se debe realizar una comparación.  
  
 *subquery*  
 Es una subconsulta que tiene un conjunto de resultados de una columna. El tipo de datos de la columna devuelta debe ser el mismo que el de *scalar_expression*.  
  
## <a name="result-types"></a>Tipos de resultado  
 **Boolean**  
  
## <a name="result-value"></a>Valor del resultado  
 SOME o ANY devuelven **TRUE** cuando la comparación especificada es TRUE para todos los pares (_scalar_expression_ **,** _x_), donde *x* es un valor del conjunto de una sola columna; en caso contrario, devuelve **FALSE**.  
  
## <a name="remarks"></a>Observaciones  
 SOME requiere que la *scalar_expression* se compare de forma positiva con al menos un valor devuelto por la subconsulta. Para ver instrucciones que requieren que *scalar_expression* se compare de forma positiva con solo un valor devuelto por la subconsulta, vea [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md). Por ejemplo, si la subconsulta devuelve los valores 2 y 3, *scalar_expression* = SOME (subconsulta) se evaluaría como TRUE para una *scalar_expression* de 2. Si la subconsulta devuelve los valores 2 y 3, *scalar_expression* = ALL (subconsulta) se evaluaría como FALSE, porque algunos de los valores de la subconsulta (el valor 3) no cumplirían los criterios de la expresión.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-running-a-simple-example"></a>A. Ejecutar un ejemplo sencillo  
 Las instrucciones siguientes crean una tabla simple y agregan los valores `1`, `2`, `3` y `4` a la columna de `ID`.  
  
```sql  
CREATE TABLE T1  
(ID INT) ;  
GO  
INSERT T1 VALUES (1) ;  
INSERT T1 VALUES (2) ;  
INSERT T1 VALUES (3) ;  
INSERT T1 VALUES (4) ;  
```  
  
 La consulta siguiente devuelve `TRUE` porque `3` es menor que alguno de los valores de la tabla.  
  
```sql  
IF 3 < SOME (SELECT ID FROM T1)  
PRINT 'TRUE'   
ELSE  
PRINT 'FALSE' ;  
```  
  
 La consulta siguiente devuelve `FALSE` porque `3` no es menor que todos los valores de la tabla.  
  
```sql  
IF 3 < ALL (SELECT ID FROM T1)  
PRINT 'TRUE'   
ELSE  
PRINT 'FALSE' ;  
```  
  
### <a name="b-running-a-practical-example"></a>B. Ejecutar un ejemplo práctico  
 En el ejemplo siguiente se crea un procedimiento almacenado que determina si todos los componentes de un `SalesOrderID` especificado en la base de datos `AdventureWorks2012` se pueden fabricar en el número de días especificado. En el ejemplo se usa una subconsulta para crear una lista de valores de `DaysToManufacture` para todos los componentes del `SalesOrderID` especificado y, a continuación, se comprueba si alguno de los valores devueltos por la subconsulta es mayor que el número de días especificado. Si cada valor de `DaysToManufacture` que se devuelve es menor que el número proporcionado, la condición es TRUE y se imprime el primer mensaje.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROCEDURE ManyDaysToComplete @OrderID INT, @NumberOfDays INT  
AS  
IF   
@NumberOfDays < SOME  
   (  
    SELECT DaysToManufacture  
    FROM Sales.SalesOrderDetail  
    JOIN Production.Product   
    ON Sales.SalesOrderDetail.ProductID = Production.Product.ProductID   
    WHERE SalesOrderID = @OrderID  
   )  
PRINT 'At least one item for this order can''t be manufactured in specified number of days.'
ELSE   
PRINT 'All items for this order can be manufactured in the specified number of days or less.' ;  
  
```  
  
 Para probar el procedimiento, ejecútelo usando `SalesOrderID``49080`, que tiene un componente que requiere `2` días y dos componentes que requieren 0 días. La primera instrucción cumple los criterios. La segunda consulta no.  
  
```sql  
EXECUTE ManyDaysToComplete 49080, 2 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `All items for this order can be manufactured in the specified number of days or less.`  
  
```  
EXECUTE ManyDaysToComplete 49080, 1 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `At least one item for this order can't be manufactured in specified number of days.`  
  
## <a name="see-also"></a>Consulte también  
 [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md)  
  
