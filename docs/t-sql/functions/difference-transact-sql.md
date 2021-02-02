---
description: DIFFERENCE (Transact-SQL)
title: DIFFERENCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DIFFERENCE
- DIFFERENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DIFFERENCE function
- comparing SOUNDEX values
- SOUNDEX values
ms.assetid: c58ca25d-d6ea-48fa-93bb-c9374b0b2a2b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1829007e42e955b900ca885cd0ed01d43b1ca0c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158387"
---
# <a name="difference-transact-sql"></a>DIFFERENCE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve un valor entero que mide la diferencia entre los valores de [SOUNDEX()](./soundex-transact-sql.md) de dos expresiones de caracteres diferentes.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DIFFERENCE ( character_expression , character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*character_expression*  
Una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) alfanumérica de datos de caracteres. *character_expression* puede ser una constante, una variable o una columna.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
**int**  
 
## <a name="remarks"></a>Observaciones  
`DIFFERENCE` compara dos valores `SOUNDEX` diferentes y devuelve un valor entero. Este valor mide el grado de coincidencia de los valores `SOUNDEX`, en una escala de 0 a 4. Un valor de 0 indica una similitud escasa o nula entre los valores de SOUNDEX; 4 indica que los valores son muy similares o incluso idénticos.  
  
`DIFFERENCE` y `SOUNDEX` tienen distinción de intercalación.  
  
## <a name="examples"></a>Ejemplos  
En la primera parte de este ejemplo se comparan los valores `SOUNDEX` de dos cadenas muy similares. En una intercalación de Latin1_General, `DIFFERENCE` devuelve un valor de `4`. En la segunda parte del ejemplo se comparan los valores de `SOUNDEX` de dos cadenas muy diferentes y, en el caso de una intercalación de Latin1_General, `DIFFERENCE` devuelve un valor de `0`.  
  
```sql  
-- Returns a DIFFERENCE value of 4, the least possible difference.  
SELECT SOUNDEX('Green'), SOUNDEX('Greene'), DIFFERENCE('Green','Greene');  
GO  
-- Returns a DIFFERENCE value of 0, the highest possible difference.  
SELECT SOUNDEX('Blotchet-Halls'), SOUNDEX('Greene'), DIFFERENCE('Blotchet-Halls', 'Greene');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----- ----- -----------   
G650  G650  4             
  
(1 row(s) affected)  
  
----- ----- -----------   
B432  G650  0             
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vea también  
 [SOUNDEX &#40;Transact-SQL&#41;](../../t-sql/functions/soundex-transact-sql.md)   
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

