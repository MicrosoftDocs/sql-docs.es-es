---
description: Funciones lógicas - IIF (Transact-SQL)
title: IIF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IIF_TSQL
- IIF
dev_langs:
- TSQL
helpviewer_keywords:
- IIF function
ms.assetid: e3ccf8ed-1cec-43ac-90b7-d8597c24b050
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b58823a6b9e6b43b3458392d1b9016c0716a2e32
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "91116361"
---
# <a name="logical-functions---iif-transact-sql"></a>Funciones lógicas - IIF (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve uno de dos valores, dependiendo de si la expresión booleana se evalúa como true o como false en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
IIF ( boolean_expression, true_value, false_value )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *boolean_expression*  
 Una expresión booleana válida.  
  
 Si este argumento no es una expresión booleana, se produce un error de sintaxis.  
  
 *true_value*  
 Valor que se devuelve si *boolean_expression* se evalúa como true.  
  
 *false_value*  
 Valor que se devuelve si *boolean_expression* se evalúa como false.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve el tipo de datos que tiene la prioridad más alta de los tipos de *true_value* y *false_value*. Para obtener más información, vea [Prioridad de tipo de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Comentarios  
 IIF es una manera abreviada para escribir una expresión CASE. Evalúa la expresión booleana pasada como primer argumento y devuelve cualquiera de los otros dos argumentos según el resultado de la evaluación. Es decir, se devuelve *true_value* si la expresión booleana es true y se devuelve *false_value* si la expresión booleana es false o desconocida. *true_value* y *false_value* pueden ser de cualquier tipo. Las mismas reglas que se aplican a la expresión CASE en las expresiones booleanas, el control de valores NULL y los tipos de valores devueltos también se aplican a IIF. Para más información, vea [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md).  
  
 El hecho de que IIF se traduzca a CASE también afecta a otros aspectos del comportamiento de esta función. Dado que las expresiones CASE solo se pueden anidar hasta 10 niveles, las instrucciones IIF también se pueden anidar únicamente hasta un máximo de 10. Además, IIF se envía de forma remota a otros servidores como una expresión CASE semánticamente equivalente, con todos los comportamientos de una expresión CASE remota.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-simple-iif-example"></a>A. Ejemplo sencillo de IIF  
  
```sql  
DECLARE @a INT = 45, @b INT = 40;  
SELECT IIF ( @a > @b, 'TRUE', 'FALSE' ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
TRUE  
  
(1 row(s) affected)  
```  
  
### <a name="b-iif-with-null-constants"></a>B. IIF con constantes NULL  
  
```sql 
SELECT IIF ( 45 > 30, NULL, NULL ) AS Result;  
```  
  
 El resultado de esta instrucción es un error.  
  
### <a name="c-iif-with-null-parameters"></a>C. IIF con parámetros NULL  
  
```sql  
DECLARE @P INT = NULL, @S INT = NULL;  
SELECT IIF ( 45 > 30, @p, @s ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
NULL  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CHOOSE &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  
