---
description: Convertir datos de C en tipos de datos SQL
title: Convertir datos de tipos de datos de C a SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from c to SQL types [ODBC], about converting
- converting data from c to SQL types [ODBC]
- data conversions from C to SQL types [ODBC], about converting
- data types [ODBC], C data types
- data types [ODBC], converting data
- SQL data types [ODBC], converting from C types
- data types [ODBC], SQL data types
- data conversions from C to SQL types [ODBC]
- C data types [ODBC], converting to SQL types
ms.assetid: ee0afe78-b58f-4d34-ad9b-616bb23653bd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a326f91f0a10e8731153817cd8c2eed7ff0656e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165390"
---
# <a name="converting-data-from-c-to-sql-data-types"></a>Convertir datos de C en tipos de datos SQL
Cuando una aplicación llama a **SQLExecute** o **SQLExecDirect**, el controlador recupera los datos para todos los parámetros enlazados con **SQLBindParameter** desde las ubicaciones de almacenamiento de la aplicación. Cuando una aplicación llama a **SQLSetPos**, el controlador recupera los datos para una operación de actualización o adición de columnas enlazadas con **SQLBindCol**. En el caso de los parámetros de datos en ejecución, la aplicación envía los datos de parámetro con **SQLPutData**. Si es necesario, el controlador convierte los datos del tipo de datos especificado por el argumento *ValueType* en **SQLBindParameter** al tipo de datos especificado por el argumento *ParameterType* en **SQLBindParameter** y, a continuación, envía los datos al origen de datos.  
  
 En la tabla siguiente se muestran las conversiones admitidas de tipos de datos de C de ODBC a tipos de datos SQL de ODBC. Un círculo relleno indica la conversión predeterminada para un tipo de datos SQL (el tipo de datos C desde el que se convertirán los datos cuando el valor de *ValueType* o el campo descriptor de SQL_DESC_CONCISE_TYPE sea SQL_C_DEFAULT). Un círculo hueco indica una conversión compatible.  
  
 El formato de los datos convertidos no se ve afectado por la configuración del país de Windows®.  
  
 ![Conversiones admitidas: de tipos de datos ODBC C a SQL](../../../odbc/reference/appendixes/media/apd1b.gif "apd1b")  
  
 En las tablas de las secciones siguientes se describe cómo el controlador o el origen de datos convierte los datos que se envían al origen de datos. los controladores son necesarios para admitir conversiones de todos los tipos de datos de C de ODBC en los tipos de datos de SQL de ODBC que admiten. Para un tipo de datos C de ODBC determinado, la primera columna de la tabla muestra los valores de entrada válidos del argumento *ParameterType* en **SQLBindParameter**. En la segunda columna se muestran los resultados de una prueba que el controlador realiza para determinar si puede convertir los datos. La tercera columna muestra el SQLSTATE devuelto para cada resultado por **SQLExecDirect**, **SQLExecute**, **SQLBulkOperations**, **SQLSetPos** o **SQLPutData**. Los datos se envían al origen de datos solo si se devuelve SQL_SUCCESS.  
  
 Si el argumento *ParameterType* de **SQLBindParameter** contiene el identificador de un tipo de datos SQL de ODBC que no se muestra en la tabla para un tipo de datos de C determinado, **SQLBindParameter** devuelve SQLSTATE 07006 (infracción de atributo de tipo de datos restringido). Si el argumento *ParameterType* contiene un identificador específico del controlador y el controlador no admite la conversión del tipo de datos de ODBC C específico a ese tipo de datos SQL específico del controlador, **SQLBINDPARAMETER** devuelve SQLSTATE HYC00 (característica opcional no implementada).  
  
 Si los argumentos *ParameterValuePtr* y *StrLen_or_IndPtr* especificados en **SQLBindParameter** son punteros nulos, esa función devuelve SQLSTATE HY009 (uso no válido de puntero nulo). Aunque no se muestra en las tablas, una aplicación establece el valor del búfer de longitud/indicador al que apunta el argumento *StrLen_or_IndPtr* de **SQLBindParameter** o el valor del argumento *StrLen_or_IndPtr* de **SQLPutData** a SQL_NULL_DATA para especificar un valor de datos SQL nulo. (El argumento *StrLen_or_IndPtr* corresponde al campo SQL_DESC_OCTET_LENGTH_PTR de APD). La aplicación establece estos valores en SQL_NTS para especificar que el valor de \* *ParameterValuePtr* en **SQLBindParameter** o \* *DataPtr* en **SQLPUTDATA** (al que apunta el campo SQL_DESC_DATA_PTR de APD) es una cadena terminada en NULL.  
  
 Los siguientes términos se usan en las tablas:  
  
-   **Longitud de bytes de los datos** : el número de bytes de datos SQL disponibles para enviar al origen de datos, si los datos se truncarán o no antes de enviarse al origen de datos. En el caso de los datos de cadena, no incluye espacio para el carácter de terminación null.  
  
-   **Longitud de bytes de columna** : número de bytes necesarios para almacenar los datos en el origen de datos.  
  
-   **Longitud de bytes de caracteres** : número máximo de bytes necesarios para mostrar los datos en formato de caracteres. Esto es lo que se define para cada tipo de datos de SQL en [el tamaño de presentación](../../../odbc/reference/appendixes/display-size.md), excepto que la longitud de bytes de caracteres está en bytes, mientras que el tamaño de presentación está en caracteres.  
  
-   **Número de dígitos** : número de caracteres que se usa para representar un número, incluido el signo menos, el separador decimal y el exponente (si es necesario).  
  
-   **Palabras en**   
     **_Italics_**  : elementos de la gramática de SQL. Para obtener la sintaxis de los elementos de gramática, vea el [Apéndice C: gramática de SQL](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md).  
  
 Esta sección contiene los temas siguientes.  
  
-   [C a SQL: Carácter](../../../odbc/reference/appendixes/c-to-sql-character.md)  
  
-   [C a SQL: Numeric](../../../odbc/reference/appendixes/c-to-sql-numeric.md)  
  
-   [C a SQL: bit](../../../odbc/reference/appendixes/c-to-sql-bit.md)  
  
-   [C a SQL: Binary](../../../odbc/reference/appendixes/c-to-sql-binary.md)  
  
-   [C a SQL: Date](../../../odbc/reference/appendixes/c-to-sql-date.md)  
  
-   [C a SQL: GUID](../../../odbc/reference/appendixes/c-to-sql-guid.md)  
  
-   [C a SQL: Time](../../../odbc/reference/appendixes/c-to-sql-time.md)  
  
-   [C a SQL: Timestamp](../../../odbc/reference/appendixes/c-to-sql-timestamp.md)  
  
-   [C a SQL: Intervalos de mes y año](../../../odbc/reference/appendixes/c-to-sql-year-month-intervals.md)  
  
-   [C a SQL: Intervalos de día y hora](../../../odbc/reference/appendixes/c-to-sql-day-time-intervals.md)  
  
-   [Ejemplos de conversión de datos de C en SQL](../../../odbc/reference/appendixes/c-to-sql-data-conversion-examples.md)
