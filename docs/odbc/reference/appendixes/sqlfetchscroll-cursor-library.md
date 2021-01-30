---
description: SQLFetchScroll (biblioteca de cursores)
title: SQLFetchScroll (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLFetchScroll function [ODBC], Cursor Library
ms.assetid: 4417e57c-31dd-475e-8fe9-eab00a459c80
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 719504a6e1e412439314ee6971cf41e05805f592
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202850"
---
# <a name="sqlfetchscroll-cursor-library"></a>SQLFetchScroll (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLFetchScroll** en la biblioteca de cursores. Para obtener información general sobre **SQLFetchScroll**, vea [función sqlfetchscroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
 La biblioteca de cursores implementa **SQLFetchScroll** llamando repetidamente a **SQLFetch** en el controlador. Transfiere los datos que recupera del controlador a los búferes de conjunto de filas proporcionados por la aplicación. También almacena en caché los datos de los archivos de memoria y de disco. Cuando una aplicación solicita un conjunto de filas nuevo, la biblioteca de cursores lo recupera según sea necesario del controlador (si no se ha capturado previamente) o de la memoria caché (si se ha capturado previamente). Por último, la biblioteca de cursores mantiene el estado de los datos almacenados en caché y devuelve esta información a la aplicación en la matriz de estado de fila.  
  
 Cuando se usa la biblioteca de cursores, las llamadas a **SQLFetchScroll** no se pueden mezclar con llamadas a **SQLFetch** o **SQLExtendedFetch**.  
  
 Cuando se usa la biblioteca de cursores, se admiten las llamadas a **SQLFetchScroll** para ODBC 2. *x* y para ODBC 3. Controladores *x* .  
  
## <a name="rowset-buffers"></a>Búferes de conjunto de filas  
 La biblioteca de cursores optimiza la transferencia de datos desde el controlador al búfer de conjunto de filas proporcionado por la aplicación si:  
  
-   La aplicación utiliza el enlace de modo de fila.  
  
-   No hay ningún byte sin usar entre los campos de la estructura que la aplicación declara para contener una fila de datos.  
  
-   Los campos en los que **SQLFetch** o **SQLFetchScroll** devuelven la longitud o el indicador de una columna siguen el búfer de esa columna y preceden al búfer para la columna siguiente. Estos campos son opcionales.  
  
 Cuando la aplicación solicita un conjunto de filas nuevo, la biblioteca de cursores recupera datos de su caché y del controlador según sea necesario. Si los conjuntos de filas nuevos y antiguos se superponen, la biblioteca de cursores puede optimizar su rendimiento reutilizando los datos de las secciones superpuestas de los búferes del conjunto de filas. Por consiguiente, los cambios no guardados en los búferes del conjunto de filas se pierden a menos que los conjuntos de filas nuevos y antiguos se superpongan y los cambios se encuentren en las secciones superpuestas de los búferes del conjunto de filas. Para guardar los cambios, una aplicación envía una instrucción UPDATE posicionada.  
  
 Tenga en cuenta que la biblioteca de cursores siempre actualiza los búferes del conjunto de filas con los datos de la memoria caché cuando una aplicación llama a **SQLFetchScroll** con el argumento *FetchOrientation* establecido en SQL_FETCH_RELATIVE y el argumento *FetchOffset* establecido en 0.  
  
 La biblioteca de cursores admite la llamada a **SQLSetStmtAttr** con un *atributo* de SQL_ATTR_ROW_ARRAY_SIZE para cambiar el tamaño del conjunto de filas mientras se abre un cursor. El nuevo tamaño del conjunto de filas tendrá efecto la próxima vez que se llame a **SQLFetchScroll** .  
  
## <a name="result-set-membership"></a>Pertenencia al conjunto de resultados  
 La biblioteca de cursores recupera datos del controlador solo cuando la aplicación los solicita. Dependiendo del origen de datos y de la configuración del atributo de instrucción SQL_CONCURRENCY, esto tiene las siguientes consecuencias:  
  
-   Los datos recuperados por la biblioteca de cursores pueden diferir de los datos que estaban disponibles en el momento en que se ejecutó la instrucción. Por ejemplo, una vez abierto el cursor, algunos controladores pueden recuperar las filas insertadas en un punto más allá de la posición actual del cursor.  
  
-   Es posible que los datos del conjunto de resultados estén bloqueados por el origen de datos de la biblioteca de cursores y, por tanto, no estén disponibles para otros usuarios.  
  
 Una vez que la biblioteca de cursores ha almacenado en memoria caché una fila de datos, no puede detectar cambios en esa fila en el origen de datos subyacente (excepto las actualizaciones posicionadas y eliminaciones que funcionan en la misma caché del cursor). Esto se debe a que, para las llamadas a **SQLFetchScroll**, la biblioteca de cursores nunca recupera los datos del origen de datos. En su lugar, vuelve a capturar los datos de su memoria caché.  
  
## <a name="scrolling"></a>Desplazarse  
 La biblioteca de cursores admite los siguientes tipos de captura en **SQLFetchScroll**.  
  
|Tipo de cursor|Tipos de captura|  
|-----------------|-----------------|  
|Solo avance|SQL_FETCH_NEXT|  
|estática|SQL_FETCH_NEXT<br /><br /> SQL_FETCH_PRIOR<br /><br /> SQL_FETCH_FIRST<br /><br /> SQL_FETCH_LAST<br /><br /> SQL_FETCH_RELATIVE<br /><br /> SQL_FETCH_ABSOLUTE<br /><br /> SQL_FETCH_BOOKMARK|  
  
## <a name="errors"></a>Errors  
 Cuando se llama a **SQLFetchScroll** y una de las llamadas a **SQLFetch** devuelve SQL_ERROR, la biblioteca de cursores continúa como se indica a continuación. Después de completar estos pasos, la biblioteca de cursores continúa el procesamiento.  
  
1.  Llama a **SQLGetDiagRec** para obtener información de error del controlador y lo envía como registro de diagnóstico en el administrador de controladores.  
  
2.  Establece el campo SQL_DIAG_ROW_NUMBER del registro de diagnóstico en el valor adecuado.  
  
3.  Establece el campo SQL_DIAG_COLUMN_NUMBER del registro de diagnóstico en el valor adecuado, si procede; de lo contrario, se establece en 0.  
  
4.  Establece el valor de la fila de error en la matriz de estado de fila en SQL_ROW_ERROR.  
  
 Una vez que la biblioteca de cursores ha llamado a **SQLFetch** varias veces en su implementación de **SQLFetchScroll**, cualquier error o ADVERTENCIA devuelto por una de las llamadas a **SQLFetch** estará en un registro de diagnóstico y se puede recuperar mediante una llamada a **SQLGetDiagRec**. Si los datos se truncaron cuando se capturaron, los datos truncados ahora residirán en la memoria caché de la biblioteca de cursores. Las llamadas posteriores a **SQLFetchScroll** para desplazarse a una fila con datos truncados devolverán los datos truncados y no se producirá ninguna advertencia porque los datos se capturan desde la memoria caché de la biblioteca de cursores. Para realizar un seguimiento de la longitud de los datos devueltos para que pueda determinar si los datos devueltos en un búfer se han truncado, una aplicación debe enlazar el búfer de longitud/indicador.  
  
## <a name="bookmark-operations"></a>Operaciones de marcador  
 La biblioteca de cursores admite la llamada a **SQLFetchScroll** con un *FetchOrientation* de SQL_FETCH_BOOKMARK. También admite la especificación de un desplazamiento en el argumento *FetchOffset* que se puede usar en la operación de marcador. Esta es la única operación de marcador que admite la biblioteca de cursores. La biblioteca de cursores no admite la llamada a **SQLBulkOperations**.  
  
 Si la aplicación ha establecido el atributo de instrucción SQL_ATTR_USE_BOOKMARKS y se ha enlazado a la columna de marcador, la biblioteca de cursores genera un marcador de longitud fija y lo devuelve a la aplicación. La biblioteca de cursores crea y mantiene los marcadores que usa; no utiliza los marcadores que se mantienen en el origen de datos. Cuando se llama a **SQLFetchScroll** para recuperar un bloque de datos que ya se ha recuperado del origen de datos, recupera los datos de la memoria caché de la biblioteca de cursores. Como resultado, la biblioteca de cursores debe crear y mantener el marcador utilizado en una llamada a **SQLFetchScroll** con un *FetchOrientation* de SQL_FETCH_BOOKMARK.  
  
## <a name="interaction-with-other-functions"></a>Interacción con otras funciones  
 Una aplicación debe llamar a **SQLFetch** o **SQLFetchScroll** antes de preparar o ejecutar cualquier instrucción UPDATE o DELETE posicionada.
