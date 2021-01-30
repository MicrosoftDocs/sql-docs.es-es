---
description: Tipos de datos de fecha y hora
title: DateTime (tipos de datos) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- time data type [ODBC]
- datetime data types [ODBC]
- date data type [ODBC]
- data types [ODBC], date
- backward compatibility [ODBC], datetime data types
- timestamp data type [ODBC]
- data types [ODBC], timestamp
- data types [ODBC], backward compatibility
- compatibility [ODBC], datetime data types
- data types [ODBC], time
ms.assetid: 6b9363c9-04bf-4492-a210-7aa15dea4af8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2bee41df18a0a05c7b7812c0a23321feec13ed7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194917"
---
# <a name="datetime-data-types"></a>Tipos de datos de fecha y hora
En ODBC *3. x*, los identificadores de los tipos de datos de fecha, hora y marca de tiempo de SQL han cambiado de SQL_DATE, SQL_TIME y SQL_TIMESTAMP (con instancias de **#define** en el archivo de encabezado 9, 10 y 11) a SQL_TYPE_DATE, SQL_TYPE_TIME y SQL_TYPE_TIMESTAMP (con instancias de **#define** en el archivo de encabezado de 91, 92 y 93), respectivamente. Los identificadores de tipo de C correspondientes han cambiado de SQL_C_DATE, SQL_C_TIME y SQL_C_TIMESTAMP a SQL_C_TYPE_DATE, SQL_C_TYPE_TIME y SQL_C_TYPE_TIMESTAMP, respectivamente, y las instancias de **#define** han cambiado en consecuencia.  
  
 El tamaño de la columna y los dígitos decimales devueltos para los tipos de datos DateTime de SQL en ODBC *3. x* son los mismos que la precisión y la escala devueltas para ellos en ODBC *2. x*. Estos valores son diferentes de los valores de los campos de descriptor SQL_DESC_PRECISION y SQL_DESC_SCALE. (Para obtener más información, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos).  
  
 Estos cambios afectan a **SQLDescribeCol**, **SQLDescribeParam** y **SQLColAttributes**; **SQLBindCol**, **SQLBindParameter** y **SQLGetData**; y **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, **SQLStatistics** y **SQLSpecialColumns**.  
  
 Un controlador ODBC *3. x* procesa las llamadas de función enumeradas en el párrafo anterior según el valor del atributo de entorno SQL_ATTR_ODBC_VERSION. Para **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, **SQLSpecialColumns** y **SQLStatistics**, si SQL_ATTR_ODBC_VERSION está establecido en SQL_OV_ODBC3, las funciones devuelven SQL_TYPE_DATE, SQL_TYPE_TIME y SQL_TYPE_TIMESTAMP en el campo data_type. La columna COLUMN_SIZE (en el conjunto de resultados devuelto por **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns** y **SQLSpecialColumns**) contiene la precisión binaria del tipo numérico aproximado. La columna NUM_PREC_RADIX (en el conjunto de resultados devuelto por **SQLColumns**, **SQLGetTypeInfo** y **SQLProcedureColumns**) contiene un valor de 2. Si SQL_ATTR_ODBC_VERSION se establece en SQL_OV_ODBC2, las funciones devuelven SQL_DATE, SQL_TIME y SQL_TIMESTAMP en el campo DATA_TYPE, la columna COLUMN_SIZE contiene la precisión decimal para el tipo numérico aproximado y la columna NUM_PREC_RADIX contiene un valor de 10.  
  
 Cuando se solicitan todos los tipos de datos en una llamada a **SQLGetTypeInfo**, el conjunto de resultados devuelto por la función contendrá SQL_TYPE_DATE, SQL_TYPE_TIME y SQL_TYPE_TIMESTAMP tal y como se define en ODBC *3. x*, y SQL_DATE, SQL_TIME y SQL_TIMESTAMP tal y como se define en ODBC *2. x*.  
  
 Debido al modo en que el administrador de controladores ODBC *3. x* realiza la asignación de los tipos de datos de fecha, hora y marca de tiempo, los controladores ODBC *3. x* solo necesitan reconocer **#defines** de 91.92 y 93 para los tipos de datos Date, Time y timestamp C especificados en los argumentos *TargetType* de **SQLBindCol** y **SQLGetData** , o el argumento *ValueType* de **SQLBindParameter**, y solo necesitan reconocer **#defines** de 91, 92 y 93 para los tipos de datos de fecha, hora y marca de tiempo de SQL especificados en el argumento *ParameterType* de **SQLBindParameter** o el argumento *DataType* de **SQLGetTypeInfo**. Para obtener más información, consulte [DateTime Data Type Changes](../../../odbc/reference/develop-app/datetime-data-type-changes.md).
