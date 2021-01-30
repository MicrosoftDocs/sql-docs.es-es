---
description: 'SQL a C: Intervalos de día y hora'
title: 'SQL a C: intervalos de Day-Time | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], day-time intervals
- day-time intervals [ODBC]
- data conversions from SQL to C types [ODBC], day-time intervals
- intervals [ODBC], converting
ms.assetid: 8ea84d69-2292-4128-89a0-f184f68abb98
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae81fdf91c59457a6453958cb1c906abdd9d53fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203042"
---
# <a name="sql-to-c-day-time-intervals"></a>SQL a C: Intervalos de día y hora

Los identificadores de los tipos de datos ODBC SQL de intervalo de tiempo de día son los siguientes:

- SQL_INTERVAL_DAY
- SQL_INTERVAL_DAY_TO_MINUTE
- SQL_INTERVAL_HOUR
- SQL_INTERVAL_DAY_TO_SECOND
- SQL_INTERVAL_MINUTE
- SQL_INTERVAL_HOUR_TO_MINUTE
- SQL_INTERVAL_SECOND
- SQL_INTERVAL_HOUR_TO_SECOND
- SQL_INTERVAL_DAY_TO_HOUR
- SQL_INTERVAL_MINUTE_TO_SECOND

En la tabla siguiente se muestran los tipos de datos de ODBC C en los que se pueden convertir los datos SQL de intervalos de tiempo. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de SQL a tipos de datos de C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).

|Identificador de tipo de C|Prueba|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|Todos los tipos de intervalo de C de día y hora|Parte de campos finales no truncada<br /><br /> Parte de campos finales truncada<br /><br /> La precisión inicial del destino no es lo suficientemente grande como para contener los datos del origen|data<br /><br /> Datos truncados<br /><br /> No definido|Longitud de los datos<br /><br /> Longitud de los datos<br /><br /> No definido|N/D<br /><br /> 01S07<br /><br /> 22015|  
|SQL_C_STINYINT [b] SQL_C_UTINYINT [b] SQL_C_USHORT [b] SQL_C_SHORT [b] SQL_C_SLONG [b] SQL_C_ULONG [b] SQL_C_NUMERIC [b] SQL_C_BIGINT [b]|La precisión del intervalo era un campo único y los datos se convirtieron sin truncamiento<br /><br /> La precisión del intervalo era un campo único y una fracción truncada<br /><br /> La precisión del intervalo era un campo único y se truncó todo<br /><br /> La precisión del intervalo no era un campo único|data<br /><br /> Datos truncados<br /><br /> Datos truncados<br /><br /> No definido|Tamaño del tipo de datos C<br /><br /> Longitud de los datos<br /><br /> Longitud de los datos<br /><br /> Tamaño del tipo de datos C|N/D<br /><br /> 01S07<br /><br /> 22003<br /><br /> 07006|  
|SQL_C_BINARY|Longitud de bytes de los datos <= *BufferLength*<br /><br /> Longitud de bytes de los datos > *BufferLength*|data<br /><br /> No definido|Longitud de los datos<br /><br /> No definido|N/D<br /><br /> 22003|  
|SQL_C_CHAR|Longitud de bytes de caracteres < *BufferLength*<br /><br /> Número de dígitos completos (en oposición a fraccionarios) < *BufferLength*<br /><br /> Número de dígitos completos (en oposición a fraccionarios) >= *BufferLength*|data<br /><br /> Datos truncados<br /><br /> No definido|Tamaño del tipo de datos C<br /><br /> Tamaño del tipo de datos C<br /><br /> No definido|N/D<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|Longitud de caracteres < *BufferLength*<br /><br /> Número de dígitos completos (en oposición a fraccionarios) < *BufferLength*<br /><br /> Número de dígitos completos (en oposición a fraccionarios) >= *BufferLength*|data<br /><br /> Datos truncados<br /><br /> No definido|Tamaño del tipo de datos C<br /><br /> Tamaño del tipo de datos C<br /><br /> No definido|N/D<br /><br /> 01004<br /><br /> 22003|  
  
 [a] un tipo SQL de intervalo de fecha y hora se puede convertir en cualquier tipo de C de intervalo de tiempo de día.  
  
 [b] si la precisión del intervalo es un campo único (uno de día, hora, minuto o segundo), el tipo SQL de intervalo se puede convertir a cualquier número exacto (SQL_C_STINYINT, SQL_C_UTINYINT, SQL_C_USHORT, SQL_C_SHORT, SQL_C_SLONG, SQL_C_ULONG o SQL_C_NUMERIC).  
  
La conversión predeterminada de un tipo SQL de intervalo es en el tipo de datos del intervalo de C correspondiente. A continuación, la aplicación enlaza la columna o el parámetro (o establece el campo SQL_DESC_DATA_PTR en el registro adecuado de ARD) para que señale a la estructura de SQL_INTERVAL_STRUCT inicializada (o pasa un puntero a la estructura de INTERVAL_STRUCT de SQL_ como el argumento *TargetValuePtr* en una llamada a **SQLGetData**).  
  
En el ejemplo siguiente se muestra cómo transferir datos de una columna de tipo SQL_INTERVAL_DAY_TO_MINUTE a la estructura SQL_INTERVAL_STRUCT de modo que vuelva como un intervalo de DAY_TO_HOUR.  

```cpp
SQL_INTERVAL_STRUCT is;  
SQLINTEGER    cbValue;  
SQLUINTEGER   days, hours;  
  
// Execute a select statement; "interval_column" is a column  
// whose data type is SQL_INTERVAL_DAY_TO_MINUTE.  
SQLExecDirect(hstmt, "SELECT interval_column FROM table", SQL_NTS);  
  
// Bind  
SQLBindCol(hstmt, 1, SQL_C_INTERVAL_DAY_TO_MINUTE, &is, sizeof(SQL_INTERVAL_STRUCT), &cbValue);  
  
// Fetch  
SQLFetch(hstmt);  
  
// Process data  
days = is.intval.day_second.day;  
hours = is.intval.day_second.hour;  
```
