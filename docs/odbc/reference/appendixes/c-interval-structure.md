---
description: Estructura de intervalo de C
title: Estructura de intervalo de C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- interval data type [ODBC], structure
- C data types [ODBC], interval
ms.assetid: 52b42b56-50aa-4ce6-8d79-0963c7a71437
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e470d0e30864c953fa3937725bcd5597bf542f41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212440"
---
# <a name="c-interval-structure"></a>Estructura de intervalo de C
Cada uno de los tipos de datos de intervalo de C que se enumeran en la sección [tipos de datos de c](../../../odbc/reference/appendixes/c-data-types.md) utiliza la misma estructura para contener los datos de intervalo. Cuando se llama a **SQLFetch**, **SQLFetchScroll** o **SQLGetData** , el controlador devuelve datos en la estructura de SQL_INTERVAL_STRUCT, utiliza el valor especificado por la aplicación para los tipos de datos de C (en la llamada a **SQLBindCol**, **SQLGetData** o **SQLBindParameter**) para interpretar el contenido de SQL_INTERVAL_STRUCT y rellena el campo de *Interval_type* de la estructura con el valor de *enumeración* correspondiente al tipo C. Tenga en cuenta que los controladores no leen el campo *interval_type* para determinar el tipo de intervalo; recuperan el valor del campo descriptor de SQL_DESC_CONCISE_TYPE. Cuando la estructura se usa para los datos de parámetros, el controlador usa el valor especificado por la aplicación en el campo SQL_DESC_CONCISE_TYPE de APD para interpretar el contenido de SQL_INTERVAL_STRUCT, incluso si la aplicación establece el valor del campo *interval_type* en un valor diferente.  
  
 Esta estructura se define de la siguiente manera:  
  
```  
typedef struct tagSQL_INTERVAL_STRUCT  
{  
   SQLINTERVAL interval_type;   
   SQLSMALLINT interval_sign;  
   union {  
         SQL_YEAR_MONTH_STRUCT   year_month;  
         SQL_DAY_SECOND_STRUCT   day_second;  
         } intval;  
} SQL_INTERVAL_STRUCT;  
typedef enum   
{  
   SQL_IS_YEAR = 1,  
   SQL_IS_MONTH = 2,  
   SQL_IS_DAY = 3,  
   SQL_IS_HOUR = 4,  
   SQL_IS_MINUTE = 5,  
   SQL_IS_SECOND = 6,  
   SQL_IS_YEAR_TO_MONTH = 7,  
   SQL_IS_DAY_TO_HOUR = 8,  
   SQL_IS_DAY_TO_MINUTE = 9,  
   SQL_IS_DAY_TO_SECOND = 10,  
   SQL_IS_HOUR_TO_MINUTE = 11,  
   SQL_IS_HOUR_TO_SECOND = 12,  
   SQL_IS_MINUTE_TO_SECOND = 13  
} SQLINTERVAL;  
  
typedef struct tagSQL_YEAR_MONTH  
{  
   SQLUINTEGER year;  
   SQLUINTEGER month;   
} SQL_YEAR_MONTH_STRUCT;  
  
typedef struct tagSQL_DAY_SECOND  
{  
   SQLUINTEGER day;  
   SQLUINTEGER hour;  
   SQLUINTEGER minute;  
   SQLUINTEGER second;  
   SQLUINTEGER fraction;  
} SQL_DAY_SECOND_STRUCT;  
```  
  
 En el campo *interval_type* del SQL_INTERVAL_STRUCT se indica a la aplicación qué estructura se mantiene en la Unión y también qué miembros de la estructura son pertinentes. El campo *interval_sign* tiene el valor SQL_FALSE si el campo inicial del intervalo es sin signo. Si se SQL_TRUE, el campo inicial es negativo. El valor del propio campo inicial siempre es sin signo, independientemente del valor de *interval_sign*. El campo *interval_sign* actúa como un bit de signo.
