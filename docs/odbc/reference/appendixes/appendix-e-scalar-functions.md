---
description: 'Apéndice E: Funciones escalares'
title: 'Apéndice E: funciones escalares | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL-92 functions [ODBC]
- scalar functions [ODBC]
- functions [ODBC], scalar
ms.assetid: 59c7cd5e-32d6-43ab-bac3-7010322d105a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 94ad0914477de0e2dfd25667755757bbc36a8b29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212535"
---
# <a name="appendix-e-scalar-functions"></a>Apéndice E: Funciones escalares
ODBC especifica los siguientes tipos de funciones escalares, con información detallada sobre cada uno de estos tipos de función proporcionados en las secciones correspondientes de este apéndice. La descripción de la función incluye la sintaxis asociada.  
  
 Este apéndice contiene los siguientes temas.  
  
-   [Funciones de cadena](../../../odbc/reference/appendixes/string-functions.md)  
  
-   [Funciones numéricas](../../../odbc/reference/appendixes/numeric-functions.md)  
  
-   [Funciones de hora, fecha e intervalo](../../../odbc/reference/appendixes/time-date-and-interval-functions.md)  
  
-   [Funciones del sistema](../../../odbc/reference/appendixes/system-functions.md)  
  
-   [Función de conversión de tipo de datos explícito](../../../odbc/reference/appendixes/explicit-data-type-conversion-function.md)  
  
-   [Función CAST de SQL-92](../../../odbc/reference/appendixes/sql-92-cast-function.md)  
  
 ODBC no impone un tipo de datos para los valores devueltos de las funciones escalares porque las funciones suelen ser específicas del origen de datos. Las aplicaciones deben usar la función escalar CONVERT siempre que sea posible para forzar la conversión del tipo de datos.  
  
## <a name="odbc-and-sql-92-scalar-functions"></a>Funciones escalares de ODBC y SQL-92  
 Las tablas de este apéndice incluyen funciones que se han agregado en ODBC 3,0 para alinearse con SQL-92. Las funciones agregadas para un tipo determinado de función escalar, tal como se define en ODBC, se indican en cada sección.  
  
 ODBC y SQL-92 clasifican sus funciones escalares de manera diferente. ODBC clasifica las funciones escalares por el tipo de argumento; SQL-92 los clasifica por valor devuelto. Por ejemplo, ODBC clasifica la función EXTRACT como una función TimeDate, ya que el argumento Extract-Field es una palabra clave DateTime y el argumento Extract-Source es una expresión de fecha y hora o de intervalo. SQL-92, por otro lado, clasifica EXTRACT como una función escalar numérica, ya que el valor devuelto es numérico.  
  
 Una aplicación puede determinar qué funciones escalares admite un controlador mediante una llamada a **SQLGetInfo**. Los tipos de información se incluyen para las clasificaciones ODBC y SQL-92 de las funciones escalares. Dado que estas clasificaciones son diferentes, la compatibilidad con algunas funciones escalares se puede indicar en tipos de información que no se corresponden con ODBC y SQL-92. Por ejemplo, la compatibilidad con EXTRACT en ODBC se indica mediante el tipo de información SQL_TIMEDATE_FUNCTIONS; la compatibilidad con EXTRACT en SQL-92, por otro lado, se indica mediante el tipo de información SQL_SQL92_NUMERIC_VALUE_FUNCTIONS.
