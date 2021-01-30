---
description: Funciones de hora, fecha e intervalo
title: Funciones de hora, fecha e intervalo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], time functions
- functions [ODBC], date functions
- interval functions [ODBC]
- functions [ODBC], interval functions
- time functions [ODBC]
- date functions [ODBC]
ms.assetid: bdf054a0-7aba-4e99-a34a-799917376fd5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e73d295613f3f11b72317d20b49a6c75be802cbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202497"
---
# <a name="time-date-and-interval-functions"></a>Funciones de hora, fecha e intervalo
En la tabla siguiente se enumeran las funciones de fecha y hora que se incluyen en el conjunto de funciones escalares de ODBC. Una aplicación puede determinar qué funciones de fecha y hora son compatibles con un controlador mediante una llamada a **SQLGetInfo** con un *tipo de información* de SQL_TIMEDATE_FUNCTIONS.  
  
 Los argumentos indicados como *timestamp_exp* pueden ser el nombre de una columna, el resultado de otra función escalar o un valor de ODBC- *Time-* escape, *ODBC-Date-escape* u *ODBC-timestamp-escape*, donde el tipo de datos subyacente podría representarse como SQL_CHAR, SQL_VARCHAR, SQL_TYPE_TIME, SQL_TYPE_DATE o SQL_TYPE_TIMESTAMP.  
  
 Los argumentos indicados como *date_exp* pueden ser el nombre de una columna, el resultado de otra función escalar o un *ODBC-Date-escape* u *ODBC-timestamp-escape*, donde el tipo de datos subyacente podría representarse como SQL_CHAR, SQL_VARCHAR, SQL_TYPE_DATE o SQL_TYPE_TIMESTAMP.  
  
 Los argumentos indicados como *time_exp* pueden ser el nombre de una columna, el resultado de otra función escalar o un valor de ODBC- *Time-escape* u *ODBC-timestamp-escape*, donde el tipo de datos subyacente podría representarse como SQL_CHAR, SQL_VARCHAR, SQL_TYPE_TIME o SQL_TYPE_TIMESTAMP.  
  
 Las funciones escalares CURRENT_DATE, CURRENT_TIME y CURRENT_TIMESTAMP TimeDate se han agregado en ODBC 3,0 para que se alineen con SQL-92.  
  
