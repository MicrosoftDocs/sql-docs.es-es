---
description: ISNULL (Transact-SQL)
title: ISNULL (Transact-SQL)
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ISNULL
- ISNULL_TSQL
- IFNULL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- replacing null values
- null values [SQL Server], ISNULL
- null values [SQL Server], replacement values
- ISNULL function
ms.assetid: 6f3e5802-864b-4e77-9862-657bb5430b68
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e7579c4f0d5c310bf8837169948ced17ee1f64a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186483"
---
# <a name="isnull-transact-sql"></a>ISNULL (Transact-SQL) 

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Sustituye el valor NULL por el valor especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ISNULL ( check_expression , replacement_value )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *check_expression*  
 Es la [expresión](../../t-sql/language-elements/expressions-transact-sql.md) que se va a comprobar si es NULL. *check_expression* puede ser de cualquier tipo.  
  
 *replacement_value*  
 Es la expresión que se devuelve si *check_expression* es NULL. *valor_de_reemplazo* debe ser de un tipo que se pueda convertir implícitamente en el tipo de *expresión_de_comprobación*.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve el mismo tipo que *check_expression*. Si se proporciona un literal NULL como *check_expression*, devuelve el tipo de datos de *replacement_value*. Si se proporciona un literal NULL como *check_expression* y no se proporciona *replacement_value*, se devuelve un **int**.  
  
## <a name="remarks"></a>Observaciones  
 El valor de *check_expression* se devuelve si no es NULL; de lo contrario, se devuelve *replacement_value* después de convertirse de forma implícita al tipo de *check_expression*, si los tipos son diferentes. *replacement_value* se puede truncar si *replacement_value* es mayor que *check_expression*.  
  
> [!NOTE]  
>  Use [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md) para devolver el primer valor distinto de NULL.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-isnull-with-avg"></a>A. Usar ISNULL con AVG  
 En el ejemplo siguiente se busca el promedio del peso de todos los productos. Sustituye el valor `50` para todas las entradas NULL en la columna `Weight` de la tabla `Product`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT AVG(ISNULL(Weight, 50))  
FROM Production.Product;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 -------------------------- 
59.79  
  
 (1 row(s) affected)
 ```  
  
### <a name="b-using-isnull"></a>B. Usar ISNULL  
 En el siguiente ejemplo se selecciona la descripción, el porcentaje de descuento, la cantidad mínima y la cantidad máxima de todas las ofertas especiales de `AdventureWorks2012`. Si la cantidad máxima de una oferta especial determinada es NULL, el valor de `MaxQty` mostrado en el conjunto de resultados es `0.00`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description, DiscountPct, MinQty, ISNULL(MaxQty, 0.00) AS 'Max Quantity'  
FROM Sales.SpecialOffer;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|  Descripción       |  DiscountPct    |   MinQty    |   Cantidad máxima       |
|  ---------------   |  -------------  |   --------  |   ---------------    |
|  No Discount       |  0.00           |   0         |   0                  |
|  Volume Discount   |  0.02           |   11        |   14                 |
|  Volume Discount   |  0,05           |   15        |   4                  |
|  Volume Discount   |  0,10           |   25        |   0                  |
|  Volume Discount   |  0,15           |   41        |   0                  |
|  Volume Discount   |  0,20           |   61        |   0                  |
|  Mountain-100 Cl   |  0.35           |   0         |   0                  |
|  Sport Helmet Di   |  0,10           |   0         |   0                  |
|  Road-650 Overst   |  0,30           |   0         |   0                  |
|  Mountain Tire S   |  0.50           |   0         |   0                  |
|  Sport Helmet Di   |  0,15           |   0         |   0                  |
|  LL Road Frame S   |  0.35           |   0         |   0                  |
|  Touring-3000 Pr   |  0,15           |   0         |   0                  |
|  Touring-1000 Pr   |  0,20           |   0         |   0                  |
|  Half-Price Peda   |  0.50           |   0         |   0                  |
|  Mountain-500 Si   |  0.40           |   0         |   0                  |

 `(16 row(s) affected)`  
  
### <a name="c-testing-for-null-in-a-where-clause"></a>C. Comprobar si hay valores NULL en una cláusula WHERE  
 No utilice ISNULL para buscar los valores NULL. Use IS NULL en su lugar. En el ejemplo siguiente se buscan todos los productos que tienen `NULL` en la columna de peso. Tenga en cuenta el espacio entre `IS` y `NULL`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, Weight  
FROM Production.Product  
WHERE Weight IS NULL;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-isnull-with-avg"></a>D. Usar ISNULL con AVG  
 En el ejemplo siguiente se busca el promedio del peso de todos los productos en una tabla de ejemplo. Sustituye el valor `50` para todas las entradas NULL en la columna `Weight` de la tabla `Product`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT AVG(ISNULL(Weight, 50))  
FROM dbo.DimProduct;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------------------------   
52.88   
```  
  
### <a name="e-using-isnull"></a>E. Usar ISNULL  
 En el ejemplo siguiente se usa ISNULL para comprobar los valores NULL en la columna `MinPaymentAmount` y mostrar el valor `0.00` para esas filas.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ResellerName,   
       ISNULL(MinPaymentAmount,0) AS MinimumPayment  
FROM dbo.DimReseller  
ORDER BY ResellerName;  
  
```  
  
 A continuación se muestra un conjunto parcial de resultados.  
  
|  ResellerName                |  MinimumPayment    |
|  -------------------------   |  --------------    |
|  Una asociación de bicicleta       |     0.0000         |
|  Una tienda de bicicletas                |     0.0000         |
|  Una tienda de bicis                |     0.0000         |
|  Una gran empresa de bicicletas     |     0.0000         |
|  La típica tienda de bicicletas         |   200.0000         |
|  Servicio y ventas aceptables  |     0.0000         |
  
### <a name="f-using-is-null-to-test-for-null-in-a-where-clause"></a>F. Usar IS NULL para probar NULL en una cláusula WHERE  
 En el ejemplo siguiente se buscan todos los productos que tienen `NULL` en la columna `Weight`. Tenga en cuenta el espacio entre `IS` y `NULL`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EnglishProductName, Weight  
FROM dbo.DimProduct  
WHERE Weight IS NULL;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [IS NULL &#40;Transact-SQL&#41;](../../t-sql/queries/is-null-transact-sql.md)   
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md)  
  
  

