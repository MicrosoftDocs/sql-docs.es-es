---
description: Tipos de datos de dBASE
title: Tipos de datos de dBASE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- Jet-based ODBC drivers [ODBC], DBasedriver
- desktop database drivers [ODBC], DBasedriver
- DBase driver [ODBC], data types
- data types [ODBC], DBase driver
- dbase data types [ODBC]
- ODBC desktop database drivers [ODBC], DBasedriver
ms.assetid: a0e31e6b-d02b-4ee2-9b37-5baf6a11c0a6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4ef5b42157a011bf719614b7378cd73733e28afc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165423"
---
# <a name="dbase-data-types"></a>Tipos de datos de dBASE
En la tabla siguiente se muestra cómo se asignan los tipos de datos de dBASE a los tipos de datos SQL de ODBC. Tenga en cuenta que no se admiten todos los tipos de datos SQL de ODBC.  
  
|dBASE, tipo de datos|Tipo de datos de ODBC|  
|---------------------|--------------------|  
|CHAR|SQL_VARCHAR|  
|DATE|SQL_DATE|  
|FLOAT [1]|SQL_DOUBLE|  
|OPERADOR|SQL_BIT|  
|Memorando|SQL_LONGVARCHAR|  
|NUMERIC (BCD)|SQL_DOUBLE|  
|OLEOBJECT [1]|SQL_LONGBINARY|  
  
 [1] solo es válido para dBASE versión 5. *x*  
  
 La precisión en dBASE III permite números con exponentes de hasta dos dígitos y en números de dBASE IV con exponentes de hasta tres dígitos. Dado que los números se almacenan como texto, se convierten en números. Si el número que se va a convertir no cabe en un campo, pueden producirse resultados no explicados.  
  
 Aunque dBASE permite especificar una precisión y una escala con un tipo de datos numérico, el controlador ODBC dBASE no lo admite. El controlador ODBC dBASE siempre devuelve una precisión de 15 y una escala de 0 para un tipo de datos numérico.  
  
 Una columna creada con el tipo de datos Numeric mediante el controlador ODBC dBASE se asigna al tipo de datos SQL_DOUBLE ODBC. Por lo tanto, los datos de esta columna están sujetos a redondeo. Este comportamiento no es el mismo que el del tipo de datos NUMERIC en dBASE (tipo N), que es el decimal codificado binario (BCD).  
  
> [!NOTE]  
>  **SQLGetTypeInfo** devuelve los tipos de datos SQL de ODBC. Todas las conversiones del Apéndice D de la *Referencia del programador de ODBC* son compatibles con los tipos de datos SQL de ODBC enumerados anteriormente en este tema.  
  
 En la tabla siguiente se muestran las limitaciones en los tipos de datos de dBASE.  
  
|Tipo de datos|Descripción|  
|---------------|-----------------|  
|CHAR|Al crear una columna CHAR de cero o una longitud no especificada, se devuelve realmente una columna de 254 bytes.|  
|Datos cifrados|El controlador dBASE no admite tablas de dBASE cifradas.|  
|OPERADOR|El controlador dBASE no puede crear un índice en una columna lógica.|  
|Memorando|La longitud máxima de una columna de memorando es de 65.500 bytes.|  
  
 Se pueden encontrar más limitaciones en los tipos de datos en limitaciones de tipos de [datos](../../odbc/microsoft/data-type-limitations.md).
