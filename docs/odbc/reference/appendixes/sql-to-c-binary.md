---
description: 'SQL a C: Binary'
title: 'SQL a C: binario | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to c types [ODBC], binary
- binary data type [ODBC]
- data conversions from SQL to C types [ODBC], binary
- binary data transfers [ODBC]
ms.assetid: 8c519072-ae4c-4d32-9d4e-775e3d3d6389
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 30fbabde654f69af7a0cb68e2606c0f2d93697c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160814"
---
# <a name="sql-to-c-binary"></a>SQL a C: Binary
Los identificadores de los tipos de datos SQL de ODBC binarios son:  
  
 SQL_BINARY  
  
 SQL_VARBINARY  
  
 SQL_LONGVARBINARY  
  
 En la tabla siguiente se muestran los tipos de datos C de ODBC en los que se pueden convertir los datos SQL binarios. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de SQL a tipos de datos de C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificador de tipo de C|Prueba|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|(Longitud de bytes de los datos) \* 2 < *BufferLength*<br /><br /> (Longitud de bytes de los datos) \* 2 >= *BufferLength*|data<br /><br /> Datos truncados|Longitud de los datos en bytes<br /><br /> Longitud de los datos en bytes|N/D<br /><br /> 01004|  
|SQL_C_WCHAR|(Longitud de caracteres de los datos) \* 2 < *BufferLength*<br /><br /> (Longitud de caracteres de los datos) \* 2 >= *BufferLength*|data<br /><br /> Datos truncados|Longitud de los datos en caracteres<br /><br /> Longitud de los datos en caracteres|N/D<br /><br /> 01004|  
|SQL_C_BINARY|Longitud de bytes de los datos <= *BufferLength*<br /><br /> Longitud de bytes de los datos > *BufferLength*|data<br /><br /> Datos truncados|Longitud de los datos en bytes<br /><br /> Longitud de los datos en bytes|N/D<br /><br /> 01004|  
  
 Cuando los datos binarios de SQL se convierten en datos de caracteres C, cada byte (8 bits) de los datos de origen se representan como dos caracteres ASCII. Estos caracteres son la representación de caracteres ASCII del número en formato hexadecimal. Por ejemplo, un archivo binario 00000001 se convierte en "01" y un binario 11111111 se convierte en "FF".  
  
 El controlador siempre convierte los bytes individuales en pares de dígitos hexadecimales y finaliza la cadena de caracteres con un byte nulo. Por este motivo, si *BufferLength* es par y es menor que la longitud de los datos convertidos, no se usa el último byte del búfer **TargetValuePtr* . (Los datos convertidos requieren un número par de bytes, el siguiente hasta el último byte es un byte nulo y no se puede usar el último byte).  
  
> [!NOTE]  
>  No se recomienda a los desarrolladores de aplicaciones enlazar datos SQL binarios a un tipo de datos de caracteres C. Esta conversión suele ser ineficaz y lenta.
