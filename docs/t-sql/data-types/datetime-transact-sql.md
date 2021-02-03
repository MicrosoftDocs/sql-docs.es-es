---
description: datetime (Transact-SQL)
title: datetime (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetime_TSQL
- datetime
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- date and time [SQL Server], datetime
- dates [SQL Server], data types
- datetime data type [SQL Server]
- data types [SQL Server], date and time
ms.assetid: 9bd1cc5b-227b-4032-95d6-7581ddcc9924
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b7ce090d303fbdaeb5abacb0522c8ae4c6dc9f9c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172568"
---
# <a name="datetime-transact-sql"></a>datetime (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Define una fecha que se combina con una hora del día con fracciones de segundos basada en un reloj de 24 horas.
  
> [!NOTE]  
>  Use los tipos de datos **time**, **date**, **datetime2** y **datetimeoffset** para nuevos trabajos. Estos tipos se alinean con el estándar SQL. Son más portátiles. **time**, **datetime2** y **datetimeoffset** proporcionan una mayor precisión de segundos. **datetimeoffset** proporciona compatibilidad de zona horaria para las aplicaciones implementadas globalmente.  
  
## <a name="datetime-description"></a>Descripción de datetime  
  
|Propiedad|Value|  
|---|---|
|Sintaxis|**datetime**|  
|Uso|DECLARE \@MyDatetime **datetime**<br /><br /> CREATE TABLE Table1 ( Column1 **datetime** )|  
|Formatos de literal de cadena predeterminados<br /><br /> (se usa para el cliente de nivel inferior)|No aplicable|  
|Intervalo de fechas|Del 01.01.53 hasta el 31.12.99|  
|Intervalo de horas|De 00:00:00 a 23:59:59,997|  
|Intervalo de ajuste de zona horaria|Ninguno|  
|Intervalos de elementos|AAAA es una cifra de cuatro dígitos comprendida entre 1753 y 9999 que representa un año.<br /><br /> MM es una cifra de dos dígitos, comprendida entre 01 y 12, que representa un mes del año especificado.<br /><br /> DD es una cifra de dos dígitos, comprendida entre 01 y 31 dependiendo del mes, que representa un día del mes especificado.<br /><br /> hh es una cifra de dos dígitos comprendida entre 00 y 23 que representa la hora.<br /><br /> mm es una cifra de dos dígitos comprendida entre 00 y 59 que representa los minutos.<br /><br /> s es una cifra de dos dígitos comprendida entre 00 y 59 que representa los segundos.<br /><br /> n* es una cifra de cero a tres dígitos comprendida entre 0 y 999 que representa las fracciones de segundos.|  
|Longitud en caracteres|19 posiciones como mínimo a 23 como máximo|  
|Tamaño de almacenamiento|8 bytes|  
|Precisión|Se redondea en incrementos de 0,000, 0,003 o 0.007 segundos|  
|Valor predeterminado|1900-01-01 00:00:00|  
|Calendario|Gregoriano (incluye el intervalo completo de años).|  
|Precisión de fracciones de segundo definida por el usuario|No|  
|Conservación y reconocimiento del ajuste de zona horaria|No|  
|Reconocimiento del horario de verano|No|  
  
## <a name="supported-string-literal-formats-for-datetime"></a>Formatos del literales de cadena compatibles para datetime  
En estas tablas se enumeran los formatos de literales de cadena compatibles para **datetime**. Excepto para ODBC, los literales de cadena de **datetime** están incluidos entre comillas simples ('), por ejemplo 'string_literaL'. Si el entorno no es **us_english**, los literales de cadena deberían estar en el formato N'string_literaL'.
  
|Numeric|Descripción|  
|---|---|
|Formatos de fecha:<br /><br /> [0]4/15/[19]96 -- (mda)<br /><br /> [0]4-15-[19]96 -- (mda)<br /><br /> [0]4.15.[19]96 -- (mda)<br /><br /> [0]4/[19]96/15 -- (mad)<br /><br /> 15/[0]4/[19]96 -- (dma)<br /><br /> 15/[19]96/[0]4 -- (dam)<br /><br /> [19]96/15/[0]4 -- (adm)<br /><br /> [19]96/[0]4/15 -- (amd)<br /><br /> Formatos de hora:<br /><br /> 14:30<br /><br /> 14:30[:20:999]<br /><br /> 14:30[:20.9]<br /><br /> 4am<br /><br /> 4 PM|Puede especificar datos de fecha con un mes en forma de número. Por ejemplo, 5/20/97 representa el veinte de mayo de 1997. Cuando use un formato numérico de fecha, especifique el mes, el día y el año en una cadena con barras diagonales (/), guiones (-) o puntos (.) como separadores. Esta cadena debe aparecer de la forma siguiente:<br /><br /> *número separador número separador número [hora] [hora]*<br /><br /> <br /><br /> Cuando el idioma establecido es **us_english**, el orden predeterminado de la fecha es mdy (mes, día, año). Para cambiar el orden de la fecha, use la instrucción [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md).<br /><br /> La configuración de SET DATEFORMAT determina cómo se interpretan los valores de fecha. Si el orden no coincide con la configuración, los valores no se interpretan como fechas. Las fechas desordenadas pueden interpretarse como fuera del intervalo o con valores erróneos. Por ejemplo, 12/10/08 se puede interpretar de seis formas distintas, en función de la configuración de DATEFORMAT. Un año en cuatro partes se interpreta como el año.|  
  
|Alfabético|Descripción|  
|---|---|
|Abr[il] [15][,] 1996<br /><br /> Abr[il] 15[,] [19]96<br /><br /> Abr[il] 1996 [15]<br /><br /> [15] Abr[il][,] 1996<br /><br /> 15 Abr[il][,][19]96<br /><br /> 15 [19]96 abr[il]<br /><br /> [15] 1996 abr[il]<br /><br /> 1996 ABR[IL] [15]<br /><br /> 1996 [15] ABR[IL]|Puede especificar los datos de la fecha con un mes especificado como el nombre completo del mes. Por ejemplo, abril o la abreviatura del mes Abr especificada en el idioma actual; las comas son opcionales y se omite el uso de mayúsculas.<br /><br /> Éstas son algunas directrices para utilizar los formatos alfabéticos de fecha:<br /><br /> 1) Incluya los datos de fecha y hora entre comillas simples ('). Para los idiomas distintos de inglés, utilice N'<br /><br /> 2) Los caracteres que se incluyen entre corchetes son opcionales.<br /><br /> 3) Si solo especifica los dos últimos dígitos del año, los valores inferiores a los dos últimos dígitos de la opción Fecha límite de año de dos dígitos pertenecen al mismo siglo que el año límite (vea [Establecer la opción de configuración del servidor Fecha límite de año de dos dígitos](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)). Los valores mayores o iguales que el valor de esta opción pertenecen al siglo anterior al año límite. Por ejemplo, si el valor de **Fecha límite de año de dos dígitos** es 2050 (el valor predeterminado), 25 se interpreta como 2025 y 50 se interpreta como 1950. Para evitar ambigüedades, use años de cuatro dígitos.<br /><br /> 4) Si falta el día, se usa el primer día del mes.<br /><br /> <br /><br /> El parámetro de sesión SET DATEFORMAT no se aplica cuando se especifica el mes de forma alfabética.|  
  
