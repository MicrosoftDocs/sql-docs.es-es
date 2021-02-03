---
description: RADIANS (Transact-SQL)
title: RADIANS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RADIANS
- RADIANS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RADIANS function
ms.assetid: e9f69951-ecda-45d9-8909-dcb716b1b1c0
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 698ff09cf6d597a727408f4c3550f7916eaa0c79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182362"
---
# <a name="radians-transact-sql"></a>RADIANS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve los radianes de una expresión numérica en grados.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
RADIANS ( numeric_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *numeric_expression*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de la categoría de tipos de datos numérico exacto o numérico aproximado, excepto para el tipo de datos **bit**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve el mismo tipo que *numeric_expression*.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-radians-to-show-00"></a>A. Utilizar RADIANS para mostrar 0.0  
 En este ejemplo se devuelve el resultado `0.0` debido a que la expresión numérica que se va a convertir en radianes es demasiado pequeña para la función `RADIANS`.  
  
```sql  
SELECT RADIANS(1e-307)  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------   
0.0                        
(1 row(s) affected)  
```  
  
### <a name="b-using-radians-to-return-the-equivalent-angle-of-a-float-expression"></a>B. Utilizar RADIANS para devolver el ángulo equivalente de una expresión float  
 En este ejemplo se toma una expresión `float` y se devuelve el valor `RADIANS` del ángulo especificado.  
  
```sql  
-- First value is -45.01.  
DECLARE @angle FLOAT  
SET @angle = -45.01  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Next value is -181.01.  
DECLARE @angle FLOAT  
SET @angle = -181.01  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Next value is 0.00.  
DECLARE @angle FLOAT  
SET @angle = 0.00  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Next value is 0.1472738.  
DECLARE @angle FLOAT  
SET @angle = 0.1472738  
SELECT 'The RADIANS of the angle is: ' +  
    CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Last value is 197.1099392.  
DECLARE @angle FLOAT  
SET @angle = 197.1099392  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------------------------------------   
The RADIANS of the angle is: -0.785573                        
(1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: -3.15922                         
(1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: 0                                
(1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: 0.00257041                       
 (1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: 3.44022                          
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vea también  
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [decimal y numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)   
 [float y real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md)   
 [int, bigint, smallint y tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)   
 [Funciones matemáticas &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [money y smallmoney &#40;Transact-SQL&#41;](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)  
  
  

