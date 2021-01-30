---
description: 'SQL a C: Carácter'
title: 'SQL a C: carácter | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], character
- character data type [ODBC]
- data conversions from SQL to C types [ODBC], character
ms.assetid: 7fdb7f38-b64d-48f2-bcb4-1ca96b2bbdb6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 78bbf45362216f8fb2a0706977347219ac6ac28b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203068"
---
# <a name="sql-to-c-character"></a>SQL a C: Carácter

Los identificadores de los tipos de datos SQL de caracteres ODBC son los siguientes:

- SQL_CHAR
- SQL_VARCHAR
- SQL_LONGVARCHAR
- SQL_WCHAR
- SQL_WVARCHAR
- SQL_WLONGVARCHAR

En la tabla siguiente se muestran los tipos de datos de ODBC C en los que se pueden convertir los datos de SQL de caracteres. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de SQL a tipos de datos de C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  

|Identificador de tipo de C|Prueba|TargetValuePtr|StrLen_or_IndPtr|SQLSTATE|
|:----------------|:---|:-------------|:---------------|:-------|
|SQL_C_CHAR|Longitud de bytes de los datos < *BufferLength*<br /><br /> Longitud de bytes de los datos >= *BufferLength*|data<br /><br /> Datos truncados|Longitud de los datos en bytes<br /><br /> Longitud de los datos en bytes|N/D<br /><br /> 01004|  
|SQL_C_WCHAR|Longitud de caracteres de los datos < *BufferLength*<br /><br /> Longitud de caracteres de los datos >= *BufferLength*|data<br /><br /> Datos truncados|Longitud de los datos en caracteres<br /><br /> Longitud de los datos en caracteres|N/D<br /><br /> 01004|  
|SQL_C_STINYINT SQL_C_UTINYINT SQL_C_TINYINT SQL_C_SBIGINT SQL_C_UBIGINT SQL_C_SSHORT SQL_C_USHORT SQL_C_SHORT SQL_C_SLONG SQL_C_ULONG SQL_C_LONG SQL_C_NUMERIC|Datos convertidos sin truncamiento [b]<br /><br /> Datos convertidos con truncamiento de dígitos fraccionarios [a]<br /><br /> La conversión de datos daría lugar a la pérdida de los dígitos enteros (en oposición a fraccionarios) [a]<br /><br /> Los datos no son un *literal numérico*[b]|data<br /><br /> Datos truncados<br /><br /> No definido<br /><br /> No definido|Número de bytes del tipo de datos C<br /><br /> Número de bytes del tipo de datos C<br /><br /> No definido<br /><br /> No definido|N/D<br /><br /> 01S07<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_FLOAT SQL_C_DOUBLE|Los datos están dentro del intervalo del tipo de datos al que se convierte el número [a]<br /><br /> Los datos están fuera del intervalo del tipo de datos al que se va a convertir el número [a]<br /><br /> Los datos no son un *literal numérico*[b]|data<br /><br /> No definido<br /><br /> No definido|Tamaño del tipo de datos C<br /><br /> No definido<br /><br /> No definido|N/D<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_BIT|Los datos son 0 o 1<br /><br /> Los datos son mayores que 0, menores que 2 y no igual a 1<br /><br /> Los datos son menores que 0 o mayor o igual que 2<br /><br /> Los datos no son un *literal numérico*|data<br /><br /> Datos truncados<br /><br /> No definido<br /><br /> No definido|1 [b]<br /><br /> 1 [b]<br /><br /> No definido<br /><br /> No definido|N/D<br /><br /> 01S07<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_BINARY|Longitud de bytes de los datos <= *BufferLength*<br /><br /> Longitud de bytes de los datos > *BufferLength*|data<br /><br /> Datos truncados|Longitud de los datos en bytes<br /><br /> Longitud de los datos|N/D<br /><br /> 01004|  
|SQL_C_TYPE_DATE|El valor de los datos es un *valor de fecha-valor* válido [a]<br /><br /> El valor de los datos es un *valor de marca de tiempo* válido; la parte de hora es cero [a]<br /><br /> El valor de los datos es un *valor de marca de tiempo* válido; la parte de hora es distinto de cero [a], [c]<br /><br /> El valor de los datos no es un valor *de fecha-valor* o *marca de tiempo* válido [a]|Datos<br /><br /> Datos<br /><br /> Datos truncados<br /><br /> No definido|6 [b]<br /><br /> 6 [b]<br /><br /> 6 [b]<br /><br /> No definido|N/D<br /><br /> N/D<br /><br /> 01S07<br /><br /> 22018|  
|SQL_C_TYPE_TIME|El valor de los datos es un *valor de hora válido y el valor de fracciones de segundo es 0*[a]<br /><br /> El valor de los datos es un *valor de marca de tiempo válido o un valor de hora válido*. la parte de fracciones de segundo es cero [a], [d]<br /><br /> El valor de los datos es un *valor de marca de tiempo* válido; la parte de fracciones de segundo es distinto de cero [a], [d], [e]<br /><br /> El valor de los datos no es un valor *de hora* o *marca de* tiempo válido [a]|Datos<br /><br /> Datos<br /><br /> Datos truncados<br /><br /> No definido|6 [b]<br /><br /> 6 [b]<br /><br /> 6 [b]<br /><br /> No definido|N/D<br /><br /> N/D<br /><br /> 01S07<br /><br /> 22018|  
|SQL_C_TYPE_TIMESTAMP|El valor de los datos es un *valor de marca de tiempo válido o un valor de hora válido*. parte de fracciones de segundo no truncada [a]<br /><br /> El valor de los datos es un *valor de marca de tiempo válido o un valor de hora válido*. parte de fracciones de segundo truncada [a]<br /><br /> El valor de los datos es un *valor de fecha-valor* válido [a]<br /><br /> El valor de los datos es un *valor de hora* válido [a]<br /><br /> El valor de los datos no es un valor *de fecha*, valor de hora o *marca de* *tiempo* válido [a]|data<br /><br /> Datos truncados<br /><br /> Datos [f]<br /><br /> Datos [g]<br /><br /> No definido|16 [b]<br /><br /> 16 [b]<br /><br /> 16 [b]<br /><br /> 16 [b]<br /><br /> No definido|N/D<br /><br /> 01S07<br /><br /> N/D<br /><br /> N/D<br /><br /> 22018|  
|Todos los tipos de intervalo de C|El valor de los datos es un *valor de intervalo* válido. sin truncamiento<br /><br /> El valor de los datos es un *valor de intervalo* válido. truncamiento de uno o más campos finales<br /><br /> Los datos son un intervalo válido; se pierde la precisión significativa del campo inicial<br /><br /> El valor de los datos no es un valor de intervalo válido|data<br /><br /> Datos truncados<br /><br /> No definido<br /><br /> No definido|Longitud de los datos en bytes<br /><br /> Longitud de los datos en bytes<br /><br /> No definido<br /><br /> No definido|N/D<br /><br /> 01S07<br /><br /> 22015<br /><br /> 22018|  
|&nbsp;|&nbsp;|&nbsp;|&nbsp;|&nbsp;|

 [a] el valor de *BufferLength* se omite para esta conversión. El controlador supone que el tamaño de **TargetValuePtr* es el tamaño del tipo de datos de C.  
  
 [b] es el tamaño del tipo de datos de C correspondiente.  
  
 [c] la parte de hora del *valor de marca* de tiempo se trunca.  
  
 [d] se omite la parte de fecha del *valor timestamp* .  
  
 [e] se trunca la parte de fracciones de segundo de la marca de tiempo.  
  
 [f] los campos de hora de la estructura de marca de tiempo se establecen en cero.  
  
 [g] los campos de fecha de la estructura de marca de tiempo se establecen en la fecha actual.  

**Espacios adicionales**

Los espacios iniciales y finales se omiten cuando los datos de caracteres SQL se convierten en cualquiera de los siguientes tipos:

- Fecha
- NUMERIC
- time
- timestamp
- datos de intervalo C
