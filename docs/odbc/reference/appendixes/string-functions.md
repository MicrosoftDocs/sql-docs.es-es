---
description: Funciones de cadena
title: Funciones de cadena | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], string functions
- string functions [ODBC]
ms.assetid: 270f669e-8aab-4db0-95a4-f2b3c69538b3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fe6c50c7ad96f1025b27f60e2de74ba31f60698f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202519"
---
# <a name="string-functions"></a>Funciones de cadena
En la tabla siguiente se enumeran las funciones de manipulación de cadenas. Una aplicación puede determinar las funciones de cadena admitidas por un controlador mediante una llamada a **SQLGetInfo** con un *tipo de información* de SQL_STRING_FUNCTIONS.  
  
## <a name="remarks"></a>Observaciones  
 Los argumentos indicados como *string_exp* pueden ser el nombre de una columna, un *literal de cadena de caracteres* o el resultado de otra función escalar, donde el tipo de datos subyacente se puede representar como SQL_CHAR, SQL_VARCHAR o SQL_LONGVARCHAR.  
  
 Los argumentos indicados como *character_exp* son una cadena de caracteres de longitud variable.  
  
 Los argumentos que se indican como *Inicio*, *longitud* o *recuento* pueden ser un *literal numérico* o el resultado de otra función escalar, donde el tipo de datos subyacente se puede representar como SQL_TINYINT, SQL_SMALLINT o SQL_INTEGER.  
  
 Las funciones de cadena que se enumeran aquí están basadas en 1; es decir, el primer carácter de la cadena es el carácter 1.  
  
 Las funciones escalares de cadena BIT_LENGTH, CHAR_LENGTH, CHARACTER_LENGTH, OCTET_LENGTH y POSITION se han agregado en ODBC 3,0 para alinearse con SQL-92.  
  
