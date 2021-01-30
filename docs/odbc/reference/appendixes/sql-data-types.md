---
description: Tipos de datos SQL
title: SQL Data Types | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL data types [ODBC]
- SQL data types [ODBC], about SQL data types
- data types [ODBC], SQL data types
ms.assetid: 1b22f985-f5e4-4779-87eb-e43329a442b1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e15771f52af0830f07f135453d08f4355a3fae8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199871"
---
# <a name="sql-data-types"></a>Tipos de datos SQL
Cada DBMS define sus propios tipos SQL. Cada controlador ODBC expone solo los tipos de datos de SQL que define el DBMS asociado. La información sobre el modo en que un controlador asigna tipos de SQL DBMS a los identificadores de tipo SQL definidos por ODBC y cómo un controlador asigna tipos SQL de DBMS a sus propios identificadores de tipo SQL específicos del controlador se devuelve a través de una llamada a **SQLGetTypeInfo**. Un controlador también devuelve los tipos de datos de SQL al describir los tipos de datos de las columnas y los parámetros a través de las llamadas a **SQLColAttribute**, **SQLColumns**, **SQLDescribeCol**, **SQLDescribeParam**, **SQLProcedureColumns** y **SQLSpecialColumns**.  
  
> [!NOTE]  
>  Los tipos de datos de SQL se incluyen en los campos SQL_DESC_ CONCISE_TYPE, SQL_DESC_TYPE y SQL_DESC_DATETIME_INTERVAL_CODE de los descriptores de implementación. Las características de los tipos de datos de SQL se incluyen en los campos SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_LENGTH y SQL_DESC_OCTET_LENGTH de los descriptores de implementación. Para obtener más información, vea [identificadores y descriptores de tipos de datos](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) más adelante en este apéndice.  
  
 Un controlador y un origen de datos dados no admiten necesariamente todos los tipos de datos de SQL que se definen en este apéndice. La compatibilidad de un controlador con los tipos de datos SQL depende del nivel de SQL-92 compatible con el controlador. Para determinar el nivel de gramática de SQL-92 compatible con el controlador, una aplicación llama a **SQLGetInfo** con el tipo de información SQL_SQL_CONFORMANCE. Además, un controlador y un origen de datos determinados pueden admitir tipos de datos SQL adicionales específicos del controlador. Para determinar qué tipos de datos admite un controlador, una aplicación llama a **SQLGetTypeInfo**. Para obtener información acerca de los tipos de datos de SQL específicos del controlador, consulte la documentación del controlador. Para obtener información acerca de los tipos de datos de un origen de datos específico, consulte la documentación de ese origen de datos.  
  
> [!IMPORTANT]  
>  Las tablas de este apéndice solo son instrucciones y muestran nombres, intervalos y límites de tipos de datos SQL usados normalmente. Un origen de datos determinado podría admitir solo algunos de los tipos de datos enumerados, y las características de los tipos de datos admitidos pueden diferir de los enumerados.  
  
 En la tabla siguiente se enumeran los identificadores de tipo SQL válidos para todos los tipos de datos de SQL. La tabla también muestra el nombre y la descripción del tipo de datos correspondiente de SQL-92 (si existe).  
  
