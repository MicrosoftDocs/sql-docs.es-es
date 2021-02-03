---
title: DATEADD (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para la función DATEADD. Esta función devuelve una fecha modificada por la parte de fecha especificada.
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEADD
- DATEADD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- add interval to date or time [SQL Server]
- subtract interval from date or time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], intervals
- date and time [SQL Server], DATEADD
- DATEADD function [SQL Server]
ms.assetid: 89c5ae32-89c6-47e1-979e-15d97908b9f1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cd0ef9cc5cae47ca5ce42ff090a6fb794674131e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183547"
---
# <a name="dateadd-transact-sql"></a>DATEADD (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función agrega un valor *number* especificado (como un entero con signo) a un atributo *datepart* determinado de un valor *date* de entrada y, después, devuelve el valor modificado.
  
Vea [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) para obtener información general sobre todos los tipos de datos y las funciones de fecha y hora de [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```sql
DATEADD (datepart , number , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*datepart*  
La parte de la *fecha* a la que `DATEADD` agrega un **número** *entero*. En esta tabla se enumeran todos los argumentos válidos de *datepart*. 

> [!NOTE]
> `DATEADD` no acepta los equivalentes de variables definidas por el usuario para los argumentos *datepart*. 
  
|*datepart*|Abreviaturas|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**, **w**|  
|**hour**|**hh**|  
|**minute**|**mi**, **n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
  
*número*  
Expresión que se puede resolver como un valor [int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md) que `DATEADD` suma a un atributo *datepart* de *date*. `DATEADD` acepta valores de variables definidas por el usuario para *number*. `DATEADD` truncará un valor *number* especificado que tiene una fracción decimal. En esta situación no se redondeará el valor *number*.
  
*date*  
Una expresión que se puede resolver en uno de los valores siguientes: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Para *date*, `DATEADD` aceptará una expresión de columna, una expresión, un literal de cadena o una variable definida por el usuario. Un valor de literal de cadena se debe resolver en un argumento **datetime**. Para evitar problemas de ambigüedad, use años de cuatro dígitos. Vea [Establecer la opción de configuración del servidor Fecha límite de año de dos dígitos](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) para obtener información sobre los años de dos dígitos.
  
## <a name="return-types"></a>Tipos de valores devueltos

El tipo de datos de valores devueltos de este método es dinámico. El tipo devuelto depende del argumento proporcionado para `date`. Si el valor de `date` es una fecha de literal de cadena, `DATEADD` devuelve un valor **datetime**. Si se proporciona otro tipo válido de datos de entrada para `date`, `DATEADD` devuelve el mismo tipo de datos. `DATEADD` produce un error si la escala de segundos del literal de cadena supera tres posiciones decimales (.nnn) o si el literal de cadena contiene la parte de desplazamiento de zona horaria.
  
## <a name="return-value"></a>Valor devuelto  
  
## <a name="datepart-argument"></a>Argumento datepart  
**dayofyear**, **day** y **weekday** devuelven el mismo valor.
  
Cada *datepart* y sus abreviaturas devuelven el mismo valor.
  
Si se cumple lo siguiente:

+ *datepart* es **month**
+ el mes de *date* tiene más días que el mes que se devuelve
+ el día de *date* no existe en el mes que se devuelve

Después, `DATEADD` devuelve el último día del mes que se devuelve. Por ejemplo, septiembre tiene 30 días; por tanto, estas instrucciones devuelven 30-09-2006 00:00:00.000:
  
```sql
SELECT DATEADD(month, 1, '20060830');
SELECT DATEADD(month, 1, '20060831');
```
  
## <a name="number-argument"></a>Argumento number  
El argumento *number* no puede superar el intervalo de **int**. En estas instrucciones, el argumento para *number* supera el intervalo de **int** en uno. Estas instrucciones devuelven el mensaje de error siguiente: "`Msg 8115, Level 16, State 2, Line 1. Arithmetic overflow error converting expression to data type int."`
  
```sql
SELECT DATEADD(year,2147483648, '20060731');  
SELECT DATEADD(year,-2147483649, '20060731');  
```  
  
## <a name="date-argument"></a>Argumento date  
`DATEADD` no aceptará un argumento *date* incrementado a un valor fuera del intervalo de su tipo de datos. En las instrucciones siguientes, el valor *number* que se agrega al valor *date* supera el intervalo del tipo de datos *date*. `DATEADD` devuelve el mensaje de error siguiente: "`Msg 517, Level 16, State 1, Line 1 Adding a value to a 'datetime' column caused overflow`".
  
```sql
SELECT DATEADD(year,2147483647, '20060731');  
SELECT DATEADD(year,-2147483647, '20060731');  
```  
  
## <a name="return-values-for-a-smalldatetime-date-and-a-second-or-fractional-seconds-datepart"></a>Valores devueltos para una fecha smalldatetime y datepart de un segundo o fracciones de segundo  
La parte correspondiente a los segundos de un valor [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) siempre es 00. Para un valor de **fecha** *smalldatetime*, se aplica lo siguiente: 

-   Para un argumento *datepart* de **second** y un valor *number* comprendido entre -30 y +29, `DATEADD` no realiza ningún cambio.  
-   Para un argumento *datepart* de **second** y un valor *number* menor que -30 o mayor de +29, `DATEADD` realiza la suma a partir de un minuto.  
-   Para un argumento *datepart* de **millisecond** y un valor *number* comprendido entre -30001 y +29998, `DATEADD` no realiza ningún cambio.  
-   Para un argumento *datepart* de **millisecond** y un valor *number* menor que -30001 o mayor de +29998, `DATEADD` realiza la suma a partir de un minuto.  
  
## <a name="remarks"></a>Observaciones  
Use `DATEADD` en las cláusulas siguientes:

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
## <a name="fractional-seconds-precision"></a>Precisión de fracciones de segundo
`DATEADD` no permite sumar un valor *datepart* de **microsecond** o **nanosecond** para los tipos de datos de *date* como **smalldatetime**, **date** y **datetime**.
  
Los milisegundos tienen una escala de 3 (0,123), los microsegundos tienen una escala de 6 (0,123456) y los nanosegundos tienen una escala de 9 (0,123456789). Los tipos de datos **time**, **datetime2** y **datetimeoffset** tienen una escala máxima de 7 (.1234567). Para un argumento *datepart* de **nanosecond**, *number* debe ser 100 antes de que aumenten las fracciones de segundos de *date*. Un valor *number* comprendido entre 1 y 49 se redondeará hacia abajo hasta 0, y un número entre 50 a 99 se redondeará hacia arriba hasta 100.
  
Estas instrucciones agregan un atributo *datepart* de **millisecond**, **microsecond** o **nanosecond**.
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT '1 millisecond', DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT '2 milliseconds', DATEADD(millisecond,2,@datetime2)  
UNION ALL  
SELECT '1 microsecond', DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT '2 microseconds', DATEADD(microsecond,2,@datetime2)  
UNION ALL  
SELECT '49 nanoseconds', DATEADD(nanosecond,49,@datetime2)  
UNION ALL  
SELECT '50 nanoseconds', DATEADD(nanosecond,50,@datetime2)  
UNION ALL  
SELECT '150 nanoseconds', DATEADD(nanosecond,150,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1 millisecond     2007-01-01 13:10:10.1121111  
2 milliseconds    2007-01-01 13:10:10.1131111  
1 microsecond     2007-01-01 13:10:10.1111121  
2 microseconds    2007-01-01 13:10:10.1111131  
49 nanoseconds    2007-01-01 13:10:10.1111111  
50 nanoseconds    2007-01-01 13:10:10.1111112  
150 nanoseconds   2007-01-01 13:10:10.1111113  
```  
  
## <a name="time-zone-offset"></a>Ajuste de zona horaria
`DATEADD` no permite la suma para el desplazamiento de zona horaria.
  
## <a name="examples"></a>Ejemplos  

### <a name="a-incrementing-datepart-by-an-interval-of-1"></a>A. Aumentar datepart en un intervalo de 1  
Cada una de estas instrucciones aumenta *datepart* en un intervalo de 1:
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT 'year', DATEADD(year,1,@datetime2)  
UNION ALL  
SELECT 'quarter',DATEADD(quarter,1,@datetime2)  
UNION ALL  
SELECT 'month',DATEADD(month,1,@datetime2)  
UNION ALL  
SELECT 'dayofyear',DATEADD(dayofyear,1,@datetime2)  
UNION ALL  
SELECT 'day',DATEADD(day,1,@datetime2)  
UNION ALL  
SELECT 'week',DATEADD(week,1,@datetime2)  
UNION ALL  
SELECT 'weekday',DATEADD(weekday,1,@datetime2)  
UNION ALL  
SELECT 'hour',DATEADD(hour,1,@datetime2)  
UNION ALL  
SELECT 'minute',DATEADD(minute,1,@datetime2)  
UNION ALL  
SELECT 'second',DATEADD(second,1,@datetime2)  
UNION ALL  
SELECT 'millisecond',DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT 'microsecond',DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT 'nanosecond',DATEADD(nanosecond,1,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Year         2008-01-01 13:10:10.1111111  
quarter      2007-04-01 13:10:10.1111111  
month        2007-02-01 13:10:10.1111111  
dayofyear    2007-01-02 13:10:10.1111111  
day          2007-01-02 13:10:10.1111111  
week         2007-01-08 13:10:10.1111111  
weekday      2007-01-02 13:10:10.1111111  
hour         2007-01-01 14:10:10.1111111  
minute       2007-01-01 13:11:10.1111111  
second       2007-01-01 13:10:11.1111111  
millisecond  2007-01-01 13:10:10.1121111  
microsecond  2007-01-01 13:10:10.1111121  
nanosecond   2007-01-01 13:10:10.1111111  
```  
  
### <a name="b-incrementing-more-than-one-level-of-datepart-in-one-statement"></a>B. Aumentar más de un nivel de datepart en una instrucción  
Cada una de estas instrucciones aumenta *datepart* en un valor *number* lo suficientemente grande como para incrementar también el siguiente valor superior *datepart* de *date*:
  
```sql
DECLARE @datetime2 datetime2;  
SET @datetime2 = '2007-01-01 01:01:01.1111111';  
--Statement                                 Result     
-------------------------------------------------------------------   
SELECT DATEADD(quarter,4,@datetime2);     --2008-01-01 01:01:01.1111111  
SELECT DATEADD(month,13,@datetime2);      --2008-02-01 01:01:01.1111111  
SELECT DATEADD(dayofyear,365,@datetime2); --2008-01-01 01:01:01.1111111  
SELECT DATEADD(day,365,@datetime2);       --2008-01-01 01:01:01.1111111  
SELECT DATEADD(week,5,@datetime2);        --2007-02-05 01:01:01.1111111  
SELECT DATEADD(weekday,31,@datetime2);    --2007-02-01 01:01:01.1111111  
SELECT DATEADD(hour,23,@datetime2);       --2007-01-02 00:01:01.1111111  
SELECT DATEADD(minute,59,@datetime2);     --2007-01-01 02:00:01.1111111  
SELECT DATEADD(second,59,@datetime2);     --2007-01-01 01:02:00.1111111  
SELECT DATEADD(millisecond,1,@datetime2); --2007-01-01 01:01:01.1121111  
```  
  
### <a name="c-using-expressions-as-arguments-for-the-number-and-date-parameters"></a>C. Utilizar las expresiones como argumentos para los parámetros number y date  
En los ejemplos siguientes se usan otros tipos de expresiones como argumentos para los parámetros *number* y *date*. En los ejemplos se usa la base de datos de AdventureWorks.
  
#### <a name="specifying-a-column-as-date"></a>Especificar una columna como fecha  
En este ejemplo se agregan `2` (dos) días a cada valor de la columna `OrderDate` para derivar una nueva columna denominada `PromisedShipDate`:
  
```sql
SELECT SalesOrderID  
    ,OrderDate   
    ,DATEADD(day,2,OrderDate) AS PromisedShipDate  
FROM Sales.SalesOrderHeader;  
```  
  
Un conjunto de resultados parcial:
  
```
SalesOrderID OrderDate               PromisedShipDate  
------------ ----------------------- -----------------------  
43659        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43660        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43661        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
...  
43702        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43703        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43704        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43705        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43706        2005-07-03 00:00:00.000 2005-07-05 00:00:00.000  
...  
43711        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
43712        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
...  
43740        2005-07-11 00:00:00.000 2005-07-13 00:00:00.000  
43741        2005-07-12 00:00:00.000 2005-07-14 00:00:00.000  
  
```  
  
#### <a name="specifying-user-defined-variables-as-number-and-date"></a>Especificar las variables definidas por el usuario como number y date  
En este ejemplo se especifican variables definidas por el usuario como argumentos para *number* y *date*:
  
```sql
DECLARE @days INT = 365,   
        @datetime DATETIME = '2000-01-01 01:01:01.111'; /* 2000 was a leap year */;  
SELECT DATEADD(day, @days, @datetime);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------------------  
2000-12-31 01:01:01.110  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-system-function-as-date"></a>Especificar la función de sistema escalar como date  
En este ejemplo se especifica `SYSDATETIME` para *date*. El valor devuelto exacto depende del día y la hora de ejecución de la instrucción:
  
```sql
SELECT DATEADD(month, 1, SYSDATETIME());  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------------------------  
2013-02-06 14:29:59.6727944  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-subqueries-and-scalar-functions-as-number-and-date"></a>Especificar subconsultas y funciones escalares como number y date  
En este ejemplo se usan subconsultas escalares, `MAX(ModifiedDate)`, como argumentos para *number* y *date*. `(SELECT TOP 1 BusinessEntityID FROM Person.Person)` actúa como argumento artificial para el parámetro "number", para mostrar cómo se selecciona un argumento *number* de una lista de valores.
  
```sql
SELECT DATEADD(month,(SELECT TOP 1 BusinessEntityID FROM Person.Person),  
    (SELECT MAX(ModifiedDate) FROM Person.Person));  
```  
  
#### <a name="specifying-numeric-expressions-and-scalar-system-functions-as-number-and-date"></a>Especificar expresiones numéricas y funciones del sistema escalares como number y date  
En este ejemplo se usa una expresión numérica (-`(10/2))`, [operadores unarios](../../mdx/unary-operators.md) (`-`), un [operador aritmético](../../mdx/arithmetic-operators.md) (`/`) y funciones del sistema escalares (`SYSDATETIME`) como argumentos para *number* y *date*.
  
```sql
SELECT DATEADD(month,-(10/2), SYSDATETIME());  
```  
  
#### <a name="specifying-ranking-functions-as-number"></a>Especificar las funciones de clasificación como number  
En este ejemplo se usa una función de categoría como argumento para *number*.
  
```sql
SELECT p.FirstName, p.LastName  
    ,DATEADD(day,ROW_NUMBER() OVER (ORDER BY  
        a.PostalCode),SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
#### <a name="specifying-an-aggregate-window-function-as-number"></a>Especificar una función de ventana agregada como number  
En este ejemplo se usa una función de ventana agregada como argumento para *number*.
  
```sql
SELECT SalesOrderID, ProductID, OrderQty  
    ,DATEADD(day,SUM(OrderQty)   
        OVER(PARTITION BY SalesOrderID),SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
  
## <a name="see-also"></a>Consulte también
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

