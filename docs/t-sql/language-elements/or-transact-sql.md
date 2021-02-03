---
description: OR (Transact-SQL)
title: OR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OR_TSQL
- OR
dev_langs:
- TSQL
helpviewer_keywords:
- conditions [SQL Server], combining
- combining conditions
- OR operator
ms.assetid: b730a256-4a63-4880-9906-65b05cd9caf2
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6ff82e085323121fd9da098dd3e0db0495f278ec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191875"
---
# <a name="or-transact-sql"></a>OR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Combina dos condiciones. Cuando en una instrucción se utiliza más de un operador lógico, los operadores OR se evalúan después de los operadores AND. Sin embargo, se puede cambiar el orden de evaluación gracias a los paréntesis.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
boolean_expression OR boolean_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *boolean_expression*  
 Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida que devuelva TRUE, FALSE o UNKNOWN.  
  
## <a name="result-types"></a>Tipos de resultado  
 **Boolean**  
  
## <a name="result-value"></a>Valor del resultado  
 OR devuelve TRUE cuando alguna de las condiciones es TRUE.  
  
## <a name="remarks"></a>Observaciones  
 En la siguiente tabla se muestra el resultado del operador OR.  
  
||true|false|DESCONOCIDO|  
|------|----------|-----------|-------------|  
|**TRUE**|TRUE|TRUE|TRUE|  
|**FALSE**|true|false|DESCONOCIDO|  
|**UNKNOWN**|true|DESCONOCIDO|DESCONOCIDO|  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se utiliza la vista `vEmployeeDepartmentHistory` para recuperar los nombres del personal de `Quality Assurance` que trabaja en el turno de tarde o en el de noche. Si se omiten los paréntesis, la consulta devuelve los empleados de `Quality Assurance` que trabajan en el turno de tarde y todos los empleados que trabajan en el turno de noche.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, Shift   
FROM HumanResources.vEmployeeDepartmentHistory  
WHERE Department = 'Quality Assurance'  
   AND (Shift = 'Evening' OR Shift = 'Night');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 FirstName    LastName         Shift 
 ------------ ---------------- ------- 
 Andreas      Berglund         Evening 
 Sootha       Charncherngkha   Night
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el ejemplo siguiente se recuperan los nombres de los empleados que ganan un `BaseRate` menor de 20 o cuya `HireDate` es el 1 de enero de 2001 o posterior.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, BaseRate, HireDate   
FROM DimEmployee  
WHERE BaseRate < 10 OR HireDate >= '2001-01-01';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  


