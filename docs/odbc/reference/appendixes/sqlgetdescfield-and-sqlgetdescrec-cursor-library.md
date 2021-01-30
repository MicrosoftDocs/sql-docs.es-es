---
description: SQLGetDescField y SQLGetDescRec (biblioteca de cursores)
title: SQLGetDescField y SQLGetDescRec (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetDescField function [ODBC], Cursor Library
- SQLGetDescRec function [ODBC], Cursor Library
ms.assetid: 1a801f22-6fea-48aa-a723-3187a2ad852b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ef395940419d593571dc2da6df6323437f2812eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202778"
---
# <a name="sqlgetdescfield-and-sqlgetdescrec-cursor-library"></a>SQLGetDescField y SQLGetDescRec (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de las funciones **SQLGetDescField** y **SQLGetDescRec** en la biblioteca de cursores. Para obtener información general sobre estas funciones, vea [función SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) y [función SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md).  
  
 La biblioteca de cursores ejecuta **SQLGetDescRec** para devolver los metadatos de las columnas de marcador. La biblioteca de cursores ejecuta **SQLGetDescField** para devolver los mismos campos devueltos por **SQLGetDescRec**, que son SQL_DESC_NAME, SQL_DESC_TYPE, SQL_DESC_DATETIME_INTERVAL_CODE, SQL_DESC_OCTET_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE y SQL_DESC_NULLABLE. Por coherencia, **SQLGetDescField** también devuelve SQL_DESC_UNNAMED.  
  
 La biblioteca de cursores ejecuta **SQLGetDescField** cuando se llama para devolver el valor de los siguientes campos que se establecen para enlazar columnas de marcador: SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR, SQL_DESC_OCTET_LENGTH_PTR y SQL_DESC_LENGTH.  
  
 La biblioteca de cursores ejecuta **SQLGetDescField** cuando se llama para devolver el valor del campo SQL_DESC_BIND_OFFSET_PTR, SQL_DESC_BIND_TYPE, SQL_DESC_ROW_ARRAY_SIZE o SQL_DESC_ROW_STATUS_PTR. Estos campos se pueden devolver para cualquier fila, no solo para la fila del marcador.  
  
 Si una aplicación llama a **SQLGetDescField** para devolver el valor de cualquier campo distinto de los mencionados anteriormente, la biblioteca de cursores pasa la llamada al controlador.
