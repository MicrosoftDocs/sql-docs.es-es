---
description: DATETIME2FROMPARTS (Transact-SQL)
title: DATETIME2FROMPARTS (Transact-SQL)
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATETIME2FROMPARTS_TSQL
- DATETIME2FROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIME2FROMPARTS function
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cbf14683e7d501ef44ccaff757ace45f7045f304
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237932"
---
# <a name="datetime2fromparts-transact-sql"></a>DATETIME2FROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve un valor **datetime2** para los argumentos de fecha y hora especificados. El valor devuelto tiene una precisión especificada por el argumento de precisión.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DATETIME2FROMPARTS ( year, month, day, hour, minute, seconds, fractions, precision )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*year*  
Expresión entera que especifica un año.
  
*month*  
Expresión entera que especifica un mes.
  
*day*  
Expresión entera que especifica un día.
  
*hour*  
Expresión entera que especifica las horas.
  
*minute*  
Expresión entera que especifica los minutos.
  
*segundos*  
Expresión entera que especifica los segundos.
  
*fractions*  
Expresión entera que especifica un valor de fracciones de segundo.
  
*precisión*  
Expresión entera que especifica la precisión del valor **datetime2** que `DATETIME2FROMPARTS` va a devolver.
  
## <a name="return-types"></a>Tipos de valores devueltos
**datetime2(** *precision* **)**
  
## <a name="remarks"></a>Comentarios  
`DATETIME2FROMPARTS` devuelve un valor **datetime2** totalmente inicializado. `DATETIME2FROMPARTS` producirá un error si al menos uno de los argumentos obligatorios tiene un valor no válido. `DATETIME2FROMPARTS` devuelve NULL si al menos uno de los argumentos obligatorios tiene un valor NULL. Pero si el argumento *precision* tiene un valor NULL, `DATETIME2FROMPARTS` producirá un error.

El argumento *fractions* depende del argumento *precision*. Por ejemplo, para un valor *precision* de 7, cada fracción representa 100 nanosegundos; si *precision* es 3, cada fracción representa un milisegundo. Para un valor de *precision* de cero, el valor de *fractions* también debe ser cero; de lo contrario, `DATETIME2FROMPARTS` generará un error.
  
Esta función se puede enviar de forma remota a servidores de [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)] y superiores. No se puede enviar de forma remota a servidores que tengan una versión inferior a [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>A. Ejemplo sin fracciones de segundo  
  
```sql
SELECT DATETIME2FROMPARTS ( 2010, 12, 31, 23, 59, 59, 0, 0 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
---------------------------  
2010-12-31 23:59:59.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Ejemplo con fracciones de segundo  
En este ejemplo se muestra el uso de los parámetros *fractions* y *precision*:
  
1.  Cuando *fractions* tiene el valor 5 y *precision* el valor 1, el valor de *fractions* representa 5/10 de un segundo.  
  
2.  Cuando *fractions* tiene el valor 50 y *precision* el valor 2, el valor de *fractions* representa 50/100 de un segundo.  
  
3.  Cuando *fractions* tiene el valor 500 y *precision* tiene el valor 3, el valor de *fractions* representa 500/1000 de un segundo.  
  
```sql
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 5, 1 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 50, 2 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 500, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------------------  
2011-08-15 14:23:44.5  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.50  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.500  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también
[datetime2 &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime2-transact-sql.md)
  
  

