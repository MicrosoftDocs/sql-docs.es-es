---
description: Método getFunctionColumns (SQLServerDatabaseMetaData)
title: Método getFunctionColumns (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e2b0e0f7-717c-48e6-bcd2-a325d938a833
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03a5bbe04c9fdf511659a2abef7e6e4135e87370
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175755"
---
# <a name="getfunctioncolumns-method-sqlserverdatabasemetadata"></a>Método getFunctionColumns (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera una descripción de los parámetros de las funciones del sistema o de usuario del catálogo y del tipo de devolución.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public ResultSet getFunctionColumns(java.lang.String catalog,  
                       java.lang.String schemaPattern,  
                       java.lang.String functionNamePattern  
                       java.lang.String columnNamePattern)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *catalog*  
  
 Objeto **String** que contiene el nombre del catálogo. Si es una cadena vacía "", el resultado incluye las funciones sin un catálogo. Si es **null**, el nombre del catálogo no se utiliza en la búsqueda.  
  
 *schemaPattern*  
  
 Objeto **String** que contiene el modelo de nombre del esquema. Si es una cadena vacía "", el resultado incluye las funciones sin un esquema. Si es **null**, el nombre del esquema no se utiliza en la búsqueda.  
  
 *functionNamePattern*  
  
 Objeto **String** que contiene el nombre de una función.  
  
 *columnNamePattern*  
  
 Objeto **String** que contiene el nombre de un parámetro.  
  
## <a name="return-value"></a>Valor devuelto  
 Objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getFunctionColumns especifica este método getFunctionColumns en la interfaz java.sql.DatabaseMetaData.  
  
 Este método devuelve solamente las funciones y parámetros que coinciden con el esquema, nombre de función y nombre de parámetro especificados dentro del catálogo indicado.  
  
 Cada fila en el conjunto de resultados incluye las siguientes columnas para una descripción del parámetro, una descripción de la columna o un tipo de devolución:  
  
|Nombre|Tipo|Descripción|  
|----------|----------|-----------------|  
|FUNCTION_CAT|**String**|Nombre de la base de datos en que reside la función.|  
|FUNCTION_SCHEM|**String**|Esquema para la función.|  
|FUNCTION_NAME|**String**|El nombre de la función.|  
|COLUMN_NAME|**String**|Nombre de un parámetro o columna.|  
|COLUMN_TYPE|**short**|**Tipo de la columna. Puede ser uno de los siguientes valores:**<br /><br /> functionColumnUnknown (0): Tipo desconocido.<br /><br /> functionColumnIn (1): Parámetro de entrada.<br /><br /> functionColumnInOut (2): Parámetro de entrada o salida.<br /><br /> functionColumnOut (3): Parámetro de salida.<br /><br /> functionReturn (4): Valor devuelto de la función.<br /><br /> functionColumnResult (5): Un parámetro o columna es una columna en el conjunto de resultados.|  
|DATA_TYPE|**smallint**|Valor del tipo de datos SQL de java.sql.Types.|  
|TYPE_NAME|**String**|El nombre del tipo de datos.|  
|PRECISION|**int**|Número total de dígitos significativos.|  
|LENGTH|**int**|La longitud de los datos, en bytes.|  
|SCALE|**short**|Número de dígitos que se encuentran a la derecha del separador decimal.|  
|RADIX|**short**|Base de tipos numéricos.|  
|NULLABLE|**short**|Indica si el parámetro o valor devuelto puede contener un valor **null**.<br /><br /> **Puede ser uno de los siguientes valores:**<br /><br /> functionNoNulls (0): El valor NULL no está permitido.<br /><br /> functionNullable (1): Se permite un valor NULL.<br /><br /> functionNullableUnknown (2): desconocida.|  
|COMENTARIOS|**String**|Comentarios sobre una columna o un parámetro.|  
|COLUMN_DEF|**String**|Valor predeterminado de la columna.<br /><br /> **Nota:** Esta información está disponible con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y es específica del controlador JDBC.|  
|SQL_DATA_TYPE|**smallint**|Esta columna es igual que la columna **DATA_TYPE**, salvo por los tipos de datos **datetime** e **interval** de ISO.<br /><br /> **Nota:** Esta información está disponible con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y es específica del controlador JDBC.|  
|SQL_DATETIME_SUB|**smallint**|El subcódigo **datetime** ISO **interval** si el valor de **SQL_DATA_TYPE** es **SQL_DATETIME** o **SQL_INTERVAL**. Para tipos de datos distintos de **datetime** e **interval** de ISO, esta columna es NULL.<br /><br /> **Nota:** Esta información está disponible con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y es específica del controlador JDBC.|  
|CHAR_OCTET_LENGTH|**int**|Longitud máxima de los parámetros o columnas basados en valores binarios o caracteres. Para los demás tipos de datos, es NULL.|  
|ORDINAL_POSITION|**int**|Para los parámetros de entrada y salida, representa la posición a partir de 1.<br /><br /> Para las columnas de conjunto de resultados, es la posición de la columna en el conjunto de resultados a partir de 1.<br /><br /> Para el valor devuelto, es 0.|  
|IS_NULLABLE|**String**|Determina la nulabilidad de un parámetro o columna.<br /><br /> Puede ser uno de los siguientes valores:<br /><br /> **SÍ**: El parámetro o la columna puede incluir valores NULL.<br /><br /> **NO**: El parámetro o la columna no puede incluir valores NULL.<br /><br /> Cadena vacía (""): desconocida.|  
|SS_TYPE_CATALOG_NAME|**String**|Nombre del catálogo que contiene el tipo definido por el usuario (UDT).|  
|SS_TYPE_SCHEMA_NAME|**String**|Nombre del esquema que contiene el tipo definido por el usuario (UDT).|  
|SS_UDT_CATALOG_NAME|**String**|Tipo definido por el usuario (UDT) del nombre completo.|  
|SS_UDT_SCHEMA_NAME|**String**|Nombre del catálogo donde se define el nombre de una colección de esquemas XML. Si no se encuentra el nombre de catálogo, esta variable contiene una cadena vacía.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|**String**|Nombre del esquema donde se define el nombre de una colección de esquemas XML. Si no se puede encontrar el nombre de esquema, esta cadena estará vacía.|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|**String**|Nombre de una colección de esquemas XML. Si no se puede encontrar el nombre, esta cadena estará vacía.|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|**String**|Nombre del catálogo que contiene el tipo definido por el usuario (UDT).|  
|SS_XML_SCHEMACOLLECTION_NAME|**String**|Nombre del esquema que contiene el tipo definido por el usuario (UDT).|  
|SS_DATA_TYPE|**tinyint**|Tipo de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que utilizan los procedimientos almacenados extendidos.<br /><br /> **Nota:** Para más información sobre los tipos de datos que devuelve [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vea "Tipos de datos (Transact-SQL)" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
