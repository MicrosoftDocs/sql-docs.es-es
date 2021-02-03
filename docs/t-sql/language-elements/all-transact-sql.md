---
description: ALL (Transact-SQL)
title: ALL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALL_TSQL
- ALL
dev_langs:
- TSQL
helpviewer_keywords:
- single-column set of values [SQL Server]
- ALL (Transact-SQL)
ms.assetid: 4b0c002e-1ffd-4425-a980-11fdc1f24af7
author: cawrites
ms.author: chadam
ms.openlocfilehash: f315735e5ad8b1c8fd9d5a54d581d66fe7e820b7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176371"
---
# <a name="all-transact-sql"></a>ALL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Compara un valor escalar con un conjunto de valores de una sola columna.  
  
 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
scalar_expression { = | <> | != | > | >= | !> | < | <= | !< } ALL ( subquery )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *scalar_expression*  
 Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida.  
  
 { = \| <> \| != \| > \| >= \| !> \| < \| <= \| !< }  
 Es un operador de comparación.  
  
 *subquery*  
 Es una subconsulta que devuelve un conjunto de resultados de una columna. El tipo de datos de la columna devuelta debe ser igual que el tipo de datos de *scalar_expression*.  
  
 Es una instrucción SELECT restringida en la que no se permiten la cláusula ORDER BY ni la palabra clave INTO.  
  
## <a name="result-types"></a>Tipos de resultado  
 **Boolean**  
  
## <a name="result-value"></a>Valor del resultado  
 Devuelve TRUE cuando la comparación especificada es TRUE para todos los pares (_scalar_expression_ **,** _x_), donde *x* es un valor del conjunto de una sola columna. En caso contrario, devuelve FALSE.  
  
## <a name="remarks"></a>Observaciones  
 ALL requiere que *scalar_expression* se compare de forma positiva con cada valor devuelto por la subconsulta. Por ejemplo, si la subconsulta devuelve los valores 2 y 3, *scalar_expression* <= ALL (subconsulta) se evaluaría como TRUE para una *scalar_expression* de 2. Si la subconsulta devuelve los valores 2 y 3, *scalar_expression* = ALL (subconsulta) se evaluaría como FALSE, porque algunos de los valores de la subconsulta (el valor 3) no cumplirían los criterios de la expresión.  
  
 Para instrucciones que requieren que *scalar_expression* se compare de forma positiva con solo un valor devuelto por la subconsulta, vea [SOME &#124; ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md).  
  
 Este artículo hace referencia a ALL cuando se utiliza con una subconsulta. ALL también se puede usar con [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md) y [SELECT](../../t-sql/queries/select-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea un procedimiento almacenado que determina si todos los componentes de un `SalesOrderID` especificado en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] se pueden fabricar en el número de días especificado. En el ejemplo se usa una subconsulta para crear una lista del número del valor de `DaysToManufacture` para todos los componentes del `SalesOrderID` específico y, a continuación, confirma que todos los `DaysToManufacture` están dentro del número de días especificado.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROCEDURE DaysToBuild @OrderID INT, @NumberOfDays INT  
AS  
IF   
@NumberOfDays >= ALL  
   (  
    SELECT DaysToManufacture  
    FROM Sales.SalesOrderDetail  
    JOIN Production.Product   
    ON Sales.SalesOrderDetail.ProductID = Production.Product.ProductID   
    WHERE SalesOrderID = @OrderID  
   )  
PRINT 'All items for this order can be manufactured in specified number of days or less.'  
ELSE   
PRINT 'Some items for this order can''t be manufactured in specified number of days or less.' ;  
```  
  
 Para probar el procedimiento, ejecútelo con `SalesOrderID 49080`, que tiene un componente que requiere `2` días y dos componentes que requieren 0 días. La primera instrucción que se indica a continuación cumple los criterios. La segunda consulta no.  
  
```sql  
EXECUTE DaysToBuild 49080, 2 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `All items for this order can be manufactured in specified number of days or less.`  
  
```sql  
EXECUTE DaysToBuild 49080, 1 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Some items for this order can't be manufactured in specified number of days or less.`  
  
## <a name="see-also"></a>Consulte también  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md)  
  
