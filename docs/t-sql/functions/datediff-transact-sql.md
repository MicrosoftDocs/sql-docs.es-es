---
title: DATEDIFF (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para la función DATEDIFF. Devuelve la diferencia numérica entre una fecha de inicio y de finalización basada en el parámetro datepart.
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEDIFF_TSQL
- DATEDIFF
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATEDIFF function [SQL Server]
- time [SQL Server], crossed boundaries
- differences in date and time [SQL Server]
- counting crossed date time boundaries [SQL Server]
- date and time [SQL Server], DATEDIFF
- dates [SQL Server], crossed boundaries
- boundary differences date and time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- interval dates [SQL Server]
- time [SQL Server], functions
- crossing date time boundaries [SQL Server]
- calculating dates times [SQL Server]
ms.assetid: eba979f2-1a8d-4cce-9d75-b74f9b519b37
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 236c082d81db4928e1697e692afef45a6770b5de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181855"
---
# <a name="datediff-transact-sql"></a>DATEDIFF (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve el recuento (como un valor entero con firma) de los límites datepart que se han cruzado entre los valores *startdate* y *enddate* especificados.
  
Vea [DATEDIFF_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-big-transact-sql.md) para obtener una función que controla las diferencias más importantes entre los valores *startdate* y *enddate*. Vea [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) para obtener información general sobre todos los tipos de datos y las funciones de fecha y hora de [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DATEDIFF ( datepart , startdate , enddate )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*datepart*  
Las unidades en las que **DATEDIFF** informa la diferencia entre _startdate_ y _enddate_. Entre las unidades de _datepart_ usadas comúnmente se incluyen `month` o `second`.

No se puede especificar el valor _datepart_ en una variable, ni como una cadena entrecomillada, como `'month'`.

En esta tabla se enumeran todos los valores válidos de _datepart_. **DATEDIFF** acepta el nombre completo de _detepart_ o cualquier abreviatura indicada del nombre completo.

|nombre de *datepart*|abreviatura de *datepart*|  
|-----------|------------|
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
| &nbsp; | &nbsp; |

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
  
Para evitar ambigüedades, use años de cuatro dígitos. Vea [Establecer la opción de configuración del servidor Fecha límite de año de dos dígitos](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) para obtener información sobre los valores de año de dos dígitos.
  
*enddate*  
Vea *startdate*.
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **int**  
  
## <a name="return-value"></a>Valor devuelto  

La diferencia **int** entre *startdate* y *enddate*, expresada en el límite establecido por *datepart*.
  
Por ejemplo, `SELECT DATEDIFF(day, '2036-03-01', '2036-02-28');` devuelve -2, que indica que 2036 debe ser un año bisiesto. Este caso significa que si comenzamos en _startdate_ "2036-03-01" y, luego, contamos -2 días, alcanzamos _endate_ "2036-02-28".
  
Para un valor devuelto fuera del intervalo de **int** (de -2.147.483.648 a +2.147.483.647) `DATEDIFF` devuelve un error.  En **millisecond**, la diferencia máxima entre *startdate* y *enddate* es de 24 días, 20 horas, 31 minutos y 23.647 segundos. Para **second**, la diferencia máxima es de 68 años, 19 días, 3 horas, 14 minutos y 7 segundos.
  
Si *startdate* y *enddate* solo tienen asignado un valor de hora y *datepart* no es un valor *datepart* de hora, `DATEDIFF` devuelve 0.
  
`DATEDIFF` no usa el componente de ajuste de zona horaria de *startdate* o *enddate* para calcular el valor devuelto.
  
Como [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) solo es preciso hasta los minutos, los segundos y milisegundos siempre se establecen en 0 en el valor devuelto cuando *startdate* o *enddate* tienen un valor **smalldatetime**.
  
Si solo se asigna un valor de hora a una variable de tipo de datos de fecha, `DATEDIFF` establece el valor de la parte de la fecha que falta en el valor predeterminado: `1900-01-01`. Si solo se asigna un valor de fecha a una variable de tipo de datos de fecha u hora, `DATEDIFF` establece el valor de la parte de la hora que falta en el valor predeterminado: `00:00:00`. Si *startdate* o *enddate* solo tienen una parte de hora y el otro solo una parte de fecha, `DATEDIFF` establece las partes de hora y fecha que faltan en los valores predeterminados.
  
Si *startdate* y *enddate* tienen tipos de datos de fecha diferentes y uno tiene más partes de hora o precisión de fracciones de segundo que el otro, `DATEDIFF` establece las partes que faltan del otro en 0.
  
## <a name="_datepart_-boundaries"></a>Límites de _datepart_

Las instrucciones siguientes tienen los mismos valores *startdate* y *enddate*. Esas fechas son adyacentes y tienen una diferencia horaria de cien nanosegundos (0,0000001 segundos). La diferencia entre *startdate* y *enddate* en cada instrucción cruza un límite de calendario u hora de su *datepart*. Cada instrucción devuelve 1.
  
```sql
SELECT DATEDIFF(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(dayofyear,   '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(hour,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(minute,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(second,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(microsecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```

Si *startdate* y *enddate* tienen valores de año diferentes, pero tienen los mismos valores de semana del calendario, `DATEDIFF` devolverá 0 para *datepart* **week**.

## <a name="remarks"></a>Observaciones  
Use `DATEDIFF` en las cláusulas `SELECT <list>`, `WHERE`, `HAVING`, `GROUP BY` y `ORDER BY`.
  
`DATEDIFF` convierte implícitamente los literales de cadena como un tipo **datetime2**. Esto significa que `DATEDIFF` no admite el formato año-día-mes cuando la fecha se pasa como una cadena. La cadena se debe convertir explícitamente a un tipo **datetime** o **smalldatetime** para poder usar el formato año-día-mes.
  
La especificación de `SET DATEFIRST` no tiene ningún efecto sobre `DATEDIFF`. `DATEDIFF` siempre usa el domingo como el primer día de la semana para garantizar que la función actúa de forma determinista.

`DATEDIFF` se puede desbordar con una precisión de **minute** o superior si la diferencia entre *enddate* y *startdate* devuelve un valor que está fuera del intervalo para **int**.
  
## <a name="examples"></a>Ejemplos  
En estos ejemplos se usan otros tipos de expresiones como argumentos para los parámetros *startdate* y *enddate*.
  
### <a name="a-specifying-columns-for-startdate-and-enddate"></a>A. Especificar las columnas para startdate y enddate  
En este ejemplo se calcula el número de límites de día que se cruzan entre las fechas en dos columnas de una tabla.
  
```sql
CREATE TABLE dbo.Duration  
    (startDate datetime2, endDate datetime2);  
    
INSERT INTO dbo.Duration(startDate, endDate)  
    VALUES ('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT DATEDIFF(day, startDate, endDate) AS 'Duration'  
    FROM dbo.Duration;  
-- Returns: 1  
```  
  
### <a name="b-specifying-user-defined-variables-for-startdate-and-enddate"></a>B. Especificar las variables definidas por el usuario para startdate y enddate  
En este ejemplo, las variables definidas por el usuario actúan como argumentos para *startdate* y *enddate*.
  
```sql
DECLARE @startdate DATETIME2 = '2007-05-05 12:10:09.3312722';  
DECLARE @enddate   DATETIME2 = '2007-05-04 12:10:09.3312722';   
SELECT DATEDIFF(day, @startdate, @enddate);  
```  
  
### <a name="c-specifying-scalar-system-functions-for-startdate-and-enddate"></a>C. Especificar las funciones de sistema escalares para startdate y enddate  
En este ejemplo se usan las funciones de sistema escalares como argumentos para *startdate* y *enddate*.
  
```sql
SELECT DATEDIFF(millisecond, GETDATE(), SYSDATETIME());  
```  
  
### <a name="d-specifying-scalar-subqueries-and-scalar-functions-for-startdate-and-enddate"></a>D. Especificar las funciones escalares y de subconsulta para startdate y enddate  
En este ejemplo se usan las funciones escalares y de subconsulta como argumentos para *startdate* y *enddate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT DATEDIFF(day,
    (SELECT MIN(OrderDate) FROM Sales.SalesOrderHeader),  
    (SELECT MAX(OrderDate) FROM Sales.SalesOrderHeader));  
```  
  
### <a name="e-specifying-constants-for-startdate-and-enddate"></a>E. Especificar las constantes para startdate y enddate  
En este ejemplo se usan constantes de caracteres como argumentos para *startdate* y *enddate*.
  
```sql
SELECT DATEDIFF(day,
   '2007-05-07 09:53:01.0376635',
   '2007-05-08 09:53:01.0376635');  
```  
  
### <a name="f-specifying-numeric-expressions-and-scalar-system-functions-for-enddate"></a>F. Especificar expresiones numéricas y funciones de sistema escalares para enddate  
En este ejemplo se usa una expresión numérica, `(GETDATE() + 1)`, y las funciones de sistema escalares `GETDATE` y `SYSDATETIME`, como argumentos para *enddate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT DATEDIFF(day, '2007-05-07 09:53:01.0376635', GETDATE() + 1)
    AS NumberOfDays  
    FROM Sales.SalesOrderHeader;  
GO  
USE AdventureWorks2012;  
GO  
SELECT
    DATEDIFF(
            day,
            '2007-05-07 09:53:01.0376635',
            DATEADD(day, 1, SYSDATETIME())
        ) AS NumberOfDays  
    FROM Sales.SalesOrderHeader;  
GO  
```  
  
### <a name="g-specifying-ranking-functions-for-startdate"></a>G. Especificar las funciones de clasificación para startdate  
En este ejemplo se usa una función de categoría como argumento para *startdate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,DATEDIFF(day, ROW_NUMBER() OVER (ORDER BY   
        a.PostalCode), SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson s   
    INNER JOIN Person.Person p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
### <a name="h-specifying-an-aggregate-window-function-for-startdate"></a>H. Especificar una función de ventana agregada para startdate  
En este ejemplo se usa una función de ventana agregada como argumento para *startdate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT soh.SalesOrderID, sod.ProductID, sod.OrderQty, soh.OrderDate,
    DATEDIFF(day, MIN(soh.OrderDate)   
        OVER(PARTITION BY soh.SalesOrderID), SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail sod  
    INNER JOIN Sales.SalesOrderHeader soh  
        ON sod.SalesOrderID = soh.SalesOrderID  
WHERE soh.SalesOrderID IN(43659, 58918);  
GO  
```  

### <a name="i-finding-difference-between-startdate-and-enddate-as-date-parts-strings"></a>I. Búsqueda de la diferencia entre startdate y enddate como cadenas de partes de fecha

```sql
-- DOES NOT ACCOUNT FOR LEAP YEARS
DECLARE @date1 DATETIME, @date2 DATETIME, @result VARCHAR(100);
DECLARE @years INT, @months INT, @days INT,
    @hours INT, @minutes INT, @seconds INT, @milliseconds INT;

SET @date1 = '1900-01-01 00:00:00.000'
SET @date2 = '2018-12-12 07:08:01.123'

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
     + CASE
            WHEN @milliseconds > 0
                THEN '.' + CAST(@milliseconds AS VARCHAR(10)) 
            ELSE ''
       END 
     + ' seconds','')

SELECT @result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 

```
118 years, 11 months, 11 days, 7 hours, 8 minutes and 1.123 seconds
```
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
En estos ejemplos se usan otros tipos de expresiones como argumentos para los parámetros *startdate* y *enddate*.
  
### <a name="j-specifying-columns-for-startdate-and-enddate"></a>J. Especificar las columnas para startdate y enddate  
En este ejemplo se calcula el número de límites de día que se cruzan entre las fechas en dos columnas de una tabla.
  
```sql
CREATE TABLE dbo.Duration 
    (startDate datetime2, endDate datetime2);
    
INSERT INTO dbo.Duration (startDate, endDate)  
    VALUES ('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT TOP(1) DATEDIFF(day, startDate, endDate) AS Duration  
    FROM dbo.Duration;  
-- Returns: 1  
```  
  
### <a name="k-specifying-scalar-subqueries-and-scalar-functions-for-startdate-and-enddate"></a>K. Especificar las funciones escalares y de subconsulta para startdate y enddate  
En este ejemplo se usan las funciones escalares y de subconsulta como argumentos para *startdate* y *enddate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEDIFF(day, (SELECT MIN(HireDate) FROM dbo.DimEmployee),  
    (SELECT MAX(HireDate) FROM dbo.DimEmployee))   
FROM dbo.DimEmployee;  
  
```  
  
### <a name="l-specifying-constants-for-startdate-and-enddate"></a>L. Especificar las constantes para startdate y enddate  
En este ejemplo se usan constantes de caracteres como argumentos para *startdate* y *enddate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEDIFF(day,
    '2007-05-07 09:53:01.0376635',
    '2007-05-08 09:53:01.0376635') FROM DimCustomer;  
```  
  
### <a name="m-specifying-ranking-functions-for-startdate"></a>M. Especificar las funciones de clasificación para startdate  
En este ejemplo se usa una función de categoría como argumento para *startdate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT FirstName, LastName,
    DATEDIFF(day, ROW_NUMBER() OVER (ORDER BY   
        DepartmentName), SYSDATETIME()) AS RowNumber  
FROM dbo.DimEmployee;  
```  
  
### <a name="n-specifying-an-aggregate-window-function-for-startdate"></a>Hora Especificar una función de ventana agregada para startdate  
En este ejemplo se usa una función de ventana agregada como argumento para *startdate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, DepartmentName,
    DATEDIFF(year, MAX(HireDate)  
        OVER (PARTITION BY DepartmentName), SYSDATETIME()) AS SomeValue  
FROM dbo.DimEmployee  
```  
  
## <a name="see-also"></a>Consulte también
[DATEDIFF_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-big-transact-sql.md)  
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
