---
description: Miembros SQLServerResultSetMetaData
title: Miembros SQLServerResultSetMetaData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 37587981-2979-49a3-a6ab-df4bfb9b8748
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d10abafa4c3fb2f3bccc7d0ee897cc55a2c92e37
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180076"
---
# <a name="sqlserverresultsetmetadata-members"></a>Miembros SQLServerResultSetMetaData
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  En las siguientes tablas se enumeran los miembros que expone la clase [SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md).  
  
## <a name="constructors"></a>Constructores  
 Ninguno.  
  
## <a name="fields"></a>Fields  
 Ninguno.  
  
## <a name="inherited-fields"></a>Campos heredados  
  
|NOMBRE|Descripción|  
|----------|-----------------|  
|java.sql.ResultSetMetaData|columnNoNulls, columnNullable, columnNullableUnknown|  
  
## <a name="methods"></a>Métodos  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[getCatalogName](../../../connect/jdbc/reference/getcatalogname-method-sqlserverresultsetmetadata.md)|Obtiene el nombre de catálogo para la tabla que incluye la columna designada.|  
|[getColumnClassName](../../../connect/jdbc/reference/getcolumnclassname-method-sqlserverresultsetmetadata.md)|Devuelve el nombre completo de la clase Java cuyas instancias se generan si se llama al método [getObject](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md) de la clase [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) para que recupere un valor en la columna.|  
|[getColumnCount](../../../connect/jdbc/reference/getcolumncount-method-sqlserverresultsetmetadata.md)|Devuelve el número de columnas del conjunto de resultados.|  
|[getColumnDisplaySize](../../../connect/jdbc/reference/getcolumndisplaysize-method-sqlserverresultsetmetadata.md)|Devuelve el ancho máximo normal, en caracteres, de la columna designada.|  
|[getColumnLabel](../../../connect/jdbc/reference/getcolumnlabel-method-sqlserverresultsetmetadata.md)|Obtiene el título que se sugiere para su utilización en copias impresas y presentaciones de la columna designada.|  
|[getColumnName](../../../connect/jdbc/reference/getcolumnname-method-sqlserverresultsetmetadata.md)|Obtiene el nombre de la columna designada.|  
|[getColumnType](../../../connect/jdbc/reference/getcolumntype-method-sqlserverresultsetmetadata.md)|Recupera el tipo SQL de la columna designada.|  
|[getColumnTypeName](../../../connect/jdbc/reference/getcolumntypename-method-sqlserverresultsetmetadata.md)|Recupera el nombre del tipo específico de base de datos de la columna designada.|  
|[getPrecision](../../../connect/jdbc/reference/getprecision-method-sqlserverresultsetmetadata.md)|Obtiene el número de dígitos decimales para la columna designada.|  
|[getScale](../../../connect/jdbc/reference/getscale-method-sqlserverresultsetmetadata.md)|Obtiene el número de dígitos que se encuentran a la derecha del separador decimal para la columna designada.|  
|[getSchemaName](../../../connect/jdbc/reference/getschemaname-method-sqlserverresultsetmetadata.md)|Obtiene el nombre del esquema de tabla para la columna designada.|  
|[getTableName](../../../connect/jdbc/reference/gettablename-method-sqlserverresultsetmetadata.md)|Obtiene el nombre de la tabla de la columna designada.|  
|[isAutoIncrement](../../../connect/jdbc/reference/isautoincrement-method-sqlserverresultsetmetadata.md)|Indica si se asigna automáticamente un número a la columna designada, lo cual la convierte en de solo lectura.|  
|[isCaseSensitive](../../../connect/jdbc/reference/iscasesensitive-method-sqlserverresultsetmetadata.md)|Indica si la columna distinguen mayúsculas de minúsculas.|  
|[isCurrency](../../../connect/jdbc/reference/iscurrency-method-sqlserverresultsetmetadata.md)|Indica si la columna designada es un valor de efectivo.|  
|[isDefinitelyWritable](../../../connect/jdbc/reference/isdefinitelywritable-method-sqlserverresultsetmetadata.md)|Indica si se realizará correctamente la operación de escritura en una columna designada.|  
|[isNullable](../../../connect/jdbc/reference/isnullable-method-sqlserverresultsetmetadata.md)|Indica la nulabilidad de los valores en la columna designada.|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverresultsetmetadata.md)|Indica si definitivamente no se puede escribir en la columna.|  
|[isSearchable](../../../connect/jdbc/reference/issearchable-method-sqlserverresultsetmetadata.md)|Indica si la columna designada se puede utilizar en una cláusula WHERE de SQL.|  
|[isSigned](../../../connect/jdbc/reference/issigned-method-sqlserverresultsetmetadata.md)|Indica si los valores en la columna designada son números con signo.|  
|[isSparseColumnSet](../../../connect/jdbc/reference/issparsecolumnset-method-sqlserverresultsetmetadata.md)|Indica si una columna de un conjunto de resultados es un conjunto de columnas dispersas.|  
|[isWritable](../../../connect/jdbc/reference/iswritable-method-sqlserverresultsetmetadata.md)|Indica si es posible que se realice correctamente una operación de escritura en la columna designada.|  
  
## <a name="inherited-methods"></a>Métodos heredados  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Consulte también  
 [Clase SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md)  
  
  