|ISO 8601|Descripción|  
|---|---|
|AAAA-MM-DDThh:mm:ss[.mmm]<br /><br /> AAAMMDD[ hh:mm:ss[.mmm]]|Ejemplos:<br /><br /> 1) 2004-05-23T14:25:10<br /><br /> 2) 2004-05-23T14:25:10.487<br /><br /> <br /><br /> Para usar el formato ISO 8601, debe especificar cada elemento del formato, incluida la **T**, los dos puntos (:) y el punto (.) que se muestran en el formato.<br /><br /> Los corchetes indican que el componente de fracción de segundo es opcional. El componente de hora se especifica en el formato de 24 horas.<br /><br /> La T indica el inicio de la parte de la hora del valor de **datetime**.<br /><br /> La ventaja de utilizar el formato ISO 8601 es que se trata de un estándar internacional con una especificación que evita ambigüedades. Además, este formato no se ve afectado por la configuración de SET DATEFORMAT o [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md).|  
  
|Sin separación|Descripción|  
|---|---|
|AAAAMMDD hh:mm:ss[.mmm]||  
  
|ODBC|Descripción|  
|---|---|
|{ ts '1998-05-02 01:23:56.123' }<br /><br /> { d '1990-10-02' }<br /><br /> { t '13:33:41' }|La API de ODBC define secuencias de escape para representar valores de fecha y de hora que ODBC llama datos de marca de tiempo. La definición del lenguaje OLE DB (DBGUID-SQL) aceptada por el proveedor OLE DB de [!INCLUDE[msCoName](../../includes/msconame-md.md)] para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también admite este formato de marca de tiempo de ODBC. Las aplicaciones que usan las API basadas en ODBC, OLE DB y ADO pueden usar este formato de marca de tiempo de ODBC para representar fechas y horas.<br /><br /> Las secuencias de escape de marca de tiempo de ODBC tienen el formato: { *literal_type* '*constant_value*' }:<br /><br /> <br /><br /> - *literal_type* especifica el tipo de secuencia de escape. Las marcas de tiempo tienen tres especificadores *literal_type*:<br />1) d = solo fecha<br />2) t = solo hora<br />3) ts = marca de tiempo (hora + fecha)<br /><br /> <br /><br /> - '*constant_value*' es el valor de la secuencia de escape. *constant_value* debe seguir estos formatos para cada *literal_type*.<br />d : aaaa-mm-dd<br />t : hh:mm:ss[.fff]<br />ts : aaaa-mm-dd hh:mm:ss[.fff]|  
  
