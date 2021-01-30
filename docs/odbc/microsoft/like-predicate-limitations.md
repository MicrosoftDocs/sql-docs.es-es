---
description: Al igual que las limitaciones de predicado
title: COMO las limitaciones de predicados | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- LIKE predicate limitations [ODBC]
- ODBC SQL grammar, LIKE predicate limitations
ms.assetid: dbd39099-caf6-4c4c-9ad8-f6c63c1bd5e4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4925ff83d8dafc1cce7c0a58a17685fcf30371f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199901"
---
# <a name="like-predicate-limitations"></a>Al igual que las limitaciones de predicado
Si los datos de una columna tienen más de 255 caracteres, la comparación LIKE solo se basará en los primeros 255 caracteres.  
  
 Una LIKE utilizada en un procedimiento solo se admite con patrones constantes. Los controladores de base de datos de escritorio admiten SQL-92 como coincidencia de patrones.  
  
 No se admite el uso de una cláusula escape en un predicado LIKE.  
  
 No se debe realizar una comparación LIKE en una columna que contenga datos de un tipo de datos float o Numeric. Los resultados pueden ser imprevisibles. Para obtener más información, vea la *Guía del programador de Microsoft Jet motor de base de datos*.
