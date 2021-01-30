---
description: 'C a SQL: Carácter'
title: 'C a SQL: carácter | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- character data type [ODBC]
- data conversions from C to SQL types [ODBC], character
- converting data from c to SQL types [ODBC], character
ms.assetid: be66188a-ebdb-4c9e-af72-c379886766fa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c387c2063bea8f16c7ec5ec90aa0271830f3e5f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212358"
---
# <a name="c-to-sql-character"></a>C a SQL: Carácter
Los identificadores para el tipo de datos C de carácter ODBC son:  
  
 SQL_C_CHAR  
  
 SQL_C_WCHAR  
  
 En la tabla siguiente se muestran los tipos de datos de ODBC SQL en los que se pueden convertir los datos de caracteres de C. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de C a tipos de datos de SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
> [!NOTE]  
>  Cuando se convierten datos de caracteres C en datos SQL de Unicode, la longitud de los datos Unicode debe ser un número par.  
  
|Identificador de tipo de SQL|Prueba|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Longitud de bytes de los datos <= longitud de la columna.<br /><br /> Longitud de bytes de los datos > la longitud de la columna.|N/D<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Longitud de caracteres de los datos <= longitud de la columna.<br /><br /> Longitud de caracteres de los datos > la longitud de la columna.|N/D<br /><br /> 22001|  
|SQL_DECIMAL<br /><br /> SQL_NUMERIC<br /><br /> SQL_TINYINT<br /><br /> SQL_SMALLINT<br /><br /> SQL_INTEGER SQL_BIGINT|Datos convertidos sin truncamiento<br /><br /> Datos convertidos con truncamiento de dígitos fraccionarios [e]<br /><br /> La conversión de datos daría lugar a la pérdida de los dígitos enteros (en oposición a fraccionarios) [e]<br /><br /> El valor de los datos no es un *literal numérico*|N/D<br /><br /> 22001<br /><br /> 22001<br /><br /> 22018|  
|SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE|Los datos están dentro del intervalo del tipo de datos al que se va a convertir el número.<br /><br /> Los datos están fuera del intervalo del tipo de datos al que se está convirtiendo el número<br /><br /> El valor de los datos no es un *literal numérico*|N/D<br /><br /> 22003<br /><br /> 22018|  
|SQL_BIT|Los datos son 0 o 1<br /><br /> Los datos son mayores que 0, menores que 2 y no igual a 1<br /><br /> Los datos son menores que 0 o mayor o igual que 2<br /><br /> Los datos no son un *literal numérico*|N/D<br /><br /> 22001<br /><br /> 22003<br /><br /> 22018|  
|SQL_BINARY<br /><br /> SQL_VARBINARY<br /><br /> SQL_LONGVARBINARY|(Longitud de bytes de datos)/2 <= longitud de bytes de columna<br /><br /> (Longitud de bytes de datos)/2 > longitud de bytes de columna<br /><br /> El valor de los datos no es un valor hexadecimal|N/D<br /><br /> 22001<br /><br /> 22018|  
|SQL_TYPE_DATE|El valor de los datos es un valor válido de *ODBC-Date-literal*<br /><br /> El valor de los datos es un *literal de fecha y hora ODBC* válido; la parte de hora es cero<br /><br /> El valor de los datos es un *literal de fecha y hora ODBC* válido; la parte de hora es distinto de cero [a]<br /><br /> El valor de los datos no es un valor de *ODBC-Date-literal* u *ODBC-timestamp-literal* válido|N/D<br /><br /> N/D<br /><br /> 22008<br /><br /> 22018|  
|SQL_TYPE_TIME|El valor de los datos es un *literal de hora ODBC* válido<br /><br /> El valor de los datos es un *literal de fecha y hora ODBC* válido; la parte de fracciones de segundo es cero [b]<br /><br /> El valor de los datos es un *literal de fecha y hora ODBC* válido; la parte de fracciones de segundo es distinto de cero [b]<br /><br /> El valor de los datos no es un *literal de fecha y hora ODBC* válido u *ODBC-timestamp-literal*|N/D<br /><br /> N/D<br /><br /> 22008<br /><br /> 22018|  
|SQL_TYPE_TIMESTAMP|El valor de los datos es un *literal de fecha y hora ODBC* válido; parte de fracciones de segundo no truncada<br /><br /> El valor de los datos es un *literal de fecha y hora ODBC* válido; parte de fracciones de segundo truncada<br /><br /> El valor de los datos es un *ODBC-Date-literal*[c] válido<br /><br /> El valor de los datos es un *literal de hora ODBC* válido [d]<br /><br /> El valor de datos no es un literal de fecha y hora *ODBC* válido, *ODBC-Time-literal* u *ODBC-timestamp-literal*|N/D<br /><br /> 22008<br /><br /> N/D<br /><br /> N/D<br /><br /> 22018|  
|Todos los tipos de intervalo SQL|El valor de los datos es un *valor de intervalo* válido. no se produce ningún truncamiento<br /><br /> El valor de los datos es un *valor de intervalo* válido. el valor de uno de los campos se trunca.<br /><br /> El valor de los datos no es un literal de intervalo válido|N/D<br /><br /> 22015<br /><br /> 22018|  
  
 [a] la parte de hora de la marca de tiempo se trunca.  
  
 [b] se omite la parte de fecha de la marca de tiempo.  
  
 [c] la parte de hora de la marca de tiempo se establece en cero.  
  
 [d] la parte de fecha de la marca de tiempo se establece en la fecha actual.  
  
 [e] el origen de datos o el controlador espera eficazmente hasta que se haya recibido toda la cadena (aunque los datos de caracteres se envíen en partes mediante llamadas a **SQLPutData**) antes de intentar realizar la conversión.  
  
 Cuando se convierten datos de caracteres C en datos SQL numéricos, de fecha, hora o de marca de tiempo, se omiten los espacios en blanco iniciales y finales.  
  
 Cuando se convierten datos de caracteres C en datos SQL binarios, cada dos bytes de datos de caracteres se convierten en un solo byte (8 bits) de datos binarios. Cada dos bytes de datos de caracteres representan un número en formato hexadecimal. Por ejemplo, "01" se convierte en un archivo binario 00000001 y "FF" se convierte en un binario 11111111.  
  
 El controlador siempre convierte pares de dígitos hexadecimales en bytes individuales y omite el byte de terminación null. Por este motivo, si la longitud de la cadena de caracteres es impar, no se convierte el último byte de la cadena (excepto el byte de terminación de NULL, si existe).  
  
> [!NOTE]  
>  No se recomienda a los desarrolladores de aplicaciones enlazar datos de caracteres C a un tipo de datos SQL binario. Esta conversión suele ser ineficaz y lenta.
