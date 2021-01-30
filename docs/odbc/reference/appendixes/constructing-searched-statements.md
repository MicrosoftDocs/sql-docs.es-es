---
description: Construcción de instrucciones Searched
title: Construir instrucciones buscadas | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- searched statements [ODBC]
- ODBC cursor library [ODBC], statement processing
- ODBC cursor library [ODBC], searched statements
- SQL statements [ODBC], cursor library
- cursor library [ODBC], statement processing
- cursor library [ODBC], searched statements
- SQL statements [ODBC], searched statements
ms.assetid: e429254c-c43f-4fbf-98b2-5f1ed53501ff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a7928e3c730a7431d7f94a90a85073a3c818ae31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165395"
---
# <a name="constructing-searched-statements"></a>Construcción de instrucciones Searched
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 Para admitir las instrucciones Update y DELETE posicionadas, la biblioteca de cursores crea una instrucción **Update** o **Delete** buscada a partir de la instrucción posicionada. Para admitir llamadas a **SQLGetData** en un bloque de datos, la biblioteca de cursores crea una instrucción **Select** de búsqueda para crear un conjunto de resultados que contenga la fila de datos actual. En cada una de estas instrucciones, la cláusula **Where** enumera los valores almacenados en la memoria caché para cada columna enlazada que devuelve SQL_PRED_SEARCHABLE o SQL_PRED_BASIC para el identificador de campo SQL_DESC_SEARCHABLE en **SQLColAttribute**.  
  
> [!CAUTION]  
>  La cláusula **Where** construida por la biblioteca de cursores para identificar la fila actual puede no identificar filas, identificar una fila diferente o identificar más de una fila.  
  
 Si una instrucción UPDATE o DELETE posicionada afecta a más de una fila, la biblioteca de cursores actualiza la matriz de estado de fila solo para la fila en la que se coloca el cursor y devuelve SQL_SUCCESS_WITH_INFO y SQLSTATE 01001 (conflicto de la operación de cursor). Si la instrucción no identifica ninguna fila, la biblioteca de cursores no actualiza la matriz de estado de fila y devuelve SQL_SUCCESS_WITH_INFO y SQLSTATE 01001 (conflicto de la operación de cursor). Una aplicación puede llamar a **SQLRowCount** para determinar el número de filas que se actualizaron o eliminaron.  
  
 Si la cláusula **Select** usada para colocar el cursor para una llamada a **SQLGetData** identifica más de una fila, no se garantiza que **SQLGetData** devuelva los datos correctos. Si no identifica ninguna fila, **SQLGetData** devuelve SQL_NO_DATA.  
  
 Si una aplicación cumple las directrices siguientes, la cláusula **Where** construida por la biblioteca de cursores debe identificar de forma única la fila actual, excepto cuando esto no es posible, por ejemplo, cuando el origen de datos contiene filas duplicadas.  
  
-   **Enlazar columnas que identifican la fila de forma única.** Si las columnas enlazadas no identifican de forma exclusiva la fila, la cláusula **Where** construida por la biblioteca de cursores podría identificar más de una fila. En una instrucción UPDATE o DELETE posicionada, esta cláusula puede hacer que se actualice o se elimine más de una fila. En una llamada a **SQLGetData**, este tipo de cláusula puede hacer que el controlador devuelva datos para la fila equivocada. Enlazar todas las columnas de una clave única garantiza que cada fila se identifica de forma única.  
  
-   **Asigne búferes de datos lo suficientemente grandes como para que no se produzca ningún truncamiento.** La memoria caché de la biblioteca de cursores es una copia de los valores de los búferes de conjunto de filas enlazados al conjunto de resultados con **SQLBindCol**. Si los datos se truncan cuando se colocan en estos búferes, también se truncarán en la memoria caché. Una cláusula **Where** construida a partir de valores truncados podría no identificar correctamente la fila subyacente en el origen de datos.  
  
-   **Especifique búferes de longitud no NULL para los datos binarios de C.** La biblioteca de cursores asigna búferes de longitud en su caché solo si el argumento *StrLen_or_IndPtr* en **SQLBindCol** no es NULL. Cuando el argumento *TargetType* es SQL_C_BINARY, la biblioteca de cursores requiere la longitud de los datos binarios para construir una cláusula **Where** a partir de los datos. Si no hay ningún búfer de longitud para una columna SQL_C_BINARY y la aplicación llama a **SQLGetData** o intenta ejecutar una instrucción UPDATE o DELETE posicionada, la biblioteca de cursores devuelve SQL_ERROR y SQLSTATE SL014 (se emitió una solicitud posicionada y no se almacenaron en búfer todos los campos de recuento de columnas).  
  
-   **Especifique búferes de longitud no NULL para las columnas que aceptan valores NULL.** La biblioteca de cursores asigna búferes de longitud en su caché solo si el argumento *StrLen_or_IndPtr* en **SQLBindCol** no es NULL. Dado que SQL_NULL_DATA se almacena en el búfer de longitud, la biblioteca de cursores presupone que cualquier columna para la que no se especifica ningún búfer de longitud no acepta valores NULL. Si no se especifica ninguna columna de longitud para una columna que admite valores NULL, la biblioteca de cursores crea una cláusula **Where** que utiliza el valor de datos de la columna. Esta cláusula no identificará correctamente la fila.
