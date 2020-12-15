---
description: Comportamiento mejorado de tipos de fecha y hora con versiones anteriores de SQL Server (ODBC)
title: Fecha y hora en las versiones de SQL (ODBC)
ms.custom: ''
ms.date: 12/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- date/time [ODBC], enhanced behavior with earlier SQL Server versions
ms.assetid: cd4e137f-dc5e-4df7-bc95-51fe18c587e0
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0d9f25706641a20a59c01d44b487ef692e9cdbb2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483387"
---
# <a name="enhanced-date-and-time-type-behavior-with-previous-sql-server-versions-odbc"></a>Comportamiento mejorado de tipos de fecha y hora con versiones anteriores de SQL Server (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  En este tema se describe el comportamiento esperado cuando una aplicación cliente que usa las características de fecha y hora mejoradas se comunica con una versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anterior a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], y cuando una aplicación cliente que usa Microsoft Data Access Components, Windows Data Access Components o una versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client anterior a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] envía los comandos a un servidor que admite las características de fecha y hora mejoradas.  
  
## <a name="down-level-client-behavior"></a>Comportamiento del cliente de nivel inferior  
 Las aplicaciones cliente que se compilaron mediante una versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antes de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Native Client ven los nuevos tipos de fecha y hora como columnas nvarchar. El contenido de la columna son las representaciones literales, como se describe en la sección "formatos de datos: cadenas y literales" de [compatibilidad de tipos de datos con las mejoras de fecha y hora de ODBC](../../relational-databases/native-client-odbc-date-time/data-type-support-for-odbc-date-and-time-improvements.md). El tamaño de columna es la longitud máxima literal de la precisión de fracciones de segundos especificada para la columna.  
  
 Las API de catálogo devolverán metadatos coherentes con el código del tipo de datos de nivel inferior que se devuelve al cliente (por ejemplo, nvarchar) y la representación asociada de nivel inferior (por ejemplo, el formato de literal adecuado). Sin embargo, el nombre del tipo de datos devuelto será el nombre del tipo real de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
 Los metadatos de la instrucción devueltos por SQLDescribeCol, SQLDescribeParam, SQGetDescField y SQLColAttribute devolverán metadatos coherentes con el tipo de nivel inferior en todos los aspectos, incluido el nombre del tipo. Un ejemplo de este tipo de nivel inferior es **nvarchar**.  
  
 Cuando una aplicación cliente de nivel inferior se ejecuta en un [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] servidor (o posterior) en el que se han realizado cambios de esquema en los tipos de fecha y hora, el comportamiento esperado es el siguiente:  
  
