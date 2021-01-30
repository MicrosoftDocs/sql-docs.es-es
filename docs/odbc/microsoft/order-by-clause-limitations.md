---
description: ORDEN por cláusula limitaciones
title: Limitaciones de la cláusula ORDER BY | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, ORDER BY clause limitations
- ORDER BY clause limitations [ODBC]
ms.assetid: fd4ddc7c-9c7e-4a0c-a781-e5427dfb2e18
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 51ec06b42e3aca1c48ef243c7032608d28b21a08
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158201"
---
# <a name="order-by-clause-limitations"></a>ORDEN por cláusula limitaciones
Si una instrucción SELECT contiene una cláusula GROUP BY y una cláusula ORDER BY, la cláusula ORDER BY solo puede contener una columna en el conjunto de resultados o una expresión en la cláusula GROUP BY.
