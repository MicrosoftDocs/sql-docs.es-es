---
description: datetime2 (Transact-SQL)
title: datetime2 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetime2
- datetime2_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- dates [SQL Server], data types
- date and time [SQL Server], datetime2
- data types [SQL Server], date and time
- datetime2 data type [SQL Server]
ms.assetid: 868017f3-214f-43ef-8536-cc1632a2288f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 837f8bdf2680d62c2b4d6992fc97c65ef59842bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161292"
---
# <a name="datetime2-transact-sql"></a>datetime2 (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Define una fecha que se combina con una hora del día basada en un reloj de 24 horas. **datetime2** se puede considerar como una extensión del tipo **datetime** existente que tiene un rango de fechas mayor, un valor predeterminado mayor de precisión fraccionaria y una precisión opcional especificada por el usuario.
  
## <a name="datetime2-description"></a>Descripción de datetime2
  
|Propiedad|Value|  
|--------------|-----------|  
|Sintaxis|**datetime2** [(*precisión de fracciones de segundo*)]|  
|Uso|DECLARE \@MyDatetime2 **datetime2(7)**<br /><br /> CREATE TABLE Table1 ( Column1 **datetime2(7)** )|  
|Formato de literal de cadena predeterminado<br /><br /> (se usa para el cliente de nivel inferior)|AAAA-MM-DD hh:mm:ss[.fracciones de segundos]<br /><br /> Para más información, vea la sección "Compatibilidad con versiones anteriores de clientes de niveles inferiores" más adelante.|  
|Intervalo de fechas|De 0001-01-01 a 31.12.99<br /><br /> Del 1 de enero del año 1 después de Cristo al 31 de diciembre de 9999|  
|Intervalo de horas|De 00:00:00 a 23:59:59.9999999|  
|Intervalo de ajuste de zona horaria|Ninguno|  
|Intervalos de elementos|AAAA es una cifra de cuatro dígitos comprendida entre 0001 y 9999 que representa un año.<br /><br /> MM es una cifra de dos dígitos comprendida entre 01 y 12 que representa un mes del año especificado.<br /><br /> DD es una cifra de dos dígitos comprendida entre 01 y 31 (dependiendo del mes) que representa un día del mes especificado.<br /><br /> hh es una cifra de dos dígitos comprendida entre 00 y 23 que representa la hora.<br /><br /> mm es una cifra de dos dígitos comprendida entre 00 y 59 que representa los minutos.<br /><br /> ss es una cifra de dos dígitos comprendida entre 00 y 59 que representa los segundos.<br /><br /> n* es una cifra de cero a siete dígitos comprendida entre 0 y 9999999 que representa las fracciones de segundos. En informática, las fracciones de segundo se truncarán cuando n > 3.|  
|Longitud en caracteres|19 posiciones como mínimo (AAAA-MM-DD hh:mm:ss) a 27 como máximo (AAAA-MM-DD hh:mm:ss .0000000)|  
|Precisión, escala|De 0 a 7 dígitos, con una precisión de 100 ns. La precisión predeterminada es 7 dígitos.|  
|Tamaño de almacenamiento<sup>1</sup>|6 bytes para una precisión inferior a 3.<br/>7 bytes para las precisiones 3 y 4.<br/>Todas las demás precisiones requieren 8 bytes<sup>2</sup>.|  
|Precisión|100 nanosegundos|  
|Valor predeterminado|1900-01-01 00:00:00|  
|Calendario|Gregoriano|  
|Precisión de fracciones de segundo definida por el usuario|Sí|  
|Conservación y reconocimiento del ajuste de zona horaria|No|  
|Reconocimiento del horario de verano|No|  

<sup>1</sup> Los valores proporcionados son del almacén sin comprimir. El uso de [compresión de datos](../../relational-databases/data-compression/data-compression.md) o [almacén de columnas](../../relational-databases/indexes/columnstore-indexes-overview.md) puede modificar el tamaño de almacenamiento para cada precisión. Además, el tamaño de almacenamiento en disco y en memoria puede diferir. Por ejemplo, los valores **datetime2** requieren siempre 8 bytes de memoria cuando se usa el modo por lotes.

<sup>2</sup> Cuando un valor **datetime2** se convierte en un valor **varbinary**, se agrega un byte adicional al valor **varbinary** para almacenar la precisión.

