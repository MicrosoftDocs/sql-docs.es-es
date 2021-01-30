---
description: Tipos de datos C
title: Tipos de datos de C | Microsoft Docs
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], C data types
- C data types [ODBC], about C data types
- C data types [ODBC]
- C buffers [ODBC]
ms.assetid: b681d260-3dbb-47df-a616-4910d727add7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5dbce167ea65daa030030bd32a909c5af3d06262
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212460"
---
# <a name="c-data-types"></a>Tipos de datos C
Los tipos de datos C de ODBC indican el tipo de datos de los búferes de C que se usan para almacenar datos en la aplicación.  
  
 Todos los controladores deben admitir todos los tipos de datos de C. Esto es necesario porque todos los controladores deben admitir todos los tipos de C a los que se pueden convertir los tipos SQL que admiten, y todos los controladores admiten al menos un tipo de SQL de caracteres. Dado que el tipo SQL de caracteres se puede convertir a y desde todos los tipos de C, todos los controladores deben admitir todos los tipos de C.  
  
 El tipo de datos C se especifica en las funciones **SQLBindCol** y **SQLGetData** con el argumento *TargetType* y en la función **SQLBindParameter** con el argumento *ValueType* . También se puede especificar llamando a **SQLSetDescField** para establecer el campo de SQL_DESC_CONCISE_TYPE de un ARD o APD, o bien llamando a **SQLSetDescRec** con el argumento de *tipo* (y el argumento de *subtipo* si es necesario) y el argumento *DESCRIPTORHANDLE* establecido en el identificador de un ARD o APD.  
  
 En las tablas siguientes se enumeran los identificadores de tipo válidos para los tipos de datos de C. La tabla también muestra el tipo de datos C de ODBC que corresponde a cada identificador y la definición de este tipo de datos.  
  
|Identificador de tipo de C|Definición de tipo C de ODBC|Tipo de C|  
|-----------------------|--------------------|------------|  
|SQL_C_CHAR|SQLCHAR|unsigned char *|  
|SQL_C_WCHAR|SQLWCHAR|wchar_t *|  
|SQL_C_SSHORT [j]|SQLSMALLINT|short int|  
|SQL_C_USHORT [j]|SQLUSMALLINT|unsigned short int|  
|SQL_C_SLONG [j]|SQLINTEGER|long int|  
|SQL_C_ULONG [j]|SQLUINTEGER|unsigned long int|  
|SQL_C_FLOAT|SQLREAL|FLOAT|  
|SQL_C_DOUBLE|SQLDOUBLE, SQLFLOAT|double|  
|SQL_C_BIT|SQLCHAR|unsigned char|  
|SQL_C_STINYINT [j]|SQLSCHAR|signed char|  
|SQL_C_UTINYINT [j]|SQLCHAR|unsigned char|  
|SQL_C_SBIGINT|SQLBIGINT|_int64 [h]|  
|SQL_C_UBIGINT|SQLUBIGINT|unsigned _int64 [h]|  
|SQL_C_BINARY|SQLCHAR|unsigned char *|  
|SQL_C_BOOKMARK [i]|Marcador|unsigned Long int [d]|  
|SQL_C_VARBOOKMARK|SQLCHAR|unsigned char *|  
|Todos los tipos de datos de intervalo de C|SQL_INTERVAL_STRUCT|Vea la sección [estructura de intervalo de C](../../../odbc/reference/appendixes/c-interval-structure.md) , más adelante en este apéndice.|  
  
 **Identificador de tipo de C** SQL_C_TYPE_DATE [C]  
  
 **Definición** de tipo C de ODBC SQL_DATE_STRUCT  
  
 **Tipo C**  
  
```  
struct tagDATE_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;    
} DATE_STRUCT;[a]  
```  
  
 **Identificador de tipo de C** SQL_C_TYPE_TIME [C]  
  
 **Definición** de tipo C de ODBC SQL_TIME_STRUCT  
  
 **Tipo C**  
  
```  
struct tagTIME_STRUCT {  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
} TIME_STRUCT;[a]  
```  
  
 **Identificador de tipo de C** SQL_C_TYPE_TIMESTAMP [C]  
  
 **Definición** de tipo C de ODBC SQL_TIMESTAMP_STRUCT  
  
 **Tipo C**  
  
```  
struct tagTIMESTAMP_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;[b]   
} TIMESTAMP_STRUCT;[a]  
```  
  
 **Identificador de tipo de C** SQL_C_NUMERIC  
  
 **Definición** de tipo C de ODBC SQL_NUMERIC_STRUCT  
  
 **Tipo C**  
  
