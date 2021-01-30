---
description: SQL_C_TCHAR
title: SQL_C_TCHAR | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sql_c_tchar [ODBC]
- pseudo-type identifiers [ODBC], SQL_C_TCHAR
- data types [ODBC], pseudo-type identifiers
ms.assetid: 9e27c8bd-ee15-4ce9-b70a-34cf1bf16f4c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a399570a15f04d8ace61664a445c5283d629bf6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193046"
---
# <a name="sql_c_tchar"></a>SQL_C_TCHAR
En realidad, el identificador de tipo SQL_C_TCHAR no identifica un tipo de datos. se trata de una macro que existe en el archivo de encabezado para la conversión Unicode. Se reemplaza por SQL_C_CHAR o SQL_C_WCHAR en función de la configuración de la **#define** Unicode. Resulta útil para una aplicación que transfiera datos de caracteres compilados como una aplicación ANSI y Unicode.
