---
description: Tamaño de columna
title: Tamaño de columna | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], column size
- size of data types [ODBC]
- SQL data types [ODBC], column characteristics
- column size of data types [ODBC]
ms.assetid: 541b83ab-b16d-4714-bcb2-3c3daa9a963b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bae1a1814ae89b2fe4b91ba9c2936df601c03301
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183667"
---
# <a name="column-size"></a>Tamaño de columna
El tamaño de la columna (o parámetro) de los tipos de datos numéricos se define como el número máximo de dígitos utilizados por el tipo de datos de la columna o del parámetro, o la precisión de los datos. En el caso de los tipos de caracteres, es la longitud en caracteres de los datos; en el caso de los tipos de datos binarios, el tamaño de columna se define como la longitud en bytes de los datos. En el caso de los tipos de datos Time, TIMESTAMP y All Interval, es el número de caracteres en la representación de caracteres de estos datos. En la tabla siguiente se muestra el tamaño de columna definido para cada tipo de datos conciso de SQL.  
  
|Identificador de tipo de SQL|Tamaño de columna|  
|-------------------------|-----------------|  
|Todos los tipos de caracteres [a], [b]|El tamaño de columna definido o máximo en caracteres de la columna o del parámetro (como se incluye en el campo descriptor de SQL_DESC_LENGTH). Por ejemplo, el tamaño de columna de una columna de caracteres de un solo byte definido como CHAR (10) es 10.|  
|SQL_DECIMAL SQL_NUMERIC|Número definido de dígitos. Por ejemplo, la precisión de una columna definida como numérica (10, 3) es 10.|  
|SQL_BIT [c]|1|  
|SQL_TINYINT [c]|3|  
|SQL_SMALLINT [c]|5|  
|SQL_INTEGER [c]|10|  
|SQL_BIGINT [c]|19 (si está firmado) o 20 (si es sin signo)|  
|SQL_REAL [c]|7|  
|SQL_FLOAT [c]|15|  
|SQL_DOUBLE [c]|15|  
|Todos los tipos binarios [a], [b]|La longitud máxima o definida en bytes de la columna o del parámetro. Por ejemplo, la longitud de una columna definida como BINARIa (10) es 10.|  
|SQL_TYPE_DATE [c]|10 (número de caracteres en el formato *AAAA-MM-DD* ).|  
|SQL_TYPE_TIME [c]|8 (el número de caracteres del formato *hh-mm-SS* ) o 9 + *s* (el número de caracteres del formato *HH: mm: SS*[. FFF...], donde *s* es la precisión en segundos).|  
|SQL_TYPE_TIMESTAMP|16 (el número de caracteres del formato *AAAA-MM-DD HH: mm* )<br /><br /> 19 (el número de caracteres del formato *AAAA-MM-DD* *HH: mm: SS* )<br /><br /> o<br /><br /> 20 + *s* (el número de caracteres del formato *AAAA-MM-DD HH: mm: SS*[. FFF...], donde *s* es la precisión en segundos).|  
|SQL_INTERVAL_SECOND|Donde *p* es la precisión inicial del intervalo y *s* es la precisión en segundos, *p* (si *s*= 0) o *p* + *s*+ 1 (si *es*>0). [ d|  
|SQL_INTERVAL_DAY_TO_SECOND|Donde *p* es la precisión inicial del intervalo y *s* es la precisión en segundos, 9 +*p* (si *s*= 0) o 10 +*p* + *s* (si *s*>0). [ d|  
|SQL_INTERVAL_HOUR_TO_SECOND|Donde *p* es la precisión inicial del intervalo y *s* es la precisión en segundos, 6 +*p* (si *s*= 0) o 7 +*p* + *s* (si *es*>0). [ d|  
|SQL_INTERVAL_MINUTE_TO_SECOND|Donde *p* es la precisión inicial del intervalo y *s* es la precisión en segundos, 3 +*p* (si *s*= 0) o 4 +*p* + *s* (si *s*>0). [ d|  
|SQL_INTERVAL_YEAR SQL_INTERVAL_MONTH SQL_INTERVAL_DAY SQL_INTERVAL_HOUR SQL_INTERVAL_MINUTE|*p*, donde *p* es la precisión inicial del intervalo. d|  
|SQL_INTERVAL_YEAR_TO_MONTH SQL_INTERVAL_DAY_TO_HOUR|3 +*p*, donde *p* es la precisión inicial del intervalo. d|  
|SQL_INTERVAL_DAY_TO_MINUTE|6 +*p*, donde *p* es la precisión inicial del intervalo. d|  
|SQL_INTERVAL_HOUR_TO_MINUTE|3 +*p*, donde *p* es la precisión inicial del intervalo. d|  
|SQL_GUID|36 (número de caracteres en el formato *aaaaaaaa-bbbb-CCCC-dddd-eeeeeeeeeeee* )|  
  
 [a] para una aplicación ODBC 1,0 que llama a **SQLSetParam** en un controlador ODBC 2,0 y, para una aplicación ODBC 2,0 que llama a **SQLBindParameter** en un controlador odbc 1,0, cuando \* *StrLen_or_IndPtr* se SQL_DATA_AT_EXEC para un tipo de SQL_LONGVARCHAR o SQL_LONGVARBINARY, el valor de *columnas* debe establecerse en la longitud total de los datos que se van a enviar, no en la precisión definida en esta tabla.  
  
 [b] si el controlador no puede determinar la longitud de la columna o el parámetro para un tipo de variable, devuelve SQL_NO_TOTAL.  
  
 [c] el argumento de *columnas* de **SQLBindParameter** se omite para este tipo de datos.  
  
 [d] para obtener reglas generales sobre la longitud de columna en los tipos de datos de intervalo, vea [longitud del tipo de datos de intervalo](../../../odbc/reference/appendixes/interval-data-type-length.md), anteriormente en este apéndice.  
  
 Los valores devueltos para el tamaño de la columna (o el parámetro) no se corresponden con los valores de un campo descriptor. Los valores pueden provienen del SQL_DESC_PRECISION o del campo SQL_DESC_LENGTH, según el tipo de datos, como se muestra en la tabla siguiente.  
  
|Tipo SQL|Campo descriptor correspondiente a<br /><br /> tamaño de columna o parámetro|  
|--------------|--------------------------------------------------------------------|  
|Todos los tipos de caracteres y binarios|LENGTH|  
|Todos los tipos numéricos|PRECISION|  
|Todos los tipos DateTime y Interval|LENGTH|  
|SQL_BIT|LENGTH|
