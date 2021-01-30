---
description: SQLRowCount (biblioteca de cursores)
title: SQLRowCount (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLRowCount function [ODBC], Cursor Library
ms.assetid: 781cf5a5-325e-4523-8633-d96d9e98277c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9a8efc6fdef2a3bef563a5289acdef55958a03be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202682"
---
# <a name="sqlrowcount-cursor-library"></a>SQLRowCount (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLRowCount** en la biblioteca de cursores. Para obtener información general sobre **SQLRowCount**, consulte la [función SQLRowCount](../../../odbc/reference/syntax/sqlrowcount-function.md).  
  
 Cuando una aplicación llama a **SQLRowCount** con la instrucción asociada al cursor, la biblioteca de cursores devuelve el número de filas de datos que ha recuperado del controlador.  
  
 Cuando una aplicación llama a **SQLRowCount** con la instrucción asociada a una instrucción UPDATE o DELETE posicionada, la biblioteca de cursores devuelve el número de filas afectadas por la instrucción.  
  
 Cuando una aplicación llama a **SQLRowCount** después de una instrucción **Select** , la biblioteca de cursores devuelve-1.
