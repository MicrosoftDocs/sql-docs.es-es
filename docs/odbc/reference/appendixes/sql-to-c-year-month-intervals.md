---
description: 'SQL a C: Intervalos de mes y año'
title: 'SQL a C: intervalos de Year-Month | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to c types [ODBC], about converting
- data conversions from SQL to C types [ODBC], year-month intervals
- intervals [ODBC], converting
- year-month intervals [ODBC]
ms.assetid: 1233634b-8214-420f-b872-3b2630105ba4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9d83ca61aa30ce1abbeb38759557f4b75a6e0b9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202983"
---
# <a name="sql-to-c-year-month-intervals"></a>SQL a C: Intervalos de mes y año

Los identificadores de los tipos de datos de ODBC SQL Interval de año-mes son los siguientes:

- SQL_INTERVAL_MONTH
- SQL_INTERVAL_YEAR
- SQL_INTERVAL_YEAR_TO_MONTH

En la tabla siguiente se muestran los tipos de datos C de ODBC en los que se pueden convertir los datos SQL de intervalos de año. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de SQL a tipos de datos de C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  

|Identificador de tipo de C|Prueba|TargetValuePtr|StrLen_or_IndPtr|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_INTERVAL_MONTH [a]<br /><br /> SQL_C_INTERVAL_YEAR [a]<br /><br /> SQL_C_INTERVAL_YEAR_TO_MONTH [a]|Parte de campos finales no truncada<br /><br /> Parte de campos finales truncada<br /><br /> La precisión inicial del destino no es lo suficientemente grande como para contener los datos del origen|data<br /><br /> Datos truncados<br /><br /> No definido|Longitud de los datos en bytes<br /><br /> Longitud de los datos en bytes<br /><br /> No definido|N/D<br /><br /> 01S07<br /><br /> 22015|  
|SQL_C_STINYINT [b]<br /><br /> SQL_C_UTINYINT [b]<br /><br /> SQL_C_USHORT [b]<br /><br /> SQL_C_SHORT [b]<br /><br /> SQL_C_SLONG [b]<br /><br /> SQL_C_ULONG [b]<br /><br /> SQL_C_NUMERIC [b]<br /><br /> SQL_C_BIGINT [b]|La precisión del intervalo era un campo único y los datos se convirtieron sin truncamiento<br /><br /> La precisión del intervalo era un campo único y se truncó todo<br /><br /> La precisión del intervalo no era un campo único|data<br /><br /> Datos truncados<br /><br /> No definido|Tamaño del tipo de datos C<br /><br /> Longitud de los datos en bytes<br /><br /> Tamaño del tipo de datos C|N/D<br /><br /> 22003<br /><br /> 22015|  
|SQL_C_BINARY|Longitud de bytes de los datos <= *BufferLength*<br /><br /> Longitud de bytes de los datos > *BufferLength*|data<br /><br /> No definido|Longitud de los datos en bytes<br /><br /> No definido|N/D<br /><br /> 22003|  
|SQL_C_CHAR|Longitud de bytes de caracteres < *BufferLength*<br /><br /> Número de dígitos completos (en oposición a fraccionarios) < *BufferLength*<br /><br /> Número de dígitos completos (en oposición a fraccionarios) >= *BufferLength*|data<br /><br /> Datos truncados<br /><br /> No definido|Tamaño del tipo de datos C<br /><br /> Tamaño del tipo de datos C<br /><br /> No definido|N/D<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|Longitud de caracteres < *BufferLength*<br /><br /> Número de dígitos completos (en oposición a fraccionarios) < *BufferLength*<br /><br /> Número de dígitos completos (en oposición a fraccionarios) >= *BufferLength*|data<br /><br /> Datos truncados<br /><br /> No definido|Tamaño del tipo de datos C<br /><br /> Tamaño del tipo de datos C<br /><br /> No definido|N/D<br /><br /> 01004<br /><br /> 22003|  
  
 [a] el tipo SQL de un intervalo de un mes se puede convertir en cualquier tipo C de intervalo de año-mes.  
  
 [b] si la precisión del intervalo es un campo único (uno de año o mes), el tipo SQL de intervalo se puede convertir a cualquier número exacto (SQL_C_STINYINT, SQL_C_UTINYINT, SQL_C_USHORT, SQL_C_SHORT, SQL_C_SLONG, SQL_C_ULONG o SQL_C_NUMERIC).  

## <a name="default-conversions"></a>Conversiones predeterminadas

La conversión predeterminada de un tipo SQL de intervalo es en el tipo de datos del intervalo de C correspondiente. A continuación, la aplicación enlaza la columna o el parámetro (o establece el campo SQL_DESC_DATA_PTR en el registro adecuado de ARD) para que señale a la estructura de SQL_INTERVAL_STRUCT inicializada (o pasa un puntero a la estructura de INTERVAL_STRUCT de SQL_ como el argumento *TargetValuePtr* en una llamada a **SQLGetData**).
