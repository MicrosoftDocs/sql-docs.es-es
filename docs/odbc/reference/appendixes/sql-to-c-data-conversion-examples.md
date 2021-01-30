---
description: SQL a los ejemplos de conversión de datos de C
title: Ejemplos de conversión de datos de SQL a C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from SQL to C types [ODBC], examples
- converting data from SQL to C types [ODBC], examples
ms.assetid: 0190c76c-7f9b-42f4-be9d-cef7284840fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f75b048dd9e40a0bef20640b73322c5c3b6e663
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203059"
---
# <a name="sql-to-c-data-conversion-examples"></a>SQL a los ejemplos de conversión de datos de C

Los ejemplos que se muestran en la tabla siguiente muestran cómo el controlador convierte los datos SQL en datos de C:  
  
|Tipo SQL<br /><br /> identificador|SQL data<br /><br /> value|Tipo de C<br /><br /> identificador|Buffer<br /><br /> length|**TargetValuePtr*|SQLSTATE|  
|-----------------------------|------------------------|---------------------------|-----------------------|------------------------|--------------|  
|SQL_CHAR|abcdef|SQL_C_CHAR|7|abcdef\0 [a]|N/D|  
|SQL_CHAR|abcdef|SQL_C_CHAR|6|abcde\0 [a]|01004|  
|SQL_DECIMAL|1234,56|SQL_C_CHAR|8|1234,56 \ 0 [a]|N/D|  
|SQL_DECIMAL|1234,56|SQL_C_CHAR|5|1234 \ 0 [a]|01004|  
|SQL_DECIMAL|1234,56|SQL_C_CHAR|4|----|22003|  
|SQL_DECIMAL|1234,56|SQL_C_FLOAT|no se tiene en cuenta|1234,56|N/D|  
|SQL_DECIMAL|1234,56|SQL_C_SSHORT|no se tiene en cuenta|1234|01S07|  
|SQL_DECIMAL|1234,56|SQL_C_STINYINT|no se tiene en cuenta|----|22003|  
|SQL_DOUBLE|1,2345678|SQL_C_DOUBLE|no se tiene en cuenta|1,2345678|N/D|  
|SQL_DOUBLE|1,2345678|SQL_C_FLOAT|no se tiene en cuenta|1,234567|N/D|  
|SQL_DOUBLE|1,2345678|SQL_C_STINYINT|no se tiene en cuenta|1|N/D|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_CHAR|11|1992-12-31 \ 0 [a]|N/D|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_CHAR|10|-----|22003|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_TIMESTAMP|no se tiene en cuenta|1992, 12, 31, 0, 0, 0, 0 [b]|N/D|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23:45:55.12|SQL_C_CHAR|23|1992-12-31 23:45:55.12 \ 0 [a]|N/D|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23:45:55.12|SQL_C_CHAR|22|1992-12-31 23:45:55.1 \ 0 [a]|01004|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23:45:55.12|SQL_C_CHAR|18|----|22003|  
  
 [a] "\ 0" representa un byte de terminación null. El controlador siempre termina en NULL: finaliza SQL_C_CHAR datos.  
  
 [b] los números de esta lista son los números almacenados en los campos de la estructura TIMESTAMP_STRUCT.
