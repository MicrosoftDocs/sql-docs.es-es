---
description: date (Transact-SQL)
title: date (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- date_TSQL
- date
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], date
- date and time [SQL Server], date
- dates [SQL Server], data types
- date data type [SQL Server]
- data types [SQL Server], date and time
ms.assetid: c963e8b4-5a85-4bd0-9d48-3f8da8f6516b
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c2391433bbbf79b439dc1c3cdf6405afbdae74e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172613"
---
# <a name="date-transact-sql"></a>date (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Define una fecha en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="date-description"></a>Descripción de date
  
|Propiedad|Value|  
|--------------|-----------|  
|Sintaxis|**date**|  
|Uso|DECLARE \@MyDate **date**<br /><br /> CREATE TABLE Table1 ( Column1 **date** )|  
|Formato de literal de cadena predeterminado<br /><br /> (se usa para el cliente de nivel inferior)|YYYY-MM-DD<br /><br /> Para más información, vea la sección "Compatibilidad con versiones anteriores de clientes de niveles inferiores" más adelante.|  
|Intervalo|De 0001-01-01 a 9999-12-31 (de 1582-10-15 a 9999-12-31 para Informatica)<br /><br /> Del 1 de enero del año 1 E. C. al 31 de diciembre de 9999 E. C. (del 15 de octubre de 1582 E. C. al 31 de diciembre de 9999 E. C. para Informatica)|  
|Intervalos de elementos|YYYY es una cifra de cuatro dígitos comprendida entre 0001 y 9999 que representa un año. Para Informatica, YYYY se limita al intervalo entre 1582 y 9999.<br /><br /> MM es una cifra de dos dígitos comprendida entre 01 y 12 que representa un mes del año especificado.<br /><br /> DD es una cifra de dos dígitos comprendida entre 01 y 31, dependiendo del mes, que representa un día del mes especificado.|  
|Longitud en caracteres|10 posiciones|  
|Precisión, escala|10, 0|  
|Tamaño de almacenamiento|3 bytes, fijo|  
|Estructura de almacenamiento|1, fecha de almacenes de enteros de 3 bytes.|  
|Precisión|Un día|  
|Valor predeterminado|1900-01-01<br /><br /> Este valor se usa para la parte de fecha anexada para la conversión implícita de **time** a **datetime2** o **datetimeoffset**.|  
|Calendario|Gregoriano|  
|Precisión de fracciones de segundo definida por el usuario|No|  
|Conservación y reconocimiento del ajuste de zona horaria|No|  
|Reconocimiento del horario de verano|No|  
  
## <a name="supported-string-literal-formats-for-date"></a>Formatos de literales de cadena compatibles para date
En las tablas siguientes se muestran los formatos de literales de cadena válidos para el tipo de datos **date**.
  
|Numeric|Descripción|  
|-------------|-----------------|  
|mdy<br /><br /> [m]m/dd/[aa]aa<br /><br /> [m]m-dd-[aa]aa<br /><br /> [m]m.dd.[aa]aa<br /><br /> mad<br /><br /> mm/[aa]aa/dd<br /><br /> mm-[aa]aa/dd<br /><br /> [m]m.[aa]aa.dd<br /><br /> dma<br /><br /> dd/[m]m/[aa]aa<br /><br /> dd-[m]m-[aa]aa<br /><br /> dd.[m]m.[aa]aa<br /><br /> dam<br /><br /> dd/[aa]aa/[m]m<br /><br /> dd-[aa]aa-[m]m<br /><br /> dd.[aa]aa.[m]m<br /><br /> amd<br /><br /> [aa]aa/[m]m/dd<br /><br /> [aa]aa-[m]m-dd<br /><br /> [aa]aa-[m]m-dd|[m]m, dd, y [aa]aa representan el mes, el día y el año en una cadena con marcas de barra diagonal (/), guiones (-) o puntos (.) como separadores.<br /><br /> Solo se admiten los años de dos o cuatro dígitos. Siempre que sea posible utilice los años de cuatro dígitos. Use la opción Fecha límite de año de dos dígitos (vea [Establecer la opción de configuración del servidor Fecha límite de año de dos dígitos](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)) para especificar un número entero entre 0001 y 9999 que represente el año límite para interpretar años de dos dígitos como años de cuatro dígitos.<br /><br /> **Nota:** Para Informatica, YYYY se limita al intervalo entre 1582 y 9999.<br /><br /> Un año de dos dígitos menor o igual que los últimos dos dígitos del año límite pertenece al mismo siglo que el año límite. Un año de dos dígitos mayor que los últimos dos dígitos del año límite pertenece al siglo anterior al año límite. Por ejemplo, si el valor del año límite de dos dígitos es 2049 (el valor predeterminado), el año de dos dígitos 49 se interpreta como 2049 y el año de dos dígitos 50 se interpreta como 1950.<br /><br /> La configuración de idioma actual está determinada por el formato de fecha predeterminado. Para cambiar el formato de fecha, use las instrucciones [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) y [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md).<br /><br /> No se admite el formato **ydm** para **date**.|  
  
