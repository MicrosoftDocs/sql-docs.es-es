---
description: Marcadores de longitud fija
title: Marcadores de Fixed-Length | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- backward compatibility [ODBC], bookmarks
- bookmarks [ODBC]
- compatibility [ODBC], bookmarks
- fixed-length bookmarks [ODBC]
ms.assetid: cbd8185e-fb03-408f-b80b-1a2e164534fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d5eb7acd97a83be0d150cec8b19a5a3e25a7bf6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194811"
---
# <a name="fixed-length-bookmarks"></a>Marcadores de longitud fija
Si un controlador ODBC *3. x* debe trabajar con una aplicación ODBC *2. x* que usa marcadores de longitud fija, el controlador debe admitir lo siguiente:  
  
-   SQL_UB_ON como un valor para la opción de la instrucción SQL_USE_BOOKMARKS. (SQL_UB_ON está en desuso en ODBC *3. x*).  
  
-   La opción de instrucción SQL_GET_BOOKMARK.
