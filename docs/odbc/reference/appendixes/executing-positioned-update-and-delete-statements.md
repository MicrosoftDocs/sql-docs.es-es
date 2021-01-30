---
description: Ejecución de instrucciones de eliminación y actualización Positioned
title: Ejecutando instrucciones Update y DELETE posicionadas | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- positioned deletes [ODBC]
- cursor library [ODBC], positioned update or delete
- positioned updates [ODBC]
- ODBC cursor library [ODBC], positioned update or delete
ms.assetid: 1d64f309-2a6e-4ad1-a6b5-e81145549c56
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d10b2e5c625a222e8e09a5e783291ce613a492b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194831"
---
# <a name="executing-positioned-update-and-delete-statements"></a>Ejecución de instrucciones de eliminación y actualización Positioned
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 Una vez que una aplicación ha capturado un bloque de datos con **SQLFetchScroll**, puede actualizar o eliminar los datos del bloque. Para ejecutar una actualización o eliminación posicionada, la aplicación:  
  
1.  Llama a **SQLSetPos** para colocar el cursor en la fila que se va a actualizar o eliminar.  
  
2.  Construye una instrucción UPDATE o DELETE posicionada con la siguiente sintaxis:  
  
     **Actualizar** *tabla-nombre*  
  
     **Establecer** *el identificador de columna* **=** {*expresión* &#124; **null**}  
  
     [**,** *identificador* **=** de columna {*expresión* &#124; **null**}]  
  
     **WHERE CURRENT of** *cursor-Name*  
  
     **Eliminar de** *TABLE-Name* **donde Current of** *cursor-Name*  
  
     La forma más fácil de construir la cláusula **set** en una instrucción UPDATE posicionada es usar marcadores de parámetros para que se actualice cada columna y usar **SQLBindParameter** para enlazarlos a los búferes del conjunto de filas para que se actualice la fila. En este caso, el tipo de datos C del parámetro será el mismo que el tipo de datos C del búfer del conjunto de filas.  
  
3.  Actualiza los búferes del conjunto de filas de la fila actual si va a ejecutar una instrucción UPDATE posicionada. Después de ejecutar correctamente una instrucción UPDATE posicionada, la biblioteca de cursores copia los valores de cada columna de la fila actual a su memoria caché.  
  
    > [!CAUTION]  
    >  Si la aplicación no actualiza correctamente los búferes del conjunto de filas antes de ejecutar una instrucción UPDATE posicionada, los datos de la memoria caché serán incorrectos después de que se ejecute la instrucción.  
  
4.  Ejecuta la instrucción UPDATE o DELETE posicionada con una instrucción diferente de la instrucción asociada al cursor.  
  
    > [!CAUTION]  
    >  La cláusula **Where** construida por la biblioteca de cursores para identificar la fila actual puede no identificar filas, identificar una fila diferente o identificar más de una fila. Para obtener más información, vea [crear instrucciones de búsqueda](../../../odbc/reference/appendixes/constructing-searched-statements.md).  
  
 Todas las instrucciones Update y DELETE posicionadas requieren un nombre de cursor. Para especificar el nombre del cursor, una aplicación llama a **SQLSetCursorName** antes de que se abra el cursor. Para usar el nombre de cursor generado por el controlador, una aplicación llama a **SQLGetCursorName** después de abrir el cursor.  
  
 Después de que la biblioteca de cursores ejecute una instrucción UPDATE o DELETE posicionada, la matriz de estado, los búferes del conjunto de filas y la caché mantenida por la biblioteca de cursores contienen los valores que se muestran en la tabla siguiente.  
  
|Instrucción usada|Valor en la matriz de estado de fila|Valores de<br /><br /> búferes de conjunto de filas|Valores de<br /><br /> búferes de caché|  
|--------------------|-------------------------------|----------------------------------|---------------------------------|  
|Actualización posicionada|SQL_ROW_UPDATED|Nuevos valores [1]|Nuevos valores [1]|  
|Eliminación posicionada|SQL_ROW_DELETED|Valores anteriores|Valores anteriores|  
  
 [1] la aplicación debe actualizar los valores de los búferes del conjunto de filas antes de ejecutar la instrucción UPDATE posicionada; después de ejecutar la instrucción UPDATE posicionada, la biblioteca de cursores copia los valores de los búferes del conjunto de filas a su memoria caché.
