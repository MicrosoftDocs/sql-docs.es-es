---
description: SQLGetFunctions (biblioteca de cursores)
title: SQLGetFunctions (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetFunctions function [ODBC], Cursor Library
ms.assetid: 931acd12-4eb6-4a78-9a77-157a18a9a2d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9414aa4d2be551ab3b6a44193f6b6c17e2deb145
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202765"
---
# <a name="sqlgetfunctions-cursor-library"></a>SQLGetFunctions (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLGetFunctions** en la biblioteca de cursores. Para obtener información general sobre **SQLGetFunctions**, consulte la [función SQLGetFunctions](../../../odbc/reference/syntax/sqlgetfunctions-function.md).  
  
 Cuando se llama a **SQLGetFunctions**, la biblioteca de cursores devuelve que admite **SQLExtendedFetch**, **SQLFetchScroll**, **SQLSetPos** y **SQLSetScrollOptions**, además de las funciones admitidas por el controlador.
