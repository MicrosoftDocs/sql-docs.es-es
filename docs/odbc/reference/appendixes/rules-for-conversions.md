---
description: Reglas para conversiones
title: Reglas para las conversiones | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- numeric data type [ODBC], literals
- conversions with numeric literals [ODBC]
- numeric literals [ODBC]
- literals [ODBC], numeric
ms.assetid: 89f846a3-001d-496a-9843-ac9c38dc1762
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b015a60e376e47a4c3c931d4739a884db23fc98b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187135"
---
# <a name="rules-for-conversions"></a>Reglas para conversiones
Las reglas de esta sección se aplican a las conversiones que implican literales numéricos. Para los fines de estas reglas, se definen los siguientes términos:  
  
-   *Asignación de almacenamiento:* Al enviar datos a una columna de tabla en una base de datos. Esto sucede durante las llamadas a **SQLExecute**, **SQLExecDirect** y **SQLSetPos**. Durante la asignación del almacén, "destino" hace referencia a una columna de base de datos y "origen" hace referencia a los datos de los búferes de la aplicación.  
  
-   *Asignación de recuperación:* Al recuperar datos de la base de datos en búferes de la aplicación. Esto sucede durante las llamadas a **SQLFetch**, **SQLGetData**, **SQLFetchScroll** y **SQLSetPos**. Durante la asignación de recuperación, "destino" hace referencia a los búferes de la aplicación y "origen" hace referencia a la columna de la base de datos.  
  
-   *CS:* Valor del origen del carácter.  
  
-   *NT:* Valor del destino numérico.  
  
-   *NS:* Valor del origen numérico.  
  
-   *CT:* Valor del destino del carácter.  
  
-   Precisión de un literal numérico exacto: el número de dígitos que contiene.  
  
-   La escala de un literal numérico exacto: el número de dígitos a la derecha del período expresado o implícito.  
  
-   La precisión de un literal numérico aproximado: la precisión de su mantisa.  
  
## <a name="character-source-to-numeric-target"></a>Origen de caracteres al destino numérico  
 A continuación se muestran las reglas para convertir de un origen de caracteres (CS) a un destino numérico (NT):  
  
1.  Reemplace CS por el valor obtenido quitando los espacios iniciales o finales en CS. Si CS no es un *literal numérico* válido, se devuelve SQLSTATE 22018 (valor de carácter no válido para la especificación de conversión).  
  
2.  Reemplace CS por el valor obtenido quitando los ceros a la izquierda antes del separador decimal, los ceros a la derecha después del separador decimal o ambos.  
  
3.  Convierta CS a NT. Si la conversión produce una pérdida de dígitos significativos, se devuelve SQLSTATE 22003 (valor numérico fuera del intervalo). Si la conversión produce la pérdida de dígitos no significativos, se devuelve SQLSTATE 01S07 (truncamiento fraccionario).  
  
## <a name="numeric-source-to-character-target"></a>Origen numérico para el destino de carácter  
 A continuación se muestran las reglas para convertir de un origen numérico (NS) a un destino de carácter (CT):  
  
1.  Deje que LT sea la longitud en caracteres de CT. Para la asignación de recuperación, LT es igual a la longitud del búfer en caracteres menos el número de bytes en el carácter de terminación null para este juego de caracteres.  
  
2.  Veces  
  
    -   Si NS es un tipo numérico exacto, deje que es sea igual a la cadena de caracteres más corta que se ajusta a la definición de *literal numérico exacto* de modo que la escala de es sea la misma que la escala de NS, y el valor interpretado de es es el valor absoluto de NS.  
  
    -   Si NS es un tipo numérico aproximado, deje que es sea una cadena de caracteres como se indica a continuación:  
  
         Caso:  
  
         Si NS es igual a 0, es es 0.  
  
         Permita que YSN sea la cadena de caracteres más corta que se ajusta a la definición de Exact-*Numeric-literal* y cuyo valor interpretado es el valor absoluto de NS. Si la longitud de YSN es menor que (*precisión* + 1) del tipo de datos NS, deje que es sea igual a YSN.  
  
         De lo contrario, es es la cadena de caracteres más corta que se ajusta a la definición del *literal numérico aproximado* cuyo valor interpretado es el valor absoluto de NS y cuya *mantisa* se compone de un único *dígito* que no es ' 0 ', seguido de un *punto* y un *entero sin signo*.  
  
3.  Caso:  
  
    -   Si NS es menor que 0, deje Y como resultado de:  
  
         '-'  &#124;&#124; ES  
  
         donde ' &#124;&#124; ' es el operador de concatenación de cadenas.  
  
         En caso contrario, deje que Y sea igual a es.  
  
4.  Deje que sea la longitud en caracteres de Y.  
  
5.  Caso:  
  
    -   Si LY es igual a LT, CT se establece en Y.  
  
    -   Si LY es menor que LT, CT se establece en Y Extended a la derecha el número de espacios adecuado.  
  
         En caso contrario (LY > LT), copie los primeros caracteres LT of Y en CT.  
  
         Caso:  
  
         Si se trata de una asignación de almacén, devuelva el error SQLSTATE 22001 (datos de cadena, truncados a la derecha).  
  
         Si se trata de una asignación de recuperación, devuelva la advertencia SQLSTATE 01004 (datos de cadena, truncados a la derecha). Cuando la copia da como resultado la pérdida de dígitos fraccionarios (que no sean ceros finales), está definido por el controlador si se produce uno de los siguientes casos:  
  
         (1) el controlador trunca la cadena en Y a una escala adecuada (que también puede ser cero) y escribe el resultado en CT.  
  
         (2) el controlador redondea la cadena en Y a una escala adecuada (que también puede ser cero) y escribe el resultado en CT.  
  
         (3) el controlador no se trunca ni se redondea, sino que solo copia los primeros caracteres LT de Y en CT.
