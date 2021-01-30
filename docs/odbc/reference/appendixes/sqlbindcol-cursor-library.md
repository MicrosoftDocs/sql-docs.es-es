---
description: SQLBindCol (biblioteca de cursores)
title: SQLBindCol (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLAllocStmt function [ODBC], Cursor Library
ms.assetid: f4dd546a-0a6c-4397-8ee7-fafa6b9da543
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 64b51f58fd51b9bcad1abd86750b024be131a8a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202944"
---
# <a name="sqlbindcol-cursor-library"></a>SQLBindCol (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLBindCol** en la biblioteca de cursores. Para obtener información general acerca de **SQLBindCol**, consulte [SQLBindCol (función](../../../odbc/reference/syntax/sqlbindcol-function.md)).  
  
 Una aplicación asigna uno o más búferes para que la biblioteca de cursores devuelva el conjunto de filas actual. Llama a **SQLBindCol** una o más veces para enlazar estos búferes con el conjunto de resultados.  
  
 Una aplicación puede llamar a **SQLBindCol** para volver a enlazar las columnas del conjunto de resultados después de haber llamado a **SQLExtendedFetch**, **SQLFetch** o **SQLFetchScroll**, siempre que el tipo de datos de C, el tamaño de la columna y los dígitos decimales de la columna enlazada sigan siendo los mismos. La aplicación no tiene que cerrar el cursor para volver a enlazar columnas a direcciones diferentes.  
  
 La biblioteca de cursores admite el establecimiento del atributo de instrucción SQL_ATTR_ROW_BIND_OFFSET_PTR para usar desplazamientos de enlace. (No es necesario llamar a **SQLBindCol** para que se produzca este reenlace). Si la biblioteca de cursores se utiliza con un controlador ODBC *3. x* , el desplazamiento de enlace no se utilizará cuando se llame a **SQLFetch** . El desplazamiento de enlace se utiliza si se llama a **SQLFetch** cuando la biblioteca de cursores se utiliza con un controlador ODBC *2. x* , ya que **SQLFetch** se asigna a **SQLExtendedFetch**.  
  
 La biblioteca de cursores admite la llamada a **SQLBindCol** para enlazar la columna de marcador.  
  
 Cuando se trabaja con un controlador ODBC *2. x* , la biblioteca de cursores devuelve SQLSTATE HY090 (cadena o longitud de búfer no válida) cuando se llama a **SQLBindCol** para establecer la longitud del búfer de una columna de marcador en un valor distinto de 4. Cuando se trabaja con un controlador ODBC *3. x* , la biblioteca de cursores permite que el búfer tenga cualquier tamaño.
