---
description: SQLSetScrollOptions (biblioteca de cursores)
title: SQLSetScrollOptions (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetScrollOptions function [ODBC], Cursor Library
ms.assetid: c5c0ac6d-a6c1-4077-8186-1644df1944f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: eaba3884e90883cded418acc46d73cc1c0dfba88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202610"
---
# <a name="sqlsetscrolloptions-cursor-library"></a>SQLSetScrollOptions (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLSetScrollOptions** en la biblioteca de cursores. Para obtener información general sobre **SQLSetScrollOptions**, consulte la [función SQLSetScrollOptions](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md).  
  
 La biblioteca de cursores admite **SQLSetScrollOptions** solo para la compatibilidad con versiones anteriores; en su lugar, las aplicaciones deben usar los atributos de instrucción SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE y SQL_ATTR_ROW_ARRAY_SIZE.