|Identificador de tipo SQL [1]|Datos de SQL típicos<br /><br /> tipo [2]|Descripción de tipo típico|  
|------------------------------|------------------------------------|------------------------------|  
|SQL_CHAR|CHAR (*n*)|Cadena de caracteres de longitud fija de cadena *n*.|  
|SQL_VARCHAR|VARCHAR (*n*)|Cadena de caracteres de longitud variable con una longitud máxima de cadena *n*.|  
|SQL_LONGVARCHAR|LONG VARCHAR|Datos de caracteres de longitud variable. La longitud máxima depende del origen de datos. 9|  
|SQL_WCHAR|WCHAR (*n*)|Cadena de caracteres Unicode de longitud fija de cadena *n*|  
|SQL_WVARCHAR|VARWCHAR (*n*)|Cadena de caracteres Unicode de longitud variable con una longitud máxima de cadena *n*|  
|SQL_WLONGVARCHAR|LONGWVARCHAR|Datos de caracteres Unicode de longitud variable. La longitud máxima depende del origen de datos|  
|SQL_DECIMAL|DECIMAL (*p*,*s*)|Valor numérico con signo, exacto, con una precisión de al menos *p* y escala *s.* (La precisión máxima está definida por el controlador). (1 <= *p* <= 15; *s*  <=  *p*). 4|  
|SQL_NUMERIC|NUMERIC (*p*,*s*)|Valor numérico con signo y exacto con una precisión *p* y escala *s* (1 <= *p* <= 15; *s*  <=  *p*). 4|  
|SQL_SMALLINT|SMALLINT|Valor numérico exacto con una precisión de 5 y una escala de 0 (con signo:-32.768 <= *n* <= 32.767, sin signo: 0 <= *n* <= 65535) [3].|  
|SQL_INTEGER|INTEGER|Valor numérico exacto con una precisión de 10 y una escala de 0 (con signo:-2 [31] <= *n* <= 2 [31]-1, sin signo: 0 <= *n* <= 2 [32]-1) [3].|  
|SQL_REAL|real|Valor numérico con signo, aproximado, con una precisión binaria de 24 (cero o valor absoluto de 10 [-38] a 10 [38]).|  
|SQL_FLOAT|FLOAT (*p*)|Valor numérico con signo, aproximado, con una precisión binaria de al menos *p*. (La precisión máxima está definida por el controlador). 5|  
|SQL_DOUBLE|DOUBLE PRECISION|Valor numérico con signo, aproximado, con una precisión binaria 53 (cero o valor absoluto de 10 [-308] a 10 [308]).|  
|SQL_BIT|BIT|Datos binarios de un solo bit. 203|  
|SQL_TINYINT|TINYINT|Valor numérico exacto con una precisión de 3 y una escala de 0 (con signo:-128 <= *n* <= 127, sin signo: 0 <= *n* <= 255) [3].|  
|SQL_BIGINT|bigint|Valor numérico exacto con una precisión de 19 (si está con signo) o 20 (si es sin signo) y escala 0 (con signo:-2 [63] <= *n* <= 2 [63]-1, sin signo: 0 <= *n* <= 2 [64]-1) [3], [9].|  
|SQL_BINARY|BINARIo (*n*)|Datos binarios de longitud fija *n*. 9|  
|SQL_VARBINARY|VARBINARY (*n*)|Datos binarios de longitud variable de longitud máxima *n*. El valor máximo lo establece el usuario. 9|  
|SQL_LONGVARBINARY|VARBINARY LARGO|Datos binarios de longitud variable. La longitud máxima depende del origen de datos. 9|  
|SQL_TYPE_DATE [6]|DATE|Campos de año, mes y día, conforme a las reglas del calendario gregoriano. (Vea [restricciones del calendario gregoriano](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md), más adelante en este apéndice).|  
|SQL_TYPE_TIME [6]|HORA (*p*)|Campos de hora, minuto y segundo, con valores válidos para horas de 00 a 23, valores válidos para minutos de 00 a 59 y valores válidos para segundos de 00 a 61. La precisión *p* indica la precisión en segundos.|  
|SQL_TYPE_TIMESTAMP [6]|MARCA de tiempo (*p*)|Campos año, mes, día, hora, minuto y segundo, con valores válidos tal y como se definen para los tipos de datos de fecha y hora.|  
|SQL_TYPE_UTCDATETIME|UTCDATETIME|Campos año, mes, día, hora, minuto, segundo, utchour y utcminute. Los campos utchour y utcminute tienen una precisión de 1/10 microsegundo.|  
|SQL_TYPE_UTCTIME|UTCTIME|Campos de hora, minuto, segundo, utchour y utcminute. Los campos utchour y utcminute tienen una precisión de 1/10 microsegundo.|  
|SQL_INTERVAL_MONTH [7]|INTERVALO mensual (*p*)|Número de meses entre dos fechas; *p* es la precisión inicial del intervalo.|  
|SQL_INTERVAL_YEAR [7]|INTERVALO año (*p*)|Número de años entre dos fechas; *p* es la precisión inicial del intervalo.|  
|SQL_INTERVAL_YEAR_TO_MONTH [7]|INTERVALO año (*p*) al mes|Número de años y meses entre dos fechas; *p* es la precisión inicial del intervalo.|  
|SQL_INTERVAL_DAY [7]|INTERVALO DAY (*p*)|Número de días entre dos fechas; *p* es la precisión inicial del intervalo.|  
|SQL_INTERVAL_HOUR [7]|INTERVALO (hora) (*p*)|Número de horas entre dos fechas/horas; *p* es la precisión inicial del intervalo.|  
|SQL_INTERVAL_MINUTE [7]|INTERVALO minuto (*p*)|Número de minutos entre dos fechas/horas; *p* es la precisión inicial del intervalo.|  
|SQL_INTERVAL_SECOND [7]|INTERVALO segundo (*p*,*q*)|Número de segundos entre dos fechas/horas; *p* es la precisión inicial del intervalo y *q* es la precisión de los segundos de intervalo.|  
|SQL_INTERVAL_DAY_TO_HOUR [7]|INTERVAL DAY (*p*) a hour|Número de días/horas entre dos fechas/horas; *p* es la precisión inicial del intervalo.|  
|SQL_INTERVAL_DAY_TO_MINUTE [7]|INTERVAL DAY (*p*) a minute|Número de días/horas/minutos entre dos fechas/horas; *p* es la precisión inicial del intervalo.|  
|SQL_INTERVAL_DAY_TO_SECOND [7]|INTERVAL DAY (*p*) a Second (*q*)|Número de días/horas/minutos/segundos entre dos fechas/horas; *p* es la precisión inicial del intervalo y *q* es la precisión de los segundos de intervalo.|  
|SQL_INTERVAL_HOUR_TO_MINUTE [7]|INTERVALO de horas (*p*) a minuto|Número de horas/minutos entre dos fechas/horas; *p* es la precisión inicial del intervalo.|  
|SQL_INTERVAL_HOUR_TO_SECOND [7]|INTERVAL HOUR (*p*) to Second (*q*)|Número de horas/minutos/segundos entre dos fechas/horas; *p* es la precisión inicial del intervalo y *q* es la precisión de los segundos de intervalo.|  
|SQL_INTERVAL_MINUTE_TO_SECOND [7]|INTERVAL MINUTE (*p*) a Second (*q*)|Número de minutos/segundos entre dos fechas/horas; *p* es la precisión inicial del intervalo y *q* es la precisión de los segundos de intervalo.|  
|SQL_GUID|GUID|GUID de longitud fija.|  
  
 [1] este es el valor devuelto en la columna DATA_TYPE mediante una llamada a **SQLGetTypeInfo**.  
  
 [2] este es el valor devuelto en la columna Nombre y crear parámetros mediante una llamada a **SQLGetTypeInfo**. La columna NAME devuelve la designación; por ejemplo, CHAR-, mientras que la columna CREATE PARAMs devuelve una lista separada por comas de parámetros de creación como precisión, escala y longitud.  
  
 [3] una aplicación utiliza **SQLGetTypeInfo** o **SQLColAttribute** para determinar si un tipo de datos determinado o una columna determinada de un conjunto de resultados es sin signo.  
  
 [4] SQL_DECIMAL y los tipos de datos de SQL_NUMERIC solo difieren en su precisión. La precisión de un DECIMAL (*p*,*s*) es una precisión decimal definida por la implementación que no es menor que *p*, mientras que la precisión de un Numeric (*p*,*s*) es exactamente igual a *p*.  
  
 [5] dependiendo de la implementación, la precisión de SQL_FLOAT puede ser 24 o 53: si es 24, el tipo de datos SQL_FLOAT es igual que SQL_REAL; Si es 53, el tipo de datos SQL_FLOAT es el mismo que el de SQL_DOUBLE.  
  
 [6] en ODBC *3. x*, los tipos de datos SQL Date, Time y timestamp son SQL_TYPE_DATE, SQL_TYPE_TIME y SQL_TYPE_TIMESTAMP, respectivamente; en ODBC *2. x*, los tipos de datos son SQL_DATE, SQL_TIME y SQL_TIMESTAMP.  
  
 [7] para obtener más información sobre los tipos de datos SQL de intervalo, vea la sección [tipos de datos de intervalo](../../../odbc/reference/appendixes/interval-data-types.md) , más adelante en este apéndice.  
  
 [8] el tipo de datos SQL_BIT tiene características diferentes de las del tipo BIT en SQL-92.  
  
 [9] este tipo de datos no tiene un tipo de datos correspondiente en SQL-92.  
  
 En esta sección se proporciona el siguiente ejemplo.  
  
-   [Conjunto de resultados de SQLGetTypeInfo de ejemplo](../../../odbc/reference/appendixes/example-sqlgettypeinfo-result-set.md)