Para más información sobre los metadatos de tipo de datos, vea [sys.systypes &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md) o [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md). La precisión y escala son variables para algunos tipos de datos de hora y fecha. Para conocer la precisión y la escala de una columna, vea [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md), [COL_LENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/col-length-transact-sql.md) o [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md).
  
## <a name="supported-string-literal-formats-for-datetime2"></a>Formatos de literales de cadena admitidos para datetime2
En las siguientes tablas se enumeran los formatos de literales de cadena ISO 8601 y ODBC admitidos para **datetime2**. Para más información sobre los formatos alfabético, numérico, sin separación y de hora de las partes de fecha y hora de **datetime2**, vea [date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md) y [time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md).
  
|ISO 8601|Descripciones|  
|---|---|
|AAAA-MM-DDThh:mm:ss[.nnnnnnn]<br /><br /> AAAA-MM-DDThh:mm:ss[.nnnnnnn]|Este formato no se ve afectado por la configuración regional de sesión de SET LANGUAGE y SET DATEFORMAT. La letra **T**, los dos puntos (:) y el punto (.) se incluyen en el literal de cadena, por ejemplo, "2007-05-02T19:58:47.1234567".|  
  
|ODBC|Descripción|  
|---|---|
|{ ts 'aaaa-mm-dd hh:mm:ss[.fracciones de segundo]' }|Específico de ODBC API:<br /><br /> El número de dígitos a la derecha del separador decimal, que representa las fracciones de segundo, se puede especificar de 0 a 7 (100 nanosegundos).|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Compatibilidad con ANSI e ISO 8601  
La compatibilidad ANSI e ISO 8601 de [date](../../t-sql/data-types/date-transact-sql.md) y [time](../../t-sql/data-types/time-transact-sql.md) es válida también para **datetime2**.
  
##  <a name="backward-compatibility-for-down-level-clients"></a>Compatibilidad con versiones anteriores de los clientes de niveles inferiores  
Algunos clientes de nivel inferior no admiten los tipos de datos **time**, **date**, **datetime2** y **datetimeoffset**. En la tabla siguiente se muestra la asignación de tipo entre una instancia de nivel superior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los clientes de nivel inferior.
  
|Tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|El formato del literal de cadena predeterminado se pasó al cliente de nivel inferior|ODBC de nivel inferior|OLEDB de nivel inferior|JDBC de nivel inferior|SQLCLIENT de nivel inferior|  
| --- | --- | --- | --- | --- | --- |
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**datetime2**|AAAA-MM-DD hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
  
## <a name="converting-date-and-time-data"></a>Convertir datos de fecha y hora
Cuando se convierte a los tipos de datos de fecha y hora, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rechaza todos los valores que no reconoce como fechas u horas. Para más información sobre cómo usar las funciones CAST y CONVERT con datos de fecha y hora, vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
### <a name="converting-other-date-and-time-types-to-the-datetime2-data-type"></a>Convertir otros tipos de fecha y hora en el tipo de datos datetime2
En esta sección se describe lo que ocurre cuando otros tipos de datos de fecha y hora se convierten en un tipo de datos **datetime2**.  
  
Cuando la conversión es desde **date**, se copian los valores de año, mes y día.  El componente de hora se establece en 00:00:00.0000000.  En el código siguiente se muestran los resultados de convertir un valor `date` en un valor `datetime2`.  
  
```sql
DECLARE @date date = '12-21-16';
DECLARE @datetime2 datetime2 = @date;

SELECT @datetime2 AS '@datetime2', @date AS '@date';
  
--Result  
--@datetime2                  @date
----------------------------- ----------
--2016-12-21 00:00:00.0000000 2016-12-21
```  
  
Cuando la conversión es desde **time(n)**, se copia el componente de hora y el componente de fecha se establece en "1900-01-01". En el siguiente ejemplo se muestran los resultados de convertir un valor `time(7)` en un valor `datetime2`.  
  
```sql
DECLARE @time time(7) = '12:10:16.1234567';
DECLARE @datetime2 datetime2 = @time;

SELECT @datetime2 AS '@datetime2', @time AS '@time';
  
--Result  
--@datetime2                  @time
----------------------------- ----------------
--1900-01-01 12:10:16.1234567 12:10:16.1234567
```  
  
