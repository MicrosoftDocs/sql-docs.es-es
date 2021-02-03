---
title: DATEDIFF_BIG (Transact-SQL)
description: DATEDIFF_BIG (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEDIFF_BIG
- DATEDIFF_BIG_TSQL
helpviewer_keywords:
- DATEDIFF_BIG function [SQL Server]
- dates [SQL Server]. functions
- date and time [SQL Server], DATEDIFF_BIG
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 19ac1693-3cfa-400d-bf83-20a9cb46599a
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 01/12/2021
ms.openlocfilehash: 2f82810525576e05d30bc8c68f74547014e4a788
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183529"
---
# <a name="datediff_big-transact-sql"></a>DATEDIFF_BIG (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Esta función devuelve el recuento (como un valor entero grande con firma) de los límites *datepart* que se han cruzado entre los valores *startdate* y *enddate* especificados.
  
Vea [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) para obtener información general sobre todos los tipos de datos y las funciones de fecha y hora de [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DATEDIFF_BIG ( datepart , startdate , enddate )  
```  

## <a name="arguments"></a>Argumentos
*datepart*  
La parte de *startdate* y *enddate* que especifica el tipo de límite cruzado.

> [!NOTE]
> `DATEDIFF_BIG` no aceptará valores *datepart* de variables definidas por el usuario ni como cadenas entre comillas.

En esta tabla se enumeran todos los nombres y las abreviaturas de los argumentos válidos de *datepart*.
  
|nombre de *datepart*| abreviatura de *datepart*|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  

> [!NOTE]
> Todos los nombres y las abreviaturas específicas de *datepart* para ese nombre de *datepart* devolverán el mismo valor.

*startdate*  
Una expresión que se puede resolver en uno de los valores siguientes:

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Para *date*, `DATEDIFF_BIG` aceptará una expresión de columna, una expresión, un literal de cadena o una variable definida por el usuario. Un valor de literal de cadena se debe resolver en un argumento **datetime**. Para evitar problemas de ambigüedad, use años de cuatro dígitos. `DATEDIFF_BIG` resta *startdate* de *enddate*. Para evitar ambigüedades, use años de cuatro dígitos. Vea [Establecer la opción de configuración del servidor Fecha límite de año de dos dígitos](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) para obtener información sobre los años de dos dígitos.
  
*enddate*  
Vea *startdate*.
  
## <a name="return-type"></a>Tipo de valor devuelto  
**bigint** con firma  
  
## <a name="return-value"></a>Valor devuelto  
Devuelve la diferencia **bigint** entre *startdate* y *enddate*, expresada en el límite establecido por *datepart*.
  
Para un valor devuelto fuera del intervalo de **bigint** (de -9.223.372.036.854.775.808 a 9.223.372.036.854.775.807) `DATEDIFF_BIG` devuelve un error. A diferencia de **, que devuelve un valor** int **y, por tanto, se puede desbordar con un valor** minute`DATEDIFF_BIG` o superior, **solo se puede desbordar si se usa la precisión de** nanosecond *, donde la diferencia entre* enddate *y* startdate es superior a 292 años, 3 meses, 10 días, 23 horas, 47 minutos y 16,8547758 segundos.
  
Si *startdate* y *enddate* solo tienen asignado un valor de hora y *datepart* no es un valor *datepart* de hora, `DATEDIFF_BIG` devuelve 0.
  
`DATEDIFF_BIG` no usa un componente de desplazamiento de zona horaria de *startdate* o *enddate* para calcular el valor devuelto.
  
Para un valor **smalldatetime** que se use para *startdate* o *enddate*, `DATEDIFF_BIG` siempre establece los segundos y milisegundos en 0 en el valor devuelto porque [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) solo es preciso hasta los minutos.
  
Si solo se asigna un valor de hora a una variable de tipo de datos de fecha, `DATEDIFF_BIG` establece el valor de la parte de la fecha que falta en el valor predeterminado: `1900-01-01`. Si solo se asigna un valor de fecha a una variable de tipo de datos de fecha u hora, `DATEDIFF_BIG` establece el valor de la parte de la hora que falta en el valor predeterminado: `00:00:00`. Si *startdate* o *enddate* solo tienen una parte de hora y el otro solo una parte de fecha, `DATEDIFF_BIG` establece las partes de hora y fecha que faltan en los valores predeterminados.
  
Si *startdate* y *enddate* tienen tipos de datos de fecha diferentes y uno tiene más partes de hora o precisión de fracciones de segundo que el otro, `DATEDIFF_BIG` establece las partes que faltan del otro en 0.
  
## <a name="datepart-boundaries"></a>Límites de datepart
Las instrucciones siguientes tienen los mismos valores *startdate* y *enddate*. Esas fechas son adyacentes y tienen una diferencia horaria de un microsegundo (0,0000001 segundos). La diferencia entre *startdate* y *enddate* en cada instrucción cruza un límite de calendario u hora de su *datepart*. Cada instrucción devuelve 1. Si *startdate* y *enddate* tienen valores de año diferentes, pero tienen los mismos valores de semana del calendario, `DATEDIFF_BIG` devolverán 0 para *datepart* **week**.

```sql
SELECT DATEDIFF_BIG(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(dayofyear,   '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(hour,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(minute,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(second,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```
  
## <a name="remarks"></a>Observaciones  
Use `DATEDIFF_BIG` en las cláusulas `SELECT <list>`, `WHERE`, `HAVING`, `GROUP BY` y `ORDER BY`.
  
`DATEDIFF_BIG` convierte implícitamente los literales de cadena como un tipo **datetime2**. Esto significa que `DATEDIFF_BIG` no admite el formato año-día-mes cuando la fecha se pasa como una cadena. La cadena se debe convertir explícitamente a un tipo **datetime** o **smalldatetime** para poder usar el formato año-día-mes.
  
La especificación de `SET DATEFIRST` no tiene ningún efecto sobre `DATEDIFF_BIG`. `DATEDIFF_BIG` siempre usa el domingo como el primer día de la semana para garantizar que la función actúa de forma determinista.

`DATEDIFF_BIG` se puede desbordar con un valor de **nanosecond** si la diferencia entre *enddate* y *startdate* devuelve un valor que está fuera del intervalo para **bigint**.
  
## <a name="examples"></a>Ejemplos 
  
### <a name="specifying-columns-for-startdate-and-enddate"></a>Especificar las columnas para startdate y enddate  
En este ejemplo se usan otros tipos de expresiones como argumentos para los parámetros *startdate* y *enddate*. Calcula el número de límites de día que se cruzan entre las fechas en dos columnas de una tabla.
  
```sql
CREATE TABLE dbo.Duration  
    (startDate datetime2, endDate datetime2);  
    
INSERT INTO dbo.Duration(startDate,endDate)  
    VALUES('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT DATEDIFF_BIG(day, startDate, endDate) AS 'Duration'  
    FROM dbo.Duration;  
-- Returns: 1  
```  

### <a name="finding-difference-between-startdate-and-enddate-as-date-parts-strings"></a>Búsqueda de la diferencia entre startdate y enddate como cadenas de partes de fecha

```sql
DECLARE @date1 DATETIME2, @date2 DATETIME2, @result VARCHAR(100)
DECLARE @years BIGINT, @months BIGINT, @days BIGINT, @hours BIGINT, @minutes BIGINT, @seconds BIGINT, @milliseconds BIGINT

SET @date1 = '0001-01-01 00:00:00.00000000'
SET @date2 = '2018-12-12 07:08:01.12345678'

SELECT @years = DATEDIFF(yy, @date1, @date2)
IF DATEADD(yy, -@years, @date2) < @date1 
SELECT @years = @years-1
SET @date2 = DATEADD(yy, -@years, @date2)

SELECT @months = DATEDIFF(mm, @date1, @date2)
IF DATEADD(mm, -@months, @date2) < @date1 
SELECT @months=@months-1
SET @date2= DATEADD(mm, -@months, @date2)

SELECT @days=DATEDIFF(dd, @date1, @date2)
IF DATEADD(dd, -@days, @date2) < @date1 
SELECT @days=@days-1
SET @date2= DATEADD(dd, -@days, @date2)

SELECT @hours=DATEDIFF(hh, @date1, @date2)
IF DATEADD(hh, -@hours, @date2) < @date1 
SELECT @hours=@hours-1
SET @date2= DATEADD(hh, -@hours, @date2)

SELECT @minutes=DATEDIFF(mi, @date1, @date2)
IF DATEADD(mi, -@minutes, @date2) < @date1 
SELECT @minutes=@minutes-1
SET @date2= DATEADD(mi, -@minutes, @date2)

SELECT @seconds=DATEDIFF(s, @date1, @date2)
IF DATEADD(s, -@seconds, @date2) < @date1 
SELECT @seconds=@seconds-1
SET @date2= DATEADD(s, -@seconds, @date2)

SELECT @milliseconds=DATEDIFF(ms, @date1, @date2)

SELECT @result= ISNULL(CAST(NULLIF(@years,0) AS VARCHAR(10)) + ' years,','')
     + ISNULL(' ' + CAST(NULLIF(@months,0) AS VARCHAR(10)) + ' months,','')    
     + ISNULL(' ' + CAST(NULLIF(@days,0) AS VARCHAR(10)) + ' days,','')
     + ISNULL(' ' + CAST(NULLIF(@hours,0) AS VARCHAR(10)) + ' hours,','')
     + ISNULL(' ' + CAST(@minutes AS VARCHAR(10)) + ' minutes and','')
     + ISNULL(' ' + CAST(@seconds AS VARCHAR(10)) 
          + CASE WHEN @milliseconds > 0 THEN '.' + CAST(@milliseconds AS VARCHAR(10)) 
               ELSE '' END 
          + ' seconds','')

SELECT @result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 

```
2017 years, 11 months, 11 days, 7 hours, 8 minutes and 1.123 seconds
```   

Vea ejemplos más estrechamente relacionados en [DATEDIFF &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-transact-sql.md).
  
## <a name="see-also"></a>Consulte también
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[DATEDIFF &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-transact-sql.md)
  
  
