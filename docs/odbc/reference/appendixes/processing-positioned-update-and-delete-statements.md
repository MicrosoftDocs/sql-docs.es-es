---
description: Procesamiento de instrucciones posicionadas Update y Delete
title: Procesando instrucciones Update y DELETE posicionadas | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- positioned deletes [ODBC]
- ODBC cursor library [ODBC], statement processing
- cursor library [ODBC], positioned update or delete
- positioned updates [ODBC]
- SQL statements [ODBC], cursor library
- ODBC cursor library [ODBC], positioned update or delete
- cursor library [ODBC], statement processing
ms.assetid: 2975dd97-48e6-4d0a-a9c7-40759a7d94c8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f2d7283524a516b81b6489b543fd3169f7f9aeb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159534"
---
# <a name="processing-positioned-update-and-delete-statements"></a>Procesamiento de instrucciones posicionadas Update y Delete
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 La biblioteca de cursores admite las instrucciones Update y DELETE posicionadas reemplazando la cláusula **WHERE CURRENT of** en tales instrucciones por una cláusula **Where** que enumera los valores almacenados en su caché para cada columna enlazada. La biblioteca de cursores pasa las instrucciones **Update** y **Delete** recién construidas al controlador para su ejecución. En el caso de las instrucciones Update posicionadas, la biblioteca de cursores actualiza su caché a partir de los valores de los búferes del conjunto de filas y establece el valor correspondiente en la matriz de estado de fila en SQL_ROW_UPDATED. En el caso de las instrucciones Delete posicionadas, establece el valor correspondiente de la matriz de estado de fila en SQL_ROW_DELETED.  
  
> [!CAUTION]  
>  La cláusula **Where** construida por la biblioteca de cursores para identificar la fila actual puede no identificar filas, identificar una fila diferente o identificar más de una fila. Para obtener más información, vea [crear instrucciones de búsqueda](../../../odbc/reference/appendixes/constructing-searched-statements.md), más adelante en este apéndice.  
  
 Las instrucciones Update y DELETE posicionadas están sujetas a las siguientes restricciones:  
  
-   Las instrucciones Update y DELETE posicionadas solo se pueden usar en los casos siguientes: cuando una instrucción **Select** genera el conjunto de resultados; Cuando la instrucción **Select** no contenía una cláusula join, **Union** o **Group by** ; y cuando las columnas que usaban un alias o una expresión en la lista de selección no se enlazaron con **SQLBindCol**.  
  
-   Si una aplicación prepara una instrucción UPDATE o DELETE posicionada, debe hacerlo después de haber llamado a **SQLFetch** o **SQLFetchScroll**. Aunque la biblioteca de cursores envía la instrucción al controlador para su preparación, cierra la instrucción y la ejecuta directamente cuando la aplicación llama a **SQLExecute**.  
  
-   Si el controlador solo admite una instrucción activa, la biblioteca de cursores captura el resto del conjunto de resultados y, a continuación, vuelve a capturar el conjunto de filas actual de su memoria caché antes de ejecutar una instrucción UPDATE o DELETE posicionada. Si, a continuación, la aplicación llama a una función que devuelve los metadatos de un conjunto de resultados (por ejemplo, **SQLNumResultCols** o **SQLDescribeCol**), la biblioteca de cursores devuelve un error.  
  
-   Si se realiza una instrucción UPDATE o DELETE posicionada en una columna de una tabla que incluye una columna de marca de tiempo que se actualiza automáticamente cada vez que se realiza una actualización, se producirá un error en todas las instrucciones UPDATE o DELETE posicionadas subsiguientes si la columna TIMESTAMP está enlazada. Esto se debe a que la instrucción UPDATE o DELETE buscada que crea la biblioteca de cursores no identificará con precisión la fila que se va a actualizar. El valor de la instrucción buscada para la columna TIMESTAMP no coincidirá con el valor actualizado automáticamente de la columna TIMESTAMP.
