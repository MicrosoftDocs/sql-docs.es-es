---
description: 'C a SQL: Intervalos de mes y año'
title: 'C a SQL: intervalos de Year-Month | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from c to SQL types [ODBC], year-month intervals
- intervals [ODBC], converting
- year-month intervals [ODBC]
- data conversions from C to SQL types [ODBC], year-month intervals
ms.assetid: a0eb7b55-9db0-4375-9210-bddec4593880
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b0748585beeae18a0b159cf131a67b4b5ea3cdd0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158581"
---
# <a name="c-to-sql-year-month-intervals"></a>C a SQL: Intervalos de mes y año
Los identificadores de los tipos de datos de ODBC C Interval de año-mes son:  
  
 SQL_C_INTERVAL_MONTH SQL_C_INTERVAL_YEAR SQL_C_INTERVAL_YEAR_TO_MONTH  
  
 En la tabla siguiente se muestran los tipos de datos de SQL de ODBC en los que se pueden convertir los datos del intervalo de año-mes. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de C a tipos de datos de SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificador de tipo de SQL|Prueba|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR [a]<br /><br /> SQL_VARCHAR [a]<br /><br /> SQL_LONGVARCHAR [a]|Longitud de bytes de columna >= longitud de bytes de caracteres<br /><br /> Longitud de bytes de columna < longitud de bytes de caracteres [a]<br /><br /> El valor de los datos no es un literal de intervalo válido|N/D<br /><br /> 22001<br /><br /> 22015|  
|SQL_WCHAR [a]<br /><br /> SQL_WVARCHAR [a]<br /><br /> SQL_WLONGVARCHAR [a]|Longitud de caracteres de columna >= longitud de caracteres de datos<br /><br /> Longitud de caracteres de columna < longitud de caracteres de los datos [a]<br /><br /> El valor de los datos no es un literal de intervalo válido|N/D<br /><br /> 22001<br /><br /> 22015|  
|SQL_TINYINT [b]<br /><br /> SQL_SMALLINT [b]<br /><br /> SQL_INTEGER [b]<br /><br /> SQL_BIGINT [b]<br /><br /> SQL_NUMERIC [b]<br /><br /> SQL_DECIMAL [b]|La conversión de un intervalo de un solo campo no ha dado como resultado el truncamiento de dígitos enteros<br /><br /> La conversión provocó el truncamiento de dígitos enteros|N/D<br /><br /> 22003|  
|SQL_INTERVAL_MONTH<br /><br /> SQL_INTERVAL_YEAR<br /><br /> SQL_INTERVAL_YEAR_TO_MONTH|El valor de los datos se convirtió sin truncamiento de ningún campo<br /><br /> Uno o más campos de valor de datos se han truncado durante la conversión|N/D<br /><br /> 22015|  
  
 [a] todos los tipos de datos de intervalo de C se pueden convertir en un tipo de datos de caracteres.  
  
 [b] si el campo de tipo de la estructura Interval es tal que el intervalo es un campo único (SQL_YEAR o SQL_MONTH), el tipo de intervalo C se puede convertir a cualquier valor numérico exacto (SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_DECIMAL o SQL_NUMERIC).  
  
 La conversión predeterminada de un tipo de intervalo C es el tipo SQL de intervalo del año correspondiente.  
  
 El controlador omite el valor de longitud/indicador al convertir los datos del tipo de datos Interval C y da por supuesto que el tamaño del búfer de datos es el tamaño del tipo de datos Interval de C. El valor de longitud/indicador se pasa en el argumento *StrLen_or_Ind* de **SQLPutData** y en el búfer especificado con el argumento *StrLen_or_IndPtr* en **SQLBindParameter**. El búfer de datos se especifica con el argumento *DataPtr* en **SQLPutData** y el argumento *ParameterValuePtr* en **SQLBindParameter**.
