---
description: 'SQL a C: Timestamp'
title: 'SQL a C: marca de tiempo | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- timestamp data type [ODBC]
- converting data from SQL to C types [ODBC], timestamp
- data conversions from SQL to C types [ODBC], timestamp
ms.assetid: 6a0617cf-d8c0-4316-8bb4-e6ddb45d7bf1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1d7881ad4ef9280ab9f2e7ac0d941ae2228b21b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202993"
---
# <a name="sql-to-c-timestamp"></a>SQL a C: Timestamp

El identificador del tipo de datos SQL de marca de tiempo de ODBC es el siguiente:

- SQL_TYPE_TIMESTAMP  

En la tabla siguiente se muestran los tipos de datos C de ODBC a los que se pueden convertir los datos SQL de marca de tiempo. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de SQL a tipos de datos de C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  

|Identificador de tipo de C|Prueba|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > longitud de bytes de caracteres<br /><br /> 20 <= *BufferLength* <= longitud de bytes de caracteres<br /><br /> *BufferLength* < 20|data<br /><br /> Datos truncados [b]<br /><br /> No definido|Longitud de los datos en bytes<br /><br /> Longitud de los datos en bytes<br /><br /> No definido|N/D<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|*BufferLength* longitud de caracteres de ><br /><br /> 20 <= *BufferLength* <= longitud de caracteres<br /><br /> *BufferLength* < 20|data<br /><br /> Datos truncados [b]<br /><br /> No definido|Longitud de los datos en caracteres<br /><br /> Longitud de los datos en caracteres<br /><br /> No definido|N/D<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Longitud de bytes de los datos <= *BufferLength*<br /><br /> Longitud de bytes de los datos > *BufferLength*|data<br /><br /> No definido|Longitud de los datos en bytes<br /><br /> No definido|N/D<br /><br /> 22003|  
|SQL_C_TYPE_DATE|La parte de hora de la marca de tiempo es cero [a]<br /><br /> La parte de hora de la marca de tiempo es distinto de cero [a]|data<br /><br /> Datos truncados [c]|6 [f]<br /><br /> 6 [f]|N/D<br /><br /> 01S07|  
|SQL_C_TYPE_TIME|La parte de la marca de tiempo de fracciones de segundo es cero [a]<br /><br /> La parte de la marca de tiempo de fracciones de segundo es distinto de cero [a]|Datos [d]<br /><br /> Datos truncados [d], [e]|6 [f]<br /><br /> 6 [f]|N/D<br /><br /> 01S07|  
|SQL_C_TYPE_TIMESTAMP|La parte de las fracciones de segundo de la marca de tiempo no se trunca [a]<br /><br /> La parte de las fracciones de segundo de la marca de tiempo se trunca [a]|Datos [e]<br /><br /> Datos truncados [e]|16 [f]<br /><br /> 16 [f]|N/D<br /><br /> 01S07|  

 [a] el valor de *BufferLength* se omite para esta conversión. El controlador supone que el tamaño de **TargetValuePtr* es el tamaño del tipo de datos de C.  
  
 [b] las fracciones de segundo de la marca de tiempo se truncan.  
  
 [c] la parte de hora de la marca de tiempo se trunca.  
  
 [d] se omite la parte de fecha de la marca de tiempo.  
  
 [e] se trunca la parte de fracciones de segundo de la marca de tiempo.  
  
 [f] es el tamaño del tipo de datos de C correspondiente.  

Cuando los datos de marca de tiempo de SQL se convierten en datos de caracteres C, la cadena resultante se encuentra en "*AAAA* - *mm* - *DD* *HH*:*mm*:*SS*[.*f...*] ", donde se pueden usar hasta nueve dígitos para las fracciones de segundo. Este formato no se ve afectado por la configuración del país de Windows®. (Excepto el separador decimal y las fracciones de segundo, se debe usar todo el formato, independientemente de la precisión del tipo de datos de marca de tiempo de SQL).
