---
description: Función CAST de SQL-92
title: SQL-92 CAST (función) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], SQL-92 functions
- SQL-92 functions [ODBC]
- CAST function [ODBC]
ms.assetid: 982f09e5-8205-41b9-98b3-8f898e24743f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f8cc377f6f87c8cff9728cbf3d211345b0a0a20f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187117"
---
# <a name="sql-92-cast-function"></a>Función CAST de SQL-92
La función **Cast** definida en SQL-92 es equivalente a la función **Convert** definida en ODBC. La sintaxis de las funciones equivalentes es la siguiente:  
  
```  
{ fn CONVERT (value-exp, data-type) } /* ODBC  
CAST (value-exp AS data-type) /* SQL92  
```  
  
 La función **Cast** de SQL-92 asigna los tipos de datos que se pueden convertir a los otros tipos de datos. (Para obtener más información, vea la especificación SQL-92). La función **Cast** se admite en el nivel de transición FIPS.  
  
 Una aplicación puede determinar la compatibilidad con la función **Cast** de la siguiente manera:  
  
1.  Llame a **SQLGetInfo** con el tipo de información SQL_SQL_CONFORMANCE. Si el valor devuelto para el tipo de información es SQL_SC_FIPS127_2_TRANSITIONAL, SQL_SC_SQL92_INTERMEDIATE o SQL_SC_SQL92_FULL, se admite la función **Cast** .  
  
2.  Si el valor devuelto del tipo de información SQL_SQL_CONFORMANCE es SQL_SC_ENTRY_LEVEL o 0, llame a **SQLGetInfo** con el tipo de información SQL_SQL92_VALUE_EXPRESSIONS. Si se establece el bit SQL_SVE_CAST, se admite la función **Cast** .
