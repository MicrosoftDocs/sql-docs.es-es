---
description: Tamaño de presentación
title: Tamaño de presentación | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- display size of data types [ODBC]
- size of data types [ODBC]
- data types [ODBC], display size
- SQL data types [ODBC], column characteristics
ms.assetid: 9f7f766f-2492-463c-aab7-f2476e222042
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a39a0f986164cf6bc14e8be6e221232dd936e4bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194878"
---
# <a name="display-size"></a>Tamaño de presentación
El tamaño de presentación de una columna es el número máximo de caracteres necesarios para mostrar los datos en forma de carácter. En la tabla siguiente se define el tamaño de presentación de cada tipo de datos SQL de ODBC.  
  
|Identificador de tipo de SQL|Tamaño de pantalla|  
|-------------------------|------------------|  
|Todos los tipos de caracteres [a]|El número de caracteres definido (para tipos fijos) o máximo (para tipos de variables) que se necesitan para mostrar los datos en formato de caracteres.|  
|SQL_DECIMAL SQL_NUMERIC|La precisión de la columna más 2 (un signo, dígitos de *precisión* y un separador decimal). Por ejemplo, el tamaño de presentación de una columna definida como numérica (10, 3) es 12.|  
|SQL_BIT|1 (1 dígito).|  
|SQL_TINYINT|4 Si está firmado (un signo y 3 dígitos) o 3 si no tiene signo (3 dígitos).|  
|SQL_SMALLINT|6 Si está firmado (un signo y 5 dígitos) o 5 si no tiene signo (5 dígitos).|  
|SQL_INTEGER|11 si está firmado (un signo y 10 dígitos) o 10 si es sin signo (10 dígitos).|  
|SQL_BIGINT|20 (un signo y 19 dígitos si están firmados o 20 dígitos si no se han firmado).|  
|SQL_REAL|14 (un signo, 7 dígitos, un separador decimal, la letra *E*, un signo y 2 dígitos).|  
|SQL_FLOAT SQL_DOUBLE|24 (un signo, 15 dígitos, un separador decimal, la letra *E*, un signo y 3 dígitos).|  
|Todos los tipos binarios [a]|La longitud definida o máxima (para tipos de variable) de la columna veces el valor 2. (Cada byte binario se representa con un número hexadecimal de 2 dígitos).|  
|SQL_TYPE_DATE|10 (una fecha con el formato *AAAA-MM-DD*).|  
|SQL_TYPE_TIME|8 (una hora con el formato *HH: mm: SS*)<br /><br /> o bien<br /><br /> 9 + *s* (una hora con el formato *HH: mm: SS*[. FFF...], donde *s* es la precisión de las fracciones de segundo).|  
|SQL_TYPE_TIMESTAMP|19 (para una marca de tiempo en el formato *AAAA-MM-DD HH: mm: SS* )<br /><br /> o bien<br /><br /> 20 + *s* (para una marca de tiempo en el formato *AAAA-MM-DD HH: mm: SS*[. FFF...], donde *s* es la precisión de las fracciones de segundo).|  
|Todos los tipos de datos de intervalo|Vea [longitud del tipo de datos de intervalo](../../../odbc/reference/appendixes/interval-data-type-length.md).|  
|SQL_GUID|36 (número de caracteres en el formato *aaaaaaaa-bbbb-CCCC-dddd-eeeeeeeeeeee*|  
  
 [a] si el controlador no puede determinar la longitud de la columna o el parámetro de los tipos de variable, devuelve SQL_NO_TOTAL.
