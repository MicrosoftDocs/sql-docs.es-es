---
description: Literales de intervalo
title: Literales de intervalo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- interval literals [ODBC]
- interval data type [ODBC], literals
ms.assetid: f9e6c3c7-4f98-483f-89d8-ebc5680f021b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1e100fcc54f9ca6cc165eece7637b7fae45f65b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184418"
---
# <a name="interval-literals"></a>Literales de intervalo
ODBC requiere que todos los controladores admitan la conversión del tipo de datos SQL_CHAR o SQL_VARCHAR en todos los tipos de datos de intervalo de C. Sin embargo, si el origen de datos subyacente no admite tipos de datos de intervalo, el controlador debe conocer el formato correcto del valor en el campo SQL_CHAR para admitir estas conversiones. Del mismo modo, ODBC requiere que cualquier tipo C de ODBC sea convertible en SQL_CHAR o SQL_VARCHAR, por lo que un controlador debe saber qué formato debe tener un intervalo almacenado en el campo de carácter. En esta sección se describe la sintaxis de los literales de intervalo, que el escritor de controladores debe utilizar para validar los campos de SQL_CHAR durante la conversión a o desde los tipos de datos de intervalo de C.  
  
> [!NOTE]  
>  La sintaxis BNF completa para los literales de intervalo se muestra en la sección sintaxis de los literales de [intervalo](../../../odbc/reference/appendixes/interval-literal-syntax.md) en el Apéndice C: gramática de SQL.  
  
 Para pasar los literales de intervalo como parte de una instrucción SQL, se define una sintaxis de cláusula de escape para los literales de intervalo. Para obtener más información, vea [literales de fecha, hora y marca de tiempo](../../../odbc/reference/develop-app/date-time-and-timestamp-literals.md).  
  
 Un literal de intervalo tiene el formato:  
  
```  
INTERVAL[<sign>] 'value' <interval qualifier>  
```  
  
 donde "INTERVAL" indica que el literal de carácter es un intervalo. El signo puede ser más o menos; está fuera de la cadena de intervalo y es opcional.  
  
 El calificador de intervalo puede ser un único campo de fecha y hora o estar compuesto por dos campos de fecha y hora, en el formato: \<*leading field*> para \<*trailing field*> .  
  
-   Cuando el intervalo se compone de un solo campo, el campo único puede ser un campo que no es de segundo y que puede ir acompañado de una precisión inicial opcional entre paréntesis. El campo DateTime único también puede ser un segundo campo que puede ir acompañado de la precisión inicial opcional, la precisión de fracciones de segundo opcional entre paréntesis o ambos. Si tanto la precisión inicial como la precisión de las fracciones de segundo están presentes en un campo de segundos, se separan mediante comas. Si el campo segundos tiene una precisión de fracciones de segundo, también debe tener una precisión inicial.  
  
-   Cuando el intervalo se compone de campos iniciales y finales, el campo inicial es un campo no de segundo que puede ir acompañado de la precisión del campo inicial del intervalo entre paréntesis. El campo final puede ser un campo que no sea de segundo o un segundo campo que puede ir acompañado de un intervalo de precisión de fracciones de segundos entre paréntesis.  
  
 La cadena de intervalo en el *valor* se incluye entre comillas simples. Puede ser un literal de año o un literal de fecha y hora. El formato de la cadena en el *valor* se determina mediante las siguientes reglas:  
  
-   La cadena contiene un valor decimal para cada campo que está implícito en \<*interval* *qualifier*> .  
  
-   Si la precisión del intervalo incluye los campos año y mes, los valores de estos campos se separan con un signo menos.  
  
-   Si la precisión del intervalo incluye los campos día y hora, los valores de estos campos están separados por un espacio.  
  
-   Si la precisión del intervalo incluye el campo hora y los campos de orden inferior (minuto y segundo), los valores de estos campos se separan mediante dos puntos.  
  
-   Si la precisión del intervalo incluye un segundo campo y la precisión de segundos expresada o implícita es distinto de cero, el carácter situado inmediatamente antes del primer dígito de la parte fraccionaria del segundo es un punto.  
  
-   Ningún campo puede tener más de dos dígitos de longitud, excepto:  
  
    -   El valor del campo inicial puede ser tan largo como la precisión inicial del intervalo expresado o implícito.  
  
    -   La parte fraccionaria del segundo campo puede ser tan larga como la precisión de segundos expresada o implícita.  
  
    -   Los campos finales siguen las restricciones habituales del calendario gregoriano. (Vea [restricciones del calendario gregoriano](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md)).  
  
 En la tabla siguiente se muestran ejemplos de literales de intervalo válidos, tal como se incluye en la cláusula escape de ODBC para los intervalos. La sintaxis de la cláusula escape es la siguiente:  
  