|Función|Descripción|  
|--------------|-----------------|  
|**ASCII (** _string_exp_ **)**  (ODBC 1,0)|Devuelve el valor de código ASCII del carácter situado más a la izquierda de *string_exp* como un entero.|  
|**BIT_LENGTH (** _string_exp_ **)**  (ODBC 3,0)|Devuelve la longitud en bits de la expresión de cadena.<br /><br /> No funciona solo para tipos de datos de cadena, por lo que no convertirá implícitamente *string_exp* en cadena sino que devolverá el tamaño (interno) de cualquier tipo de datos que se proporcione.|  
|**Char (** _código_ **)**  (ODBC 1,0)|Devuelve el carácter que tiene el valor de código ASCII especificado por el *código*. El valor del *código* debe estar entre 0 y 255; de lo contrario, el valor devuelto depende del origen de datos.|  
|**CHAR_LENGTH (** _string_exp_ **)**  (ODBC 3,0)|Devuelve la longitud en caracteres de la expresión de cadena, si la expresión de cadena es de un tipo de datos de caracteres; de lo contrario, devuelve la longitud en bytes de la expresión de cadena (el entero más pequeño no es menor que el número de bits dividido entre 8). (Esta función es igual que la función CHARACTER_LENGTH).|  
|**CHARACTER_LENGTH (** _string_exp_ **)**  (ODBC 3,0)|Devuelve la longitud en caracteres de la expresión de cadena, si la expresión de cadena es de un tipo de datos de caracteres; de lo contrario, devuelve la longitud en bytes de la expresión de cadena (el entero más pequeño no es menor que el número de bits dividido entre 8). (Esta función es igual que la función CHAR_LENGTH).|  
|**Concat (** _string_exp1_,_string_exp2_**)**  (ODBC 1,0)|Devuelve una cadena de caracteres que es el resultado de concatenar *string_exp2* a *string_exp1*. La cadena resultante es dependiente de DBMS. Por ejemplo, si la columna representada por *string_exp1* contenía un valor null, DB2 devolverá null, pero SQL Server devolverá la cadena que no sea NULL.|  
|**Difference (** _string_exp1_,_string_exp2_**)**  (ODBC 2,0)|Devuelve un valor entero que indica la diferencia entre los valores devueltos por la función SOUNDEX para *string_exp1* y *string_exp2*.|  
|**Insert (** _string_exp1_, *Inicio*, *longitud* _string_exp2_**)**  (ODBC 1,0)|Devuelve una cadena de caracteres en la que los caracteres de *longitud* se han eliminado de *string_exp1*, empezando por el *Inicio* y donde se ha insertado *string_exp2* en *string_exp,* comenzando en el *Inicio*.|  
|**LCASE (** _string_exp_ **)** (ODBC 1,0)|Devuelve una cadena igual a la de *string_exp*, con todos los caracteres en mayúsculas convertidos a minúsculas.|  
|**Left (** _string_exp_, _Count_**)** (ODBC 1,0)|Devuelve los caracteres de *recuento* más a la izquierda de *string_exp*.|  
|**Longitud (** _string_exp_ **)** (ODBC 1,0)|Devuelve el número de caracteres de *string_exp,* excluyendo los espacios en blanco finales.<br /><br /> La **longitud** solo acepta cadenas. Por consiguiente, convertirá implícitamente *string_exp* en una cadena y devolverá la longitud de esta cadena (no el tamaño interno del tipo de valor).|  
|**Buscar (** _string_exp1_, *string_exp2*[, *Start*]**)** (ODBC 1,0)|Devuelve la posición inicial de la primera aparición de *string_exp1* dentro de *string_exp2*. La búsqueda de la primera aparición de *string_exp1* comienza con la posición del primer carácter en *string_exp2* a menos que se especifique el argumento opcional *Start*. Si se especifica *Start* , la búsqueda comienza con la posición de carácter indicada por el valor de *Start*. La primera posición de carácter de *string_exp2* se indica mediante el valor 1. Si no se encuentra *string_exp1* en *string_exp2*, se devuelve el valor 0.<br /><br /> Si una aplicación puede llamar a la función escalar LOCALIZAda con los argumentos *string_exp1*, *string_exp2* y *start* , el controlador devuelve SQL_FN_STR_LOCATE cuando se llama a **SQLGetInfo** con una *opción* de SQL_STRING_FUNCTIONS. Si la aplicación puede llamar a la función escalar LOCATE solo con los argumentos *string_exp1* y *string_exp2* , el controlador devuelve SQL_FN_STR_LOCATE_2 cuando se llama a **SQLGetInfo** con una *opción* de SQL_STRING_FUNCTIONS. Los controladores que admiten la llamada a la función de búsqueda con dos o tres argumentos devuelven SQL_FN_STR_LOCATE y SQL_FN_STR_LOCATE_2.|  
|**LTrim (** _string_exp_ **)** (ODBC 1,0)|Devuelve los caracteres de *string_exp*, con los espacios en blanco iniciales que se han quitado.|  
|**OCTET_LENGTH (** _string_exp_ **)** (ODBC 3,0)|Devuelve la longitud en bytes de la expresión de cadena. El resultado es el entero más pequeño que no es inferior al número de bits dividido por 8.<br /><br /> No funciona solo para tipos de datos de cadena, por lo que no convertirá implícitamente *string_exp* en cadena sino que devolverá el tamaño (interno) de cualquier tipo de datos que se proporcione.|  
|**Posición (** _character_exp_ **en** _character_exp_**)** (ODBC 3,0)|Devuelve la posición de la primera expresión de carácter en la segunda expresión de caracteres. El resultado es un valor numérico exacto con una precisión definida por la implementación y una escala de 0.|  
|**REPEAT (** _string_exp,_ _Count_**)** (ODBC 1,0)|Devuelve una cadena de caracteres formada por *string_exp* tiempos de *recuento* repetidos.|  
|**Replace (** _string_exp1_, *string_exp2*, _string_exp3_**)** (ODBC 1,0)|Busque *string_exp1* foroccurrences de *string_exp2* y reemplace por *string_exp3*.|  
|**Right (** _string_exp_, _Count_**)** (ODBC 1,0)|Devuelve los caracteres de *recuento* situados más a la derecha de *string_exp*.|  
|**Rtrim (** _string_exp_ **)** (ODBC 1,0)|Devuelve los caracteres de *string_exp* con los espacios en blanco finales quitados.|  
|**Soundex (** _string_exp_ **)** (ODBC 2,0)|Devuelve una cadena de caracteres dependiente del origen de datos que representa el sonido de las palabras de *string_exp*. Por ejemplo, SQL Server devuelve un código SOUNDEX de 4 dígitos; Oracle devuelve una representación fonética de cada palabra.|  
|**Espacio (** _recuento_ **)** (ODBC 2,0)|Devuelve una cadena de caracteres que consta de espacios de *recuento* .|  
|**Substring (** _string_exp_, *Inicio*, longitud **)** (ODBC 1,0)|Devuelve una cadena de caracteres que se deriva de *string_exp*, comenzando en la posición de caracteres especificada por *Start* para los caracteres de *longitud* .|  
|**UCASE (** _string_exp_ **)** (ODBC 1,0)|Devuelve una cadena igual a la de *string_exp*, con todos los caracteres en minúsculas convertidos en mayúsculas.|
