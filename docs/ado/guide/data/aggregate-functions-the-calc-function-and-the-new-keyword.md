---
description: Las funciones de agregado, la función CALC y la palabra clave NEW
title: Funciones de agregado, la función CALC y la palabra clave NEW | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], functions
- CALC function [ADO]
- NEW keyword [ADO]
- aggregate functions [ADO]
ms.assetid: 0590b466-2a36-49a2-868e-028ef5e49394
author: rothja
ms.author: jroth
ms.openlocfilehash: 458a0f051e9bbf21c04b0c84125a1c438693f00d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037841"
---
# <a name="aggregate-functions-the-calc-function-and-the-new-keyword"></a>Las funciones de agregado, la función CALC y la palabra clave NEW
La forma de datos admite las siguientes funciones. El nombre asignado al capítulo que contiene la columna en la que se va a operar es el *alias de capítulo*.  
  
 Un alias de capítulo puede ser completo, que consta de un nombre de columna de capítulo que conduce al segmento que contiene el *nombre de columna,* separados por puntos. Por ejemplo, si el capítulo principal, chap1, contiene un capítulo secundario, chap2, que tiene una columna amount, AMT, el nombre completo sería chap1. chap2. AMT.  
  
|Funciones de agregado|Descripción|  
|-------------------------|-----------------|  
|SUM (*Chapter-alias*.*nombre de columna*)|Calcula la suma de todos los valores de la columna especificada.|  
|AVG (*Chapter-alias*.*nombre de columna*)|Calcula el promedio de todos los valores de la columna especificada.|  
|MAX (*Chapter-alias*.*nombre de columna*)|Calcula el valor máximo de la columna especificada.|  
|MIN (*Chapter-alias*.*nombre de columna*)|Calcula el valor mínimo de la columna especificada.|  
|COUNT (*Chapter-alias*[.*nombre de columna*])|Cuenta el número de filas del alias especificado. Si se especifica una columna, solo se incluyen en el recuento las filas cuya columna no sea NULL.|  
|STDEV (*Chapter-alias*.*nombre de columna*)|Calcula la desviación estándar de la columna especificada.|  
|ANY (*alias de capítulo*.*nombre de columna*)|Valor de la columna especificada. ANY tiene un valor de predicción solo cuando el valor de la columna es el mismo para todas las filas del capítulo.<br /><br /> **Nota:** Si la columna no contiene el mismo valor para todas las filas del capítulo, el comando SHAPE devuelve arbitrariamente uno de los valores para que sea el valor de la función ANY.|  
  
|Expresión calculada|Descripción|  
|---------------------------|-----------------|  
|CALC (*expresión*)|Calcula una expresión arbitraria, pero solo en la fila del conjunto de **registros** que contiene la función Calc. Se permite cualquier expresión que use estas [funciones Visual Basic para aplicaciones (VBA)](./visual-basic-for-applications-functions.md) .|  
  
|NEW (palabra clave)|Descripción|  
|-----------------|-----------------|  
|NUEVO *campo-tipo* [(*ancho* &#124; *escala* &#124; *precisión* &#124; *error* [, *escala* &#124; *error*])]|Agrega una columna vacía del tipo especificado al conjunto de **registros**.|  
  
 El *tipo de campo* que se pasa con la palabra clave New puede ser cualquiera de los siguientes tipos de datos.  
  
|OLE DB tipos de datos|Tipo de datos ADO equivalente (s)|  
|-----------------------|-----------------------------------|  
|DBTYPE_BSTR|adBSTR|  
|DBTYPE_BOOL|adBoolean|  
|DBTYPE_DECIMAL|adDecimal|  
|DBTYPE_UI1|adUnsignedTinyInt|  
|DBTYPE_I1|adTinyInt|  
|DBTYPE_UI2|adUnsignedSmallInt|  
|DBTYPE_UI4|adUnsignedInt|  
|DBTYPE_I8|adBigInt|  
|DBTYPE_UI8|adUnsignedBigInt|  
|DBTYPE_GUID|adGuid|  
|DBTYPE_BYTES|adBinary, AdVarBinary, adLongVarBinary|  
|DBTYPE_STR|adChar, advarchar, adLongVarChar|  
|DBTYPE_WSTR|adWChar, adVarWChar, adLongVarWChar|  
|DBTYPE_NUMERIC|adNumeric|  
|DBTYPE_DBDATE|adDBDate|  
|DBTYPE_DBTIME|adDBTime|  
|DBTYPE_DBTIMESTAMP|adDBTimeStamp|  
|DBTYPE_VARNUMERIC|adVarNumeric|  
|DBTYPE_FILETIME|adFileTime|  
|DBTYPE_ERROR|adError|  
  
 Cuando el nuevo campo es de tipo decimal (en OLE DB, DBTYPE_DECIMAL, o en ADO, adDecimal), debe especificar los valores de precisión y escala.  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de forma de datos](./data-shaping-example.md)   
 [Gramática de forma formal](./formal-shape-grammar.md)   
 [Comandos Shape en General](./shape-commands-in-general.md)