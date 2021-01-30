---
description: Longitud de datos de columna
title: Longitud de datos de columna | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- column data [ODBC]
- length of column data [ODBC]
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: c762c881-ebe0-4eac-84d5-f30281fc3eca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: be0ad52f040c195d4e8eeab9d307aa8f92bcf62d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184407"
---
# <a name="length-of-column-data"></a>Longitud de datos de columna
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 La biblioteca de cursores crea un búfer en la memoria caché para cada búfer de longitud/indicador enlazado al conjunto de resultados con **SQLBindCol**. Usa los valores de estos búferes para construir una cláusula **Where** cuando emula instrucciones UPDATE o DELETE posicionadas. Actualiza estos búferes de los búferes del conjunto de filas cuando captura datos del origen de datos y cuando ejecuta instrucciones Update posicionadas.  
  
 Si el tipo C de un búfer de datos es SQL_C_CHAR o SQL_C_BINARY y el valor de longitud/indicador es SQL_NTS, la longitud de la cadena de los datos se coloca en el búfer de longitud/indicador.  
  
> [!NOTE]  
>  La biblioteca de cursores no actualiza su memoria caché para una columna si **StrLen_or_IndPtr* en el búfer del conjunto de filas correspondiente es SQL_DATA_AT_EXEC o el resultado de la macro SQL_LEN_DATA_AT_EXEC.
