---
title: Tipos de datos y funciones de fecha y hora
description: Vínculos a los artículos sobre tipos de datos y funciones de fecha y hora.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 09/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- dates [SQL Server]
- date and time [SQL Server], all data types and functions
- date and time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 83e378a2-6e89-4c80-bc4f-644958d9e0a9
author: cawrites
ms.author: chadam
monikerRange: = azure-sqldw-latest||= azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: b1147dbb76217a819716a7ec740017ee5f4d4397
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183553"
---
# <a name="date-and-time-data-types-and-functions-transact-sql"></a>Tipos de datos y funciones de fecha y hora (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

En las secciones de este tema se describen todos los tipos de datos y funciones de fecha y hora de [!INCLUDE[tsql](../../includes/tsql-md.md)].

- [Tipos de datos de fecha y hora](#DateandTimeDataTypes)
- [Funciones de fecha y hora](#DateandTimeFunctions)
  - [Función que devuelve valores de fecha y hora del sistema](#GetSystemDateandTimeValues)
  - [Funciones que devuelven partes de fecha y hora](#GetDateandTimeParts)
  - [Funciones que devuelven valores de fecha y hora de sus elementos](#fromParts)
  - [Funciones que devuelven valores de diferencia de fecha y hora](#GetDateandTimeDifference)
  - [Funciones que modifican valores de fecha y hora](#ModifyDateandTimeValues)
  - [Funciones que establecen o devuelven funciones de formato de sesión](#SetorGetSessionFormatFunctions)
  - [Funciones que validan valores de fecha y hora](#ValidateDateandTimeValues)
- [Temas relacionados con la fecha y hora](#DateandTimeRelatedTopics)

## <a name="date-and-time-data-types"></a><a name="DateandTimeDataTypes"></a> Tipos de datos de fecha y hora

En la tabla siguiente se muestran los tipos de datos de fecha y hora de [!INCLUDE[tsql](../../includes/tsql-md.md)]:

|Tipo de datos|Formato|Intervalo|Precisión|Tamaño de almacenamiento (bytes)|Precisión de fracciones de segundo definida por el usuario|Ajuste de zona horaria|
|---|---|---|---|---|---|---|
|[time](../../t-sql/data-types/time-transact-sql.md)|hh:mm:ss[.nnnnnnn]|De 00:00:00.0000000 a 23:59:59.9999999|100 nanosegundos|3 to 5|Sí|No|
|[date](../../t-sql/data-types/date-transact-sql.md)|YYYY-MM-DD|De 0001-01-01 a 31.12.99|1 día|3|No|No|
|[smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md)|AAAA-MM-DD hh:mm:ss|De 1900-01-01 a 2079-06-06|1 minuto.|4|No|No|
|[datetime](../../t-sql/data-types/datetime-transact-sql.md)|AAAA-MM-DD hh:mm:ss[.nnn]|De 1753-01-01 a 9999-12-31|0,00333 segundos|8|No|No|
|[datetime2](../../t-sql/data-types/datetime2-transact-sql.md)|AAAA-MM-DD hh:mm:ss[.nnnnnnn]|De 0001-01-01 00:00:00.0000000 a 9999-12-31 23:59:59.9999999|100 nanosegundos|De 6 a 8|Sí|No|
|[datetimeoffset](../../t-sql/data-types/datetimeoffset-transact-sql.md)|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|De 0001-01-01 00:00:00.0000000 a 9999-12-31 23:59:59.9999999 (en UTC)|100 nanosegundos|De 8 a 10|Sí|Sí|

> [!NOTE]
> El tipo de datos [rowversion ](../../t-sql/data-types/rowversion-transact-sql.md) de [!INCLUDE[tsql](../../includes/tsql-md.md)] no es un tipo de datos de fecha y hora. **timestamp** es un sinónimo en desuso de **rowversion**.

## <a name="date-and-time-functions"></a><a name="DateandTimeFunctions"></a> Funciones de fecha y hora

En las tablas siguientes se enumeran las funciones de fecha y hora de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para obtener más información sobre el determinismo, vea [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

### <a name="function-that-return-system-date-and-time-values"></a><a name="GetSystemDateandTimeValues"></a> Función que devuelve valores de fecha y hora del sistema

[!INCLUDE[tsql](../../includes/tsql-md.md)] deriva todos los valores de fecha y hora del sistema operativo del equipo en el que se ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

#### <a name="higher-precision-system-date-and-time-functions"></a>Funciones de fecha y hora del sistema de precisión elevada

[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] deriva los valores de fecha y hora mediante la API de Windows GetSystemTimeAsFileTime(). La exactitud depende del hardware del equipo y de la versión de Windows en la que se ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La precisión de esta API se fija en 100 nanosegundos. Use la API de Windows GetSystemTimeAdjustment() para determinar la precisión.

|Función|Sintaxis|Valor devuelto|Tipo de datos devuelto|Determinismo|
|---|---|---|---|---|
|[SYSDATETIME](../../t-sql/functions/sysdatetime-transact-sql.md)|SYSDATETIME ()|Devuelve un valor **datetime2(7)** que contiene la fecha y hora del equipo en el que se ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El valor devuelto no incluye el desplazamiento de zona horaria.|**datetime2(7)**|No determinista|
|[SYSDATETIMEOFFSET](../../t-sql/functions/sysdatetimeoffset-transact-sql.md)|SYSDATETIMEOFFSET ( )|Devuelve un valor **datetimeoffset(7)** que contiene la fecha y hora del equipo en el que se ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El valor devuelto incluye el desplazamiento de zona horaria.|**datetimeoffset(7)**|No determinista|
|[SYSUTCDATETIME](../../t-sql/functions/sysutcdatetime-transact-sql.md)|SYSUTCDATETIME ( )|Devuelve un valor **datetime2(7)** que contiene la fecha y hora del equipo en el que se ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La función devuelve los valores de fecha y hora como una hora universal coordinada (UTC).|**datetime2(7)**|No determinista|

#### <a name="lower-precision-system-date-and-time-functions"></a>Funciones de fecha y hora del sistema de precisión baja

|Función|Sintaxis|Valor devuelto|Tipo de datos devuelto|Determinismo|
|---|---|---|---|---|
|[CURRENT_TIMESTAMP](../../t-sql/functions/current-timestamp-transact-sql.md)|CURRENT_TIMESTAMP|Devuelve un valor **datetime** que contiene la fecha y hora del equipo en el que se ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El valor devuelto no incluye el desplazamiento de zona horaria.|**datetime**|No determinista|
|[GETDATE](../../t-sql/functions/getdate-transact-sql.md)|GETDATE ( )|Devuelve un valor **datetime** que contiene la fecha y hora del equipo en el que se ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El valor devuelto no incluye el desplazamiento de zona horaria.|**datetime**|No determinista|
|[GETUTCDATE](../../t-sql/functions/getutcdate-transact-sql.md)|GETUTCDATE ( )|Devuelve un valor **datetime** que contiene la fecha y hora del equipo en el que se ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La función devuelve los valores de fecha y hora como una hora universal coordinada (UTC).|**datetime**|No determinista|

### <a name="functions-that-return-date-and-time-parts"></a><a name="GetDateandTimeParts"></a> Funciones que devuelven partes de fecha y hora

|Función|Sintaxis|Valor devuelto|Tipo de datos devuelto|Determinismo|
|--------------|------------|------------------|----------------------|-----------------|
|[DATENAME](../../t-sql/functions/datename-transact-sql.md)|DATENAME (*datepart*, *date*)|Devuelve una cadena de caracteres que representa el parámetro *datepart* especificado de la fecha especificada.|**nvarchar**|No determinista|
|[DATEPART](../../t-sql/functions/datepart-transact-sql.md)|DATEPART (*datepart*, *date*)|Devuelve un entero que representa el parámetro *datepart* especificado del parámetro *date* especificado.|**int**|No determinista|
|[DAY](../../t-sql/functions/day-transact-sql.md)|DAY (*date*)|Devuelve un entero que representa la parte del día del parámetro *date* especificado.|**int**|Determinista|
|[MONTH](../../t-sql/functions/month-transact-sql.md)|MONTH (*date*)|Devuelve un entero que representa la parte del mes del parámetro *date* especificado.|**int**|Determinista|
|[YEAR](../../t-sql/functions/year-transact-sql.md)|YEAR (*date*)|Devuelve un entero que representa la parte del año del parámetro *date* especificado.|**int**|Determinista|

### <a name="functions-that-return-date-and-time-values-from-their-parts"></a><a name="fromParts"></a> Funciones que devuelven valores de fecha y hora de sus partes

|Función|Sintaxis|Valor devuelto|Tipo de datos devuelto|Determinismo|
|---|---|---|---|---|
|[DATEFROMPARTS](../../t-sql/functions/datefromparts-transact-sql.md)|DATEFROMPARTS ( *año*, *mes*, *día* )|Devuelve un valor de tipo **date** para el año, mes y día especificados.|**date**|Determinista|
|[DATETIME2FROMPARTS](../../t-sql/functions/datetime2fromparts-transact-sql.md)|DATETIME2FROMPARTS ( *año*, *mes*, *día*, *hora*, *minuto*, *segundos*, *fracciones*, *precisión*)|Devuelve un valor **datetime2** para la fecha y la hora especificadas, con la precisión indicada.|**datetime2(** _precision_ **)**|Determinista|
|[DATETIMEFROMPARTS](../../t-sql/functions/datetimefromparts-transact-sql.md)|DATETIMEFROMPARTS ( *año*, *mes*, *día*, *hora*, *minuto*, *segundos*, *milisegundos*)|Devuelve un valor **datetime** para la fecha y la hora especificadas.|**datetime**|Determinista|
|[DATETIMEOFFSETFROMPARTS](../../t-sql/functions/datetimeoffsetfromparts-transact-sql.md)|DATETIMEOFFSETFROMPARTS ( *año*, *mes*, *día*, *hora*, *minuto*, *segundos*, *fracciones*, *desplazamiento_de_hora*, *desplazamiento_de_minuto*, *precisión*)|Devuelve un valor **datetimeoffset** para la fecha y la hora especificadas, con los desplazamientos y la precisión indicados.|**datetimeoffset(** _precision_ **)**|Determinista|
|[SMALLDATETIMEFROMPARTS](../../t-sql/functions/smalldatetimefromparts-transact-sql.md)|SMALLDATETIMEFROMPARTS ( *año*, *mes*, *día*, *hora*, *minuto* )|Devuelve un valor **smalldatetime** de la fecha y la hora especificadas.|**smalldatetime**|Determinista|
|[TIMEFROMPARTS](../../t-sql/functions/timefromparts-transact-sql.md)|TIMEFROMPARTS ( *hora*, *minuto*, *segundos*, *fracciones*, *precisión* )|Devuelve un valor **time** para la hora especificada, con la precisión indicada.|**time(** _precision_ **)**|Determinista|

### <a name="functions-that-return-date-and-time-difference-values"></a><a name="GetDateandTimeDifference"></a> Funciones que devuelven valores de diferencia de fecha y hora

|Función|Sintaxis|Valor devuelto|Tipo de datos devuelto|Determinismo|
|---|---|---|---|---|
|[DATEDIFF](../../t-sql/functions/datediff-transact-sql.md)|DATEDIFF (*datepart*, *startdate*, *enddate*)|Devuelve el número de límites *datepart* de fecha y hora que se han superado entre dos fechas especificadas.|**int**|Determinista|
|[DATEDIFF_BIG](../../t-sql/functions/datediff-big-transact-sql.md)|DATEDIFF_BIG (*datepart*, *startdate*, *enddate*)|Devuelve el número de límites *datepart* de fecha y hora que se han superado entre dos fechas especificadas.|**bigint**|Determinista|

### <a name="functions-that-modify-date-and-time-values"></a><a name="ModifyDateandTimeValues"></a> Funciones que modifican valores de fecha y hora

|Función|Sintaxis|Valor devuelto|Tipo de datos devuelto|Determinismo|
|---|---|---|---|---|
|[DATEADD](../../t-sql/functions/dateadd-transact-sql.md)|DATEADD (*datepart*, *number*, *date*)|Devuelve un nuevo valor **datetime** al agregar un intervalo al *datepart* especificado del *date* indicado.|El tipo de datos del argumento *date*|Determinista|
|[EOMONTH](../../t-sql/functions/eomonth-transact-sql.md)|EOMONTH ( *fecha_inicio* [, *mes_para_agregar* ] )|Devuelve el último día del mes que contiene la fecha especificada, con un desplazamiento opcional.|El tipo de valor devuelto es el tipo del argumento *start_date*, o bien como alternativa, el tipo de datos **date**.|Determinista|
|[SWITCHOFFSET](../../t-sql/functions/switchoffset-transact-sql.md)|SWITCHOFFSET (*DATETIMEOFFSET*, *time_zone*)|SWITCHOFFSET cambia el desplazamiento de zona horaria de un valor DATETIMEOFFSET y conserva el valor UTC.|**datetimeoffset** con la precisión fraccionaria del argumento *DATETIMEOFFSET*|Determinista|
|[TODATETIMEOFFSET](../../t-sql/functions/todatetimeoffset-transact-sql.md)|TODATETIMEOFFSET (*expression*, *time_zone*)|TODATETIMEOFFSET transforma un valor datetime2 en un valor datetimeoffset. *TODATETIMEOFFSET* interpreta el valor datetime2 en la hora local, para el valor time_zone especificado.|**datetimeoffset** con la precisión fraccionaria del argumento *datetime*|Determinista|

### <a name="functions-that-set-or-return-session-format-functions"></a><a name="SetorGetSessionFormatFunctions"></a> Funciones que establecen o devuelven funciones de formato de sesión

|Función|Sintaxis|Valor devuelto|Tipo de datos devuelto|Determinismo|
|---|---|---|---|---|
|[@@DATEFIRST](../../t-sql/functions/datefirst-transact-sql.md)|@@DATEFIRST|Devuelve el valor actual, para la sesión, de SET DATEFIRST.|**tinyint**|No determinista|
|[SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md)|SET DATEFIRST { *number* &#124; **\@**_number_var_ }|Establece el primer día de la semana en un número del 1 al 7.|No aplicable|No aplicable|
|[SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)|SET DATEFORMAT { *formato* &#124; **@**_format_var_ }|Determina el orden de los componentes de la fecha (mes/día/año) para escribir datos de tipo **datetime** o **smalldatetime**.|No aplicable|No aplicable|
|[@@LANGUAGE](../../t-sql/functions/language-transact-sql.md)|@@LANGUAGE|Devuelve el nombre del idioma que se está usando. @@LANGUAGE no es ninguna función de fecha u hora. Sin embargo, la configuración de idioma puede afectar a la salida de las funciones de fecha.|No aplicable|No aplicable|
|[SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md)|SET LANGUAGE { [ N ] **'**_language_**'** &#124; **\@**_language_var_ }|Establece el entorno del idioma de la sesión y los mensajes del sistema. SET LANGUAGE no es ninguna función de fecha u hora. Sin embargo, la configuración de idioma afecta a la salida de las funciones de fecha.|No aplicable|No aplicable|
|[sp_helplanguage](../../relational-databases/system-stored-procedures/sp-helplanguage-transact-sql.md)|**sp_helplanguage** [ [ **@language =** ] **"**_idioma_**"** ]|Devuelve información sobre los formatos de fecha de todos los idiomas compatibles. **sp_helplanguage** no es un procedimiento almacenado de fecha u hora. Sin embargo, la configuración de idioma afecta a la salida de las funciones de fecha.|No aplicable|No aplicable|

### <a name="functions-that-validate-date-and-time-values"></a><a name="ValidateDateandTimeValues"></a> Funciones que validan valores de fecha y hora

|Función|Sintaxis|Valor devuelto|Tipo de datos devuelto|Determinismo|
|---|---|---|---|---|
|[ISDATE](../../t-sql/functions/isdate-transact-sql.md)|ISDATE (*expression*)|Determina si una expresión de entrada **datetime** o **smalldatetime** tiene un valor de fecha u hora válido.|**int**|ISDATE solo es determinista si se usa con la función CONVERT, cuando se especifica el parámetro de estilo CONVERT y cuando el estilo no es igual a 0, 100, 9 ni 109.|

## <a name="date-and-time-related-topics"></a><a name="DateandTimeRelatedTopics"></a> Temas relacionados con la fecha y hora

|Tema|Descripción|
|-----------|-----------------|
|[FORMAT](../../t-sql/functions/format-transact-sql.md)|Devuelve un valor con formato con el formato y la referencia cultural opcional especificados. Use la función FORMAT para aplicar formato específico de la configuración regional de los valores de fecha/hora y de número como cadenas.|
|[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)|Proporciona información sobre la conversión de los valores de fecha y hora de literales de cadena y otros formatos de fecha y hora.|
|[Escribir instrucciones Transact-SQL internacionales](../../relational-databases/collations/write-international-transact-sql-statements.md)|Proporciona directrices para la portabilidad de bases datos y aplicaciones de bases de datos que utilizan instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] de un idioma a otro, o que admiten varios idiomas.|
|[Funciones escalares de ODBC &#40;Transact-SQL&#41;](../../t-sql/functions/odbc-scalar-functions-transact-sql.md)|Proporciona información sobre funciones escalares de ODBC que se pueden usar en instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)]. Esto incluye las funciones de fecha y hora ODBC.|
|[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)|Proporciona la conversión de zona horaria.|

## <a name="see-also"></a>Consulte también

- [Funciones](../../t-sql/functions/functions.md)
- [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
