---
description: SQLSetDescField y SQLSetDescRec (biblioteca de cursores)
title: SQLSetDescField y SQLSetDescRec (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetDescField function [ODBC], Cursor Library
- SQLSetDescRec function [ODBC], Cursor Library
ms.assetid: 4ccff067-85cd-4bfa-a6cd-7f28051fb5b9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c97bbb38b5050461d3ca305d8ac895d693567be3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202644"
---
# <a name="sqlsetdescfield-and-sqlsetdescrec-cursor-library"></a>SQLSetDescField y SQLSetDescRec (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de las funciones **SQLSetDescField** y **SQLSetDescRec** en la biblioteca de cursores. Para obtener información general sobre estas funciones, vea [función SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) y [función SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
 La biblioteca de cursores ejecuta **SQLSetDescField** cuando se llama para devolver el valor de los campos establecidos para las columnas de marcador:  
  
 SQL_DESC_DATA_PTR  
  
 SQL_DESC_INDICATOR_PTR  
  
 SQL_DESC_OCTET_LENGTH_PTR  
  
 SQL_DESC_LENGTH  
  
 SQL_DESC_OCTET_LENGTH  
  
 SQL_DESC_DATETIME_INTERVAL_CODE  
  
 SQL_DESC_SCALE  
  
 SQL_DESC_PRECISION  
  
 SQL_DESC_TYPE  
  
 SQL_DESC_NAME  
  
 SQL_DESC_UNNAMED  
  
 SQL_DESC_NULLABLE  
  
 La biblioteca de cursores ejecuta llamadas a **SQLSetDescRec** para una columna de marcador.  
  
 Cuando se trabaja con un controlador ODBC *2. x* , la biblioteca de cursores devuelve SQLSTATE HY090 (cadena o longitud de búfer no válida) cuando se llama a **SQLSetDescField** o **SQLSetDescRec** para establecer el SQL_DESC_OCTET_LENGTH campo para el registro de marcador de un ARD en un valor distinto de 4. Cuando se trabaja con un controlador ODBC *3. x* , la biblioteca de cursores permite que el búfer tenga cualquier tamaño.  
  
 La biblioteca de cursores ejecuta **SQLSetDescField** cuando se llama para devolver el valor del campo SQL_DESC_BIND_OFFSET_PTR, SQL_DESC_BIND_TYPE, SQL_DESC_ROW_ARRAY_SIZE o SQL_DESC_ROW_STATUS_PTR. Estos campos se pueden devolver para cualquier fila, no solo para la fila del marcador.  
  
 La biblioteca de cursores no ejecuta **SQLSetDescField** para cambiar ningún campo de descriptor que no sea los campos mencionados anteriormente. Si una aplicación llama a **SQLSetDescField** para establecer cualquier otro campo mientras se carga la biblioteca de cursores, la llamada se pasa al controlador.  
  
 La biblioteca de cursores permite cambiar dinámicamente los campos SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR y SQL_DESC_OCTET_LENGTH_PTR de cualquier fila de un descriptor de fila de aplicación (después de una llamada a **SQLExtendedFetch**, **SQLFetch** o **SQLFetchScroll**). El campo SQL_DESC_OCTET_LENGTH_PTR se puede cambiar a un puntero nulo solo para desenlazar el búfer de longitud de una columna.  
  
 La biblioteca de cursores no admite el cambio del campo de SQL_DESC_BIND_TYPE en un APD o ARD cuando un cursor está abierto. El campo SQL_DESC_BIND_TYPE solo se puede cambiar después de cerrar el cursor y antes de abrir un nuevo cursor. Los únicos campos descriptores que la biblioteca de cursores admite cambiar cuando un cursor está abierto son SQL_DESC_ARRAY_STATUS_PTR, SQL_DESC_BIND_OFFSET_PTR, SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR, SQL_DESC_OCTET_LENGTH_PTR y SQL_DESC_ROWS_PROCESSED_PTR.  
  
 La biblioteca de cursores no admite la modificación del campo de SQL_DESC_COUNT de ARD después de llamar a **SQLExtendedFetch** o **SQLFetchScroll** y antes de que se haya cerrado el cursor.
