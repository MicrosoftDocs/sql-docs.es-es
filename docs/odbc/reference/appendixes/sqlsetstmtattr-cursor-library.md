---
description: SQLSetStmtAttr (biblioteca de cursores)
title: SQLSetStmtAttr (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetStmtAttr function [ODBC], Cursor Library
ms.assetid: 6018a733-c2c8-4047-92ec-92cf85031767
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a395052df9eb5623e63463e3e79722769583721
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202584"
---
# <a name="sqlsetstmtattr-cursor-library"></a>SQLSetStmtAttr (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLSetStmtAttr** en la biblioteca de cursores. Para obtener información general sobre **SQLSetStmtAttr**, consulte la [función SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
 La biblioteca de cursores admite los siguientes atributos de instrucción con **SQLSetStmtAttr**:  

:::row:::
    :::column:::
        SQL_ATTR_CONCURRENCY  
        SQL_ATTR_CURSOR_TYPE  
        SQL_ATTR_FETCH_BOOKMARK_PTR  
        SQL_ATTR_PARAM_BIND_OFFSET_PTR  
        SQL_ATTR_PARAM_BIND_TYPE  
    :::column-end:::
    :::column:::
        SQL_ATTR_ROW_BIND_OFFSET_PTR  
        SQL_ATTR_ROW_BIND_TYPE  
        SQL_ATTR_ROWSET_ARRAY_SIZE  
        SQL_ATTR_SIMULATE_CURSOR  
        SQL_ATTR_USE_BOOKMARKS  
    :::column-end:::
:::row-end:::

 La biblioteca de cursores solo admite los valores SQL_CURSOR_FORWARD_ONLY y SQL_CURSOR_STATIC del atributo de instrucción SQL_ATTR_CURSOR_TYPE.  
  
 En el caso de los cursores de solo avance, la biblioteca de cursores admite el valor SQL_CONCUR_READ_ONLY del atributo de instrucción SQL_ATTR_CONCURRENCY. En el caso de los cursores estáticos, la biblioteca de cursores admite los valores SQL_CONCUR_READ_ONLY y SQL_CONCUR_VALUES del atributo de instrucción SQL_ATTR_CONCURRENCY.  
  
 La biblioteca de cursores solo admite el valor SQL_SC_NON_UNIQUE del atributo de instrucción SQL_ATTR_SIMULATE_CURSOR.  
  
 Aunque la especificación de ODBC admite llamadas a **SQLSetStmtAttr** con los atributos SQL_ATTR_PARAM_BIND_TYPE o SQL_ATTR_ROW_BIND_TYPE después de haber llamado a **SQLFetch** o **SQLFetchScroll** , la biblioteca de cursores no lo hace. Antes de que pueda cambiar el tipo de enlace en la biblioteca de cursores, la aplicación debe cerrar el cursor. La biblioteca de cursores admite el cambio de los atributos de instrucción SQL_ATTR_ROW_BIND_OFFSET_PTR, SQL_ATTR_PARAM_BIND_OFFSET_PTR, SQL_ATTR_ROWS_FETCHED_PTR y SQL_ATTR_PARAMS_PROCESSED_PTR cuando se abre un cursor.  
  
 Una aplicación puede llamar a **SQLSetStmtAttr** con un **atributo** de SQL_ATTR_ROW_ARRAY_SIZE para cambiar el tamaño del conjunto de filas mientras se abre un cursor. El nuevo tamaño del conjunto de filas tendrá efecto la próxima vez que se llame a **SQLFetchScroll** o **SQLFetch** .  
  
 La biblioteca de cursores admite el establecimiento del atributo de instrucción SQL_ATTR_PARAM_BIND_OFFSET_PTR o SQL_ATTR_ROW_BIND_OFFSET_PTR para habilitar desplazamientos de enlace. El desplazamiento de enlace no se utilizará para las llamadas a **SQLFetch** cuando la biblioteca de cursores se utiliza con ODBC 2. controlador *x* .  
  
 La biblioteca de cursores permite establecer el atributo de instrucción SQL_ATTR_USE_BOOKMARKS en SQL_UB_VARIABLE.
