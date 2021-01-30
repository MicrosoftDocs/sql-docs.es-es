---
description: 'C a SQL: Timestamp'
title: 'C a SQL: marca de tiempo | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from C to SQL types [ODBC], timestamp
- timestamp data type [ODBC]
- converting data from c to SQL types [ODBC], timestamp
ms.assetid: 0e08bfff-68f9-4648-9558-09b57fea08ad
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 54632816fb7013639e1cdb761fbc8df53c461a0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158590"
---
# <a name="c-to-sql-timestamp"></a>C a SQL: Timestamp
El identificador del tipo de datos de marca de tiempo de ODBC C es:  
  
 SQL_C_TYPE_TIMESTAMP  
  
 En la tabla siguiente se muestran los tipos de datos de ODBC SQL en los que se pueden convertir los datos de marca de tiempo de C. Para obtener una explicación de las columnas y los términos de la tabla, vea [convertir datos de C a tipos de datos de SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificador de tipo de SQL|Prueba|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Longitud de bytes de columna >= longitud de bytes de caracteres<br /><br /> 19 <= longitud de bytes de columna < longitud de bytes de caracteres<br /><br /> Longitud de bytes de columna < 19<br /><br /> El valor de los datos no es una marca de tiempo válida|N/D<br /><br /> 22001<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Longitud de caracteres de columna >= longitud de caracteres de datos<br /><br /> 19 <= longitud de caracteres de columna < longitud de caracteres de los datos<br /><br /> Longitud de caracteres de columna < 19<br /><br /> El valor de los datos no es una marca de tiempo válida|N/D<br /><br /> 22001<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_DATE|Los campos de hora son cero<br /><br /> Los campos de hora son distintos de cero<br /><br /> El valor de los datos no contiene una fecha válida.|N/D<br /><br /> 22008<br /><br /> 22007|  
|SQL_TYPE_TIME|Los campos de fracciones de segundo son cero [a]<br /><br /> Los campos de fracciones de segundo son distintos de cero [a]<br /><br /> El valor de los datos no contiene una hora válida|N/D<br /><br /> 22008<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|Los campos de fracciones de segundo no se truncan<br /><br /> Los campos de fracciones de segundo se truncan<br /><br /> El valor de los datos no es una marca de tiempo válida|N/D<br /><br /> 22008<br /><br /> 22007|  
  
 [a] se omiten los campos de fecha de la estructura de marca de tiempo.  
  
 Para obtener información sobre qué valores son válidos en una estructura de SQL_C_TIMESTAMP, vea [tipos de datos de C](../../../odbc/reference/appendixes/c-data-types.md), anteriormente en este apéndice.  
  
 Cuando los datos de marca de tiempo de C se convierten en datos de SQL de caracteres, los datos de caracteres resultantes se encontraban en "*yyyy* - *mm* - *DD* *HH*:*mm*:*SS*[.*f...*] ".  
  
 El controlador omite el valor de longitud/indicador al convertir los datos del tipo de datos timestamp C y da por supuesto que el tamaño del búfer de datos es el tamaño del tipo de datos timestamp de C. El valor de longitud/indicador se pasa en el argumento *StrLen_or_Ind* de **SQLPutData** y en el búfer especificado con el argumento *StrLen_or_IndPtr* en **SQLBindParameter**. El búfer de datos se especifica con el argumento *DataPtr* en **SQLPutData** y el argumento *ParameterValuePtr* en **SQLBindParameter**.