|Alfabético|Descripción|  
|------------------|-----------------|  
|mes [dd][,] aaaa<br /><br /> mes dd[,] [aa]<br /><br /> mes aaaa [dd]<br /><br /> [dd] mes[,] aaaa<br /><br /> dd mes[,][aa]aa<br /><br /> dd [aa]aa mes<br /><br /> [dd] aaaa mes<br /><br /> aaaa mes [dd]<br /><br /> aaaa [dd] mes|**mes** representa el nombre completo del mes o la abreviatura del mes en el idioma actual. Las comas son opcionales y se omite el uso de mayúsculas.<br /><br /> Para evitar ambigüedades, use años de cuatro dígitos.<br /><br /> Si falta el día, se usará el primer día del mes.|  
  
|ISO 8601|Descripción|  
|--------------|----------------|  
|YYYY-MM-DD<br /><br /> YYYYMMDD|Igual que el estándar SQL. Este es el único formato que se define como estándar internacional.|  
  
|Sin separación|Descripción|  
|-----------------|-----------------|  
|[aa]aammdd<br /><br /> aaaa[mm][dd]|Los datos de tipo **date** se pueden especificar con cuatro, seis u ocho dígitos. Una cadena de seis u ocho dígitos se interpreta siempre como **amd**. El mes y el día deben ser siempre de dos dígitos. Una cadena de cuatro dígitos se interpreta como el año.|  
  
|ODBC|Descripción|  
|----------|-----------------|  
|{ d 'aaaa-mm-dd' }|Específico de la API de ODBC.|  
  
|Formato W3C XML|Descripción|  
|--------------------|-----------------|  
|aaaa-mm-ddTZD|Se admite para el uso de XML y SOAP.<br /><br /> DZH es el designador de zona horaria (Z o + hh: mm o -hh:mm).<br /><br /> -   hh:mm representa el desplazamiento de zona horaria. hh es una cifra de dos dígitos, de 0 a 14, que representa el número de horas del ajuste de zona horaria.<br />-   MM es una cifra de dos dígitos, de 0 a 59, que representa el número de minutos adicionales en el desplazamiento de zona horaria.<br />-   + (más) o - (menos) es el signo que se usa obligatoriamente para indicar el desplazamiento de zona horaria. Este signo indica que, para obtener la hora local, el desplazamiento de zona horaria se suma a la hora UTC (Hora universal coordinada) o se resta de ella. El intervalo válido de ajuste de zona horaria es de -14: 00 a +14: 00.|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Compatibilidad con ANSI e ISO 8601  
**date** cumple la definición del estándar ANSI SQL para el calendario Gregoriano: "NOTA 85: Los tipos de datos datetime permitirán que las fechas en el formato Gregoriano se almacenen en el intervalo de fechas de 01-01-0001 CE a 12-31-9999 CE".
  
El formato predeterminado del literal de cadena (que se usa para clientes de nivel inferior) cumple con el formato del estándar SQL, que se define como AAAA-MM-DD. Este formato es igual que la definición de ISO 8601 para DATE.
  
> [!NOTE]  
>  Para Informatica, el intervalo está comprendido entre 1582-10-15 (15 de octubre de 1582 E. C.) y 9999-12-31 31 de diciembre de 9999 E. C.).  
  
## <a name="backward-compatibility-for-down-level-clients"></a>Compatibilidad con versiones anteriores de clientes de niveles inferiores
Algunos clientes de nivel inferior no admiten los tipos de datos **time**, **date**, **datetime2** y **datetimeoffset**. En la tabla siguiente se muestra la asignación de tipo entre una instancia de nivel superior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los clientes de nivel inferior.
  
|Tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|El formato del literal de cadena predeterminado se pasó al cliente de nivel inferior|ODBC de nivel inferior|OLEDB de nivel inferior|JDBC de nivel inferior|SQLCLIENT de nivel inferior|  
| --- | --- | --- | --- | --- | --- |
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**datetime2**|AAAA-MM-DD hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR o SQL_VARCHAR|DBTYPE_WSTR o DBTYPE_STR|Java.sql.String|Cadena o SqString|  
  
## <a name="converting-date-and-time-data"></a>Convertir datos de fecha y hora
Cuando se realiza la conversión a los tipos de datos de fecha y hora, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rechaza todos los valores que no reconoce como fechas u horas. Para más información sobre cómo usar las funciones CAST y CONVERT con datos de fecha y hora, vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
### <a name="converting-date-to-other-date-and-time-types"></a>Convertir tipos date a otros tipos de fecha y hora

