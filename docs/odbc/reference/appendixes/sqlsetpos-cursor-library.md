---
description: SQLSetPos (biblioteca de cursores)
title: SQLSetPos (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetPos function [ODBC], Cursor Library
ms.assetid: 574399c3-2bb2-4d19-829c-7c77bd82858d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bf97529f4fd411581738459accffc805a444a8ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202614"
---
# <a name="sqlsetpos-cursor-library"></a>SQLSetPos (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLSetPos** en la biblioteca de cursores. Para obtener información general sobre **SQLSetPos**, consulte [SQLSetPos (función](../../../odbc/reference/syntax/sqlsetpos-function.md)).  
  
 La biblioteca de cursores admite la SQL_POSITION operación solo para el argumento *Operation* en **SQLSetPos**. Admite el valor SQL_LOCK_NO_CHANGE solo para el argumento *LockType* .  
  
 Si el controlador no admite operaciones masivas, la biblioteca de cursores devuelve SQLSTATE HYC00 (controlador no compatible) cuando se llama a **SQLSetPos** con *RowNumber* igual a 0. No se recomienda este comportamiento del controlador.  
  
 La biblioteca de cursores no admite las operaciones SQL_UPDATE y SQL_DELETE en una llamada a **SQLSetPos**. La biblioteca de cursores implementa una instrucción UPDATE o DELETE de SQL mediante la creación de una instrucción UPDATE o DELETE buscada con una cláusula WHERE que enumera los valores almacenados en su caché para cada columna enlazada. Para obtener más información, vea [procesar las instrucciones Update posicionada y DELETE](../../../odbc/reference/appendixes/processing-positioned-update-and-delete-statements.md).  
  
 Si el controlador no admite cursores estáticos, una aplicación que trabaje con la biblioteca de cursores debe llamar a **SQLSetPos** solo en un conjunto de filas capturado por **SQLExtendedFetch** o **SQLFetchScroll**, no por **SQLFetch**. La biblioteca de cursores implementa **SQLExtendedFetch** y **SQLFetchScroll** realizando llamadas repetidas de **SQLFetch** (con un tamaño de conjunto de filas de 1) en el controlador. La biblioteca de cursores pasa las llamadas a **SQLFetch**, por otro lado, a través del controlador. Si se llama a **SQLSetPos** en un conjunto de filas de varias filas capturado por **SQLFetch** cuando el controlador no admite cursores estáticos, se producirá un error en la llamada porque **SQLSetPos** no funciona con los cursores de solo avance. Esto ocurrirá incluso si una aplicación ha llamado correctamente a **SQLSetStmtAttr** para establecer SQL_ATTR_CURSOR_TYPE en SQL_CURSOR_STATIC, que la biblioteca de cursores admite aunque el controlador no admita cursores estáticos.
