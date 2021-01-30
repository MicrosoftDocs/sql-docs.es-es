---
description: Identificadores y descriptores de tipo de datos
title: Identificadores y descriptores de tipo de datos | Microsoft Docs
ms.custom: ''
ms.date: 02/02/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], identifiers
- identifiers [ODBC], data types
- descriptors [ODBC], data types
- verbose data types [ODBC]
- data types [ODBC], descriptors
- concise data types [ODBC]
ms.assetid: f0077c9b-8eb2-4b5f-8c4c-7436fdef37ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0537c2b4f76814dfedf2750fa72ee0a13ed61175
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165350"
---
# <a name="data-type-identifiers-and-descriptors"></a>Identificadores y descriptores de tipo de datos
Los tipos de datos que se enumeran en las secciones tipos de datos de [SQL](../../../odbc/reference/appendixes/sql-data-types.md) y tipos de datos de [C](../../../odbc/reference/appendixes/c-data-types.md) descritos anteriormente en este apéndice son tipos de datos "concisos": cada identificador hace referencia a un único tipo de datos. Hay una correspondencia uno a uno entre el identificador y el tipo de datos. Sin embargo, los descriptores no usan un valor único para identificar los tipos de datos. En algunos casos, usan un tipo de datos "verbose" y un subcódigo de tipo. En todos los tipos de datos excepto los tipos de datos datetime e Interval, el identificador de tipo detallado es el mismo que el identificador de tipo conciso y el valor de SQL_DESC_DATETIME_INTERVAL_CODE es igual a 0. En el caso de los tipos de datos datetime e Interval, sin embargo, un tipo detallado (SQL_DATETIME o SQL_INTERVAL) se almacena en SQL_DESC_TYPE, se almacena un tipo conciso en SQL_DESC_CONCISE_TYPE y un subcódigo para cada tipo conciso se almacena en SQL_DESC_DATETIME_INTERVAL_CODE. Establecer uno de estos campos afecta a los demás. Para obtener más información sobre estos campos, vea la descripción de la función [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) .  
  
 Cuando se establece el campo SQL_DESC_TYPE o SQL_DESC_CONCISE_TYPE para algunos tipos de datos, los campos SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_LENGTH, SQL_DESC_PRECISION y SQL_DESC_SCALE se establecen automáticamente en los valores predeterminados, según corresponda para el tipo de datos. Para obtener más información, vea la descripción del campo SQL_DESC_TYPE en [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Si alguno de los valores predeterminados establecidos no es adecuado, la aplicación debe establecer explícitamente el campo descriptor mediante una llamada a **SQLSetDescField**.  
  
 En la tabla siguiente se muestra el identificador de tipo conciso, el identificador de tipo detallado y el subcódigo de tipo para cada identificador de tipo DateTime e Interval de SQL y. Como se indica en esta tabla, en los tipos de datos datetime y Interval, los campos SQL_DESC_TYPE y SQL_DESC_DATETIME_INTERVAL_CODE tienen las mismas constantes de manifiesto tanto para los tipos de datos SQL (en los descriptores de implementación) como para los tipos de datos de C (en los descriptores de aplicación).  
  
|Tipo SQL conciso|Tipo de C conciso|Tipo detallado|DATETIME_INTERVAL_CODE|  
|----------------------|--------------------|------------------|------------------------------|  
|SQL_TYPE_DATE|SQL_C_TYPE_DATE|SQL_DATETIME|SQL_CODE_DATE|  
|SQL_TYPE_TIME|SQL_C_TYPE_TIME|SQL_DATETIME|SQL_CODE_TIME|  
|SQL_TYPE_TIMESTAMP|SQL_C_TYPE_TIMESTAMP|SQL_DATETIME|SQL_CODE_TIMESTAMP|  
|SQL_INTERVAL_MONTH|SQL_C_INTERVAL_MONTH|SQL_INTERVAL|SQL_CODE_MONTH|  
|SQL_INTERVAL_YEAR|SQL_C_INTERVAL_YEAR|SQL_INTERVAL|SQL_CODE_YEAR|  
|SQL_INTERVAL_YEAR_TO_MONTH|SQL_C_INTERVAL_YEAR_TO_MONTH|SQL_INTERVAL|SQL_CODE_YEAR_TO_MONTH|  
|SQL_INTERVAL_DAY|SQL_C_INTERVAL_DAY|SQL_INTERVAL|SQL_CODE_DAY|  
|SQL_INTERVAL_HOUR|SQL_C_INTERVAL_HOUR|SQL_INTERVAL|SQL_CODE_HOUR|  
|SQL_INTERVAL_MINUTE|SQL_C_INTERVAL_MINUTE|SQL_INTERVAL|SQL_CODE_MINUTE|  
|SQL_INTERVAL_SECOND|SQL_C_INTERVAL_SECOND|SQL_INTERVAL|SQL_CODE_SECOND|  
|SQL_INTERVAL_DAY_TO_HOUR|SQL_C_INTERVAL_DAY_TO_HOUR|SQL_INTERVAL|SQL_CODE_DAY_TO_HOUR|  
|SQL_INTERVAL_DAY_TO_MINUTE|SQL_C_INTERVAL_DAY_TO_MINUTE|SQL_INTERVAL|SQL_CODE_DAY_TO_MINUTE|  
|SQL_INTERVAL_DAY_TO_SECOND|SQL_C_INTERVAL_DAY_TO_SECOND|SQL_INTERVAL|SQL_CODE_DAY_TO_SECOND|  
|SQL_INTERVAL_HOUR_TO_MINUTE|SQL_C_INTERVAL_HOUR_TO_MINUTE|SQL_INTERVAL|SQL_CODE_HOUR_TO_MINUTE|  
|SQL_INTERVAL_HOUR_TO_SECOND|SQL_C_INTERVAL_HOUR_TO_SECOND|SQL_INTERVAL|SQL_CODE_HOUR_TO_SECOND|  
|SQL_INTERVAL_MINUTE_TO_SECOND|SQL_C_INTERVAL_MINUTE_TO_SECOND|SQL_INTERVAL|SQL_CODE_MINUTE_TO_SECOND|