|Tipo de SQL Server 2005|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]Tipo de  (o posterior)|Tipo de cliente ODBC|Conversión del resultado (SQL a C)|Conversión de parámetros (C a SQL)|  
|--------------------------|----------------------------------------------|----------------------|------------------------------------|---------------------------------------|  
|Datetime|Fecha|SQL_C_TYPE_DATE|Aceptar|ACEPTAR (1)|  
|||SQL_C_TYPE_TIMESTAMP|Los campos de hora se establecen en cero.|OK (2)<br /><br /> Se produce un error si el campo de hora es distinto de cero. Funciona con [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||Time(0)|SQL_C_TYPE_TIME|Aceptar|ACEPTAR (1)|  
|||SQL_C_TYPE_TIMESTAMP|Los campos de fecha se establecen en la fecha actual.|OK (2)<br /><br /> Fecha omitida. Se produce un error si las fracciones de segundo son distintas de cero. Funciona con [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||Time(7)|SQL_C_TIME|Error: literal de hora no válido.|ACEPTAR (1)|  
|||SQL_C_TYPE_TIMESTAMP|Error: literal de hora no válido.|ACEPTAR (1)|  
||Datetime2 (3)|SQL_C_TYPE_TIMESTAMP|Aceptar|ACEPTAR (1)|  
||Datetime2 (7)|SQL_C_TYPE_TIMESTAMP|Aceptar|El valor se redondeará a la fracción 1/300 de segundo por conversión del cliente.|  
|Smalldatetime|Fecha|SQL_C_TYPE_DATE|Aceptar|Aceptar|  
|||SQL_C_TYPE_TIMESTAMP|Los campos de hora se establecen en cero.|OK (2)<br /><br /> Se produce un error si el campo de hora es distinto de cero. Funciona con [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||Time(0)|SQL_C_TYPE_TIME|Aceptar|Aceptar|  
|||SQL_C_TYPE_TIMESTAMP|Los campos de fecha se establecen en la fecha actual.|OK (2)<br /><br /> Fecha omitida. Se produce un error si las fracciones de segundo son distintas de cero.<br /><br /> Funciona con [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||Datetime2(0)|SQL_C_TYPE_TIMESTAMP|Aceptar|Aceptar|  
|||||

## <a name="key-to-symbols"></a>Clave de los símbolos  
  
|Símbolo|Significado|  
|------------|-------------|  
|1|Si funcionaba con [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], debe continuar funcionando con una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|2|Una aplicación que funcionaba con [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] podría producir un error con una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|||

 Observe que solo se han considerado los cambios de esquema comunes. A continuación, se indican las cambios comunes:  
  
-   Usar un nuevo tipo donde de forma lógica una aplicación requiere un único valor de fecha u hora. Sin embargo, se forzó a la aplicación a que usara datetime o smalldatetime debido a la falta de tipos de fecha y hora independientes.  
  
-   Usar un nuevo tipo para obtener más precisión o exactitud en fracciones de segundo.  
  
-   Cambiar a datetime2 porque éste es el datatype de fecha y hora preferido.  
  
### <a name="column-metadata-returned-by-sqlcolumns-sqlprocedurecolumns-and-sqlspecialcolumns"></a>Metadatos de columna devueltos por SQLColumns, SQLProcedureColumns y SQLSpecialColumns  
 Para los tipos de fecha y hora se devuelven los siguientes valores de columna:  
  
|Tipo de columna|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|-----------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_WVARCHAR|SQL_WVARCHAR|  
|TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|COLUMN_SIZE|10|8, 10.. 16|16|23|19, 21..27|26, 28..34|  
|BUFFER_LENGTH|20|16, 20.. 32|16|16|38, 42.. 54|52, 56.. 68|  
|DECIMAL_DIGITS|NULL|NULL|0|3|NULL|NULL|  
|SQL_DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_DATETIME|SQL_DATETIME|SQL_WVARCHAR|SQL_WVARCHAR|  
|SQL_DATETIME_SUB|NULL|NULL|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|NULL|NULL|  
|CHAR_OCTET_LENGTH|NULL|NULL|NULL|NULL|NULL|NULL|  
|SS_DATA_TYPE|0|0|111|111|0|0|  
||||||||

 SQLSpecialColumns no devuelve SQL_DATA_TYPE, SQL_DATETIME_SUB, CHAR_OCTET_LENGTH o SS_DATA_TYPE.  
  
### <a name="data-type-metadata-returned-by-sqlgettypeinfo"></a>Metadatos de tipo de dato devueltos por SQLGetTypeInfo  
 Para los tipos de fecha y hora se devuelven los siguientes valores de columna:  
  
|Tipo de columna|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|-----------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_WVARCHAR|SQL_WVARCHAR|  
|COLUMN_SIZE|10|16|16|23|27|34|  
|LITERAL_PREFIX|'|'|'|'|'|'|  
|LITERAL_SUFFIX|'|'|'|'|'|'|  
|CREATE_PARAMS|NULL|NULL|NULL|NULL|NULL|NULL|  
|NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|  
|CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|BUSCABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|  
|UNSIGNED_ATTRIBUTE|NULL|NULL|NULL|NULL|NULL|NULL|  
|FXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|AUTO_UNIQUE_VALUE|NULL|NULL|NULL|NULL|NULL|NULL|  
|LOCAL_TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|MINIMUM_SCALE|NULL|NULL|0|3|NULL|NULL|  
|MAXIMUM_SCALE|NULL|NULL|0|3|NULL|NULL|  
|SQL_DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_DATETIME|SQL_DATETIME|SQL_WVARCHAR|SQL_WVARCHAR|  
|SQL_DATETIME_SUB|NULL|NULL|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|NULL|NULL|  
|NUM_PREC_RADIX|NULL|NULL|NULL|NULL|NULL|NULL|  
|INTERVAL_PRECISION|NULL|NULL|NULL|NULL|NULL|NULL|  
|USERTYPE|0|0|12|22|0|0|  
||||||||

## <a name="down-level-server-behavior"></a>Comportamiento de un servidor de nivel inferior  
 Cuando se conecta a una instancia de servidor de una versión anterior a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], cualquier intento de usar los nuevos tipos de servidor o códigos de metadatos asociados y campos de descriptor hará que se devuelva un SQL_ERROR. Se generará un registro de diagnóstico con SQLSTATE HY004 y un mensaje que indica que el tipo de datos SQL para la versión del servidor en la conexión no es válido o con 07006 y "Infracción del atributo de tipo de datos restringido".  
  
## <a name="see-also"></a>Consulte también  
 [Mejoras de fecha y hora &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
