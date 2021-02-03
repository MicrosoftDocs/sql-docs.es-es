---
title: DATEPART (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para la función DATEPART. Esta función devuelve un entero que se corresponde con el parámetro datepart de una fecha especificada.
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEPART_TSQL
- DATEPART
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], DATEPART
- dateparts [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dateparts [SQL Server], datepart
- comparing dates times [SQL Server]
- DATEPART function [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 15f1a5bc-4c0c-4c48-848d-8ec03473e6c1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 610697bdc65acf22418d8c31c72b3e1a1d294693
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181832"
---
# <a name="datepart-transact-sql"></a>DATEPART (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Esta función devuelve un entero que representa el parámetro *datepart* especificado del parámetro *date* especificado.
  
Vea [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) para obtener información general sobre todos los tipos de datos y las funciones de fecha y hora de [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DATEPART ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*datepart*  
La parte específica del argumento *date* para el que `DATEPART` va a devolver un valor **integer**. En esta tabla se enumeran todos los argumentos válidos de *datepart*.

> [!NOTE]
> `DATEPART` no acepta los equivalentes de variables definidas por el usuario para los argumentos *datepart*.
  
|*datepart*|Abreviaturas|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**tzoffset**|**tz**|  
|**iso_week**|**isowk**, **isoww**|  
  
*date*  
Una expresión que se resuelve en uno de los tipos de datos siguientes: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Para *date*, `DATEPART` aceptará una expresión de columna, una expresión, un literal de cadena o una variable definida por el usuario. Para evitar problemas de ambigüedad, use años de cuatro dígitos. Vea [Establecer la opción de configuración del servidor Fecha límite de año de dos dígitos](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) para obtener información sobre los años de dos dígitos.
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **int**  
  
## <a name="return-value"></a>Valor devuelto  
Cada *datepart* y sus abreviaturas devuelven el mismo valor.
  
El valor devuelto depende del entorno del idioma definido mediante [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) y la opción de configuración de servidor [Configurar el idioma predeterminado](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) del inicio de sesión. El valor devuelto depende de [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) si *date* es un literal de cadena de ciertos formatos. SET DATEFORMAT no cambia el valor devuelto cuando la fecha es una expresión de columna de un tipo de datos de hora o fecha.
  
En esta tabla se enumeran todos los argumentos *datepart*, con los correspondientes valores devueltos para la instrucción `SELECT DATEPART(datepart,'2007-10-30 12:15:32.1234567 +05:10')`. El argumento *date* tiene un tipo de datos **datetimeoffset(7)** . Las dos últimas posiciones del valor devuelto *datepart* **nanosecond** siempre son `00` y este valor tiene una escala de 9:

**0,123456700**
  
|*datepart*|Valor devuelto|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|10|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|3|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**tzoffset, tz**|310|  
|**iso_week, isowk, isoww**|44|  
  
## <a name="week-and-weekday-datepart-arguments"></a>Argumentos de la parte de fecha semana y día de la semana
Si *datepart* es **week** (**wk**, **ww**) o **weekday** (**dw**), el valor devuelto de `DATEPART` depende del valor establecido mediante [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md).
  
El 1 de enero de todos los años es el número de inicio para _datepart_ **week**. Por ejemplo:

DATEPART (**wk**, "Jan 1, *xxx* x") = 1

donde *xxxx* es cualquier año.
  
En esta tabla se muestran los valores devueltos para *datepart* **week** y **weekday** para "2007-04-21" para cada argumento de SET DATEFIRST. El 1 de enero de 2007 es lunes. El 21 de abril de 2007 es un sábado. Para Inglés de EE.UU.,

`SET DATEFIRST 7 -- ( Sunday )`

sirve como valor predeterminado. Después de establecer DATEFIRST, use esta instrucción SQL sugerida para los valores de tabla de datepart:

`SELECT DATEPART(week, '2007-04-21 '), DATEPART(weekday, '2007-04-21 ')`
  
|SET DATEFIRST<br /><br /> Argumento|week<br /><br /> devuelto|weekday<br /><br /> devuelto|  
|---|---|---|
|1|16|6|  
|2|17|5|  
|3|17|4|  
|4|17|3|  
|5|17|2|  
|6|17|1|  
|7|16|7|  
  
## <a name="year-month-and-day-datepart-arguments"></a>Argumentos de datepart year, month y day  
Los valores devueltos para DATEPART (**year**, *date*), DATEPART (**month**, *date*) y DATEPART (**day**, *date*) son los mismos que los que devuelven las funciones [YEAR](../../t-sql/functions/year-transact-sql.md), [MONTH](../../t-sql/functions/month-transact-sql.md) y [DAY](../../t-sql/functions/day-transact-sql.md), respectivamente.
  
## <a name="iso_week-datepart"></a>iso_week datepart  
ISO 8601 incluye el sistema ISO de fecha-semana, un sistema de numeración para las semanas. Cada semana se asocia al año en el que cae el jueves. Por ejemplo, la semana 1 de 2004 (2004W01) abarcaba del lunes 29 de diciembre de 2003 al domingo 4 de enero de 2004. En las regiones y países Europeos normalmente se usa este estilo de numeración. En las regiones y países que no son Europaos normalmente no se usa.

Nota: El número más alto de la semana en un año puede ser 52 o 53.
  
Es posible que los sistemas de numeración que se usan en otros países o regiones no se ajusten a las normas ISO. En esta tabla se muestran seis posibilidades:
  
|Primer día de la semana|La primera semana del año contiene|Semanas asignadas dos veces|Usado por/en|  
|---|---|---|---|
|Domingo|1 de enero,<br /><br /> El primer sábado,<br /><br /> 1-7 días del año|Sí|Estados Unidos|  
|Lunes|1 de enero,<br /><br /> El primer domingo,<br /><br /> 1-7 días del año|Sí|La mayoría de los países Europeos y Reino Unido|  
|Lunes|4 de enero,<br /><br /> El primer jueves,<br /><br /> 4-7 días del año|No|ISO 8601, Noruega y Suecia|  
|Lunes|7 de enero,<br /><br /> El primer lunes,<br /><br /> 7 días del año|No||  
|Miércoles|1 de enero,<br /><br /> El primer martes,<br /><br /> 1-7 días del año|Sí||  
|Sábado|1 de enero,<br /><br /> El primer viernes,<br /><br /> 1-7 días del año|Sí||  
  
## <a name="tzoffset"></a>tzoffset  
`DATEPART` devuelve el valor **tzoffset** (**tz**) como el número de minutos (con signo). Esta instrucción devuelve un desplazamiento de zona horaria de 310 minutos:
  
```sql
SELECT DATEPART (tzoffset, '2007-05-10  00:00:01.1234567 +05:10');  
```  
`DATEPART` representa el valor tzoffset de esta forma:
- Para datetimeoffset y datetime2, tzoffset devuelve el desplazamiento de tiempo en minutos, donde el desplazamiento de datetime2 siempre es 0 minutos.
- Para los tipos de datos que se pueden convertir implícitamente en **datetimeoffset** o **datetime2**, `DATEPART` devuelve el desplazamiento de tiempo en minutos. Excepción: otros tipos de datos de fecha y hora.
- El resto de tipos de parámetros producirán un error.
  
  
## <a name="smalldatetime-date-argument"></a>Argumento date smalldatetime  
Para un valor [date](../../t-sql/data-types/smalldatetime-transact-sql.md) *smalldatetime*, `DATEPART` devuelve los segundos como 00.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-a-date-argument"></a>Valor predeterminado devuelto por una parte de fecha que no se encuentra en un argumento date  
Si el tipo de datos del argumento *date* no contiene el parámetro *datepart* especificado, `DATEPART` devolverá el valor predeterminado para ese parámetro *datepart* solo cuando se especifique un valor literal para *date*.
  
Por ejemplo, el valor predeterminado de año-mes-día de cualquier tipo de datos **date** es 1900-01-01. Esta instrucción tiene argumentos de la parte de fecha para *datepart*, un argumento de hora para *date* y devuelve `1900, 1, 1, 1, 2`.
  
```sql
SELECT DATEPART(year, '12:10:30.123')  
    ,DATEPART(month, '12:10:30.123')  
    ,DATEPART(day, '12:10:30.123')  
    ,DATEPART(dayofyear, '12:10:30.123')  
    ,DATEPART(weekday, '12:10:30.123');  
```  
  
Si *date* se especifica como variable o columna de tabla, y el tipo de datos de esa variable o columna no tiene especificado *datepart*, `DATEPART` devuelve el error 9810. En este ejemplo, la variable *\@t* tiene un tipo de datos **time**. Se produce un error en el ejemplo porque el año de la parte de fecha no es válido para el tipo de datos **time**:
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATEPART(year, @t);  
```  
  
## <a name="fractional-seconds"></a>Fracciones de segundo
En estas instrucciones se muestra que `DATEPART` devuelve las fracciones de segundo:
  
```sql
SELECT DATEPART(millisecond, '00:00:01.1234567'); -- Returns 123  
SELECT DATEPART(microsecond, '00:00:01.1234567'); -- Returns 123456  
SELECT DATEPART(nanosecond,  '00:00:01.1234567'); -- Returns 123456700  
```  
  
## <a name="remarks"></a>Observaciones  
`DATEPART` se puede usar en las cláusulas SELECT list, WHERE, HAVING, GROUP BY y ORDER BY.
  
DATEPART convierte de forma implícita los literales de cadena como un tipo **datetime2** en [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Esto significa que DATENAME no admite el formato año-día-mes cuando se pasa la fecha como cadena. La cadena se debe convertir explícitamente a un tipo **datetime** o **smalldatetime** para poder usar el formato año-día-mes.
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se devuelve el año base. El año base ayuda con los cálculos de fecha. En el ejemplo, un número especifica la fecha. Observe que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interpreta 0 como el 1 de enero de 1900.
  
```sql
SELECT DATEPART(year, 0), DATEPART(month, 0), DATEPART(day, 0);  

-- Returns: 1900    1    1 
```  
  
En este ejemplo se devuelve el día de la fecha `12/20/1974`.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (day,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 20
```  
  
En este ejemplo se devuelve el año de la fecha `12/20/1974`.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (year,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 1974
```  
  
## <a name="see-also"></a>Consulte también
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