|Función|Descripción|  
|--------------|-----------------|  
|**CURRENT_DATE ()** (ODBC 3,0)|Devuelve la fecha actual.|  
|**CURRENT_TIME [(** *precisión temporal* **)]** (ODBC 3,0)|Devuelve la hora local actual. El argumento de *precisión temporal* determina la precisión en segundos del valor devuelto.|  
|**CURRENT_TIMESTAMP**<br /> **[(** *marca de tiempo-precisión* **)]** (ODBC 3,0)|Devuelve la fecha local y la hora local actuales como un valor de marca de tiempo. El argumento *timestamp-Precision* determina la precisión en segundos de la marca de tiempo devuelta.|  
|**CURDATE ()** (ODBC 1,0)|Devuelve la fecha actual.|  
|**CURTIME ()** (ODBC 1,0)|Devuelve la hora local actual.|  
|**DAYNAME (** *date_exp* **)** (ODBC 2,0)|Devuelve una cadena de caracteres que contiene el nombre específico del origen de datos del día (por ejemplo, de domingo a sábado o de sol. a Sat. para un origen de datos que usa inglés, o bien Sonntag a Samstag para un origen de datos que usa el alemán) para la parte del día de *date_exp*.|  
|**DAYOFMONTH (** *date_exp* **)** (ODBC 1,0)|Devuelve el día del mes basándose en el campo de mes en *date_exp* como un valor entero en el intervalo de 1-31.|  
|**DAYOFWEEK (** *date_exp* **)** (ODBC 1,0)|Devuelve el día de la semana basándose en el campo de la semana en *date_exp* como un valor entero en el intervalo de 1-7, donde 1 representa el domingo.|  
|**DAYOFYEAR (** *date_exp* **)** (ODBC 1,0)|Devuelve el día del año basándose en el campo año de *date_exp* como un valor entero en el intervalo de 1-366.|  
|**Extract (** *Extract-Field desde* *Extract-Source* **)** (ODBC 3,0)|Devuelve la parte de *Extracto-campo* del *origen* de la extracción. El argumento *Extract-Source* es una expresión de fecha y hora o de intervalo. El argumento *Extract-Field* puede ser una de las palabras clave siguientes:<br /><br /> AÑO MES/MINUTO HORA SEGUNDO<br /><br /> La precisión del valor devuelto está definida por la implementación. La escala es 0 a menos que se especifique SECOND, en cuyo caso la escala no es menor que la precisión de las fracciones de segundo del campo de origen de la *extracción* .|  
|**Hora (** *time_exp* **)** (ODBC 1,0)|Devuelve la hora según el campo de hora de *time_exp* como un valor entero en el intervalo de 0-23.|  
|**Minute (** *time_exp* **)** (ODBC 1,0)|Devuelve el minuto según el campo de minuto en *time_exp* como un valor entero en el intervalo de 0-59.|  
|**Month (** *date_exp* **)** (ODBC 1,0)|Devuelve el mes basado en el campo de mes en *date_exp* como un valor entero en el intervalo de 1-12.|  
|**MONTHNAME (** *date_exp* **)** (ODBC 2,0)|Devuelve una cadena de caracteres que contiene el nombre específico del origen de datos del mes (por ejemplo, de enero a diciembre o de ene. hasta Dec. para un origen de datos que usa inglés o bien Januar a Dezember para un origen de datos que usa el alemán) para la parte de mes de *date_exp*.|  
|**Now ()** (ODBC 1,0)|Devuelve la fecha y hora actuales como un valor de marca de tiempo.|  
|**Quarter (** *date_exp* **)** (ODBC 1,0)|Devuelve el trimestre en *date_exp* como un valor entero en el intervalo de 1-4, donde 1 representa el 1 de enero hasta el 31 de marzo.|  
|**Second (** *time_exp* **)** (ODBC 1,0)|Devuelve el segundo basándose en el segundo campo de *time_exp* como un valor entero en el intervalo de 0-59.|
|**TIMESTAMPADD (** *intervalo*, *integer_exp*, *timestamp_exp* **)** (ODBC 2,0)|Devuelve la marca de tiempo calculada agregando *integer_exp* intervalos de tipo *Interval* a *timestamp_exp*. Los valores válidos de *Interval* son las palabras clave siguientes:<br /><br /> SQL_TSI_FRAC_SECOND<br /><br /> SQL_TSI_SECOND<br /><br /> SQL_TSI_MINUTE<br /><br /> SQL_TSI_HOUR<br /><br /> SQL_TSI_DAY<br /><br /> SQL_TSI_WEEK<br /><br /> SQL_TSI_MONTH<br /><br /> SQL_TSI_QUARTER<br /><br /> SQL_TSI_YEAR<br /><br /> donde las fracciones de segundo se expresan en milmillonésimas de segundo. Por ejemplo, la siguiente instrucción SQL devuelve el nombre de cada empleado y su fecha de aniversario de un año:<br /><br /> `SELECT NAME, {fn  TIMESTAMPADD(SQL_TSI_YEAR, 1, HIRE_DATE)} FROM  EMPLOYEES`<br /><br /> Si *timestamp_exp* es un valor de hora y *Interval* especifica días, semanas, meses, trimestres o años, la parte de la fecha de *timestamp_exp* se establece en la fecha actual antes de calcular la marca de tiempo resultante.<br /><br /> Si *timestamp_exp* es un valor de fecha y *Interval* especifica fracciones de segundos, segundos, minutos u horas, la parte de la hora de *timestamp_exp* se establece en 0 antes de calcular la marca de tiempo resultante.<br /><br /> Una aplicación determina los intervalos que admite un origen de datos mediante una llamada a **SQLGetInfo** con la opción SQL_TIMEDATE_ADD_INTERVALS.|  
|**TIMESTAMPDIFF (** *intervalo*, *timestamp_exp1*, *timestamp_exp2* **)** (ODBC 2,0)|Devuelve el número entero de intervalos de tipo *Interval* por el que *timestamp_exp2* es mayor que *timestamp_exp1*. Los valores válidos de *Interval* son las palabras clave siguientes:<br /><br /> SQL_TSI_FRAC_SECOND<br /><br /> SQL_TSI_SECOND<br /><br /> SQL_TSI_MINUTE<br /><br /> SQL_TSI_HOUR<br /><br /> SQL_TSI_DAY<br /><br /> SQL_TSI_WEEK<br /><br /> SQL_TSI_MONTH<br /><br /> SQL_TSI_QUARTER<br /><br /> SQL_TSI_YEAR<br /><br /> donde las fracciones de segundo se expresan en milmillonésimas de segundo. Por ejemplo, la siguiente instrucción SQL devuelve el nombre de cada empleado y el número de años empleados:<br /><br /> `SELECT NAME, {fn  TIMESTAMPDIFF(SQL_TSI_YEAR, {fn CURDATE()}, HIRE_DATE)} FROM EMPLOYEES`<br /><br /> Si una de las expresiones de marca de tiempo es un valor de hora e *Interval* especifica días, semanas, meses, trimestres o años, la parte de fecha de esa marca de tiempo se establece en la fecha actual antes de calcular la diferencia entre las marcas de tiempo.<br /><br /> Si alguna de las expresiones de marca de tiempo es un valor de fecha y *Interval* especifica las fracciones de segundo, segundos, minutos u horas, la parte de hora de esa marca de tiempo se establece en 0 antes de calcular la diferencia entre las marcas de tiempo.<br /><br /> Una aplicación determina los intervalos que admite un origen de datos mediante una llamada a **SQLGetInfo** con la opción SQL_TIMEDATE_DIFF_INTERVALS.|  
|**Semana (** *date_exp* **)** (ODBC 1,0)|Devuelve la semana del año basándose en el campo de la semana en *date_exp* como un valor entero en el intervalo de 1-53.|  
|**Año (** *date_exp* **)** (ODBC 1,0)|Devuelve el año según el campo año de *date_exp* como valor entero. El intervalo depende del origen de datos.|
