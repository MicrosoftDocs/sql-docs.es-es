---
description: Operadores de comparación (Transact-SQL)
title: Operadores de comparación (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], testing
- operators [Transact-SQL], comparison
- testing expressions
- Boolean data type
- Boolean expressions
- comparing expressions
- comparison operators [SQL Server]
ms.assetid: b0cc68ef-3029-484c-a917-0c15dcbc230d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0c59b82a018a7d58c1eafdd607a1cf79e0003600
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208198"
---
# <a name="comparison-operators-transact-sql"></a>Operadores de comparación (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Los operadores de comparación comprueban si dos expresiones son iguales. Se pueden usar en todas las expresiones, excepto en las de los tipos de datos **text**, **ntext** o **image**. En la siguiente tabla se presentan los operadores de comparación [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
|Operator|Significado|  
|--------------|-------------|  
|[= (Es igual a)](../../t-sql/language-elements/equals-transact-sql.md)|Igual a|  
|[> (Mayor que)](../../t-sql/language-elements/greater-than-transact-sql.md)|Mayor que|  
|[< (Menor que)](../../t-sql/language-elements/less-than-transact-sql.md)|Menor que|  
|[>= (Mayor o igual a)](../../t-sql/language-elements/greater-than-or-equal-to-transact-sql.md)|Mayor o igual que|  
|[<= (Menor o igual a)](../../t-sql/language-elements/less-than-or-equal-to-transact-sql.md)|Menor o igual que|  
|[<> (No es igual a)](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)|No es igual a|  
|[!= (No es igual a)](../../t-sql/language-elements/not-equal-to-transact-sql-exclamation.md)|No es igual a (no es del estándar ISO)|  
|[\!< (No menor que)](../../t-sql/language-elements/not-less-than-transact-sql.md)|No es menor que (no es del estándar ISO)|  
|[\!> (no mayor que)](../../t-sql/language-elements/not-greater-than-transact-sql.md)|No es mayor que (no es del estándar ISO)|  
  
## <a name="boolean-data-type"></a>Tipo de datos Boolean  
 El resultado de un operador de comparación es del tipo de datos **Boolean**. Tiene tres valores: TRUE, FALSE y UNKNOWN. Las expresiones que devuelven tipos de datos **Boolean** se conocen como expresiones booleanas.  
  
 A diferencia de los otros tipos de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el tipo de datos **Boolean** no se puede especificar como tipo de datos de una columna o variable de una tabla y no se puede devolver en un conjunto de resultados.  
  
 Cuando SET ANSI_NULLS es ON, un operador con una o dos expresiones NULL devuelve UNKNOWN. Cuando SET ANSI_NULLS es OFF, se aplican las mismas reglas, excepto para los operadores es igual a (=) y no es igual a (<>). Cuando SET ANSI_NULLS es OFF, estos operadores tratan NULL como un valor conocido, equivalente a cualquier otro valor NULL, y solo devuelven TRUE o FALSE (nunca UNKNOWN).  
  
 Las expresiones con tipos de datos **Boolean** se usan en la cláusula WHERE para filtrar las filas que cumplen las condiciones de búsqueda y en las instrucciones de lenguaje de control de flujo tales como IF y WHILE, por ejemplo:  
  
```syntaxsql  
-- Uses AdventureWorks  
  
DECLARE @MyProduct INT;  
SET @MyProduct = 750;  
IF (@MyProduct <> 0)  
   SELECT ProductID, Name, ProductNumber  
   FROM Production.Product  
   WHERE ProductID = @MyProduct;  
```  
  
## <a name="see-also"></a>Vea también  
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)  
  
  
