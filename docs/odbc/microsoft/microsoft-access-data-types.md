---
description: Tipos de datos de Microsoft Access
title: Tipos de datos de Microsoft Access | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], Access driver
- Jet-based ODBC drivers [ODBC], Access driver
- desktop database drivers [ODBC], Access driver
- Access driver [ODBC], data types
- access data types [ODBC]
- data types [ODBC], Access driver
ms.assetid: b537348a-bea0-4bd6-84a4-52a75292957f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0e7a55c6929d63bf036c9f1585d2eb039f6fec77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205707"
---
# <a name="microsoft-access-data-types"></a>Tipos de datos de Microsoft Access
En la tabla siguiente se muestran los tipos de datos de Microsoft Access, los tipos de datos usados para crear tablas y los tipos de datos SQL de ODBC.  
  
|Tipo de datos de Microsoft Access|Tipo de datos (CREATETABLE)|Tipo de datos SQL de ODBC|  
|--------------------------------|-------------------------------|------------------------|  
|BIGBINARY [1]|LONGBINARY|SQL_LONGVARBINARY|  
|BINARY|BINARY|SQL_BINARY|  
|BIT|BIT|SQL_BIT|  
|BLOQUE|BLOQUE|SQL_INTEGER|  
|CURRENCY|MONEDA|SQL_NUMERIC|  
|FECHA Y HORA|DATETIME|SQL_TIMESTAMP|  
|GUID|GUID|SQL_GUID|  
|BINARIO LARGO|LONGBINARY|SQL_LONGVARBINARY|  
|TEXTO LARGO|LONGTEXT|SQL_LONGVARCHAR [2] SQL_WLONGVARCHAR [3]|  
|Memorando|LONGTEXT|SQL_LONGVARCHAR [2] SQL_WLONGVARCHAR [3]|  
|NUMBER (FieldSize = SINGLE)|SENCILLA|SQL_REAL|  
|NUMBER (FieldSize = DOUBLE)|DOUBLE|SQL_DOUBLE|  
|NUMBER (FieldSize = BYTE)|BYTE SIN SIGNO|SQL_TINYINT|  
|NUMBER (FieldSize = entero)|SHORT|SQL_SMALLINT|  
|NUMBER (FieldSize = entero largo)|LONG|SQL_INTEGER|  
|NUMERIC|NUMERIC|SQL_NUMERIC|  
|OLE|LONGBINARY|SQL_LONGVARBINARY|  
|TEXT|VARCHAR|SQL_VARCHAR [1] SQL_WVARCHAR [2]|  
|VARBINARY|VARBINARY|SQL_VARBINARY|  
  
 [1] acceso solo a aplicaciones 4,0. Longitud máxima de 4000 bytes. Comportamiento similar a LONGBINARY.  
  
 [2] solo aplicaciones ANSI.  
  
 [3] solo aplicaciones Unicode y Access 4,0.  
  
> [!NOTE]  
>  **SQLGetTypeInfo** devuelve los tipos de datos de ODBC. No devolverá todos los tipos de datos de Microsoft Access si se asigna más de un tipo de acceso de Microsoft al mismo tipo de datos de ODBC SQL. Todas las conversiones del Apéndice D de la *Referencia del programador de ODBC* son compatibles con los tipos de datos de SQL que se enumeran en la tabla anterior.  
  
 En la tabla siguiente se muestran las limitaciones en los tipos de datos de Microsoft Access.  
  
|Tipo de datos|Descripción|  
|---------------|-----------------|  
|BINARY, VARBINARY y VARCHAR|Al crear una columna BINARY, VARBINARY o VARCHAR de cero o una longitud no especificada, se devuelve realmente una columna de 510 bytes.|  
|BYTE|Aunque un campo de número de acceso de Microsoft con un valor de FieldSize igual a BYTE es sin signo, se puede insertar un número negativo en el campo al usar el controlador de Microsoft Access.|  
|CHAR, LONGVARCHAR y VARCHAR|Un literal de cadena de caracteres puede contener cualquier carácter ANSI (1-255 decimal). Use dos comillas simples (' ') consecutivas para representar una comilla simple (').<br /><br /> Los procedimientos deben usarse para pasar datos de caracteres al usar cualquier carácter especial en una columna de tipo de datos de caracteres.|  
|DATE|Los valores de fecha deben estar delimitados según el formato de fecha canónico de ODBC o delimitado por el delimitador de fecha y hora ("#"). De lo contrario, Microsoft Access tratará el valor como una expresión aritmética y no generará una advertencia o un error.<br /><br /> Por ejemplo, la fecha "5 de marzo de 1996" se debe representar como {d ' 1996-03-05 '} o #03/05/1996 #; de lo contrario, si solo se envía 03/05/1993, Microsoft Access lo evaluará como 3 dividido entre 5 dividido entre 1996. Este valor se redondea al entero 0 y, dado que el día cero se asigna a 1899-12-31, se trata de la fecha utilizada.<br /><br /> No se puede usar un carácter de barra vertical (&#124;) en un valor de fecha, incluso si se incluye entre comillas.|  
|GUID|Tipo de datos limitado a Microsoft Access 4,0.|  
|NUMERIC|Tipo de datos limitado a Microsoft Access 4,0.|  
  
 Se pueden encontrar más limitaciones en los tipos de datos en limitaciones de tipos de [datos](../../odbc/microsoft/data-type-limitations.md).
