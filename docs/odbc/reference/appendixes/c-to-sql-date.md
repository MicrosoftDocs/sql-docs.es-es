---
description: 'C a SQL: Date'
title: 'C a SQL: fecha | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- date data type [ODBC]
- converting data from c to SQL types [ODBC], date
- data conversions from C to SQL types [ODBC], date
ms.assetid: bea087d3-911f-418b-b483-d2b5b334da19
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a0724309642b8a6dc640b6159715927544d74733
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207845"
---
# <a name="c-to-sql-date"></a>C a SQL: Date
El identificador para el tipo de datos C de ODBC de fecha es:  
  
 SQL_C_TYPE_DATE  
  
 En la tabla siguiente se muestran los tipos de datos de ODBC SQL en los que se pueden convertir datos de fecha C. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de C a tipos de datos de SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificador de tipo de SQL|Prueba|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Longitud de bytes de columna >= 10<br /><br /> Longitud de bytes de columna < 10<br /><br /> El valor de los datos no es una fecha válida|N/D<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Longitud de caracteres de columna >= 10<br /><br /> Longitud de caracteres de columna < 10<br /><br /> El valor de los datos no es una fecha válida|N/D<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_DATE|El valor de los datos es una fecha válida<br /><br /> El valor de los datos no es una fecha válida|N/D<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|El valor de los datos es una fecha válida [a]<br /><br /> El valor de los datos no es una fecha válida|N/D<br /><br /> 22007|  
  
 [a] la parte de hora de la marca de tiempo se establece en cero.  
  
 Para obtener información sobre qué valores son válidos en una estructura de SQL_C_TYPE_DATE, vea [tipos de datos de C](../../../odbc/reference/appendixes/c-data-types.md), anteriormente en este apéndice.  
  
 Cuando los datos de la fecha C se convierten en datos SQL de caracteres, los datos de caracteres resultantes se escriben en el formato "*AAAA* - *mm* - *DD*".  
  
 El controlador omite el valor de longitud/indicador al convertir los datos del tipo de datos de fecha C y da por supuesto que el tamaño del búfer de datos es el tamaño del tipo de datos de fecha C. El valor de longitud/indicador se pasa en el argumento *StrLen_or_Ind* de **SQLPutData** y en el búfer especificado con el argumento *StrLen_or_IndPtr* en **SQLBindParameter**. El búfer de datos se especifica con el argumento *DataPtr* en **SQLPutData** y el argumento *ParameterValuePtr* en **SQLBindParameter**.
