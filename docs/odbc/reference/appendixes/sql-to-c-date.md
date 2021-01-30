---
description: 'SQL a C: Date'
title: 'SQL a C: fecha | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], date
- date data type [ODBC]
- data conversions from SQL to C types [ODBC], date
ms.assetid: 703c7960-9cf4-4d7a-9920-53b29c184f97
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9909d0344c2df53212be54e4c695b121c0717e78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203052"
---
# <a name="sql-to-c-date"></a>SQL a C: Date
El identificador para el tipo de datos SQL de ODBC de fecha es:  
  
 SQL_TYPE_DATE  
  
 En la tabla siguiente se muestran los tipos de datos de ODBC C en los que se pueden convertir datos SQL de fecha. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de SQL a tipos de datos de C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificador de tipo de C|Prueba|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > longitud de bytes de caracteres<br /><br /> 11 <= *BufferLength* <= longitud de bytes de caracteres<br /><br /> *BufferLength* < 11|data<br /><br /> Datos truncados<br /><br /> No definido|10<br /><br /> Longitud de los datos en bytes<br /><br /> No definido|N/D<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|*BufferLength* longitud de caracteres de ><br /><br /> 11 <= *BufferLength* <= longitud de caracteres<br /><br /> *BufferLength* < 11|data<br /><br /> Datos truncados<br /><br /> No definido|10<br /><br /> Longitud de los datos en caracteres<br /><br /> No definido|N/D<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Longitud de bytes de los datos <= *BufferLength*<br /><br /> Longitud de bytes de los datos > *BufferLength*|data<br /><br /> No definido|Longitud de los datos en bytes<br /><br /> No definido|N/D<br /><br /> 22003|  
|SQL_C_TYPE_DATE|Ninguno [a]|data|6 [c]|N/D|  
|SQL_C_TYPE_TIMESTAMP|Ninguno [a]|Datos [b]|16 [c]|N/D|  
  
 [a] el valor de *BufferLength* se omite para esta conversión. El controlador supone que el tamaño de **TargetValuePtr* es el tamaño del tipo de datos de C.  
  
 [b] los campos de hora de la estructura de marca de tiempo se establecen en cero.  
  
 [c] es el tamaño del tipo de datos de C correspondiente.  
  
 Cuando se convierten datos de SQL de fecha en datos de caracteres C, la cadena resultante tiene el formato "*AAAA* - *mm* - *DD*". Este formato no se ve afectado por la configuración del país de Windows®.
