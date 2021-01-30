---
description: SQLExtendedFetch (biblioteca de cursores)
title: SQLExtendedFetch (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLExtendedFetch function [ODBC], Cursor Library
ms.assetid: 06fbf06f-127b-475c-b636-7b784918475d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 266d408b765ce5025a7f275c87cc6b548e6e49bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202865"
---
# <a name="sqlextendedfetch-cursor-library"></a>SQLExtendedFetch (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLExtendedFetch** en la biblioteca de cursores. Para obtener información general sobre **SQLExtendedFetch**, consulte la [función SQLExtendedFetch](../../../odbc/reference/syntax/sqlextendedfetch-function.md).  
  
 La biblioteca de cursores implementa **SQLExtendedFetch** mediante una llamada repetida a **SQLFetch** en el controlador.  
  
 La biblioteca de cursores admite la llamada a **SQLExtendedFetch** con un *FetchOrientation* de SQL_FETCH_BOOKMARK.  
  
 Cuando se usa la biblioteca de cursores, las llamadas a **SQLExtendedFetch** no se pueden mezclar con llamadas a **SQLFetchScroll** o **SQLFetch**.
