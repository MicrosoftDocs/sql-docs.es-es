---
description: Reemplazar predeterminado inicial y la precisión de segundos para los tipos de datos Interval
title: Invalidar la precisión de los segundos y el inicial para los tipos de datos de intervalo | Microsoft Docs
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
- interval leading precision [ODBC]
- interval precision [ODBC]
ms.assetid: 3d65493f-dce7-4d29-9f59-c63a4e47918c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a79bf67c0a8ccc988d9e171be9760be98c1dc5fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160842"
---
# <a name="overriding-default-leading-and-seconds-precision-for-interval-data-types"></a>Reemplazar predeterminado inicial y la precisión de segundos para los tipos de datos Interval
Cuando el campo de SQL_DESC_TYPE de una ARD se establece en un tipo de fecha y hora o de intervalo, al llamar a **SQLBindCol** o **SQLSetDescField**, el campo de SQL_DESC_PRECISION (que contiene la precisión de los segundos de intervalo) se establece en los valores predeterminados siguientes:  
  
-   6 para la marca de tiempo y todos los tipos de datos de intervalo con un segundo componente.  
  
-   0 para el resto de tipos de datos.  
  
 En todos los tipos de datos de intervalo, el campo descriptor de SQL_DESC_DATETIME_INTERVAL_PRECISION, que contiene la precisión del campo inicial del intervalo, se establece en un valor predeterminado de 2.  
  
 Cuando el campo de SQL_DESC_TYPE de una APD se establece en un tipo de fecha y hora o de intervalo, llamando a **SQLBindParameter** o **SQLSetDescField**, los campos SQL_DESC_PRECISION y SQL_DESC_DATETIME_INTERVAL_PRECISION de la APD se establecen en el valor predeterminado especificado anteriormente. Esto es válido para los parámetros de entrada, pero no para los parámetros de entrada/salida ni de salida.  
  
 Una llamada a **SQLSetDescRec** establece la precisión inicial del intervalo en el valor predeterminado, pero establece la precisión de los segundos del intervalo (en el campo SQL_DESC_PRECISION) en el valor de su argumento de *precisión* .  
  
 Si alguno de los valores predeterminados proporcionados previamente no es aceptable para una aplicación, la aplicación debe establecer el SQL_DESC_PRECISION o SQL_DESC_DATETIME_INTERVAL_PRECISION campo mediante una llamada a **SQLSetDescField**.  
  
 Si la aplicación llama a **SQLGetData** para devolver datos a un tipo DateTime o Interval C, se usan la precisión inicial del intervalo predeterminado y la precisión de los segundos del intervalo. Si alguno de los valores predeterminados no es aceptable, la aplicación debe llamar a **SQLSetDescField** para establecer cualquier campo de descriptor o **SQLSetDescRec** para establecer SQL_DESC_PRECISION. La llamada a **SQLGetData** debe tener un *TargetType* de SQL_ARD_TYPE para usar los valores de los campos de descriptor.  
  
 Cuando se llama a **SQLPutData** , la precisión inicial del intervalo y la precisión de los segundos del intervalo se leen de los campos del registro del descriptor que corresponden al parámetro o la columna de datos en ejecución, que son campos de APD para las llamadas a los campos **SQLExecute** o **SQLExecDirect**, o ARD para las llamadas a **SQLBulkOperations** o **SQLSetPos**.
