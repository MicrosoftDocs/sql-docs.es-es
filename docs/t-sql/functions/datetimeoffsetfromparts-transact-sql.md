---
description: DATETIMEOFFSETFROMPARTS (Transact-SQL)
title: DATETIMEOFFSETFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATETIMEOFFSETFROMPARTS_TSQL
- DATETIMEOFFSETFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEOFFSETFROMPARTS function
ms.assetid: 463da1f4-b4b6-45a3-9a95-ea1f99575542
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9f8ce697a414d095e91214626b4ae4f8e1e2a83b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237920"
---
# <a name="datetimeoffsetfromparts-transact-sql"></a>DATETIMEOFFSETFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve un valor **datetimeoffset** para los argumentos de fecha y hora especificados. El valor devuelto tiene una precisión especificada por el argumento precision y un desplazamiento especificado por los argumentos de desplazamiento.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DATETIMEOFFSETFROMPARTS ( year, month, day, hour, minute, seconds, fractions, hour_offset, minute_offset, precision )  
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
  
*hour_offset*  
Expresión entera que especifica la parte de hora del desplazamiento de zona horaria.  
  
*minute_offset*  
Expresión entera que especifica la parte de los minutos del desplazamiento de zona horaria.  
  
*precisión*  
Valor literal entero que especifica la precisión del valor **datetimeoffset** que `DATETIMEOFFSETFROMPARTS` va a devolver.  
  
## <a name="return-types"></a>Tipos de valores devueltos
**datetimeoffset(** *precision* **)**  
  
## <a name="remarks"></a>Comentarios  

`DATETIMEOFFSETFROMPARTS` devuelve un tipo de datos **datetimeoffset** totalmente inicializado. Los argumentos de desplazamiento representan el desplazamiento de zona horaria. En el caso de los argumentos de desplazamiento omitidos, `DATETIMEOFFSETFROMPARTS` supone un desplazamiento de zona horaria de `00:00`; en otras palabras, no hay ningún desplazamiento de zona horaria. En el caso de los argumentos de desplazamiento especificados, `DATETIMEOFFSETFROMPARTS` espera valores para ambos argumentos y ambos valores positivos o negativos. Si *minute_offset* tiene un valor y *hour_offset* no tiene ningún valor, `DATETIMEOFFSETFROMPARTS` generará un error. `DATETIMEOFFSETFROMPARTS` generará un error si los demás argumentos tienen valores no válidos. Si al menos uno de los argumentos requeridos tiene un valor `NULL`, `DATETIMEOFFSETFROMPARTS` va a devolver `NULL`. Pero si el argumento *precision* tiene un valor `NULL`, `DATETIMEOFFSETFROMPARTS` generará un error.  
  
El argumento *fractions* depende del argumento precision. Por ejemplo, para un valor precision de 7, cada fracción representa 100 nanosegundos; si precision es 3, cada fracción representa un milisegundo. Para un valor de precision de cero, el valor de fractions también debe ser cero; de lo contrario, `DATETIMEOFFSETFROMPARTS` generará un error.  
  
Esta función se puede enviar de forma remota a servidores de [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)] y superiores. No se puede enviar de forma remota a servidores que tengan una versión inferior a [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>A. Ejemplo sin fracciones de segundo  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2010, 12, 31, 14, 23, 23, 0, 12, 0, 7 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
----------------------------------
2010-12-31 14:23:23.0000000 +12:00  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Ejemplo con fracciones de segundo  

En este ejemplo se muestra el uso de los parámetros *fractions* y *precision*:  

1. Cuando *fractions* tiene el valor 5 y *precision* el valor 1, el valor de *fractions* representa 5/10 de un segundo.  

2. Cuando *fractions* tiene el valor 50 y *precision* el valor 2, el valor de *fractions* representa 50/100 de un segundo.  

3. Cuando *fractions* tiene el valor 500 y *precision* tiene el valor 3, el valor de *fractions* representa 500/1000 de un segundo.  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 5, 12, 30, 1 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 50, 12, 30, 2 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 500, 12, 30, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------------------------------  
2011-08-15 14:30:00.5 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.50 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.500 +12:30  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también
[datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  


