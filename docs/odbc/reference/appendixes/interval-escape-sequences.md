---
description: Secuencias de escape de intervalo
title: Secuencias de escape de intervalo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- interval literals [ODBC]
- escape sequences [ODBC], interval
- ODBC escape sequences [ODBC], interval
ms.assetid: 303e8dab-8f13-4fa5-857f-15cc1f75bdd6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0badac832ee4cf4e29f148dbd39a7989536b9c29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176501"
---
# <a name="interval-escape-sequences"></a>Secuencias de escape de intervalo
ODBC utiliza secuencias de escape para los literales de intervalo. La sintaxis de esta secuencia de escape es la siguiente:  
  
 {*Interval-literal*}  
  
 Para obtener la sintaxis BNF de *Interval-literal*, consulte la sección [Sintaxis de literales de intervalo](../../../odbc/reference/appendixes/interval-literal-syntax.md) más adelante en este apéndice.  
  
 Se admite la secuencia de escape de literal de intervalo si el origen de datos admite los tipos de datos de intervalo. Una aplicación debe llamar a **SQLGetTypeInfo** para determinar si se admiten estos tipos de datos.
