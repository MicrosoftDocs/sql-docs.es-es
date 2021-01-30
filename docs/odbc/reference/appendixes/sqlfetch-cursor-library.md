---
description: SQLFetch (biblioteca de cursores)
title: SQLFetch (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLFetch function [ODBC], Cursor Library
ms.assetid: 35a0d493-778b-4fb1-84ee-a13540e2fe0e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7e0ed0b94d4ad66c22ef1a29055c7a0144e4f941
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202855"
---
# <a name="sqlfetch-cursor-library"></a>SQLFetch (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLFetch** en la biblioteca de cursores. Para obtener información general sobre **SQLFetch**, vea [función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
 Cuando se usa la biblioteca de cursores, las llamadas a **SQLFetch** no se pueden mezclar con llamadas a **SQLFetchScroll** o **SQLExtendedFetch**.  
  
 Si se llama a **SQLFetch** con SQL_ATTR_ROW_ARRAY_SIZE establecido en un valor mayor que 1, la biblioteca de cursores pasará la llamada al controlador. Si el controlador es un ODBC 2. *x* , se omitirá el tamaño del conjunto de filas y la llamada a **SQLFetch** devolverá una sola fila de datos.  
  
 Si la biblioteca de cursores se usa con ODBC 2. el controlador *x* , un desplazamiento de enlace (tal y como se define en el atributo de instrucción SQL_ATTR_ROW_BIND_OFFSET_PTR), no se utiliza cuando se llama a **SQLFetch** .  
  
 Cuando se carga la biblioteca de cursores, una aplicación no puede llamar a **SQLFetch** para capturar columnas de marcador. La biblioteca de cursores pasa la llamada a **SQLFetch** a través del controlador, pero las llamadas de función para habilitar marcadores y enlazar la columna de marcador se interceptan mediante la biblioteca de cursores.