## <a name="rounding-of-datetime-fractional-second-precision"></a>Redondeo de precisión de las fracciones de segundo de datetime  
Los valores de **datetime** se redondean con incrementos de 0,000; 0,003 o 0,007 segundos, como se muestra en esta tabla.
  
|Valor especificado por el usuario|Valor almacenado por el sistema|  
|---|---|
|01/01/98 23:59:59.999|1998-01-02 00:00:00.000|  
|01/01/98 23:59:59.995<br /><br /> 01/01/98 23:59:59.996<br /><br /> 01/01/98 23:59:59.997<br /><br /> 01/01/98 23:59:59.998|1998-01-01 23:59:59.997|  
|01/01/98 23:59:59.992<br /><br /> 01/01/98 23:59:59.993<br /><br /> 01/01/98 23:59:59.994|1998-01-01 23:59:59.993|  
|01/01/98 23:59:59.990<br /><br /> 01/01/98 23:59:59.991|1998-01-01 23:59:59.990|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Compatibilidad con ANSI e ISO 8601  
**datetime** no es compatible con ANSI o ISO 8601.
  
##  <a name="converting-date-and-time-data"></a><a name="_datetime"></a> Convertir datos de fecha y hora  
Cuando se convierte a los tipos de datos de fecha y hora, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rechaza todos los valores que no reconoce como fechas u horas. Para más información sobre cómo usar las funciones CAST y CONVERT con datos de fecha y hora, vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
### <a name="converting-other-date-and-time-types-to-the-datetime-data-type"></a>Convertir otros tipos de fecha y hora en el tipo de datos datetime 
En esta sección se describe lo que ocurre cuando otros tipos de datos de fecha y hora se convierten en un tipo de datos **datetime**.  
  
Cuando la conversión es desde **date**, se copian los valores de año, mes y día. El componente de hora se establece en 00:00:00.000. En el código siguiente se muestran los resultados de convertir un valor `date` en un valor `datetime`.  
  
```sql
DECLARE @date date = '12-21-16';  
DECLARE @datetime datetime = @date;  
  
SELECT @datetime AS '@datetime', @date AS '@date';  
  
--Result  
--@datetime               @date  
------------------------- ----------  
--2016-12-21 00:00:00.000 2016-12-21  
```  
  
Cuando la conversión es desde **time(n)**, se copia el componente de hora y el componente de fecha se establece en "1900-01-01". Cuando la precisión de las fracciones del valor de **time(n)** sea superior a tres dígitos, el valor se truncará para ajustarse. En el siguiente ejemplo se muestran los resultados de convertir un valor `time(4)` en un valor `datetime`.  
  
```sql
DECLARE @time time(4) = '12:10:05.1237';  
DECLARE @datetime datetime = @time;  
  
SELECT @datetime AS '@datetime', @time AS '@time';  
  
--Result  
--@datetime               @time  
------------------------- -------------  
--1900-01-01 12:10:05.123 12:10:05.1237  
```  
  
Cuando la conversión es desde **smalldatetime**, se copian las horas y los minutos. Los segundos y las fracciones de segundo se establecen en 0. En el código siguiente se muestran los resultados de convertir un valor `smalldatetime` en un valor `datetime`.  
  
```sql
DECLARE @smalldatetime smalldatetime = '12-01-16 12:32';  
DECLARE @datetime datetime = @smalldatetime;  
  
SELECT @datetime AS '@datetime', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@datetime               @smalldatetime  
------------------------- -----------------------  
--2016-12-01 12:32:00.000 2016-12-01 12:32:00  
```  
  
Cuando la conversión es desde **datetimeoffset(n)**, se copian los componentes de fecha y hora. Se trunca la zona horaria. Cuando la precisión de las fracciones del valor de **datetimeoffset(n)** sea superior a tres dígitos, el valor se truncará. En el siguiente ejemplo se muestran los resultados de convertir un valor `datetimeoffset(4)` en un valor `datetime`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '1968-10-23 12:45:37.1234 +10:0';  
DECLARE @datetime datetime = @datetimeoffset;  
  
SELECT @datetime AS '@datetime', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@datetime               @datetimeoffset  
------------------------- ------------------------------  
--1968-10-23 12:45:37.123 1968-10-23 12:45:37.1237 +01:0   
```  
  
Cuando la conversión es desde **datetime2(n)**, se copian la fecha y la hora. Cuando la precisión de las fracciones del valor de **datetime2(n)** sea superior a tres dígitos, el valor se truncará. En el siguiente ejemplo se muestran los resultados de convertir un valor `datetime2(4)` en un valor `datetime`.  
  
```sql
DECLARE @datetime2 datetime2(4) = '1968-10-23 12:45:37.1237';  
DECLARE @datetime datetime = @datetime2;  
  
SELECT @datetime AS '@datetime', @datetime2 AS '@datetime2';  
  
--Result  
--@datetime               @datetime2  
------------------------- ------------------------  
--1968-10-23 12:45:37.123 1968-10-23 12:45:37.1237  
```  
  
## <a name="examples"></a>Ejemplos  
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
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
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
  
  