Cuando la conversión es desde **smalldatetime**, se copian las horas y los minutos. Los segundos y las fracciones de segundo se establecen en 0. En el código siguiente se muestran los resultados de convertir un valor `smalldatetime` en un valor `datetime2`.  
  
```sql
DECLARE @smalldatetime smalldatetime = '12-01-16 12:32';
DECLARE @datetime2 datetime2 = @smalldatetime;

SELECT @datetime2 AS '@datetime2', @smalldatetime AS '@smalldatetime'; 
  
--Result  
--@datetime2                  @smalldatetime
----------------------------- -----------------------
--2016-12-01 12:32:00.0000000 2016-12-01 12:32:00 
```  
  
Cuando la conversión es desde **datetimeoffset(n)**, se copian los componentes de fecha y hora. Se trunca la zona horaria. En el siguiente ejemplo se muestran los resultados de convertir un valor `datetimeoffset(7)` en un valor `datetime2`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(7) = '2016-10-23 12:45:37.1234567 +10:0';
DECLARE @datetime2 datetime2 = @datetimeoffset;

SELECT @datetime2 AS '@datetime2', @datetimeoffset AS '@datetimeoffset'; 
  
--Result  
--@datetime2                  @datetimeoffset
----------------------------- ----------------------------------
--2016-10-23 12:45:37.1234567 2016-10-23 12:45:37.1234567 +10:00
```  

Cuando la conversión es desde **datetime**, se copian la fecha y la hora. La precisión de las fracciones se amplía a 7 dígitos. En el siguiente ejemplo se muestran los resultados de convertir un valor `datetime` en un valor `datetime2`.

```sql
DECLARE @datetime datetime = '2016-10-23 12:45:37.333';
DECLARE @datetime2 datetime2 = @datetime;

SELECT @datetime2 AS '@datetime2', @datetime AS '@datetime';
   
--Result  
--@datetime2                  @datetime
------------------------- ---------------------------
--2016-10-23 12:45:37.3333333 2016-10-23 12:45:37.333
```  

> [!NOTE]
> Por debajo del nivel de compatibilidad de base de datos 130, las conversiones implícitas de los tipos de datos datetime a datetime2 muestran una mayor precisión al reflejar las fracciones de milisegundos, lo que se traduce en diferentes valores convertidos, como se observa en el ejemplo anterior. Use una conversión explícita del tipo de datos datetime2 siempre que haya un escenario de comparación mixto entre tipos de datos datetime y datetime2. Para obtener más información, consulte este [artículo del servicio de soporte técnico de Microsoft](https://support.microsoft.com/help/4010261).

### <a name="converting-string-literals-to-datetime2"></a>Convertir literales de cadena a datetime2  
Las conversiones de literales de cadena en tipos de fecha y hora son posibles cuando todas las partes de las cadenas están en formatos válidos. En caso contrario, se generará un error en el tiempo de ejecución. Las conversiones implícitas o explícitas que no especifican un estilo (desde tipos de fecha y hora hasta literales de cadena) estarán en el formato predeterminado de la sesión actual. En la siguiente tabla se muestran las reglas para convertir un literal de cadena al tipo de datos **datetime2**.
  
|Literal de cadena de entrada|**datetime2(n)**|  
|---|---|
|DATE de ODBC|Los literales de cadena de ODBC se asignan al tipo de datos **datetime**. Cualquier operación de asignación de los literales de DATETIME de ODBC a tipos **datetime2** provocará una conversión implícita entre **datetime** y este tipo, tal y como se define en las reglas de conversión.|  
|TIME de ODBC|Vea la regla anterior de DATE de ODBC.|  
|DATETIME DE ODBC|Vea la regla anterior de DATE de ODBC.|  
|Solo DATE|La parte de TIME tiene como valor predeterminado 00:00:00.|  
|Solo TIME|La parte de DATE tiene como valor predeterminado 1900-1-1.|  
|Solo TIMEZONE|Se proporcionan los valores predeterminados.|  
|DATE + TIME|Trivial|  
|DATE + TIMEZONE|No permitido.|  
|TIME + TIMEZONE|La parte de DATE tiene como valor predeterminado 1900-1-1. Se omite la entrada de TIMEZONE.|  
|DATE + TIME + TIMEZONE|Se usará DATETIME local.|  
  
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
  
  
