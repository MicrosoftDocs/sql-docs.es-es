---
description: Miembros de SQLServerPreparedStatement
title: Miembros SQLServerPreparedStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2363902f-d4c6-4cd4-a5fc-86079eb9e418
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 74ae525bdd1c05eb01f9bce98cba9ef987087a30
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167993"
---
# <a name="sqlserverpreparedstatement-members"></a>Miembros de SQLServerPreparedStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  En las siguientes tablas se enumeran los miembros que expone la clase [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).  
  
## <a name="constructors"></a>Constructores  
 Ninguno.  
  
## <a name="fields"></a>Fields  
 Ninguno.  
  
## <a name="inherited-fields"></a>Campos heredados  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="methods"></a>Métodos  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)|Agrega un conjunto de parámetros al lote de comandos para este objeto Statement.|  
|[cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Cancela la instrucción de SQL que se esté ejecutando en esos momentos en este objeto Statement.|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md)|Vacía la lista actual de comandos SQL para este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[clearParameters](../../../connect/jdbc/reference/clearparameters-method-sqlserverpreparedstatement.md)|Borra inmediatamente los valores de parámetro actuales.|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Borra todas las advertencias que se notifican en este objeto Statement.|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverpreparedstatement.md)|Libera inmediatamente los recursos de la base de datos y de JDBC de este objeto Statement en lugar de esperar a que se liberen automáticamente.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)|Ejecuta la instrucción SQL en este objeto Statement, que puede ser cualquier tipo de instrucción SQL.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverpreparedstatement.md)|Envía un lote de comandos a la base de datos para que se ejecute. Si todos los comandos se ejecutan correctamente, devuelve una matriz de recuentos de actualizaciones.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)|Ejecuta la consulta SQL en este objeto Statement y devuelve el objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que genera la consulta.|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)|Ejecuta la instrucción SQL en este objeto Statement, que debe ser una instrucción INSERT, UPDATE, MERGE o DELETE de SQL; o una instrucción SQL que no devuelva nada, como una instrucción DDL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) que generó este objeto Statement.|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera la dirección para capturar las filas de las tablas de base de datos que es el valor predeterminado para los conjuntos de resultados que se generaron a partir de este objeto Statement.|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el número de filas del conjunto de resultados que sea el tamaño de captura predeterminado para los objetos del conjunto de resultados que generó este objeto Statement.|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera cualquiera clave generada automáticamente que se cree como resultado de ejecutar este objeto Statement.|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el número máximo de bytes que se pueden devolver para valores de columna de caracteres y binarios en un objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto Statement.|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el número máximo de filas que puede contener un objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto Statement.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverpreparedstatement.md)|Recupera un objeto [SQLServerResultSetMetaData Class](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md) que contiene información sobre las columnas del objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que se devolverá cuando este objeto Statement se ejecute.|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Se desplaza al próximo resultado de este objeto Statement.|  
|[getParameterMetaData](../../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md)|Recupera el número, tipos y propiedades de los parámetros para este objeto Statement.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el modo de almacenamiento en búfer de respuesta para este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el número de segundos que [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] esperará la ejecución de este objeto Statement.|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el resultado actual como un objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera la simultaneidad del conjunto de resultados para los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto Statement.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera la capacidad de alojamiento del conjunto de resultados para los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto Statement.|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el tipo del conjunto de resultados para los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto Statement.|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el resultado actual como un recuento de actualizaciones.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera la primera advertencia que notifican las llamadas en este objeto Statement.|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Indica si se ha cerrado este objeto Statement.|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Devuelve un valor que indica si una instrucción se puede agregar al grupo de instrucciones proporcionado por el usuario.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverpreparedstatement.md)|Indica si este objeto de instrucción es un contenedor para la interfaz especificada.|  
|[setArray](../../../connect/jdbc/reference/setarray-method-sqlserverpreparedstatement.md)|Establece el número de parámetro designado para el objeto Array determinado.|  
|[setAsciiStream](../../../connect/jdbc/reference/setasciistream-method-sqlserverpreparedstatement.md)|Establece el número de parámetro designado para el objeto InputStream determinado.|  
|[setBigDecimal](../../../connect/jdbc/reference/setbigdecimal-method-sqlserverpreparedstatement.md)|Establece el número de parámetro designado para el objeto BigDecimal determinado.|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el flujo de entrada especificado.|  
|[setBlob](../../../connect/jdbc/reference/setblob-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el objeto Blob especificado.|  
|[setboolean](../../../connect/jdbc/reference/setboolean-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el valor **Boolean** especificado.|  
|[setByte](../../../connect/jdbc/reference/setbyte-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el valor **byte** especificado.|  
|[setBytes](../../../connect/jdbc/reference/setbytes-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en la matriz de bytes especificada.|  
|[setCharacterStream](../../../connect/jdbc/reference/setcharacterstream-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el objeto Reader especificado.|  
|[setClob](../../../connect/jdbc/reference/setclob-method-sqlserverpreparedstatement.md)|Establece el parámetro designado para el objeto Clob determinado.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el nombre de cursor de SQL para la cadena especificada, el cual utilizarán métodos ulteriores.|  
|[setDate](../../../connect/jdbc/reference/setdate-method-sqlserverpreparedstatement.md)|Establece el parámetro designado para el valor de fecha especificado.|  
|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlserverpreparedstatement.md)|Establece el valor de la columna especificada en el valor de la [clase DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[setDouble](../../../connect/jdbc/reference/setdouble-method-sqlserverpreparedstatement.md)|Establece el parámetro designado para el valor **double** especificado.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el modo de procesamiento de escape.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Ofrece una sugerencia al controlador JDBC sobre la dirección en la que se deberían procesar las filas del conjunto de resultados.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Ofrece una sugerencia al controlador JDBC sobre el número de filas que se deberían capturar en la base de datos cuando se necesitan más filas.|  
|[setFloat](../../../connect/jdbc/reference/setfloat-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el valor **float** especificado.|  
|[setInt](../../../connect/jdbc/reference/setint-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el valor **int** especificado.|  
|[setLong](../../../connect/jdbc/reference/setlong-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el valor **long** especificado.|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el límite para el número máximo de bytes en una columna [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que almacena valores de caracteres o binarios en el número establecido de bytes.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el límite del número máximo de filas que cualquier objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) puede contener para el número determinado.|  
|[setNCharacterStream](../../../connect/jdbc/reference/setncharacterstream-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el objeto Reader especificado.|  
|[setNClob](../../../connect/jdbc/reference/setnclob-method-sqlserverpreparedstatement.md)|Establece el parámetro designado para el objeto especificado.|  
|[setNull](../../../connect/jdbc/reference/setnull-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en un valor NULL, según en tipo de parámetro que se va a establecer.|  
|[setNString](../../../connect/jdbc/reference/setnstring-method-int-java-lang-string.md)|Establece el parámetro designado en el objeto **String** especificado.|  
|[setObject](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)|Establece el valor del parámetro designado utilizando el objeto determinado.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Solicita que una instrucción se agrupe o no. De forma predeterminada, un objeto SQLServerPreparedStatement se puede agrupar cuando se crea.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el número de segundos que esperará el controlador antes de que se ejecute un objeto Statement según el número de segundos especificado.|  
|[setRef](../../../connect/jdbc/reference/setref-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el objeto Ref especificado.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el modo de almacenamiento en búfer de respuesta para este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) en **String full** sin distinción entre mayúsculas y minúsculas o en **adaptive**.|  
|[setShort](../../../connect/jdbc/reference/setshort-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el valor **short** especificado.|  
|[setString](../../../connect/jdbc/reference/setstring-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el valor **String** especificado.|  
|[setSQLXML](../../../connect/jdbc/reference/setsqlxml-method-sqlserverpreparedstatement.md)|Establece el parámetro designado en el objeto **SQLXML** especificado.|  
|[setTime](../../../connect/jdbc/reference/settime-method-sqlserverpreparedstatement.md)|Establece el parámetro designado para el valor de hora especificado.|  
|[setTimestamp](../../../connect/jdbc/reference/settimestamp-method-sqlserverpreparedstatement.md)|Establece el parámetro designado para el valor de marca de tiempo especificado.|  
|[setUnicodeStream](../../../connect/jdbc/reference/setunicodestream-method-sqlserverpreparedstatement.md)|Establece el número de parámetro designado en el flujo de entrada especificado, que tendrá el número de bytes indicado.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverpreparedstatement.md)|Establece el parámetro designado para el valor de URL especificado.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverpreparedstatement.md)|Devuelve un objeto que implementa la interfaz especificada para permitir el acceso a los métodos específicos de [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Métodos heredados  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerStatement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, isPoolable, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setPoolable, setQueryTimeout|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Statement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setQueryTimeout|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Consulte también  
 [Clase SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