```  
struct tagSQL_NUMERIC_STRUCT {  
   SQLCHAR precision;  
   SQLSCHAR scale;  
   SQLCHAR sign[g];  
   SQLCHAR val[SQL_MAX_NUMERIC_LEN];[e], [f]   
} SQL_NUMERIC_STRUCT;  
```  
  
 **Identificador de tipo de C** SQL_C_GUID  
  
 **Definición** de tipo C de ODBC SQLGUID  
  
 **Tipo C**  
  
```  
struct tagSQLGUID {  
   DWORD Data1;  
   WORD Data2;  
   WORD Data3;  
   BYTE Data4[8];  
} SQLGUID;[k]  
```  
  
 [a] los valores de los campos año, mes, día, hora, minuto y segundo en los tipos de datos de DateTime C deben cumplir las restricciones del calendario gregoriano. (Vea [las restricciones del calendario gregoriano](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md) más adelante en este apéndice).  
  
 [b] el valor del campo de fracción es el número de milmillonésimas de un segundo y va de 0 a 999.999.999 (1 menor que 1 mil millones). Por ejemplo, el valor del campo de fracción para un medio segundo es 500 millones, para una milésima de segundo (un milisegundo) es 1 millón, para una millonésimas de segundo (un microsegundo) es 1.000 y para un millonésimas de un segundo (un nanosegundo) es 1.  
  
 [c] en ODBC 2. *x*, los tipos de datos de fecha, hora y marca de tiempo de C son SQL_C_DATE, SQL_C_TIME y SQL_C_TIMESTAMP.  
  
 [d] las aplicaciones ODBC 3 *. x* deben usar SQL_C_VARBOOKMARK, no SQL_C_BOOKMARK. Cuando una aplicación ODBC 3 *. x* funciona con ODBC 2. *x* , el administrador de controladores ODBC 3 *. x* asignará SQL_C_VARBOOKMARK a SQL_C_BOOKMARK.  
  
 [e] un número se almacena en el campo *Val* de la estructura SQL_NUMERIC_STRUCT como un entero con escala, en el modo Little Endian (el byte más a la izquierda es el byte menos significativo). Por ejemplo, el número 10,001 base 10, con una escala de 4, se escala a un entero de 100010. Dado que se 186AA en formato hexadecimal, el valor de SQL_NUMERIC_STRUCT sería "AA 86 01 00 00... 00 ", con el número de bytes definido por la **#define** SQL_MAX_NUMERIC_LEN.  
  
 Para obtener más información acerca de **SQL_NUMERIC_STRUCT**, vea [HOWTO: recuperar datos numéricos con SQL_NUMERIC_STRUCT](retrieve-numeric-data-sql-numeric-struct-kb222831.md).  
  
 [f] los campos de precisión y escala del SQL_C_NUMERIC tipo de datos areused para la entrada de una aplicación y para la salida del controlador a la aplicación. Cuando el controlador escribe un valor numérico en el SQL_NUMERIC_STRUCT, utilizará su propio valor predeterminado específico del controlador como valor para el campo de *precisión* y usará el valor del campo SQL_DESC_SCALE del descriptor de la aplicación (cuyo valor predeterminado es 0) para el campo *escala* . Una aplicación puede proporcionar sus propios valores para precisión y escala estableciendo los campos SQL_DESC_PRECISION y SQL_DESC_SCALE del descriptor de la aplicación.  
  
 [g] el campo de signo es 1 si es positivo; 0 si es negativo.  
  
 [h] es posible que algunos compiladores no proporcionen _int64.  
  
 [i] _SQL_C_BOOKMARK ha quedado en desuso en ODBC 3 *. x*.  
  
 [j] _SQL_C_SHORT, SQL_C_LONG y SQL_C_TINYINT se han reemplazado en ODBC por tipos con signo y sin signo: SQL_C_SSHORT y SQL_C_USHORT, SQL_C_SLONG y SQL_C_ULONG y SQL_C_STINYINT y SQL_C_UTINYINT. Un controlador ODBC 3 *. x* que debe funcionar con ODBC 2. las aplicaciones *x* deben admitir SQL_C_SHORT, SQL_C_LONG y SQL_C_TINYINT, porque cuando se les llama, el administrador de controladores los pasa al controlador.  
  
 [k] SQL_C_GUID solo se puede convertir en SQL_CHAR o SQL_WCHAR.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Estructuras de entero de 64 bits](../../../odbc/reference/appendixes/64-bit-integer-structures.md)  
  
## <a name="see-also"></a>Consulte también  
 [Tipos de datos C en ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md)
