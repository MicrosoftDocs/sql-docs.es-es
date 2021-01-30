---
description: Tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación-ODBC
title: Tamaño de columna, dígitos decimales, longitud de octetos de transferencia, tamaño de presentación | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- display size of data types [ODBC]
- data types [ODBC], column size
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- data types [ODBC], display size
- decimal digits of data types [ODBC]
- data types [ODBC], decimal digits
- SQL data types [ODBC], column characteristics
- column size of data types [ODBC]
- data types [ODBC], transfer octet length
ms.assetid: 723107a1-be08-4ea3-a8c0-b2c45d38d1aa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a23183fd60d947771cb041af977d518157ac573
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182535"
---
# <a name="column-size-decimal-digits-transfer-octet-length-and-display-size---odbc"></a>Tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación-ODBC
Los tipos de datos se caracterizan por su tamaño de columna (o parámetro), dígitos decimales, longitud y tamaño de presentación. Las siguientes funciones ODBC devuelven estos atributos para un parámetro en una instrucción SQL o para un tipo de datos SQL en un origen de datos. Cada función ODBC devuelve un conjunto diferente de estos atributos, como se indica a continuación:  
  
-   **SQLDescribeCol** devuelve el tamaño de columna y los dígitos decimales de las columnas que describe.  
  
-   **SQLDescribeParam** devuelve el tamaño del parámetro y los dígitos decimales de los parámetros que describe. **SQLBindParameter** establece el tamaño del parámetro y los dígitos decimales para un parámetro en una instrucción SQL.  
  
-   Las funciones de catálogo **SQLColumns**, **SQLProcedureColumns** y **SQLGetTypeInfo** devuelven los atributos de una columna de una tabla, un conjunto de resultados o un parámetro de procedimiento y los atributos de catálogo de los tipos de datos del origen de datos. **SQLColumns** devuelve el tamaño de la columna, los dígitos decimales y la longitud de una columna en las tablas especificadas (como la tabla base, la vista o una tabla del sistema). **SQLProcedureColumns** devuelve el tamaño de la columna, los dígitos decimales y la longitud de una columna en un procedimiento. **SQLGetTypeInfo** devuelve el tamaño máximo de la columna y los dígitos decimales mínimos y máximos de un tipo de datos SQL en un origen de datos.  
  
 Los valores devueltos por estas funciones para el tamaño de la columna o del parámetro corresponden a "precisión", tal y como se define en ODBC 2. *x*. Sin embargo, los valores no se corresponden necesariamente con los valores devueltos en SQL_DESC_PRECISION o en cualquier otro campo de descriptor. Lo mismo se aplica a los dígitos decimales, que corresponden a "escala" tal y como se define en ODBC 2. *x*. No se corresponde necesariamente con los valores devueltos en SQL_DESC_SCALE o en cualquier otro campo de descriptor, pero procede de campos de descriptor diferentes en función del tipo de datos. Para obtener más información, vea [tamaño de columna](../../../odbc/reference/appendixes/column-size.md) y [dígitos decimales](../../../odbc/reference/appendixes/decimal-digits.md).  
  
 Del mismo modo, los valores de longitud de octetos de transferencia no provienen de SQL_DESC_LENGTH. Proceden del SQL_DESC_OCTET_LENGTH de un campo de un descriptor para todos los tipos de caracteres y binarios. No hay ningún campo de descriptor que contenga esta información para otros tipos.  
  
 El valor de tamaño de presentación de todos los tipos de datos se corresponde con el valor de un solo campo de descriptor, SQL_DESC_DISPLAY_SIZE.  
  
 Los campos de descriptor describen las características de un conjunto de resultados. Los campos de descriptor no contienen valores válidos sobre los datos antes de la ejecución de la instrucción. Los valores de tamaño de columna, dígitos decimales y tamaño de presentación devueltos por **SQLColumns**, **SQLProcedureColumns** y **SQLGetTypeInfo**, por otro lado, devuelven características de los objetos de base de datos, como las columnas de tabla y los tipos de datos, que existen en el catálogo del origen de datos. Del mismo modo, en su conjunto de resultados, **SQLColAttribute** devuelve el tamaño de la columna, los dígitos decimales y la longitud del octeto de transferencia de las columnas en el origen de datos; Estos valores no son necesariamente los mismos que los valores de los campos de descriptor SQL_DESC_PRECISION, SQL_DESC_SCALE y SQL_DESC_OCTET_LENGTH.  
  
 Para obtener más información sobre estos campos de descriptor, vea [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 Temas relacionados:  
  
-   [Tamaño de columna](../../../odbc/reference/appendixes/column-size.md)  
-   [Dígitos decimales](../../../odbc/reference/appendixes/decimal-digits.md)  
-   [Transferir la longitud en octetos](../../../odbc/reference/appendixes/transfer-octet-length.md)  
-   [Tamaño de presentación](../../../odbc/reference/appendixes/display-size.md)
