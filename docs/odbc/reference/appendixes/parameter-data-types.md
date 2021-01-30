---
description: Tipos de datos de parámetro
title: Tipos de datos de parámetros | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], parameters
- parameter data type [ODBC]
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: fd7e99d8-d26a-408c-9733-6ffccde99f75
author: David-Engel
ms.author: v-daenge
ms.reviewer: ''
ms.openlocfilehash: fbc01ee7f6c4e7cd3ca7dbe4deb6e78e5ff78449
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160829"
---
# <a name="parameter-data-types"></a>Tipos de datos de parámetro
Aunque cada parámetro especificado con **SQLBindParameter** se define mediante un tipo de datos de SQL, los parámetros de una instrucción SQL no tienen ningún tipo de datos intrínseco. Por lo tanto, los marcadores de parámetros se pueden incluir en una instrucción SQL solo si sus tipos de datos se pueden inferir de otro operando en la instrucción. Por ejemplo, en una expresión aritmética como? + COLUMN1, el tipo de datos del parámetro se puede inferir a partir del tipo de datos de la columna con nombre representada por COLUMN1. Una aplicación no puede usar un marcador de parámetro si no se puede determinar el tipo de datos.  
  
 En la tabla siguiente se describe cómo se determina un tipo de datos para varios tipos de parámetros, de acuerdo con SQL-92. Para obtener una especificación más completa sobre cómo deducir el tipo de parámetro cuando se usan otras cláusulas SQL, vea la especificación SQL-92.  
  
|Ubicación del parámetro|Tipo de datos asumido|  
|---------------------------|-----------------------|  
|Un operando de un operador aritmético binario o de comparación|Igual que el otro operando|  
|Primer operando de una cláusula **between**|Igual que el segundo operando|  
|El segundo o tercer operando de una cláusula **between**|Igual que el primer operando|  
|Una expresión utilizada con **en**|Igual que el primer valor o la columna resultado de la subconsulta|  
|Un valor utilizado con **en**|Igual que la expresión o el primer valor si hay un marcador de parámetro en la expresión|  
|Un valor de patrón usado con **like**|VARCHAR|  
|Un valor de actualización usado con **Update**|Igual que la columna de actualización|
