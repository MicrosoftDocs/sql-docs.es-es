---
description: DATENAME (Transact-SQL)
title: DATENAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATENAME_TSQL
- DATENAME
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATENAME function [SQL Server
- functions [SQL Server], time
- functions [SQL Server], date and time
- dateparts [SQL Server], datename
- date and time [SQL Server], DATENAME
- comparing dates times [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 11855b56-c554-495d-aad4-ba446990153b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2c6181880dec4b519e01d4006c94039164757476
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181839"
---
# <a name="datename-transact-sql"></a>DATENAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve una cadena de caracteres que representa el parámetro *datepart* especificado del argumento *date* especificado.

Vea [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) para obtener información general sobre todos los tipos de datos y las funciones de fecha y hora de [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DATENAME ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*datepart*  
La parte específica del argumento *date* que `DATENAME` va a devolver. En esta tabla se enumeran todos los argumentos válidos de *datepart*.

> [!NOTE]
> `DATENAME` no acepta los equivalentes de variables definidas por el usuario para los argumentos *datepart*.
  
|*datepart*|Abreviaturas|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**weekday**|**dw, w**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**TZoffset**|**tz**|  
|**ISO_WEEK**|**ISOWK, ISOWW**|  
  
*date*  

Una expresión que se puede resolver en uno de los tipos de datos siguientes: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Para *date*, `DATENAME` aceptará una expresión de columna, una expresión, un literal de cadena o una variable definida por el usuario. Para evitar problemas de ambigüedad, use años de cuatro dígitos. Vea [Establecer la opción de configuración del servidor Fecha límite de año de dos dígitos](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) para obtener información sobre los años de dos dígitos.
  
## <a name="return-type"></a>Tipo de valor devuelto  
**nvarchar**
  
## <a name="return-value"></a>Valor devuelto  
  
-   Cada *datepart* y sus abreviaturas devuelven el mismo valor.  
  
El valor devuelto depende del entorno del idioma definido mediante [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) y la opción de configuración de servidor [Configurar el idioma predeterminado](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) del inicio de sesión. El valor devuelto depende de [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) si *date* es un literal de cadena de ciertos formatos. SET DATEFORMAT no cambia el valor devuelto cuando la fecha es una expresión de columna de un tipo de datos de hora o fecha.
  
Cuando el parámetro *date* tiene un argumento de tipo de datos **date**, el valor devuelto depende de la configuración especificada mediante [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md).
  
## <a name="tzoffset-datepart-argument"></a>Argumento datepart TZoffset  
Si el argumento *datepart* es **TZoffset** (**tz**) y el argumento *date* no tiene desplazamiento de zona horaria, `DATEADD` devuelve 0.
  
## <a name="smalldatetime-date-argument"></a>Argumento date smalldatetime  
Si *date* es [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md), `DATENAME` devuelve los segundos como 00.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-the-date-argument"></a>Valor predeterminado devuelto por datepart que no se encuentra en el argumento date  
Si el tipo de datos del argumento *date* no contiene el parámetro *datepart* especificado, `DATENAME` devolverá el valor predeterminado *datepart* solo si el argumento *date* tiene un valor literal.
  
Por ejemplo, el valor predeterminado de año-mes-día de cualquier tipo de datos **date** es 1900-01-01. Esta instrucción tiene argumentos de la parte de fecha para *datepart*, un argumento de hora para *date* y `DATENAME` devuelve `1900, January, 1, 1, Monday`.
  
```sql
SELECT DATENAME(year, '12:10:30.123')  
    ,DATENAME(month, '12:10:30.123')  
    ,DATENAME(day, '12:10:30.123')  
    ,DATENAME(dayofyear, '12:10:30.123')  
    ,DATENAME(weekday, '12:10:30.123');  
```  
  
Si *date* se especifica como variable o columna de tabla, y el tipo de datos de esa variable o columna no tiene especificado *datepart*, `DATENAME` devuelve el error 9810. En este ejemplo, la variable *\@t* tiene un tipo de datos **time**. Se produce un error en el ejemplo porque el año de la parte de fecha no es válido para el tipo de datos **time**:
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATENAME(year, @t);  
```  
  
## <a name="remarks"></a>Observaciones  

Use `DATENAME` en las cláusulas siguientes:

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], DATENAME convierte implícitamente los literales de cadena como un tipo **datetime2**. Esto significa que `DATENAME` no admite el formato año-día-mes cuando la fecha se pasa como una cadena. La cadena se debe convertir explícitamente a un tipo **datetime** o **smalldatetime** para poder usar el formato año-día-mes.
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se devuelven las partes de fecha para la fecha especificada. Sustituya un valor *datepart* de la tabla para el argumento `datepart` en la instrucción SELECT:
  
`SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');`
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Valor devuelto|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Octubre|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Martes|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

En este ejemplo se devuelven las partes de fecha para la fecha especificada. Sustituya un valor *datepart* de la tabla para el argumento `datepart` en la instrucción SELECT:
  
```sql
SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Valor devuelto|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Octubre|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Martes|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
## <a name="see-also"></a>Consulte también
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

