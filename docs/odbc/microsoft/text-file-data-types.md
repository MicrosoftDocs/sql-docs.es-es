---
description: Tipos de datos de archivo de texto
title: Tipos de datos de archivos de texto | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- text file driver [ODBC], data types
- ODBC desktop database drivers [ODBC], text file driver
- desktop database drivers [ODBC], text file driver
- text file data types [ODBC]
- Jet-based ODBC drivers [ODBC], text file driver
ms.assetid: e113112e-ae42-469e-8e4b-a365a10d9071
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b1e447d2f20c8efb5e0c0cddc793baf73384e556
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182536"
---
# <a name="text-file-data-types"></a>Tipos de datos de archivo de texto
En la tabla siguiente se muestra cómo se asignan los tipos de datos de texto a tipos de datos SQL de ODBC. Tenga en cuenta que no todos los tipos de datos SQL de ODBC son compatibles con el controlador de texto ODBC.  
  
|Text (tipo de datos)|Tipo de datos de ODBC|  
|--------------------|--------------------|  
|CHAR|SQL_VARCHAR|  
|DATETIME|SQL_TIMESTAMP|  
|FLOAT|SQL_DOUBLE|  
|INTEGER|SQL_INTEGER|  
|LONGCHAR|SQL_LONGVARCHAR|  
  
> [!NOTE]  
>  **SQLGetTypeInfo** devuelve los tipos de datos de ODBC. Todas las conversiones del Apéndice D de la *Referencia del programador de ODBC* son compatibles con los tipos de datos de SQL que se enumeran en la tabla anterior.  
  
 En la tabla siguiente se muestran las limitaciones en los tipos de datos de texto.  
  
|Tipo de datos|Descripción|  
|---------------|-----------------|  
|CHAR|Al crear una columna CHAR de cero o una longitud no especificada, se devuelve realmente una columna de 255 bits.<br /><br /> En los archivos delimitados, una columna CHAR puede tener o no delimitadores de comillas dobles al principio y al final; en los archivos de longitud fija, las comillas dobles no se usan como delimitadores.|  
|DATETIME|MM-DD-YY (por ejemplo, 01-17-92)<br /><br /> MMM-DD-AA (por ejemplo, ene-17-92)<br /><br /> DD-MMM-AA (por ejemplo, 17-Ene-92)<br /><br /> AAAA-MM-DD (por ejemplo, 1992-01-17)<br /><br /> AAAA-MMM-DD (por ejemplo, 1992-ene-17)<br /><br /> No se permiten separadores de fecha mixtos dentro de una tabla.<br /><br /> El texto ISAM da formato a un campo de fecha y hora en el formato Estados Unidos o europeo, en función de la configuración internacional del panel de control de Windows.|  
|FLOAT|El ancho máximo incluye el signo y el separador decimal. En Schema.ini, el ancho se indica de la siguiente manera:<br /><br /> 14,083 es el ancho FLOAT 6<br /><br /> -14,083 es el ancho flotante 7<br /><br /> + 14,083 es el ancho flotante 7<br /><br /> 14083. es de tipo FLOAT 6<br /><br /> ODBC siempre devuelve 8 para las columnas de tipo FLOAT.<br /><br /> Las columnas FLOAT también pueden estar en notación científica, por ejemplo:<br /><br /> -3.04 e + 2 es el ancho flotante 8<br /><br /> 25E4 es el ancho flotante 4<br /><br /> **Nota:** No se pueden mezclar notación decimal y científica en una columna.<br /><br /> Los valores NULL se representan mediante una cadena acolchada en blanco en archivos de longitud fija y se omiten en archivos delimitados.<br /><br /> Los datos float se pueden rellenar con espacios en blanco iniciales.|  
|INTEGER|Los valores válidos para las columnas de enteros son de 32767 a-32766.<br /><br /> En Schema.ini, el ancho se indica de la siguiente manera:<br /><br /> 14083 es el ancho entero 5<br /><br /> 0 es el ancho entero 1<br /><br /> ODBC siempre devuelve 4 para las columnas de tipo entero.<br /><br /> El ancho máximo incluye un signo. El ancho máximo de una columna de enteros es 11, aunque el ancho puede ser mayor debido a los espacios en blanco que se permiten en las tablas de formato fijo.|  
|LONGCHAR|El límite teórico en el ancho de una columna LONGCHAR en una tabla de longitud fija o delimitada es 65500K. El texto ISAM es más probable que proporcione compatibilidad confiable hasta aproximadamente 32 k.|  
  
 Se pueden encontrar más limitaciones en los tipos de datos en limitaciones de tipos de [datos](../../odbc/microsoft/data-type-limitations.md).
