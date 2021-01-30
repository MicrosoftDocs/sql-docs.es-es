---
description: Funciones numéricas
title: Funciones numéricas | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], numeric functions
- numeric functions [ODBC]
ms.assetid: 4fa548dc-e8b0-4179-92ff-81d6a79d10c3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bef4a075eefffd0f9ec4b918189d40ae71ea78ec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207130"
---
# <a name="numeric-functions"></a>Funciones numéricas
En la tabla siguiente se describen las funciones numéricas que se incluyen en el conjunto de funciones escalares de ODBC. Al llamar a **SQLGetInfo** con un *tipo de información* de SQL_NUMERIC_FUNCTIONS, una aplicación puede determinar qué funciones numéricas son compatibles con un controlador.  
  
 Todas las funciones numéricas devuelven valores de tipo de datos SQL_FLOAT excepto ABS, ROUND, TRUNCAte, SIGN, FLOOR y CEILING, que devuelven valores del mismo tipo de datos que los parámetros de entrada.  
  
 Los argumentos indicados como *numeric_exp* pueden ser el nombre de una columna, el resultado de otra función escalar o un *valor numérico* l, donde el tipo de datos subyacente podría representarse como SQL_NUMERIC, SQL_DECIMAL, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_FLOAT, SQL_REAL o SQL_DOUBLE.  
  
 Los argumentos indicados como *float_exp* pueden ser el nombre de una columna, el resultado de otra función escalar o un *literal numérico*, donde el tipo de datos subyacente se puede representar como SQL_FLOAT.  
  
 Los argumentos indicados como *integer_exp* pueden ser el nombre de una columna, el resultado de otra función escalar o un *literal numérico*, donde el tipo de datos subyacente se puede representar como SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER o SQL_BIGINT.  
  
 Las funciones escalares CURRENT_DATE, CURRENT_TIME y CURRENT_TIMESTAMP se han agregado en ODBC 3,0 para que se alineen con SQL-92.  
  
|Función|Descripción|  
|--------------|-----------------|  
|**ABS (** _numeric_exp_ **)**  (ODBC 1,0)|Devuelve el valor absoluto de *numeric_exp*.|  
|**ACOS (** _float_exp_ **)**  (ODBC 1,0)|Devuelve el arcocoseno de *float_exp* como ángulo, expresado en radianes.|  
|**Asin (** _float_exp_ **)**  (ODBC 1,0)|Devuelve el arcoseno de *float_exp* como ángulo, expresado en radianes.|  
|**Atan (** _float_exp_ **)**  (ODBC 1,0)|Devuelve el arco tangente de *float_exp* como un ángulo, expresado en radianes.|  
|**ATAN2 (** _float_exp1_, _float_exp2_**)**  (ODBC 2,0)|Devuelve el arco tangente de las coordenadas *x* *e y,* especificado por *float_exp1* y *float_exp2*, respectivamente, como un ángulo, expresado en radianes.|  
|**Ceiling (** _numeric_exp_ **)**  (ODBC 1,0)|Devuelve el número entero más pequeño mayor o igual que *numeric_exp*. El valor devuelto es del mismo tipo de datos que el parámetro de entrada.|  
|**Cos (** _float_exp_ **)**  (ODBC 1,0)|Devuelve el coseno de *float_exp*, donde *float_exp* es un ángulo expresado en radianes.|  
|**Cot (** _float_exp_ **)**  (ODBC 1,0)|Devuelve la cotangente de *float_exp*, donde *float_exp* es un ángulo expresado en radianes.|  
|**Grados (** _numeric_exp_ **)**  (ODBC 2,0)|Devuelve el número de grados convertido desde *numeric_exp* radianes.|  
|**Exp (** _float_exp_ **)**  (ODBC 1,0)|Devuelve el valor exponencial de *float_exp*.|  
|**Floor (** _numeric_exp_ **)**  (ODBC 1,0)|Devuelve el entero más grande menor o igual que *numeric_exp*. El valor devuelto es del mismo tipo de datos que el parámetro de entrada.|  
|**Registro (** _float_exp_ **)**  (ODBC 1,0)|Devuelve el logaritmo natural de *float_exp*.|  
|**Log10 (** _float_exp_ **)**  (ODBC 2,0)|Devuelve el logaritmo en base 10 de *float_exp*.|  
|**Mod (** _integer_exp1_, _integer_exp2_**)**  (ODBC 1,0)|Devuelve el resto (módulo) de *integer_exp1* dividido por *integer_exp2*.|  
|**PI ()**  (ODBC 1,0)|Devuelve el valor constante de PI como un valor de punto flotante.|  
|**Power (** _numeric_exp_, _integer_exp_**)**  (ODBC 2,0)|Devuelve el valor de *numeric_exp* a la potencia de *integer_exp*.|  
|**Radianes (** _numeric_exp_ **)**  (ODBC 2,0)|Devuelve el número de radianes convertidos de *numeric_exp* grados.|  
|**Rand (**[*integer_exp*]**)**  (ODBC 1,0)|Devuelve un valor de punto flotante aleatorio utilizando *integer_exp* como valor de inicialización opcional.|  
|**Round (** _numeric_exp_, _integer_exp_**)**  (ODBC 2,0)|Devuelve *numeric_exp* redondeado a *integer_exp* posiciones a la derecha del separador decimal. Si *integer_exp* es negativo, *numeric_exp* se redondea a &#124;*integer_exp*&#124; lugares a la izquierda del separador decimal.|  
|**Sign (** _numeric_exp_ **)**  (ODBC 1,0)|Devuelve un indicador del signo de *numeric_exp*. Si *numeric_exp* es menor que cero, se devuelve-1. Si *numeric_exp* es igual a cero, se devuelve 0. Si *numeric_exp* es mayor que cero, se devuelve 1.|  
|**Sin (** _float_exp_ **)**  (ODBC 1,0)|Devuelve el seno de *float_exp*, donde *float_exp* es un ángulo expresado en radianes.|  
|**Sqrt (** _float_exp_ **)**  (ODBC 1,0)|Devuelve la raíz cuadrada de *float_exp*.|  
|**Tan (** _float_exp_ **)**  (ODBC 1,0)|Devuelve la tangente de *float_exp*, donde *float_exp* es un ángulo expresado en radianes.|  
|**TRUNCATE (** _numeric_exp_, _integer_exp_**)**  (ODBC 2,0)|Devuelve *numeric_exp* truncado en *integer_exp* posiciones a la derecha del separador decimal. Si *integer_exp* es negativo, *numeric_exp* se trunca a &#124;*integer_exp*&#124; lugares a la izquierda del separador decimal.|