En esta tabla se describe lo que ocurre cuando un tipo de datos **date** se convierte a otros tipos de datos de fecha y hora.
  
Cuando es una conversión a **time(n)** , se produce un error en la conversión y se muestra el mensaje de error 206: "Conflicto de tipos de operandos: date es incompatible con time".
  
Si la conversión es a **datetime**, se copia la fecha. En el código siguiente se muestran los resultados de convertir un valor `date` en un valor `datetime`.
  
```sql
DECLARE @date date= '12-10-25';  
DECLARE @datetime datetime= @date;  
  
SELECT @date AS '@date', @datetime AS '@datetime';  
  
--Result  
--@date      @datetime  
------------ -----------------------  
--2025-12-10 2025-12-10 00:00:00.000  
--  
--(1 row(s) affected)  
```  
  
Cuando la conversión es a **smalldatetime** y el valor **date** se encuentra en el intervalo de [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md), el componente de fecha se copia y el componente de hora se establece en 00:00:00.000. Cuando el valor **date** está fuera del rango de un valor **smalldatetime**, se produce el mensaje de error 242: "La conversión de un tipo de datos date a un tipo de datos smalldatetime da como resultado un valor que no se encuentra dentro del intervalo"; y el valor **smalldatetime** se establece en NULL. En el código siguiente se muestran los resultados de convertir un valor `date` en un valor `smalldatetime`.
  
```sql
DECLARE @date date= '1912-10-25';  
DECLARE @smalldatetime smalldatetime = @date;  
  
SELECT @date AS '@date', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@date      @smalldatetime  
------------ -----------------------  
--1912-10-25 1912-10-25 00:00:00  
--  
--(1 row(s) affected)  
```  
  
Cuando la conversión es a **datetimeoffset(n)** , se copia la fecha y la hora se establece en 00:00.0000000 +00:00. En el código siguiente se muestran los resultados de convertir un valor `date` en un valor `datetimeoffset(3)`.
  
```sql
DECLARE @date date = '1912-10-25';  
DECLARE @datetimeoffset datetimeoffset(3) = @date;  
  
SELECT @date AS '@date', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@date      @datetimeoffset  
------------ ------------------------------  
--1912-10-25 1912-10-25 00:00:00.000 +00:00  
--  
--(1 row(s) affected)  
```  
  
Cuando la conversión es a **datetime2(n)** , se copia el componente de hora y el componente de fecha se establece en 00:00.000000. En el código siguiente se muestran los resultados de convertir un valor `date` en un valor `datetime2(3)`.
  
```sql
DECLARE @date date = '1912-10-25'  
DECLARE @datetime2 datetime2(3) = @date;  
  
SELECT @date AS '@date', @datetime2 AS '@datetime2(3)';  
  
--Result  
--@date      @datetime2(3)  
------------ -----------------------  
--1912-10-25 1912-10-25 00:00:00.000  
--  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-date"></a>Convertir literales de cadena en fechas
Las conversiones de literales de cadena a tipos de fecha y hora son posibles cuando todas las partes de las cadenas están en formatos válidos. En caso contrario, se generará un error en el tiempo de ejecución. Las conversiones implícitas o explícitas que no especifican un estilo (desde tipos de fecha y hora hasta literales de cadena) estarán en el formato predeterminado de la sesión actual. En esta tabla se muestran las reglas para convertir un literal de cadena al tipo de datos **date**.
  
|Literal de cadena de entrada|**date**|  
|---|---|
|DATE de ODBC|Los literales de cadena de ODBC se asignan al tipo de datos **datetime**. Cualquier operación de asignación de los literales de DATETIME de ODBC a un tipo **date** provoca una conversión implícita entre **datetime** y el tipo que definen las reglas de conversión.|  
|TIME de ODBC|Vea la regla anterior de DATE de ODBC.|  
|DATETIME DE ODBC|Vea la regla anterior de DATE de ODBC.|  
|Solo DATE|Trivial|  
|Solo TIME|Se proporcionan los valores predeterminados.|  
|Solo TIMEZONE|Se proporcionan los valores predeterminados.|  
|DATE + TIME|Se usa la parte DATE de la cadena de entrada.|  
|DATE + TIMEZONE|No permitido.|  
|TIME + TIMEZONE|Se proporcionan los valores predeterminados.|  
|DATE + TIME + TIMEZONE|Se usará la parte DATE de DATETIME local.|  
  
## <a name="examples"></a>Ejemplos  
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
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|Tipo de datos|Output|  
|---------------|------------|  
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29.1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  

Incorporado por primera vez en SQL Server 2008.

## <a name="see-also"></a>Consulte también
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
