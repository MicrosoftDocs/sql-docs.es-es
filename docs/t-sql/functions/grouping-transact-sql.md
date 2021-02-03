---
description: GROUPING (Transact-SQL)
title: GROUPING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUPING
- GROUPING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- null values [SQL Server], GROUPING function
- grouping columns
- ROLLUP operator
- GROUP BY clause, GROUPING function
- GROUPING function
- CUBE operator
ms.assetid: 4efa3868-1fc4-4626-8fb1-e863cc03e422
author: cawrites
ms.author: chadam
ms.openlocfilehash: fe8a8f781c72e0b188ca87f9d71af8dff4aeab89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182380"
---
# <a name="grouping-transact-sql"></a>GROUPING (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Indica si una expresión de columna especificada en una lista GROUP BY es agregada o no. GROUPING devuelve 1 para agregado y 0 para no agregado, en el conjunto de resultados. GROUPING solo se puede usar en la lista de las cláusulas SELECT \<select>, HAVING y ORDER BY si se especifica GROUP BY.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
GROUPING ( <column_expression> )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 \<column_expression>  
 Es una columna o una expresión que contiene una columna en una cláusula [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md).  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **tinyint**  
  
## <a name="remarks"></a>Observaciones  
 GROUPING se utiliza para distinguir entre los valores NULL devueltos por ROLLUP, CUBE o GROUPING SETS y los valores NULL normales. El valor NULL devuelto como resultado de una operación ROLLUP, CUBE o GROUPING SETS es un uso especial de NULL. Actúa como marcador de posición de columna en el conjunto de resultados y significa "todos".  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se agrupa `SalesQuota` y se agregan las cantidades de `SaleYTD` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La función `GROUPING` se aplica a la columna `SalesQuota`.  
  
```sql 
SELECT SalesQuota, SUM(SalesYTD) 'TotalSalesYTD', GROUPING(SalesQuota) AS 'Grouping'  
FROM Sales.SalesPerson  
GROUP BY SalesQuota WITH ROLLUP;  
GO  
```  
  
 El conjunto de resultados muestra dos valores NULL bajo `SalesQuota`. El primer valor `NULL` representa el grupo de valores NULL de esta columna en la tabla. El segundo valor `NULL` se encuentra en la fila de resumen que agrega la operación ROLLUP. La fila de resumen indica las cantidades de `TotalSalesYTD` de todos los grupos `SalesQuota`, como señala el valor `1` en la columna `Grouping`.  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 SalesQuota     TotalSalesYTD       Grouping  
------------   -----------------   --------  
NULL           1533087.5999          0  
250000.00      33461260.59           0  
300000.00      9299677.9445          0  
NULL           44294026.1344         1  

(4 row(s) affected)
```  
  
## <a name="see-also"></a>Consulte también  
 [GROUPING_ID &#40;Transact-SQL&#41;](../../t-sql/functions/grouping-id-transact-sql.md)   
 [GROUP BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-group-by-transact-sql.md)  
  
  
