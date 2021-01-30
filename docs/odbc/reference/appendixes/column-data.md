---
description: Datos de columna
title: Datos de columna | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- column data [ODBC]
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: 0425818c-9469-493f-9e3c-fc03d9411c5c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1287a4deadc6f35bf83f6c42dcebc633f6557fed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212343"
---
# <a name="column-data"></a>Datos de columna
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 La biblioteca de cursores crea un búfer en la memoria caché para cada búfer de datos enlazado al conjunto de resultados con **SQLBindCol**. Usa los valores de estos búferes para construir una cláusula **Where** cuando emula una instrucción UPDATE o DELETE posicionada. Actualiza estos búferes de los búferes del conjunto de filas cuando captura datos del origen de datos y cuando ejecuta instrucciones Update posicionadas.  
  
 Cuando la biblioteca de cursores actualiza su memoria caché de los búferes del conjunto de filas, transfiere los datos según el tipo de datos C especificado en **SQLBindCol**. Por ejemplo, si el tipo de datos C de un búfer de conjunto de filas es SQL_C_SLONG, la biblioteca de cursores transfiere cuatro bytes de datos. Si es SQL_C_CHAR y *BufferLength* es 10, la biblioteca de cursores transfiere 10 bytes de datos. La biblioteca de cursores no realiza ninguna comprobación de tipos ni conversiones en los datos que transfiere.  
  
> [!NOTE]  
>  La biblioteca de cursores no actualiza su memoria caché para una columna si **StrLen_or_IndPtr* en el búfer del conjunto de filas correspondiente es SQL_DATA_AT_EXEC o el resultado de la macro SQL_LEN_DATA_AT_EXEC.  
  
 Cuando actualiza una columna, un origen de datos pone en blanco los datos de caracteres de longitud fija y ceros los datos binarios de longitud fija, según sea necesario. Por ejemplo, un origen de datos almacena "Smith" en una columna CHAR (10) como "Smith". La biblioteca de cursores no rellena en blanco ni rellenan los datos en los búferes del conjunto de filas cuando copia estos datos a su memoria caché después de ejecutar una instrucción UPDATE posicionada. Por lo tanto, si una aplicación requiere que los valores de la memoria caché de la biblioteca de cursores se rellenen en blanco o se rellenen con ceros, debe rellenar en blanco o rellenar con ceros los valores de los búferes del conjunto de filas antes de ejecutar una instrucción UPDATE posicionada.
