---
description: Funciones matemáticas (Transact-SQL)
title: Funciones matemáticas (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- calculations [SQL Server]
- mathematical functions [SQL Server]
- functions [SQL Server], mathematical
ms.assetid: 46495a2e-81d0-4677-9d72-9db083cd1023
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2529117d5087a083fc32da30689129dd49733907
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195991"
---
# <a name="mathematical-functions-transact-sql"></a>Funciones matemáticas (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Las siguientes funciones escalares realizan un cálculo, normalmente basado en valores de entrada proporcionados como argumentos, y devuelven un valor numérico:  
  
- [ABS](../../t-sql/functions/abs-transact-sql.md)
- [ACOS](../../t-sql/functions/acos-transact-sql.md)
- [ASIN](../../t-sql/functions/asin-transact-sql.md)
- [ATAN](../../t-sql/functions/atan-transact-sql.md)
- [ATN2](../../t-sql/functions/atn2-transact-sql.md)
- [CEILING](../../t-sql/functions/ceiling-transact-sql.md)
- [COS](../../t-sql/functions/cos-transact-sql.md)
- [COT](../../t-sql/functions/cot-transact-sql.md)
- [DEGREES](../../t-sql/functions/degrees-transact-sql.md)
- [EXP](../../t-sql/functions/exp-transact-sql.md)
- [FLOOR](../../t-sql/functions/floor-transact-sql.md)
- [LOG](../../t-sql/functions/log-transact-sql.md)
- [LOG10](../../t-sql/functions/log10-transact-sql.md)
- [PI](../../t-sql/functions/pi-transact-sql.md)
- [POWER](../../t-sql/functions/power-transact-sql.md)
- [RADIANS](../../t-sql/functions/radians-transact-sql.md)  
- [RAND](../../t-sql/functions/rand-transact-sql.md)  
- [ROUND](../../t-sql/functions/round-transact-sql.md)  
- [SIGN](../../t-sql/functions/sign-transact-sql.md)  
- [SIN](../../t-sql/functions/sin-transact-sql.md)  
- [SQRT](../../t-sql/functions/sqrt-transact-sql.md)  
- [SQUARE](../../t-sql/functions/square-transact-sql.md)  
- [TAN](../../t-sql/functions/tan-transact-sql.md)
  
> [!NOTE]  
> Las funciones aritméticas, como ABS, CEILING, DEGREES, FLOOR, POWER, RADIANS y SIGN, devuelven un valor del mismo tipo de datos que el valor de entrada. Las funciones trigonométricas y otras funciones, incluidas EXP, LOG, LOG10, SQUARE y SQRT, convierten sus valores de entrada a **float** y devuelven un valor de tipo **float**.  
  
Todas las funciones matemáticas, excepto RAND, son funciones deterministas. Esto significa que devuelven los mismos resultados cada vez que se llaman con un conjunto específico de valores de entrada. RAND es determinista solo cuando se especifica un parámetro de inicialización. Para más información sobre el determinismo de las funciones, vea [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="see-also"></a>Consulte también

- [&#40;Transact-SQL&#41;](../../t-sql/language-elements/arithmetic-operators-transact-sql.md)
- [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)  
