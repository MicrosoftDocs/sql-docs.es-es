---
description: 'SQL a C: Time'
title: 'SQL a C: tiempo | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], time
- time data type [ODBC]
- data conversions from SQL to C types [ODBC], time
ms.assetid: 6dc59973-7bb5-40f1-87c8-5bf68b3bf2ee
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: db1882befdb1b295ce247cb62601bbec4ba3db5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203006"
---
# <a name="sql-to-c-time"></a>SQL a C: Time
El identificador para el tipo de datos SQL de tiempo ODBC es:  
  
 SQL_TYPE_TIME  
  
 En la tabla siguiente se muestran los tipos de datos de ODBC C en los que se pueden convertir los datos de SQL. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de SQL a tipos de datos de C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificador de tipo de C|Prueba|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > longitud de bytes de caracteres<br /><br /> *9*  <=  *BufferLength* <= longitud de bytes de caracteres<br /><br /> *BufferLength* < 9|data<br /><br /> Datos truncados [a]<br /><br /> No definido|Longitud de los datos en bytes<br /><br /> Longitud de los datos en bytes<br /><br /> No definido|N/D<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|*BufferLength* longitud de caracteres de ><br /><br /> *9*  <=  *BufferLength* <= longitud de caracteres<br /><br /> *BufferLength* < 9|data<br /><br /> Datos truncados [a]<br /><br /> No definido|Longitud de los datos en caracteres<br /><br /> Longitud de los datos en caracteres<br /><br /> No definido|N/D<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Longitud de bytes de los datos <= *BufferLength*<br /><br /> Longitud de bytes de los datos > *BufferLength*|data<br /><br /> No definido|Longitud de los datos en bytes<br /><br /> No definido|N/D<br /><br /> 22003|  
|SQL_C_TYPE_TIME|Ninguno [b]|data|6 [d]|N/D|  
|SQL_C_TYPE_TIMESTAMP|Ninguno [b]|Datos [c]|16 [d]|N/D|  
  
 [a] las fracciones de segundo de la hora se truncan.  
  
 [b] el valor de *BufferLength* se omite para esta conversión. El controlador supone que el tamaño de **TargetValuePtr* es el tamaño del tipo de datos de C.  
  
 [c] los campos de fecha de la estructura de marca de tiempo se establecen en la fecha actual y el campo de fracciones de segundo de la estructura de marca de tiempo se establece en cero.  
  
 [d] es el tamaño del tipo de datos de C correspondiente.  
  
 Cuando los datos de SQL se convierten en datos de caracteres C, la cadena resultante tiene el formato "*HH*:*mm*:*SS*". Este formato no se ve afectado por la configuración del país de Windows®.
