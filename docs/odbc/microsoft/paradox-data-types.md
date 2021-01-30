---
description: Tipos de datos de Paradox
title: Tipos de datos de Paradox | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], Paradox driver
- desktop database drivers [ODBC], Paradox driver
- Paradox data types [ODBC]
- Jet-based ODBC drivers [ODBC], Paradox driver
- data types [ODBC], Paradox driver
- Paradox driver [ODBC], data types
ms.assetid: 0c9e5d21-9321-49f8-a055-69459e1c9c85
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ee49ff2df24a57daa481ea650f57c09a7712400e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203511"
---
# <a name="paradox-data-types"></a>Tipos de datos de Paradox
El controlador ODBC Paradox asigna tipos de datos de Paradox a tipos de datos SQL de ODBC. En la tabla siguiente se enumeran todos los tipos de datos de Paradox y se muestran los tipos de datos SQL de ODBC a los que están asignados.  
  
|Tipo de datos de Paradox|Tipo de datos de ODBC|  
|-----------------------|--------------------|  
|ALFABÉTICO|SQL_VARCHAR|  
|INCREMENTO AUTOMÁTICO [1]|SQL_INTEGER|  
|BCD [1]|SQL_DOUBLE|  
|BYTES [1]|SQL_BINARY|  
|DATE|SQL_DATE|  
|IMAGEN [2]|SQL_LONGVARBINARY|  
|LOGICAL [1]|SQL_BIT|  
|LARGO [1]|SQL_INTEGER|  
|MEMORANDO [2]|SQL_LONGVARCHAR|  
|MONEY [1]|SQL_DOUBLE|  
|NUMBER|SQL_DOUBLE|  
|SHORT|SQL_SMALLINT|  
|HORA [1]|SQL_TIMESTAMP|  
|MARCA DE TIEMPO [1]|SQL_TIMESTAMP|  
  
 [1] solo es válido para las versiones 5 de Paradox. *x*.  
  
 [2] solo es válido para las versiones 4 de Paradox. *x* y 5. *x*.  
  
> [!NOTE]  
>  **SQLGetTypeInfo** devuelve los tipos de datos SQL de ODBC. Todas las conversiones del Apéndice D de la *Referencia del programador de ODBC* son compatibles con los tipos de datos SQL de ODBC enumerados anteriormente en este tema.  
  
 En la tabla siguiente se muestran las limitaciones en los tipos de datos de Paradox.  
  
|Tipo de datos|Descripción|  
|---------------|-----------------|  
|ALFABÉTICO|Al crear una columna alfanumérica de cero o una longitud no especificada, se devuelve realmente una columna de 255 bytes.|  
|BYTES|Si inserta NULL en una columna binaria con el controlador Paradox5, se cambia a 0.|  
|LONG|El valor negativo máximo admitido por el controlador de Paradox para el tipo de datos Long en Paradox 5. *x* no es-2 ^ 31 (-2147483648), ya que debe ser debido a que Long se asigna al tipo de datos ODBC SQL_INTEGER. El valor negativo máximo admitido para Long es en realidad-2 ^ 31 + 1 (-2147483647).|  
|timestamp|Cuando un valor se inserta en una columna de marca de tiempo por el controlador de Paradox y, posteriormente, se recupera de la columna, el valor recuperado puede diferir del valor insertado en un segundo debido al redondeo.|  
  
 Se pueden encontrar más limitaciones en los tipos de datos en limitaciones de tipos de [datos](../../odbc/microsoft/data-type-limitations.md).
