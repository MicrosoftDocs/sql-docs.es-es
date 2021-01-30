---
description: Sintaxis de literales de intervalo
title: Sintaxis de los literales de intervalo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- literals [ODBC], interval
- interval literals [ODBC]
- ODBC literals [ODBC], interval
ms.assetid: 2f2d22c1-51d6-4055-9f5a-53bc31e9fea0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5a58ddbde1be09f16fec79fcff13147a805febfd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208523"
---
# <a name="interval-literal-syntax"></a>Sintaxis de literales de intervalo
La sintaxis siguiente se usa para los literales de intervalo en ODBC.  
  
 *Interval-literal:: = Interval* [+*&#124;*-] Interval *-cadena Interval-Qualifier*  
  
 *Interval-String* :: = *Quote* { *year-month-literal* &#124; *Day-Time-literal* } *Quote*  
  
 *Year-month-literal* :: = *years-Value* &#124; [*years-Value* -] *months-Value*  
  
 *Day-Time-literal* :: = *day-time-Interval* &#124; *tiempo-intervalo*  
  
 *Day-Time-Interval* :: = *Days-Value* [*hours-Value* [:*minutes-Value*[:*seconds-Value*]]]  
  
 *Time-Interval* :: = *hours-Value* [:*minutes-Value* [:*seconds-Value* ]]  
  
 &#124; *minutos-valor* [:*seconds-valor* ]  
  
 &#124; *segundos-valor*  
  
 *years-Value* :: = *DateTime-Value*  
  
 *months-Value* :: = *DateTime-Value*  
  
 *Days-Value* :: = *DateTime-Value*  
  
 *hours-Value* :: = *DateTime-Value*  
  
 *minutes-Value* :: = *DateTime-Value*  
  
 *seconds-Value* :: = *seconds-Integer-Value* [. [ *segundos-fracción*] ]  
  
 *segundos: entero-valor* :: = *sin signo: entero*  
  
 *segundos-fracción* :: = *sin signo-entero*  
  
 *DateTime-Value* :: = *unsigned-Integer*  
  
 *Interval-Qualifier* :: = *Start-Field* TO *End-* Field &#124; *campo de fecha y hora único*  
  
 *Start-Field* :: = *nonsecond-DateTime-Field* [(*Interval-interlineado-campo-precisión* )]  
  
 *campo final* :: = *non-Second-datetime-Field* &#124; segundo [(*Interval-fraccionario-seconds-precisión*)]  
  
 *Single-DateTime-Field* :: = *nonsecond-DateTime-Field* [(*Interval-inicial-field-Precision*)] &#124; segundo [(*Interval-inicial-precisión-campo* [, (*Interval-fraccionario-seconds-precisión*)]  
  
 *DateTime-Field* :: = *nonsecond-datetime-Field* &#124; Second  
  
 *no Second-DateTime-Field* :: = Year &#124; month &#124; Day &#124; hour &#124; minuto  
  
 *Interval-fraccionario-segundos-precisión* :: = *sin signo: entero*  
  
 *Interval-inicial-Field-Precision* :: = *unsigned-Integer*  
  
 *Quote* :: = '  
  
 *unsigned-Integer* :: = *digit...*
