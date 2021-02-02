---
title: FORMAT (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para la función FORMAT.
ms.date: 08/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FORMAT_TSQL
- FORMAT
dev_langs:
- TSQL
helpviewer_keywords:
- FORMAT function
ms.assetid: dad6f24c-b8d9-4dbe-a561-9b167b8f20c8
author: cawrites
ms.author: chadam
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: 173692e6a56113a02fedd89f3d6935b053de3222
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158987"
---
# <a name="format-transact-sql"></a>FORMAT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Devuelve un valor con formato con el formato y la referencia cultural opcional especificados. Use la función FORMAT para aplicar formato específico de la configuración regional de los valores de fecha/hora y de número como cadenas. Para las conversiones de tipos de datos generales, use CAST o CONVERT.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
FORMAT( value, format [, culture ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

 *value*  
 Expresión de un tipo de datos compatible a la que se va a dar formato. Para obtener una lista de tipos válidos, vea la tabla de la sección Comentarios.  
  
 *format*  
 Patrón de formato **nvarchar**.  
  
 El argumento *format* debe contener una cadena de formato de .NET Framework válida, ya sea como una cadena de formato estándar (por ejemplo, "C" o "D") o como un modelo de caracteres personalizados para los valores de fecha y numéricos (por ejemplo, "DD de MMMM, aaaa (dddd)"). No se admite el formato compuesto. Para obtener una explicación completa de estos modelos de formato, vea la documentación de .NET Framework sobre el formato de cadena en general, los formatos de fecha y hora personalizados, y los formatos de número personalizados. Un buen punto de partida es el tema "[Formatting Types](/dotnet/standard/base-types/formatting-types)" (Tipos de formato).  
  
 *referencia cultural*  
 Argumento opcional de tipo **nvarchar** que especifica una referencia cultural.  
  
 Si no se proporciona el argumento *culture*, se usará el idioma de la sesión actual. Este idioma se establece implícitamente, o explícitamente mediante la instrucción SET LANGUAGE. *culture* acepta como argumento cualquier referencia cultural compatible con .NET Framework; no se limita a los idiomas admitidos explícitamente por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si el argumento *culture* no es válido, FORMAT desencadena un error.  
  
## <a name="return-types"></a>Tipos de valor devuelto

 **nvarchar** o null  
  
 La longitud del valor devuelto viene determinada por *format*.  
  
## <a name="remarks"></a>Observaciones

 FORMAT devuelve NULL para los errores distintos de *culture* que no son *válidos*. Por ejemplo, se devuelve NULL si el valor especificado en *format* no es válido.  

 La función FORMAT es no determinista.
  
 FORMAT se basa en la presencia de Common Language Runtime (CLR) de .NET Framework.  
  
 Esta función no se puede enviar de forma remota puesto que depende de la presencia del CLR. El envío remoto de una función que necesita CLR produciría un error en el servidor remoto.  
  
 FORMAT se basa en las reglas de formato de CLR, que dictan que los signos de punto y de dos puntos deben incluir caracteres de escape. Por tanto, cuando la cadena de formato (segundo parámetro) contiene un signo de punto o de dos puntos, estos deben incluir caracteres de escape, como una barra diagonal inversa, cuando un valor de entrada (primer parámetro) es del tipo de datos **time**. Vea [D. FORMAT con tipos de datos de tiempo](#ExampleD).  
  
 En esta tabla se muestran los tipos de datos aceptables para el argumento *value*, junto con sus tipos equivalentes de asignación de .NET Framework.  
  
|Category|Tipo|Tipo de .NET|  
|--------------|----------|---------------|  
|Numeric|bigint|Int64|  
|Numeric|int|Int32|  
|Numeric|SMALLINT|Int16|  
|Numeric|TINYINT|Byte|  
|Numeric|Decimal|SqlDecimal|  
|Numeric|NUMERIC|SqlDecimal|  
|Numeric|FLOAT|Double|  
|Numeric|real|Single|  
|Numeric|SMALLMONEY|Decimal|  
|Numeric|money|Decimal|  
|Fecha y hora|date|DateTime|  
|Fecha y hora|time|TimeSpan|  
|Fecha y hora|datetime|DateTime|  
|Fecha y hora|smalldatetime|DateTime|  
|Fecha y hora|datetime2|DateTime|  
|Fecha y hora|datetimeoffset|DateTimeOffset|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-simple-format-example"></a>A. Ejemplo de FORMAT sencillo

 En el ejemplo siguiente se devuelve una fecha simple con formato para distintas referencias culturales.  
  
```sql  
DECLARE @d DATE = '11/22/2020';
SELECT FORMAT( @d, 'd', 'en-US' ) 'US English'  
      ,FORMAT( @d, 'd', 'en-gb' ) 'Great Britain English'  
      ,FORMAT( @d, 'd', 'de-de' ) 'German'  
      ,FORMAT( @d, 'd', 'zh-cn' ) 'Simplified Chinese (PRC)';  
  
SELECT FORMAT( @d, 'D', 'en-US' ) 'US English'  
      ,FORMAT( @d, 'D', 'en-gb' ) 'Great Britain English'  
      ,FORMAT( @d, 'D', 'de-de' ) 'German'  
      ,FORMAT( @d, 'D', 'zh-cn' ) 'Chinese (Simplified PRC)';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
US English  Great Britain English German     Simplified Chinese (PRC)  
----------  --------------------- ---------- ------------------------  
11/22/2020  22/11/2020            22.11.2020 2020/11/22 
  
US English                  Great Britain English  German                      Chinese (Simplified PRC)  
--------------------------- ---------------------- --------------------------  ---------------------------------------  
Sunday, November 22, 2020   22 November 2020       Sonntag, 22. November 2020  2020年11月22日  
  
```  
  
### <a name="b-format-with-custom-formatting-strings"></a>B. FORMAT con cadenas de formato personalizado

 En el ejemplo siguiente se muestran valores numéricos de formato especificando un formato personalizado. En el ejemplo se da por supuesto que la fecha actual es el 27 de septiembre de 2012. Para más información sobre estos y otros formatos personalizados, vea [Cadenas con formato numérico personalizado](/dotnet/standard/base-types/custom-numeric-format-strings).  
  
```sql  
DECLARE @d DATE = GETDATE();  
SELECT FORMAT( @d, 'dd/MM/yyyy', 'en-US' ) AS 'Date'  
       ,FORMAT(123456789,'###-##-####') AS 'Custom Number';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Date        Custom Number  
----------  -------------  
22/11/2020  123-45-6789  
  
```  
  
### <a name="c-format-with-numeric-types"></a>C. FORMAT con tipos numéricos

 En este ejemplo se devuelven 5 filas de la tabla **Sales.CurrencyRate** de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La columna **EndOfDateRate** se almacena como el tipo **money** en la tabla. En este ejemplo, la columna se devuelve sin formato y después con formato especificando el formato Number de .NET, el formato General y los tipos de formato Currency. Para más información sobre estos y otros formatos numéricos, vea [Cadenas con formato numérico estándar](/dotnet/standard/base-types/standard-numeric-format-strings).  
  
```sql  
SELECT TOP(5) CurrencyRateID, EndOfDayRate  
            ,FORMAT(EndOfDayRate, 'N', 'en-us') AS 'Number Format'  
            ,FORMAT(EndOfDayRate, 'G', 'en-us') AS 'General Format'  
            ,FORMAT(EndOfDayRate, 'C', 'en-us') AS 'Currency Format'  
FROM Sales.CurrencyRate  
ORDER BY CurrencyRateID;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CurrencyRateID EndOfDayRate  Numeric Format  General Format  Currency Format  
-------------- ------------  --------------  --------------  ---------------  
1              1.0002        1.00            1.0002          $1.00  
2              1.55          1.55            1.5500          $1.55  
3              1.9419        1.94            1.9419          $1.94  
4              1.4683        1.47            1.4683          $1.47  
5              8.2784        8.28            8.2784          $8.28  
  
```  
  
 En este ejemplo se especifica la referencia cultural de alemán (de-de).  
  
```sql  
SELECT TOP(5) CurrencyRateID, EndOfDayRate  
      ,FORMAT(EndOfDayRate, 'N', 'de-de') AS 'Numeric Format'  
      ,FORMAT(EndOfDayRate, 'G', 'de-de') AS 'General Format'  
      ,FORMAT(EndOfDayRate, 'C', 'de-de') AS 'Currency Format'  
FROM Sales.CurrencyRate  
ORDER BY CurrencyRateID;  
```  
  
```
CurrencyRateID EndOfDayRate  Numeric Format  General Format  Currency Format  
-------------- ------------  --------------  --------------  ---------------  
1              1.0002        1,00            1,0002          1,00 &euro;  
2              1.55          1,55            1,5500          1,55 &euro;  
3              1.9419        1,94            1,9419          1,94 &euro;  
4              1.4683        1,47            1,4683          1,47 &euro;  
5              8.2784        8,28            8,2784          8,28 &euro;  
  
```  
  
### <a name="d-format-with-time-data-types"></a><a name="ExampleD"></a> D. FORMAT con tipos de datos de tiempo

 FORMAT devuelve NULL en estos casos porque `.` y `:` no incluyen caracteres de escape.  
  
```sql  
SELECT FORMAT(cast('07:35' as time), N'hh.mm');   --> returns NULL  
SELECT FORMAT(cast('07:35' as time), N'hh:mm');   --> returns NULL  
```  
  
 FORMAT devuelve una cadena con formato porque `.` y `:` incluyen caracteres de escape.  
  
```sql  
SELECT FORMAT(cast('07:35' as time), N'hh\.mm');  --> returns 07.35  
SELECT FORMAT(cast('07:35' as time), N'hh\:mm');  --> returns 07:35  
```  

FORMAT devuelve la hora actual con el formato AM o PM especificado.

```sql
SELECT FORMAT(SYSDATETIME(), N'hh:mm tt'); -- returns 03:46 PM
SELECT FORMAT(SYSDATETIME(), N'hh:mm t'); -- returns 03:46 P
```

FORMAT devuelve el tiempo especificado en formato AM.

```sql
select FORMAT(CAST('2018-01-01 01:00' AS datetime2), N'hh:mm tt') -- returns 01:00 AM
select FORMAT(CAST('2018-01-01 01:00' AS datetime2), N'hh:mm t')  -- returns 01:00 A
```

FORMAT devuelve el tiempo especificado en formato PM.

```sql
select FORMAT(CAST('2018-01-01 14:00' AS datetime2), N'hh:mm tt') -- returns 02:00 PM
select FORMAT(CAST('2018-01-01 14:00' AS datetime2), N'hh:mm t') -- returns 02:00 P
```
  
FORMAT devuelve el tiempo especificado en formato 24h.

```sql
select FORMAT(CAST('2018-01-01 14:00' AS datetime2), N'HH:mm') -- returns 14:00
```
  
## <a name="see-also"></a>Consulte también

- [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
- [STR &#40;Transact-SQL&#41;](../../t-sql/functions/str-transact-sql.md)  
- [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)