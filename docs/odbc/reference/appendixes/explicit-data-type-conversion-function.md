---
description: Función de conversión de tipo de datos explícito
title: Función de conversión de tipo de datos explícita | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- explicit data type conversion functions [ODBC]
- data type conversion functions [ODBC]
- functions [ODBC], explicit data type conversion functions
ms.assetid: d5789450-b668-4753-96c8-6789e955e7ed
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 927c911b839e7aa07b087edb0fb3b457d0825b6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194819"
---
# <a name="explicit-data-type-conversion-function"></a>Función de conversión de tipo de datos explícito
La conversión explícita de tipos de datos se especifica en términos de definiciones de tipos de datos SQL.  
  
 La sintaxis de ODBC para la función de conversión de tipos de datos explícita no restringe las conversiones. La validez de las conversiones específicas de un tipo de datos a otro tipo de datos se determinará por cada implementación específica del controlador. El controlador, dado que traduce la sintaxis de ODBC en la sintaxis nativa, rechazan las conversiones que, aunque sean válidas en la sintaxis de ODBC, no son compatibles con el origen de datos. La función de ODBC **SQLGetInfo**, con las opciones de conversión (como SQL_CONVERT_BIGINT, SQL_CONVERT_BINARY, SQL_CONVERT_INTERVAL_YEAR_MONTH, etc.), proporciona una forma de consultar sobre las conversiones admitidas por el origen de datos.  
  
 El formato de la función **Convert** es:  
  
 **Convert (** _value_exp_, _data_type_**)**  
  
 La función devuelve el valor especificado por *value_exp* convertido en el *data_type* especificado, donde *data_type* es una de las palabras clave siguientes:  

:::row:::
    :::column:::
        SQL_BIGINT  
        SQL_BINARY  
        SQL_BIT  
        SQL_CHAR  
        SQL_DATE  
        SQL_DECIMAL  
        SQL_DOUBLE  
        SQL_FLOAT  
        SQL_GUID  
        SQL_INTEGER  
        SQL_INTERVAL_DAY  
        SQL_INTERVAL_DAY_TO_HOUR  
    :::column-end:::
    :::column:::
        SQL_INTERVAL_DAY_TO_MINUTE  
        SQL_INTERVAL_DAY_TO_SECOND  
        SQL_INTERVAL_HOUR  
        SQL_INTERVAL_HOUR_TO_MINUTE  
        SQL_INTERVAL_HOUR_TO_SECOND  
        SQL_INTERVAL_MINUTE  
        SQL_INTERVAL_MINUTE_TO_SECOND  
        SQL_INTERVAL_MONTH  
        SQL_INTERVAL_SECOND  
        SQL_INTERVAL_YEAR  
        SQL_INTERVAL_YEAR_TO_MONTH  
        SQL_LONGVARBINARY  
    :::column-end:::
    :::column:::
        SQL_LONGVARCHAR  
        SQL_NUMERIC  
        SQL_REAL  
        SQL_SMALLINT  
        SQL_TIME  
        SQL_TIMESTAMP  
        SQL_TINYINT  
        SQL_VARBINARY  
        SQL_VARCHAR  
        SQL_WCHAR  
        SQL_WLONGVARCHAR  
        SQL_WVARCHAR  
    :::column-end:::
:::row-end:::

 La sintaxis de ODBC para la función de conversión de tipos de datos explícitos no admite la especificación del formato de conversión. Si el origen de datos subyacente admite la especificación de formatos explícitos, un controlador debe especificar un valor predeterminado o implementar la especificación de formato.  
  
 El argumento *value_exp* puede ser un nombre de columna, el resultado de otra función escalar o un literal numérico o de cadena. Por ejemplo:  
  
```  
{ fn CONVERT( { fn CURDATE() }, SQL_CHAR ) }  
```  
  
 convierte el resultado de la función escalar CURDATE en una cadena de caracteres.  
  
 Dado que ODBC no impone un tipo de datos para los valores devueltos de las funciones escalares (dado que las funciones son a menudo específicas del origen de datos), las aplicaciones deben usar la función escalar CONVERT siempre que sea posible para forzar la conversión de tipos de datos.  
  
 En los dos ejemplos siguientes se muestra el uso de la función **Convert** . En estos ejemplos se supone la existencia de una tabla denominada EMPLOYEes, con una columna EMPNO de tipo SQL_SMALLINT y una columna EMPNAME de tipo SQL_CHAR.  
  
 Si una aplicación especifica la siguiente instrucción SQL:  
  
```  
SELECT EMPNO FROM EMPLOYEES WHERE {fn CONVERT(EMPNO,SQL_CHAR)} LIKE '1%'  
```  
  
-   Un controlador para ORACLE traduce la instrucción SQL a:  
  
    ```  
    SELECT EMPNO FROM EMPLOYEES WHERE to_char(EMPNO) LIKE '1%'  
    ```  
  
-   Un controlador para SQL Server traduce la instrucción SQL a:  
  
    ```  
    SELECT EMPNO FROM EMPLOYEES WHERE convert(char,EMPNO) LIKE '1%'  
    ```  
  
 Si una aplicación especifica la siguiente instrucción SQL:  
  
```  
SELECT {fn ABS(EMPNO)}, {fn CONVERT(EMPNAME,SQL_SMALLINT)}  
   FROM EMPLOYEES WHERE EMPNO <> 0  
```  
  
-   Un controlador para ORACLE traduce la instrucción SQL a:  
  
    ```  
    SELECT abs(EMPNO), to_number(EMPNAME) FROM EMPLOYEES WHERE EMPNO <> 0  
    ```  
  
-   Un controlador para SQL Server traduce la instrucción SQL a:  
  
    ```  
    SELECT abs(EMPNO), convert(smallint, EMPNAME) FROM EMPLOYEES  
       WHERE EMPNO <> 0  
    ```  
  
-   Un controlador para Ingres traduce la instrucción SQL a:  
  
    ```  
    SELECT abs(EMPNO), int2(EMPNAME) FROM EMPLOYEES WHERE EMPNO <> 0  
    ```
