---
description: Funciones lógicas - CHOOSE (Transact-SQL)
title: CHOOSE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHOOSE
- CHOOSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHOOSE function
ms.assetid: 1c382c83-7500-4bae-bbdc-c1dbebd3d83f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6aad317f1b8677b2c9d1c5de1013df7beb303d7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208837"
---
# <a name="logical-functions---choose-transact-sql"></a>Funciones lógicas - CHOOSE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el elemento en el índice especificado de una lista de valores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CHOOSE ( index, val_1, val_2 [, val_n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *índice*  
 Expresión entera que representa un índice de base uno de la lista de elementos que le siguen.  
  
 Si el valor de índice proporcionado tiene un tipo de datos numérico distinto de **int**, el valor se convierte implícitamente en un entero. Si el valor de índice supera los límites de la matriz de valores, CHOOSE devuelve NULL.  
  
 *val_1 … val_n*  
 Lista de valores separados por comas de cualquier tipo de datos.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve el tipo de datos con la mayor prioridad del conjunto de tipos pasados a la función. Para obtener más información, vea [Prioridad de tipo de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Comentarios  
 CHOOSE actúa como un índice de una matriz, donde la matriz consta de los argumentos que siguen al argumento de índice. El argumento de índice determina cuál de los valores siguientes se devolverá.  
  
## <a name="examples"></a>Ejemplos  

### <a name="a-simple-choose-example"></a>A. Ejemplo CHOOSE sencillo

 En el ejemplo siguiente se devuelve el tercer elemento de la lista de valores que se proporciona.  
 
```sql 
SELECT CHOOSE ( 3, 'Manager', 'Director', 'Developer', 'Tester' ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
-------------  
Developer  
  
(1 row(s) affected)  
```  

### <a name="b-simple-choose-example-based-on-column"></a>B. Ejemplo CHOOSE sencillo basado en columna

 En el ejemplo siguiente se devuelve una cadena de caracteres simple basada en el valor de la columna `ProductCategoryID`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductCategoryID, CHOOSE (ProductCategoryID, 'A','B','C','D','E') AS Expression1  
FROM Production.ProductCategory;  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ProductCategoryID Expression1  
----------------- -----------  
3                 C  
1                 A  
2                 B  
4                 D  
  
(4 row(s) affected)  
  
```  

### <a name="c-choose-in-combination-with-month"></a>C. CHOOSE en combinación con MONTH
  
 El siguiente ejemplo devuelve la temporada en la que se contrató a un empleado. La función MONTH se usa para devolver el valor de mes de la columna `HireDate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT JobTitle, HireDate, CHOOSE(MONTH(HireDate),'Winter','Winter', 'Spring','Spring','Spring','Summer','Summer',   
                                                  'Summer','Autumn','Autumn','Autumn','Winter') AS Quarter_Hired  
FROM HumanResources.Employee  
WHERE  YEAR(HireDate) > 2005  
ORDER BY YEAR(HireDate);  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
JobTitle                                           HireDate   Quarter_Hired  
-------------------------------------------------- ---------- -------------  
Sales Representative                               2006-11-01 Autumn  
European Sales Manager                             2006-05-18 Spring  
Sales Representative                               2006-07-01 Summer  
Sales Representative                               2006-07-01 Summer  
Sales Representative                               2007-07-01 Summer  
Pacific Sales Manager                              2007-04-15 Spring  
Sales Representative                               2007-07-01 Summer  
  
```  
  
## <a name="see-also"></a>Vea también  
 [IIF &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-iif-transact-sql.md)  
  
  
