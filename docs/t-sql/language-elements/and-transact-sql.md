---
description: AND (Transact-SQL)
title: AND (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AND_TSQL
- AND
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], TRUE
- "TRUE"
- AND, about AND operators
- AND
- combining expressions
ms.assetid: b61d7f8d-5a51-49b7-91dd-f6190a5a0fb9
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5558851589fe1eb5f77d5fea55e40fa4c6d0ba75
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176387"
---
# <a name="and-transact-sql"></a>AND (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Combina dos expresiones booleanas y devuelve **TRUE** cuando ambas expresiones son **TRUE**. Cuando se usa más de un operador lógico en una instrucción, en primer lugar se evalúan los operadores **AND**. Puede cambiar el orden de evaluación gracias a los paréntesis.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
boolean_expression AND boolean_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *boolean_expression*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida que devuelve un valor booleano: **TRUE**, **FALSE** o **UNKNOWN**.  
  
## <a name="result-types"></a>Tipos de resultado  
 **Boolean**  
  
## <a name="result-value"></a>Valor del resultado  
 Devuelve TRUE cuando ambas expresiones son TRUE.  
  
## <a name="remarks"></a>Comentarios  
 En la siguiente tabla se muestran los resultados de la comparación de los valores TRUE y FALSE mediante el operador AND.  
  
||true|false|DESCONOCIDO|  
|------|----------|-----------|-------------|  
|**TRUE**|true|false|DESCONOCIDO|  
|**FALSE**|FALSE|FALSE|false|  
|**UNKNOWN**|DESCONOCIDO|FALSE|DESCONOCIDO|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-the-and-operator"></a>A. Utilizar el operador AND  
 En el ejemplo siguiente se selecciona información sobre los empleados que tienen el cargo de `Marketing Assistant` y más de `41` horas de vacaciones disponibles.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT  BusinessEntityID, LoginID, JobTitle, VacationHours   
FROM HumanResources.Employee  
WHERE JobTitle = 'Marketing Assistant'  
AND VacationHours > 41 ;  
```  
  
### <a name="b-using-the-and-operator-in-an-if-statement"></a>B. Utilizar el operador AND en una instrucción IF  
 En los ejemplos siguientes se muestra cómo utilizar AND en una instrucción IF. En la primera instrucción, `1 = 1` y `2 = 2` son true; por consiguiente, el resultado es true. En el segundo ejemplo, el argumento `2 = 17` es false; por consiguiente, el resultado es false.  
  
```sql  
IF 1 = 1 AND 2 = 2  
BEGIN  
   PRINT 'First Example is TRUE'  
END  
ELSE PRINT 'First Example is FALSE' ;  
GO  
  
IF 1 = 1 AND 2 = 17  
BEGIN  
   PRINT 'Second Example is TRUE'  
END  
ELSE PRINT 'Second Example is FALSE' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
