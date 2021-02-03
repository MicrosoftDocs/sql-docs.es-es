---
description: hora (Transact-SQL)
title: time (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- time_TSQL
- time
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- time [SQL Server]
- date and time [SQL Server], time
- data types [SQL Server], date and time
- time data type [SQL Server]
ms.assetid: 30a6c681-8190-48e4-94d0-78182290a402
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7425622f808eccb983b73670b57ec3295de15fb3
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237352"
---
# <a name="time-transact-sql"></a>hora (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Define una hora de un día. La hora no distingue la zona horaria y está basada en un reloj de 24 horas.  
  
  > [!NOTE]  
  > Se proporciona información de Informatica para clientes de PDW que usen el conector de Informatica. 
  
## <a name="time-description"></a>Descripción de time  
  
|Propiedad|Value|  
|--------------|-----------|  
|Sintaxis|**time** [ (*escala de fracciones de segundo*) ]|  
|Uso|DECLARE \@MyTime **time(7)**<br /><br /> CREATE TABLE Table1 ( Column1 **time(7)** )|  
|*escala de fracciones de segundo*|Especifica el número de dígitos de la parte fraccionaria de los segundos.<br /><br /> Este valor puede ser un entero de 0 a 7. En Informatica, puede ser un entero de 0 a 3.<br /><br /> La escala predeterminada de las fracciones es 7 (100 ns).|  
|Formato de literal de cadena predeterminado<br /><br /> (se usa para el cliente de nivel inferior)|hh:mm:ss[.nnnnnnn] en Informatica)<br /><br /> Para obtener más información, consulte la sección [Compatibilidad con versiones anteriores de clientes de niveles inferiores](#BackwardCompatibilityforDownlevelClients).|  
|Intervalo|De 00:00:00.0000000 a 23:59:59.9999999 (de 00:00:00.000 a 23:59:59.999 en Informatica)|  
|Intervalos de elementos|hh es una cifra de dos dígitos, comprendida entre 0 y 23, que representa la hora.<br /><br /> mm es una cifra de dos dígitos, comprendida entre 0 y 59, que representa los minutos.<br /><br /> ss es una cifra de dos dígitos, comprendida entre 0 y 59, que representa los segundos.<br /><br /> n\* es una cifra de cero a siete dígitos comprendida entre 0 y 9999999 que representa las fracciones de segundos. En Informatica, n\* es una cifra de cero a tres dígitos comprendida entre 0 y 999.|  
|Longitud en caracteres|De 8 posiciones como mínimo (hh:mm:ss) a 16 como máximo (hh:mm:ss.nnnnnnn). En Informatica, el máximo es 12 (hh:mm:ss.nnn).|  
|Precisión, escala<br /><br /> (el usuario especifica solo la escala)|Vea la tabla siguiente.|  
|Tamaño de almacenamiento|5 bytes (fijo) es el valor predeterminado con el valor predeterminado de 100 ns de precisión de fracciones de segundo. En Informatica, el valor predeterminado es 4 bytes y fijo, con el valor predeterminado de 1 ms de precisión de fracciones de segundo.|  
|Precisión|100 nanosegundos (1 milisegundo en Informatica)|  
|Valor predeterminado|00:00:00<br /><br /> Este valor se usa para la parte de fecha anexada en la conversión implícita de **date** a **datetime2** o **datetimeoffset**.|  
|Precisión de fracciones de segundo definida por el usuario|Yes|  
|Conservación y reconocimiento del ajuste de zona horaria|No|  
|Reconocimiento del horario de verano|No|  
  
|Escala especificada|Resultado (precisión, escala)|Longitud de la columna (bytes)|Fracciones<br /><br /> segundos<br /><br /> Precisión|  
|---------------------|---------------------------------|-----------------------------|------------------------------------------|  
|**time**|(16,7) [(12,3) en Informatica]|5 (4 en Informatica)|7 (3 en Informatica)|  
|**time(0)**|(8,0)|3|0-2|  
|**time(1)**|(10,1)|3|0-2|  
|**time(2)**|(11,2)|3|0-2|  
|**time(3)**|(12,3)|4|3-4|  
|**time(4)**<br /><br /> No se admite en Informatica.|(13,4)|4|3-4|  
|**time(5)**<br /><br /> No se admite en Informatica.|(14,5)|5|5-7|  
|**time(6)**<br /><br /> No se admite en Informatica.|(15,6)|5|5-7|  
|**time(7)**<br /><br /> No se admite en Informatica.|(16,7)|5|5-7|  
  
## <a name="supported-string-literal-formats-for-time"></a>Formatos de literales de cadena compatibles para time  
 En la siguiente tabla se muestran los formatos de literales de cadena válidos para el tipo de datos **time**.  
  
|SQL Server|Descripción|  
|----------------|-----------------|  
|hh:mm[:ss][:fracciones de segundo][AM][PM]<br /><br /> hh:mm[:ss][.fracciones de segundo][AM][PM]<br /><br /> hhAM[PM]<br /><br /> hh AM[PM]|El valor de hora 0 representa la hora desde medianoche (AM), sin tener en cuenta si se especifica AM. PM no se puede especificar si la hora es igual a 0.<br /><br /> Las horas de 01 a 11 representan horas antes del mediodía si no se especifica AM ni PM. Si se especifica AM, los valores representan las horas antes del mediodía. Si se especifica PM, los valores representan las horas después del mediodía.<br /><br /> El valor de hora 12 representa el mediodía si no se especifica AM ni PM. Si se especifica AM, el valor representa la hora que empieza a medianoche. Si se especifica PM, el valor representa la hora que empieza a mediodía. Por ejemplo, 12:01 es 1 minuto después del mediodía, igual que 12:01 PM, mientras que 12:01 AM es 1 minuto después de medianoche. Especificar 12:01 AM es lo mismo que 00:01 ó 00:01 AM.<br /><br /> Los valores de hora de 13 a 23 representan horas después del mediodía si no se especifica AM o PM. Si se especifica PM, los valores también representan las horas después del mediodía. No es posible especificar AM si el valor de hora es de 13 a 23.<br /><br /> Un valor de hora de 24 no es válido. Para representar la medianoche, use 12:00 AM o 00:00.<br /><br /> Los milisegundos se pueden preceder de dos puntos (:) o un punto (.). Si se usan dos puntos, el número indica milésimas de segundo. Si se usa un punto, un único dígito indica décimas de segundo, dos dígitos indica centésimas de segundo y tres dígitos indica milésimas de segundo. Por ejemplo, 12:30:20:1 indica las 12:30, 20 segundos y una milésima; 12:30:20.1 indica las 12:30, 20 segundos y una décima.|  
  
|ISO 8601|Notas|  
|--------------|-----------|  
|hh:mm:ss<br /><br /> hh:mm[:ss][.fracciones de segundo]|hh es una cifra de dos dígitos, de 0 a 23, que representa el número de horas del ajuste de zona horaria.<br /><br /> mm es una cifra de dos dígitos, de 0 a 59, que representa el número de minutos adicionales en el ajuste de zona horaria.|  
  
|ODBC|Notas|  
|----------|-----------|  
|{t 'hh:mm:ss[.fracciones de segundo]'}|Específico de la API de ODBC.|  
  
## <a name="compliance-with-ansi-and-iso-8601-standards"></a>Compatibilidad con las normas ANSI e ISO 8601  
 No se admite el uso de la hora 24 para representar la medianoche y la inserción de un segundo intercalar respecto a 59, como se define en el estándar ISO 8601 (5.3.2 y 5.3), por compatibilidad con versiones anteriores y para mantener la coherencia con los tipos de fecha y hora existentes.  
  
 El formato predeterminado del literal de cadena (que se usa para el cliente de nivel inferior) se ajustará al formato del estándar SQL, definido como hh:mm:ss[.nnnnnnn]. Este formato es similar a la definición de ISO 8601 para TIME, sin incluir las fracciones de segundo.  
  
##  <a name="backward-compatibility-for-down-level-clients"></a><a name="BackwardCompatibilityforDownlevelClients"></a> Compatibilidad con versiones anteriores de los clientes de niveles inferiores  
 Algunos clientes de nivel inferior no admiten los tipos de datos **time**, **date**, **datetime2** y **datetimeoffset**. En la tabla siguiente se muestra la asignación de tipo entre una instancia de nivel superior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los clientes de nivel inferior.  
  
|Tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|El formato del literal de cadena predeterminado se pasó al cliente de nivel inferior|ODBC de nivel inferior|OLEDB de nivel inferior|JDBC de nivel inferior|SQLCLIENT de nivel inferior|  
|-----------------------------------------|----------------------------------------------------------------|----------------------|-----------------------|----------------------|---------------------------|  
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**datetime2**|AAAA-MM-DD hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
  
## <a name="converting-date-and-time-data"></a>Convertir datos de fecha y hora  
 Cuando se convierte a los tipos de datos de fecha y hora, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rechaza todos los valores que no reconoce como fechas u horas. Para más información sobre cómo usar las funciones CAST y CONVERT con datos de fecha y hora, vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
### <a name="converting-timen-data-type-to-other-date-and-time-types"></a>Convertir el tipo de datos time(n) en otros tipos de fecha y hora  
 En esta sección se describe lo que ocurre cuando un tipo de datos **time** se convierte a otros tipos de datos de fecha y hora.  
  
 Cuando la conversión es a **time(n)**, se copian las horas, los minutos y los segundos. Cuando la precisión de destino es menor que la precisión de origen, las fracciones de segundo se redondean para ajustarse a la precisión de destino. En el siguiente ejemplo se muestran los resultados de convertir un valor `time(4)` en un valor `time(3)`.  
  
```sql
DECLARE @timeFrom time(4) = '12:34:54.1237';  
DECLARE @timeTo time(3) = @timeFrom;  
  
SELECT @timeTo AS 'time(3)', @timeFrom AS 'time(4)';  
  
--Results  
--time(3)      time(4)  
-------------- -------------  
--12:34:54.124 12:34:54.1237  
--  
--(1 row(s) affected)  
```  
  
 Cuando es una conversión a **date**, se produce un error en la conversión y se muestra el mensaje de error 206: "Conflicto de tipos de operandos: date es incompatible con time".  
  
 Cuando la conversión es a **datetime**, se copian los valores de hora, minuto y segundo y el componente de fecha se establece en '1900-01-01'. Cuando la precisión de las fracciones de segundo del valor de **time(n)** es superior a tres dígitos, el resultado de **datetime** se truncará. En el código siguiente se muestran los resultados de convertir un valor `time(4)` en un valor `datetime`.  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetime datetime= @time;  
SELECT @time AS '@time', @datetime AS '@datetime';  
  
--Result  
--@time         @datetime  
--------------- -----------------------  
--12:15:04.1237 1900-01-01 12:15:04.123  
--  
--(1 row(s) affected)  
  
```  
  
 Cuando la conversión es a **smalldatetime**, la fecha se establece en '1900-01-01' y se redondean los valores de hora y minuto. Los segundos y las fracciones de segundo se establecen en 0. En el código siguiente se muestran los resultados de convertir un valor `time(4)` en un valor `smalldatetime`.  
  
```sql
-- Shows rounding up of the minute value.  
DECLARE @time time(4) = '12:15:59.9999';   
DECLARE @smalldatetime smalldatetime= @time;    
SELECT @time AS '@time', @smalldatetime AS '@smalldatetime';   
  
--Result  
@time            @smalldatetime  
---------------- -----------------------  
12:15:59.9999    1900-01-01 12:16:00--  
--(1 row(s) affected)  
  
-- Shows rounding up of the hour value.  
DECLARE @time time(4) = '12:59:59.9999';   
DECLARE @smalldatetime smalldatetime= @time;    
  
SELECT @time AS '@time', @smalldatetime AS '@smalldatetime';  
@time            @smalldatetime  
---------------- -----------------------  
12:59:59.9999    1900-01-01 13:00:00  
  
(1 row(s) affected)  
  
```  
  
 Cuando la conversión es a **datetimeoffset(n)**, la fecha se establece en '1900-01-01' y la hora se copia. El ajuste de zona horaria se establece en +00:00. Cuando la precisión de las fracciones de segundo del valor de **time(n)** es mayor que la precisión del valor de **datetimeoffset(n)**, el valor se redondea para ajustarse. El ejemplo siguiente muestra los resultados de convertir un valor de `time(4)` en un tipo `datetimeoffset(3)`.  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetimeoffset datetimeoffset(3) = @time;  
  
SELECT @time AS '@time', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@time         @datetimeoffset  
--------------- ------------------------------  
--12:15:04.1237 1900-01-01 12:15:04.124 +00:00  
--  
--(1 row(s) affected)  
  
```  
  
 Cuando la conversión es a **datetime2(n)**, la fecha se establece en '1900-01-01', se copia el componente de hora y el ajuste de zona horaria se establece en 00:00. Cuando la precisión de las fracciones de segundo del valor de **datetime2(n)** es mayor que el valor de **time(n)**, el valor se redondeará para ajustarse.  En el siguiente ejemplo se muestran los resultados de convertir un valor `time(4)` en un valor `datetime2(2)`.  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetime2 datetime2(3) = @time;  
  
SELECT @datetime2 AS '@datetime2', @time AS '@time';  
  
--Result  
--@datetime2              @time  
------------------------- -------------  
--1900-01-01 12:15:04.124 12:15:04.1237  
--  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-timen"></a>Convertir literales de cadena a time(n)  
 Las conversiones de literales de cadena en tipos de fecha y hora son posibles cuando todas las partes de las cadenas están en formatos válidos. En caso contrario, se generará un error en el tiempo de ejecución. Las conversiones implícitas o explícitas que no especifican un estilo (desde tipos de fecha y hora hasta literales de cadena) estarán en el formato predeterminado de la sesión actual. En la siguiente tabla se muestran las reglas para convertir un literal de cadena al tipo de datos **time**.  
  
|Literal de cadena de entrada|Regla de conversión|  
|--------------------------|---------------------|  
|DATE de ODBC|Los literales de cadena de ODBC se asignan al tipo de datos **datetime**. Cualquier operación de asignación de los literales de DATETIME de ODBC a tipos **time** provocará una conversión implícita entre **datetime** y este tipo, tal y como se define en las reglas de conversión.|  
|TIME de ODBC|Vea la regla de DATE de ODBC anterior.|  
|DATETIME DE ODBC|Vea la regla de DATE de ODBC anterior.|  
|Solo DATE|Se proporcionan los valores predeterminados.|  
|Solo TIME|Trivial|  
|Solo TIMEZONE|Se proporcionan los valores predeterminados.|  
|DATE + TIME|Se usa la parte de TIME de la cadena de entrada.|  
|DATE + TIMEZONE|No permitido.|  
|TIME + TIMEZONE|Se usa la parte de TIME de la cadena de entrada.|  
|DATE + TIME + TIMEZONE|Se usará la parte de TIME de DATETIME local.|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-comparing-date-and-time-data-types"></a>A. Comparar tipos de datos de fecha y hora  
 En el siguiente ejemplo se comparan los resultados de convertir una cadena a cada tipo de datos **date** y **time**.  
  
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
|---------------|------------|  
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
###  <a name="b-inserting-valid-time-string-literals-into-a-time7-column"></a><a name="ExampleB"></a> B. Insertar literales de cadena time válidos en una columna time(7)  
 En la tabla siguiente se enumeran varios literales de cadena que se pueden insertar en una columna de tipo de datos **time(7)** con los valores que se almacenan en dicha columna.  
  
|Tipo de formato de literal de cadena|Literal de cadena insertado|Valor de time(7) almacenado|Descripción|  
|--------------------------------|-----------------------------|------------------------------------|-----------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01:123AM'|01:01:01.1230000|Si dos puntos (:) preceden la precisión en fracciones de segundo, la escala no puede tener más de tres posiciones ya que, de lo contrario, se producirá un error.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567 AM'|01:01:01.1234567|Si se especifica AM o PM, la hora se almacenará en formato de 24 horas sin el literal AM o PM|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567 PM'|13:01:01.1234567|Si se especifica AM o PM, la hora se almacenará en formato de 24 horas sin el literal AM o PM|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567PM'|13:01:01.1234567|El espacio antes de AM o PM es opcional.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01AM'|01:00:00.0000000|Si solo se especifica la hora, todos los demás valores son 0.|  
|SQL Server|'01 AM'|01:00:00.0000000|El espacio antes de AM o PM es opcional.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01'|01:01:01.0000000|Si no se especifican las fracciones de segundo, cada posición definida por el tipo de datos será 0.|  
|ISO 8601|'01:01:01.1234567'|01:01:01.1234567|Para cumplir las especificaciones del estándar ISO 8601, use el formato de 24 horas, no el formato AM o PM.|  
|ISO 8601|'01:01:01.1234567 +01:01'|01:01:01.1234567|Se puede insertar el designador opcional de diferencia de zona horaria (TZD), aunque no se almacenará.|  
  
### <a name="c-inserting-time-string-literal-into-columns-of-each-date-and-time-date-type"></a>C. Insertar literales de cadena time en columnas de tipo de datos date y time  
 En la siguiente tabla, la primera columna muestra un literal de cadena time que se insertará en una columna de la tabla de base de datos con el tipo de datos date o time indicado en la segunda columna. La tercera columna muestra el valor que se almacenará en la columna de la tabla de base de datos.  
  
|Literal de cadena insertado|Tipo de datos de columna|Valor almacenado en la columna|Descripción|  
|-----------------------------|----------------------|------------------------------------|-----------------|  
|'12:12:12.1234567'|**time(7)**|12:12:12.1234567|Si la precisión en fracciones de segundo supera el valor especificado en la columna, la cadena quedará truncada y no generará un error.|  
|'07-05-2007'|**date**|NULL|Cualquier valor de time producirá un error en la instrucción INSERT.|  
|'12:12:12'|**smalldatetime**|1900-01-01 12:12:00|Cualquier valor de precisión en fracciones de segundo producirá un error en la instrucción INSERT.|  
|'12:12:12.123'|**datetime**|1900-01-01 12:12:12.123|Cualquier precisión en segundos de más de tres posiciones producirá un error en la instrucción INSERT.|  
|'12:12:12.1234567'|**datetime2(7)**|1900-01-01 12:12:12.1234567|Si la precisión en fracciones de segundo supera el valor especificado en la columna, la cadena quedará truncada y no generará un error.|  
|'12:12:12.1234567'|**datetimeoffset(7)**|1900-01-01 12:12:12.1234567 +00:00|Si la precisión en fracciones de segundo supera el valor especificado en la columna, la cadena quedará truncada y no generará un error.|  
  
## <a name="see-also"></a>Consulte también  
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
