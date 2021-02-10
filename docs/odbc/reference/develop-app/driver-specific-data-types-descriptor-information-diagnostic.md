---
description: Tipos de datos específicos del controlador, Descriptor tipos, tipos de información, tipos de diagnóstico y atributos
title: 'Tipos de Driver-Specific: datos, descriptores, información, diagnóstico | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver-specific diagnostic values [ODBC]
- diagnostic information [ODBC], driver-specific values
- ODBC drivers [ODBC], driver-specific diagnostic values
ms.assetid: ad4c76d3-5191-4262-b47c-5dd1d19d1154
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03cd6b0ed9a424a161f88f4bd525941895d3d201
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062570"
---
# <a name="driver-specific-data-types-descriptor-types-information-types-diagnostic-types-and-attributes"></a>Tipos de datos específicos del controlador, Descriptor tipos, tipos de información, tipos de diagnóstico y atributos
Los controladores pueden asignar valores específicos del controlador para lo siguiente:  
  
-   **Indicadores de tipo de datos SQL** Se usan en *ParameterType* en **SQLBindParameter** y en *DataType* en **SQLGetTypeInfo** y se devuelven mediante **SQLColAttribute**, **SQLColumns**, **SQLDescribeCol**, **SQLGetTypeInfo**, **SQLDescribeParam**, **SQLProcedureColumns** y **SQLSpecialColumns**.  
  
-   **Campos de descriptor** Se usan en *FieldIdentifier* en **SQLColAttribute**, **SQLGetDescField** y **SQLSetDescField**.  
  
-   **Campos de diagnóstico** Se usan en *DiagIdentifier* en **SQLGetDiagField** y **SQLGetDiagRec**.  
  
-   **Tipos de información** Se usan en *InfoType* en **SQLGetInfo**.  
  
-   **Atributos de la conexión y de la instrucción** Se usan en el *atributo* en **SQLGetConnectAttr**, **SQLGetStmtAttr**, **SQLSetConnectAttr** y **SQLSetStmtAttr**.  
  
 Para cada uno de estos elementos, hay dos conjuntos de valores: los valores reservados para su uso por ODBC y los valores reservados para que los usen los controladores. Antes de implementar valores específicos del controlador, un escritor de controladores debe solicitar un valor para cada tipo, campo o atributo específico del controlador del grupo abierto. Para el nuevo desarrollo de controladores, use el intervalo descrito en la tabla siguiente. El administrador de controladores ODBC 3,8 no generará un error si se utiliza un valor desconocido que no está en el intervalo que se describe a continuación. Sin embargo, las versiones posteriores del administrador de controladores pueden generar un error si se reciben valores desconocidos que no están en el intervalo.  
  
 Cuando cualquiera de estos valores se pasa a una función ODBC, el controlador debe comprobar si el valor es válido. Los controladores devuelven SQLSTATE HYC00 (característica opcional no implementada) para los valores específicos del controlador que se aplican a otros controladores.  
  
 A partir de ODBC 3,8, los escritores de controladores pueden asignar atributos específicos del controlador dentro de un intervalo reservado.  
  
> [!NOTE]  
>  El administrador de controladores ODBC 3,8 no valida ni aplica estos intervalos por compatibilidad con versiones anteriores. Sin embargo, es posible que una versión futura del administrador de controladores las aplique.  
  
|Tipo de atributo|Tipo de datos de ODBC|Base de intervalo específico del controlador|Límite de intervalo específico del controlador|Constante ODBC para el intervalo de valores específicos del controlador|  
|--------------------|--------------------|---------------------------------|----------------------------------|---------------------------------------------------------|  
|Indicadores de tipo de datos SQL|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_SQL_TYPE_BASE|  
|Campos de descriptor|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_DESCRIPTOR_BASE|  
|Campos de diagnóstico|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_DIAGNOSTIC_BASE|  
|Tipos de información|SQLUSMALLINT|0x4000|0x7FFF|SQL_DRIVER_INFO_TYPE_BASE|  
|Atributos de conexión|SQLINTEGER|0x00004000|0x00007FFF|SQL_DRIVER_CONNECT_ATTR_BASE|  
|Atributos de instrucción|SQLINTEGER|0x00004000|0x00007FFF|SQL_DRIVER_STATEMENT_ATTR_BASE|  
  
> [!NOTE]  
>  Los tipos de datos específicos del controlador, los campos de descriptor, los campos de diagnóstico, los tipos de información, los atributos de instrucción y los atributos de conexión deben describirse en la documentación del controlador. Cuando cualquiera de estos valores se pasa a una función ODBC, el controlador debe comprobar si el valor es válido. Los controladores devuelven SQLSTATE HYC00 (característica opcional no implementada) para los valores específicos del controlador que se aplican a otros controladores.  
  
 Los valores base se definen para facilitar el desarrollo de controladores. Por ejemplo, los atributos de diagnóstico específicos del controlador se pueden definir con el siguiente formato:  
  
```  
SQL_DRIVER_DIAGNOSTIC_BASE+0, SQL_DRIVER_DIAGNOSTIC_BASE +1  
```