> [!NOTE]  
>  *{Intervalo de signo de intervalo-intervalo de cadenas-calificador}*  
  
|Cláusula escape literal|Intervalo especificado|  
|---------------------------|------------------------|  
|{INTERVAL ' 326 ' YEAR (4)}|Especifica un intervalo de 326 años. La precisión inicial del intervalo es 4.|  
|{INTERVAL ' 326 ' MONTH (3)}|Especifica un intervalo de 326 meses. La precisión inicial del intervalo es 3.|  
|{INTERVAL ' 3261 ' DAY (4)}|Especifica un intervalo de 3261 días. La precisión inicial del intervalo es 4.|  
|{INTERVALO ' 163 ' HORA (3)}|Especifica un intervalo de 163 días. La precisión inicial del intervalo es 3.|  
|{INTERVALO ' 163 ' MINUTO (3)}|Especifica un intervalo de 163 minutos. La precisión inicial del intervalo es 3.|  
|{INTERVAL ' 223,16 ' SECOND (3, 2)}|Especifica un intervalo de 223,16 segundos. La precisión inicial del intervalo es 3 y la precisión de los segundos es 2.|  
|{INTERVAL ' 163-11 ' YEAR (3) AL MES}|Especifica un intervalo de 163 años y 11 meses. La precisión inicial del intervalo es 3.|  
|{INTERVAL ' 163 12 ' DAY (3) A HOUR}|Especifica un intervalo de 163 días y 12 horas. La precisión inicial del intervalo es 3.|  
|{INTERVAL ' 163 12:39 ' DAY (3) A MINUTE}|Especifica un intervalo de 163 días, 12 horas y 39 minutos. La precisión inicial del intervalo es 3.|  
|{INTERVAL ' 163 12:39:59.163 ' DAY (3) A SECOND (3)}|Especifica un intervalo de 163 días, 12 horas, 39 minutos y 59,163 segundos. La precisión inicial del intervalo es 3 y la precisión de los segundos es 3.|  
|{INTERVALO ' 163:39 ' HORA (3) A MINUTO}|Especifica un intervalo de 163 horas y 39 minutos. La precisión inicial del intervalo es 3.|  
|{INTERVAL ' 163:39:59.163 ' HOUR (3) TO SECOND (4)}|Especifica un intervalo de 163 horas, 39 minutos y 59,163 segundos. La precisión inicial del intervalo es 3 y la precisión de los segundos es 4.|  
|{INTERVAL ' 163:59.163 ' MINUTE (3) TO SECOND (5)}|Especifica un intervalo de 163 minutos y 59,163 segundos. La precisión inicial del intervalo es 3 y la precisión de los segundos es 5.|  
|{INTERVAL-' 16 23:39:56.23 ' DAY TO SECOND}|Especifica un intervalo de menos de 16 días, 23 horas, 39 minutos y 56,23 segundos. La precisión inicial implícita es 2 y la precisión de segundos implícita es 6.|  
  
 En la tabla siguiente se muestran ejemplos de literales de intervalo no válidos:  
  
|Cláusula escape literal|Motivo por el que no es válido|  
|---------------------------|------------------------|  
|{INTERVALO ' 163 ' HORA (2)}|La precisión inicial del intervalo es 2, pero el valor del campo inicial es 163.|  
|{INTERVALO ' 223,16 ' SEGUNDO (2, 2)}<br /><br /> {INTERVAL ' 223,16 ' SECOND (3, 1)}|En el primer ejemplo, la precisión inicial es demasiado pequeña y, en el segundo ejemplo, la precisión de los segundos es demasiado pequeña.|  
|{INTERVALO ' 223,16 ' SEGUNDO}<br /><br /> {INTERVALO ' 223 ' AÑO}|Dado que no se especifica la precisión inicial, el valor predeterminado es 2, que es demasiado pequeño para contener el literal especificado.|  
|{INTERVALO ' 22,1234567 ' SEGUNDO}|La precisión de los segundos no se especifica, por lo que el valor predeterminado es 6. El literal tiene siete dígitos después del separador decimal.|  
|{INTERVAL ' 163-13 ' YEAR (3) AL MES}<br /><br /> {INTERVAL ' 163 65 ' DAY (3) A HOUR}<br /><br /> {INTERVAL ' 163 62:39 ' DAY (3) A MINUTE}<br /><br /> {INTERVAL ' 163 12:125:59.163 ' DAY (3) A SECOND (3)}<br /><br /> {INTERVALO ' 163:144 ' HORA (3) A MINUTO}<br /><br /> {INTERVAL ' 163:567:234.163 ' HOUR (3) TO SECOND (4)}<br /><br /> {INTERVAL ' 163:591.163 ' MINUTE (3) TO SECOND (5)}|El campo final no sigue las reglas del calendario gregoriano.|
