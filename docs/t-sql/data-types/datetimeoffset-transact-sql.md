---
description: datetimeoffset (Transact-SQL)
title: datetimeoffset (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetimeoffset_TSQL
- datetimeoffset
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- date and time [SQL Server], datetimeoffset
- datetimeoffset data type [SQL Server]
- dates [SQL Server], data types
- data types [SQL Server], date and time
- time zones [SQL Server]
ms.assetid: a0455b71-ca25-476e-a7a8-0770f1860bb7
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 76913dfeded63354a8597027380e7e966ff73f31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172582"
---
# <a name="datetimeoffset-transact-sql"></a>datetimeoffset (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Define una fecha que se combina con una hora del día con reconocimiento de zona horaria y basado en un reloj de 24 horas.

## <a name="datetimeoffset-description"></a>Descripción de datetimeoffset
  
|Propiedad|Value|  
|---|---|
|Sintaxis|**datetimeoffset** [(*precisión de fracciones de segundo*)]|  
|Uso|DECLARE \@MyDatetimeoffset **datetimeoffset(7)**<br /><br /> CREATE TABLE Table1 ( Column1 **datetimeoffset(7)** )|  
|Formatos de literal de cadena predeterminados (utilizados para el cliente de nivel inferior)|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [{+&#124;-}hh:mm]<br /><br /> Para más información, vea la sección "Compatibilidad con versiones anteriores de clientes de niveles inferiores" más adelante.|  
|Intervalo de fechas|De 0001-01-01 a 31.12.99<br /><br /> Del 1 de enero del año 1 E. C. al 31 de diciembre de 9999 E. C.|  
|Intervalo de horas|De 00:00:00 a 23:59:59.9999999|  
|Intervalo de ajuste de zona horaria|De -14:00 a +14:00|  
|Intervalos de elementos|AAAA es una cifra de cuatro dígitos comprendida entre 0001 y 9999 que representa un año.<br /><br /> MM es una cifra de dos dígitos, comprendida entre 01 y 12, que representa un mes del año especificado.<br /><br /> DD es una cifra de dos dígitos, comprendida entre 01 y 31 dependiendo del mes, que representa un día del mes especificado.<br /><br /> hh es una cifra de dos dígitos comprendida entre 00 y 23 que representa la hora.<br /><br /> mm es una cifra de dos dígitos comprendida entre 00 y 59 que representa los minutos.<br /><br /> s es una cifra de dos dígitos comprendida entre 00 y 59 que representa los segundos.<br /><br /> n* es una cifra de cero a siete dígitos comprendida entre 0 y 9999999 que representa las fracciones de segundos.<br /><br /> hh es una cifra de dos dígitos comprendida entre -14 y 14. <br /><br /> mm es una cifra de dos dígitos comprendida entre 00 y 59.|  
|Longitud en caracteres|De 26 posiciones como mínimo (AAAA-MM-DD hh:mm:ss {+&#124;-}hh:mm) a 34 como máximo (AAAA-MM-DD hh:mm:ss.nnnnnnn {+&#124;-}hh:mm)|  
|Precisión, escala|Vea la tabla siguiente.|  
|Tamaño de almacenamiento|10 bytes, fijo es el valor predeterminado con el valor predeterminado de 100 ns de precisión de fracciones de segundo.|  
|Precisión|100 nanosegundos|  
|Valor predeterminado|1900-01-01 00:00:00 00:00|  
|Calendario|Gregoriano|  
|Precisión de fracciones de segundo definida por el usuario|Yes|  
|Conservación y reconocimiento del ajuste de zona horaria|Yes|  
|Reconocimiento del horario de verano|No|  
  
|Escala especificada|Resultado (precisión, escala)|Longitud de la columna (bytes)|Precisión de fracciones de segundo|  
|---|---|---|---|
|**datetimeoffset**|(34,7)|10|7|  
|**datetimeoffset(0)**|(26,0)|8|0-2|  
|**datetimeoffset(1)**|(28,1)|8|0-2|  
|**datetimeoffset(2)**|(29,2)|8|0-2|  
|**datetimeoffset(3)**|(30,3)|9|3-4|  
|**datetimeoffset(4)**|(31,4)|9|3-4|  
|**datetimeoffset(5)**|(32,5)|10|5-7|  
|**datetimeoffset(6)**|(33,6)|10|5-7|  
|**datetimeoffset(7)**|(34,7)|10|5-7|  
  
## <a name="supported-string-literal-formats-for-datetimeoffset"></a>Formatos de literales de cadena admitidos para datetimeoffset
En esta tabla se enumeran los formatos de literales de cadena ISO 8601 admitidos para **datetimeoffset**. Para más información sobre los formatos alfabético, numérico, sin separación y de hora de las partes de fecha y hora de **datetimeoffset**, vea [date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md) y [time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md).
  
|ISO 8601|Descripción|  
|---|---|
|YYYY-MM-DDThh:mm:ss[.nnnnnnn][{+&#124;-}hh:mm]|Estos dos formatos no se ven afectados por la configuración regional de sesión de SET LANGUAGE y SET DATEFORMAT. No se permiten espacios entre las partes de **datetimeoffset** y **datetime**.|  
|AAAA-MM-DDThh:mm:ss[.nnnnnnn]Z (UTC)|Este formato por definición de ISO indica que la parte **datetime** se debería expresar en Hora universal coordinada (UTC). Por ejemplo, 1999-12-12 12:30:30.12345 -07: 00 se deberían representar como 1999-12-12 19:30:30.12345 Z.|  
  
## <a name="time-zone-offset"></a>Ajuste de zona horaria
El desplazamiento de zona horaria especifica el desplazamiento de zona a partir de Hora UTC para un valor **time** o **datetime**. El ajuste de zona horaria se puede representar como [+ | -] hh:mm:
-   hh es una cifra de dos dígitos comprendidos entre 00 a 14 y representa el número de horas en el ajuste de zona horaria.  
-   mm es una cifra de dos dígitos comprendidos entre 00 a 59 y representa el número minutos adicionales en el ajuste de zona horaria.  
-   \+ (más) o - (menos) es el signo que se usa obligatoriamente para indicar un ajuste de zona horaria. Esto indica si el ajuste de zona horaria se agrega o resta de la hora UTC para obtener la hora local. El intervalo válido de ajuste de zona horaria es de -14: 00 a +14: 00.  
  
El intervalo del ajuste de zona horaria sigue la norma de W3C XML para la definición del esquema XSD y es ligeramente diferente de la definición estándar de SQL 2003, de 12:59 a +14: 00.
  
El parámetro de tipo opcional *precisión de fracciones de segundo* especifica el número de dígitos para la parte fraccionaria de los segundos. Este valor puede ser un entero con 0 a 7 (100 nanosegundos). El valor predeterminado de *precisión de fracciones de segundo* es 100 ns (siete dígitos para la parte fraccionaria de los segundos).
  
Los datos se almacenan en la base de datos y se procesan, comparan, ordena e indizan en el servidor como en UTC. El ajuste de zona horaria se conservará en la base de datos para la recuperación.
  
Se supone que el desplazamiento de zona horaria determinada reconoce el horario de verano (DST) y se ajusta para cualquier **datetime** determinado dentro del periodo DST.
  
Para el tipo **datetimeoffset**, los valores **datetime** UTC y local (al desplazamiento de la zona horaria persistente o convertido) se validarán durante las operaciones de inserción, actualización, aritmética, conversión o asignación. La detección de cualquier valor **datetime** UTC o local (al desplazamiento de la zona horaria persistente o convertido) que no sea válido generará un error de valor no válido. Por ejemplo, 9999-12-31 10:10:00 son válidos en UTC, pero se desbordan en la hora local al ajuste de zona horaria +13: 50.
  
Para convertir una fecha a su correspondiente valor **datetimeoffset** en una zona horaria de destino, vea [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md).
  
## <a name="ansi-and-iso-8601-compliance"></a>Compatibilidad con ANSI e ISO 8601  
Las secciones de compatibilidad con ANSI e ISO 8601 de los temas sobre [date](../../t-sql/data-types/date-transact-sql.md) y [time](../../t-sql/data-types/time-transact-sql.md) se aplican también a **datetimeoffset**.
  
## <a name="backward-compatibility-for-down-level-clients"></a>Compatibilidad con versiones anteriores de clientes de niveles inferiores
Algunos clientes de nivel inferior no admiten los tipos de datos **time**, **date**, **datetime2** y **datetimeoffset**. En la tabla siguiente se muestra la asignación de tipo entre una instancia de nivel superior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los clientes de nivel inferior.
  
|Tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|El formato del literal de cadena predeterminado se pasó al cliente de nivel inferior|ODBC de nivel inferior|OLEDB de nivel inferior|JDBC de nivel inferior|SQLCLIENT de nivel inferior|  
|---|---|---|---|---|---|
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**datetime2**|AAAA-MM-DD hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
  
## <a name="converting-date-and-time-data"></a>Convertir datos de fecha y hora
Cuando se convierte a los tipos de datos de fecha y hora, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rechaza todos los valores que no reconoce como fechas u horas. Para más información sobre cómo usar las funciones CAST y CONVERT con datos de fecha y hora, vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
### <a name="converting-datetimeoffset-data-type-to-other-date-and-time-types"></a>Convertir el tipo de datos datetimeoffset en otros tipos de fecha y hora
En esta tabla se describe lo que ocurre cuando un tipo de datos **datetimeoffset** se convierte a otros tipos de datos de fecha y hora.
  
Al convertir a **date**, se copian el año, el mes y el día. En el código siguiente se muestran los resultados de convertir un valor `datetimeoffset(4)` en un valor `date`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10 +01:00';  
DECLARE @date date= @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @date AS 'date';  
  
--Result  
--@datetimeoffset                date  
-------------------------------- ----------  
--2025-12-10 12:32:10.0000 +01:0 2025-12-10  
--  
--(1 row(s) affected)  
  
```  
  
Si la conversión es a **time(n)**, se copian la hora, los minutos, los segundos y las fracciones de segundo. Se trunca el valor de zona horaria. Cuando la precisión del valor de **datetimeoffset(n)** es mayor que la precisión del valor de **time(n)**, el valor se redondea. En el código siguiente se muestran los resultados de convertir un valor `datetimeoffset(4)` en un valor `time(3)`.
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10.1237 +01:0';  
DECLARE @time time(3) = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @time AS 'time';  
  
--Result  
--@datetimeoffset                time  
-------------------------------- ------------  
-- 2025-12-10 12:32:10.1237 +01:00    12:32:10.124  
  
--  
--(1 row(s) affected)  
  
```  
  
Cuando se convierte a **datetime**, se copian los valores de fecha y hora, y se trunca la zona horaria. Cuando la precisión de las fracciones del valor de **datetimeoffset(n)** es superior a tres dígitos, el valor se trunca. En el código siguiente se muestran los resultados de convertir un valor `datetimeoffset(4)` en un valor `datetime`.
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10.1237 +01:0';  
DECLARE @datetime datetime = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @datetime AS 'datetime';  
  
--Result  
--@datetimeoffset                datetime  
-------------------------------- -----------------------  
--2025-12-10 12:32:10.1237 +01:0 2025-12-10 12:32:10.123  
--  
--(1 row(s) affected)  
```  
  
Para las conversiones a **smalldatetime**, se copia la fecha y la hora. Los minutos se redondean hacia arriba con respecto al valor de los segundos y los segundos se establecen en 0. En el código siguiente se muestran los resultados de convertir un valor `datetimeoffset(3)` en un valor `smalldatetime`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(3) = '1912-10-25 12:24:32 +10:0';  
DECLARE @smalldatetime smalldatetime = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@datetimeoffset                @smalldatetime  
-------------------------------- -----------------------  
--1912-10-25 12:24:32.000 +10:00 1912-10-25 12:25:00  
--  
--(1 row(s) affected)  
```  
  
Si la conversión es a **datetime2(n)**, se copia la fecha y la hora al valor **datetime2** y se trunca la zona horaria. Cuando la precisión del valor de **datetime2(n)** es mayor que la precisión del valor de **datetimeoffset(n)**, el valor de las fracciones de segundo se trunca para ajustarse. En el código siguiente se muestran los resultados de convertir un valor `datetimeoffset(4)` en un valor `datetime2(3)`.
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '1912-10-25 12:24:32.1277 +10:0';  
DECLARE @datetime2 datetime2(3)=@datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset', @datetime2 AS '@datetime2';  
  
--Result  
@datetimeoffset                    @datetime2  
---------------------------------- ----------------------  
1912-10-25 12:24:32.1277 +10:00    1912-10-25 12:24:32.12  
  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-datetimeoffset"></a>Convertir literales de cadena a datetimeoffset
Las conversiones de literales de cadena en tipos de fecha y hora son posibles cuando todas las partes de las cadenas están en formatos válidos. En caso contrario, se generará un error en el tiempo de ejecución. Las conversiones implícitas o explícitas que no especifican un estilo (desde tipos de fecha y hora hasta literales de cadena) estarán en el formato predeterminado de la sesión actual. En esta tabla se muestran las reglas para convertir un literal de cadena al tipo de datos **datetimeoffset**.
  
|Literal de cadena de entrada|**datetimeoffset(n)**|  
|---|---|
|DATE de ODBC|Los literales de cadena de ODBC se asignan al tipo de datos **datetime**. Cualquier operación de asignación de los literales de DATETIME de ODBC a tipos **datetimeoffset** provocará una conversión implícita entre **datetime** y este tipo, tal y como se define en las reglas de conversión.|  
|TIME de ODBC|Vea la regla anterior de DATE de ODBC.|  
|DATETIME DE ODBC|Vea la regla anterior de DATE de ODBC.|  
|Solo DATE|La parte de TIME tiene como valor predeterminado 00:00:00. TIMEZONE tiene como valor predeterminado +00:00.|  
|Solo TIME|La parte de DATE tiene como valor predeterminado 1900-1-1. TIMEZONE tendrá como valor predeterminado +00:00.|  
|Solo TIMEZONE|Se proporcionan los valores predeterminados.|  
|DATE + TIME|TIMEZONE tiene como valor predeterminado +00:00.|  
|DATE + TIMEZONE|No permitida|  
|TIME + TIMEZONE|La parte de DATE tiene como valor predeterminado 1900-1-1.|  
|DATE + TIME + TIMEZONE|Trivial|  
  
## <a name="examples"></a>Ejemplos  
En el siguiente ejemplo se comparan los resultados de convertir una cadena a cada tipo de datos **date** y **time**.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29.1234567+12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset'  
    ,CAST('2007-05-08 12:35:29.1234567+12:15' AS datetimeoffset(7)) AS  
        'datetimeoffset IS08601';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|Tipo de datos|Output|  
|---|---|
|**Time**|12:35:29. 1234567|  
|**Fecha**|2007-05-08|  
|**Smalldatetime**|2007-05-08 12:35:00|  
|**Datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**Datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>Consulte también
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  
