---
description: Usar identificadores de tipo de datos
title: Usar identificadores de tipo de datos | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], identifiers
- identifiers [ODBC], data types
ms.assetid: 467e0c0c-a818-4737-8a24-3d8e15c7e162
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cd38f5ddb62a28bc3ec2658dca621769e13ab481
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202434"
---
# <a name="using-data-type-identifiers"></a>Usar identificadores de tipo de datos
Las aplicaciones usan identificadores de tipo de datos de dos maneras: para describir sus búferes en el controlador y para recuperar metadatos sobre el conjunto de resultados del controlador, de modo que puedan determinar qué tipo de búferes de C usar para almacenar los datos. Las aplicaciones llaman a las siguientes funciones para realizar estas tareas:  
  
-   **SQLBindParameter**, **SQLBindCol** y **SQLGetData** : para describir el tipo de datos C de los búferes de la aplicación.  
  
-   **SQLBindParameter** : para describir el tipo de datos SQL de los parámetros dinámicos.  
  
-   **SQLColAttribute** y **SQLDescribeCol** : para recuperar los tipos de datos SQL de las columnas del conjunto de resultados.  
  
-   **SQLDescribeParameter** : para recuperar los tipos de datos SQL de los parámetros.  
  
-   **SQLColumns**, **SQLProcedureColumns** y **SQLSpecialColumns** : para recuperar los tipos de datos SQL de diversa información de esquema  
  
-   **SQLGetTypeInfo** : para recuperar una lista de tipos de datos admitidos  
  
 Los identificadores de tipo de datos se almacenan en el campo SQL_DESC_CONCISE_TYPE de un descriptor. Las funciones de descriptor **SQLSetDescField** y **SQLSetDescRec** se pueden usar con los tipos adecuados para realizar las tareas enumeradas en la lista anterior. Para obtener más información, vea [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).
