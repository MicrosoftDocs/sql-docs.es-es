---
description: smalldatetime (Transact-SQL)
title: smalldatetime (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- smalldatetime_TSQL
- smalldatetime
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- smalldatetime data type [SQL Server]
- dates [SQL Server], data types
- date and time [SQL Server], smalldatetime
- data types [SQL Server], date and time
ms.assetid: 68b74610-d54c-4c8e-b4b2-7e3747546ee0
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6d089b8b3b012cdc4d83948543f2bdd0da3bd5ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187814"
---
# <a name="smalldatetime-transact-sql"></a>smalldatetime (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Define una fecha que se combina con una hora del día. La hora está en un formato de día de 24 horas , con segundos siempre a cero (: 00) y sin fracciones de segundo.
  
> [!NOTE]  
>  Use los tipos de datos **time**, **date**, **datetime2** y **datetimeoffset** para nuevos trabajos. Estos tipos se alinean con el estándar SQL. Son más portátiles. **time**, **datetime2** y **datetimeoffset** proporcionan una mayor precisión de segundos. **datetimeoffset** proporciona compatibilidad de zona horaria para las aplicaciones implementadas globalmente.  
  
## <a name="smalldatetime-description"></a>Descripción de smalldatetime
  
|Propiedad|Value|
|--------|-----|
|Sintaxis|**smalldatetime**|  
|Uso|DECLARE \@MySmalldatetime **smalldatetime**<br /><br /> CREATE TABLE Table1 ( Column1 **smalldatetime** )|  
|Formatos de literal de cadena predeterminados<br /><br /> (se usa para el cliente de nivel inferior)|No aplicable|  
|Intervalo de fechas|De 1900-01-01 a 2079-06-06<br /><br /> Del 1 de enero de 1900 hasta el 6 de junio de 2079|  
|Intervalo de horas|De 00:00:00 a 23:59:59<br /><br /> 2007-05-09 23:59:59 se redondeará a<br /><br /> 2007-05-10 00:00:00|  
|Intervalos de elementos|AAAA es una cifra de cuatro dígitos comprendida entre 1900 y 2079 que representa un año.<br /><br /> MM es una cifra de dos dígitos, comprendida entre 01 y 12, que representa un mes del año especificado.<br /><br /> DD es una cifra de dos dígitos, comprendida entre 01 y 31 dependiendo del mes, que representa un día del mes especificado.<br /><br /> hh es una cifra de dos dígitos comprendida entre 00 y 23 que representa la hora.<br /><br /> mm es una cifra de dos dígitos comprendida entre 00 y 59 que representa los minutos.<br /><br /> s es una cifra de dos dígitos comprendida entre 00 y 59 que representa los segundos. Los valores que son 29.998 segundos o menos se redondean al minuto más cercano. Los valores de 29.999 segundos o más se redondean al minuto más cercano.|  
|Longitud en caracteres|19 posiciones como máximo|  
|Tamaño de almacenamiento|4 bytes, fijo.|  
|Precisión|Un minuto|  
|Valor predeterminado|1900-01-01 00:00:00|  
|Calendario|Gregoriano<br /><br /> (No incluye el intervalo completo de años).|  
|Precisión de fracciones de segundo definida por el usuario|No|  
|Conservación y reconocimiento del ajuste de zona horaria|No|  
|Reconocimiento del horario de verano|No|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Compatibilidad con ANSI e ISO 8601  
**smalldatetime** no es compatible con ANSI o ISO 8601.
  
## <a name="converting-date-and-time-data"></a>Convertir datos de fecha y hora
Cuando se convierte a los tipos de datos de fecha y hora, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rechaza todos los valores que no reconoce como fechas u horas. Para más información sobre cómo usar las funciones CAST y CONVERT con datos de fecha y hora, vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
### <a name="converting-smalldatetime-to-other-date-and-time-types"></a>Convertir tipos smalldatetime a otros tipos de fecha y hora
En esta tabla se describe lo que ocurre cuando un tipo de datos **smalldatetime** se convierte a otros tipos de datos de fecha y hora.
  
Cuando la conversión es a **date**, se copian los valores de año, mes y día. En el código siguiente se muestran los resultados de convertir un valor `smalldatetime` en un valor `date`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @date date = @smalldatetime  
  
SELECT @smalldatetime AS '@smalldatetime', @date AS 'date';  
  
--Result  
--@smalldatetime          date  
------------------------- ----------  
--1955-12-13 12:43:00     1955-12-13  
--  
--(1 row(s) affected)  
```  
  
Cuando la conversión es a **time(n)** , se copian las horas, los minutos y los segundos. Las fracciones de segundo se establecen en 0. En el código siguiente se muestran los resultados de convertir un valor `smalldatetime` en un valor `time(4)`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @time time(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @time AS 'time';  
  
--Result  
--@smalldatetime          time  
------------------------- -------------  
--1955-12-13 12:43:00     12:43:00.0000  
--  
--(1 row(s) affected)  
```  
  
Cuando la conversión es a **datetime**, el valor de **smalldatetime** se copia en el valor de **datetime**. Las fracciones de segundo se establecen en 0. En el código siguiente se muestran los resultados de convertir un valor `smalldatetime` en un valor `datetime`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetime datetime = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetime AS 'datetime';  
  
--Result  
--@smalldatetime          datetime  
------------------------- -----------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.000  
--  
--(1 row(s) affected)  
```  
  
Cuando la conversión es a **datetimeoffset(n)** , el valor de **smalldatetime** se copia en el valor de **datetimeoffset(n)** . Las fracciones de segundo se establecen en 0 y el ajuste de zona horaria se establece en +00: 0. En el código siguiente se muestran los resultados de convertir un valor `smalldatetime` en un valor `datetimeoffset(4)`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetimeoffset datetimeoffset(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetimeoffset AS 'datetimeoffset(4)';  
  
--Result  
--@smalldatetime          datetimeoffset(4)  
------------------------- ------------------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.0000 +00:0  
--  
--(1 row(s) affected)  
```  
  
Cuando la conversión es a **datetime2(n)** , el valor de **smalldatetime** se copia en el valor de **datetime2(n)** . Las fracciones de segundo se establecen en 0. En el código siguiente se muestran los resultados de convertir un valor `smalldatetime` en un valor `datetime2(4)`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetime2 datetime2(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetime2 AS ' datetime2(4)';  
  
--Result  
--@smalldatetime           datetime2(4)  
------------------------- ------------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.0000  
--  
--(1 row(s) affected)  
```  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-casting-string-literals-with-seconds-to-smalldatetime"></a>A. Convertir literales de cadena con segundos a smalldatetime  
En el ejemplo siguiente se compara la conversión de los segundos presentes en los literales de cadena a `smalldatetime`.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29'     AS smalldatetime)  
    ,CAST('2007-05-08 12:35:30'     AS smalldatetime)  
    ,CAST('2007-05-08 12:59:59.998' AS smalldatetime);  
```  
  
|Entrada|Salida|  
|---|---|
|2007-05-08 12:35:29|2007-05-08 12:35:00|  
|2007-05-08 12:35:30|2007-05-08 12:36:00|  
|2007-05-08 12:59:59,998|2007-05-08 13:00:00|  
  
### <a name="b-comparing-date-and-time-data-types"></a>B. Comparar tipos de datos de fecha y hora  
En el ejemplo siguiente se comparan los resultados de convertir una cadena a cada tipo de datos de fecha y hora.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset';  
```  
  
|Tipo de datos|Output|  
|---|---|
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>Consulte también
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
