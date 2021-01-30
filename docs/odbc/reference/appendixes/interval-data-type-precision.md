---
description: Precisión del tipo de datos de intervalo
title: Precisión del tipo de datos Interval | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- precision [ODBC], interval data types
- seconds precision [ODBC]
- interval data type [ODBC], precision
- precision [ODBC]
- interval leading precision [ODBC]
- interval precision [ODBC]
ms.assetid: eb73bd77-2e7e-4498-a266-4d7c990a0d56
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 103b19e43f4ba0cea160bfd53aa178404adfe781
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208534"
---
# <a name="interval-data-type-precision"></a>Precisión del tipo de datos de intervalo
La precisión de un tipo de datos Interval incluye la precisión inicial del intervalo, la precisión del intervalo y la precisión de los segundos.  
  
 El campo inicial de un intervalo es un número con signo. El número máximo de dígitos para el campo inicial viene determinado por una cantidad llamada *precisión inicial del intervalo,* que forma parte de la declaración del tipo de datos. Por ejemplo, la declaración: la hora de intervalo (5) a minuto tiene un intervalo de precisión inicial de 5; el campo de hora puede tomar valores comprendidos entre-99999 y 99999. La precisión inicial del intervalo se encuentra en el campo SQL_DESC_DATETIME_INTERVAL_PRECISION del registro del descriptor.  
  
 La lista de campos de los que se compone un tipo de datos de intervalo se denomina *precisión de intervalo*. No es un valor numérico, ya que el término "precisión" puede implicar. Por ejemplo, la precisión de intervalo del intervalo de tipo DAY a SECOND es la lista de día, hora, minuto, segundo. No hay ningún campo de descriptor que contenga este valor; la precisión del intervalo siempre puede determinarse mediante el tipo de datos Interval.  
  
 Cualquier tipo de datos de intervalo que tenga un segundo campo tiene una *precisión de segundos*. Es el número de dígitos decimales permitidos en la parte fraccionaria del valor de segundos. Esto es diferente que para otros tipos de datos, donde precisión indica el número de dígitos antes del separador decimal. La precisión en segundos de un tipo de datos de intervalo es el número de dígitos después del separador decimal. Por ejemplo, si la precisión de segundos se establece en 6, el número 123456 en el campo de fracción se interpretaría como. 123456 y el número 1230 se interpretaría como. 001230. Para otros tipos de datos, esto se conoce como escala. La precisión de los segundos de intervalo está contenida en el campo SQL_DESC_PRECISION del descriptor. Si la precisión del componente de fracciones de segundo del valor de intervalo de SQL es mayor que la que se puede mantener en la estructura de intervalo de C, se define con el controlador si el valor de las fracciones de segundo del intervalo de SQL se redondea o se trunca cuando se convierte en la estructura de intervalo de C.  
  
 Cuando el campo SQL_DESC_CONCISE_TYPE está establecido en un tipo de datos de intervalo, el campo SQL_DESC_TYPE se establece en SQL_INTERVAL y el SQL_DESC_DATETIME_INTERVAL_CODE se establece en el código del tipo de datos Interval. El campo SQL_DESC_DATETIME_INTERVAL_PRECISION se establece automáticamente en la precisión inicial del intervalo predeterminado de 2 y el campo SQL_DESC_PRECISION se establece automáticamente en la precisión de segundos del intervalo predeterminado de 6. Si alguno de estos valores no es adecuado, la aplicación debe establecer explícitamente el campo descriptor mediante una llamada a **SQLSetDescField**.
