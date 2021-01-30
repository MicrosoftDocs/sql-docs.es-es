---
description: Convertir datos de SQL a tipos de datos C
title: Convertir datos de tipos de datos de SQL a C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from SQL to C types [ODBC]
- data conversions from SQL to C types [ODBC], about converting
- data types [ODBC], C data types
- data types [ODBC], converting data
- data types [ODBC], SQL data types
- SQL data types [ODBC], converting to C types
- converting data from SQL to c types [ODBC]
- converting data from SQL to c types [ODBC], about converting
- C data types [ODBC], converting from SQL types
ms.assetid: 029727f6-d3f0-499a-911c-bcaf9714e43b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5cd8b0790426d9f3461d8ff1600471867ce94b55
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165381"
---
# <a name="converting-data-from-sql-to-c-data-types"></a>Convertir datos de SQL a tipos de datos C
Cuando una aplicación llama a **SQLFetch**, **SQLFetchScroll** o **SQLGetData**, el controlador recupera los datos del origen de datos. Si es necesario, convierte los datos del tipo de datos en el que el controlador los recuperó en el tipo de datos especificado por el argumento *TargetType* en **SQLBindCol** o **SQLGetData.** Por último, almacena los datos en la ubicación a la que apunta el argumento *TargetValuePtr* de **SQLBindCol** o **SQLGetData** (y el campo SQL_DESC_DATA_PTR del ARD).  
  
 En la tabla siguiente se muestran las conversiones admitidas de tipos de datos SQL de ODBC a tipos de datos C de ODBC. Un círculo relleno indica la conversión predeterminada para un tipo de datos SQL (el tipo de datos C al que se convertirán los datos cuando el valor de *TargetType* sea SQL_C_DEFAULT). Un círculo hueco indica una conversión compatible.  
  
 Para que una aplicación ODBC *3. x* funcione con un controlador ODBC *2. x* , puede que no se admita la conversión de tipos de datos específicos del controlador.  
  
 El formato de los datos convertidos no se ve afectado por la configuración del país de Windows®.  
  
 En las tablas de las secciones siguientes se describe cómo el controlador o el origen de datos convierte los datos recuperados del origen de datos; los controladores son necesarios para admitir las conversiones a todos los tipos de datos C de ODBC de los tipos de datos SQL de ODBC que admiten. Para un tipo de datos SQL de ODBC determinado, la primera columna de la tabla enumera los valores de entrada válidos del argumento *TargetType* en **SQLBindCol** y **SQLGetData**. En la segunda columna se muestran los resultados de una prueba, a menudo con el argumento *BufferLength* especificado en **SQLBindCol** o **SQLGetData**, que el controlador realiza para determinar si puede convertir los datos. Para cada resultado, la tercera y cuarta columnas enumeran los valores colocados en los búferes especificados por los argumentos *TargetValuePtr* y *StrLen_or_IndPtr* especificados en **SQLBindCol** o **SQLGetData** una vez que el controlador ha intentado convertir los datos. (El argumento *StrLen_or_IndPtr* corresponde al campo SQL_DESC_OCTET_LENGTH_PTR de ARD). La última columna muestra el SQLSTATE devuelto para cada resultado por **SQLFetch**, **SQLFetchScroll** o **SQLGetData**.  
  
 Si el argumento *TargetType* de **SQLBindCol** o **SQLGetData** contiene un identificador para un tipo de datos C de ODBC que no se muestra en la tabla para un tipo de datos SQL de ODBC determinado, **SQLFetch**, **SQLFetchScroll** o **SQLGetData** devuelve SQLSTATE 07006 (infracción de atributo de tipo de datos restringida). Si el argumento *TargetType* contiene un identificador que especifica una conversión de un tipo de datos de SQL específico del controlador a un tipo de datos C de ODBC y esta conversión no es compatible con el controlador, **SQLFetch**, **SQLFETCHSCROLL** o **SQLGetData** devuelve SQLSTATE HYC00 (característica opcional no implementada).  
  
 Aunque no se muestra en las tablas, el controlador devuelve SQL_NULL_DATA en el búfer especificado por el argumento *StrLen_or_IndPtr* cuando el valor de datos SQL es NULL. Para obtener una explicación del uso de *StrLen_or_IndPtr* cuando se realizan varias llamadas para recuperar datos, vea la descripción de la función [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md). Cuando los datos de SQL se convierten en datos de caracteres C, el número de caracteres devueltos en \* *StrLen_or_IndPtr* no incluye el byte de terminación null. Si *TargetValuePtr* es un puntero nulo, **SQLGETDATA** devuelve SQLSTATE HY009 (uso no válido de puntero nulo); en **SQLBindCol**, este desenlaza la columna.  
  
 En las tablas se usan los siguientes términos y convenciones:  
  
-   La **longitud de bytes de los datos** es el número de bytes de datos de C disponibles para devolver en **TargetValuePtr*, independientemente de si los datos se truncarán o no antes de que se devuelvan a la aplicación. En el caso de los datos de cadena, no incluye el espacio para el carácter de terminación null.  
  
-   **Longitud de bytes de caracteres** es el número total de bytes necesarios para mostrar los datos en formato de caracteres. Esto es como se define para cada tipo de datos de C en la sección [tamaño de presentación](../../../odbc/reference/appendixes/display-size.md), excepto en que la longitud de bytes de caracteres está en bytes mientras que el tamaño de presentación está en caracteres.  
  
-   Las palabras en *cursiva* representan los argumentos de la función o los elementos de la gramática de SQL. Para obtener la sintaxis de los elementos de gramática, vea el [Apéndice C: gramática de SQL](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md).  
  
 Esta sección contiene los temas siguientes.  
  
-   [SQL a C: Carácter](../../../odbc/reference/appendixes/sql-to-c-character.md)  
  
-   [SQL a C: Numeric](../../../odbc/reference/appendixes/sql-to-c-numeric.md)  
  
-   [SQL a C: bit](../../../odbc/reference/appendixes/sql-to-c-bit.md)  
  
-   [SQL a C: Binary](../../../odbc/reference/appendixes/sql-to-c-binary.md)  
  
-   [SQL a C: Date](../../../odbc/reference/appendixes/sql-to-c-date.md)  
  
-   [SQL a C: GUID](../../../odbc/reference/appendixes/sql-to-c-guid.md)  
  
-   [SQL a C: Time](../../../odbc/reference/appendixes/sql-to-c-time.md)  
  
-   [SQL a C: Timestamp](../../../odbc/reference/appendixes/sql-to-c-timestamp.md)  
  
-   [SQL a C: Intervalos de mes y año](../../../odbc/reference/appendixes/sql-to-c-year-month-intervals.md)  
  
-   [SQL a C: Intervalos de día y hora](../../../odbc/reference/appendixes/sql-to-c-day-time-intervals.md)  
  
-   [SQL a los ejemplos de conversión de datos de C](../../../odbc/reference/appendixes/sql-to-c-data-conversion-examples.md)
