---
description: Miembros SQLServerCallableStatement
title: Miembros SQLServerCallableStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apitype: Assembly
ms.assetid: 5ebdc186-e50f-4d14-bbf4-95af5051e4a4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f134ee20c60dc45b216122f76482f6a331fa9a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172867"
---
# <a name="sqlservercallablestatement-members"></a>Miembros SQLServerCallableStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  En las siguientes tablas se enumeran los miembros que expone la clase [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).  
  
## <a name="constructors"></a>Constructores  
 Ninguno.  
  
## <a name="fields"></a>Campos  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="inherited-fields"></a>Campos heredados  
 Ninguno.  
  
## <a name="methods"></a>Métodos  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Agrega un conjunto de parámetros al lote de comandos para este objeto CallableStatement.|  
|[cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Cancela la instrucción de SQL que se esté ejecutando en esos momentos en este objeto CallableStatement.|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Vacía la lista actual de comandos SQL para este objeto CallableStatement.|  
|[clearParameters](../../../connect/jdbc/reference/clearparameters-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Borra inmediatamente los valores de parámetro actuales.|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Borra todas las advertencias que se notifican en este objeto CallableStatement.|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Libera inmediatamente los recursos de la base de datos y de JDBC de este objeto CallableStatement en lugar de esperar a que se liberen automáticamente.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Ejecuta la instrucción SQL en este objeto CallableStatement, que puede ser cualquier tipo de instrucción SQL.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Envía un lote de comandos a la base de datos para que se ejecute. Si todos los comandos se ejecutan correctamente, devuelve una matriz de recuentos de actualizaciones.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Ejecuta la consulta SQL en este objeto CallableStatement y devuelve el objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que genera la consulta.|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Ejecuta la instrucción SQL en este objeto CallableStatement, que debe ser una instrucción INSERT, UPDATE, MERGE o DELETE de SQL; o una instrucción SQL que no devuelva nada, como una instrucción DDL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el objeto CallableStatement que generó este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)|Recupera el valor de la columna especificada como un objeto [Clase DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera la dirección para capturar las filas de las tablas de base de datos que es el valor predeterminado para los conjuntos de resultados que se generaron a partir de este objeto CallableStatement.|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el número de filas del conjunto de resultados que sea el tamaño de captura predeterminado para los objetos del conjunto de resultados que generó este objeto CallableStatement.|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera cualquiera clave generada automáticamente que se cree como resultado de ejecutar este objeto CallableStatement.|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el número máximo de bytes que se pueden devolver para valores de columna de caracteres y binarios en un objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto CallableStatement.|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el número máximo de filas que puede contener un objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que este objeto CallableStatement haya generado.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Recupera un objeto [SQLServerResultSetMetaData Class](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md) que contiene información sobre las columnas del objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que se devolverá cuando este objeto CallableStatement se ejecute.|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Se desplaza al próximo resultado de este objeto CallableStatement.|  
|[getParameterMetaData](../../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Recupera el número, tipos y propiedades de los parámetros para este objeto CallableStatement.|  
|[getArray](../../../connect/jdbc/reference/getarray-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto Array.|  
|[getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un flujo de caracteres **ASCII**.|  
|[getBigDecimal](../../../connect/jdbc/reference/getbigdecimal-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como java.math.BigDecimal.|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un flujo binario de bytes ininterrumpidos.|  
|[getBlob](../../../connect/jdbc/reference/getblob-method-sqlservercallablestatement.md)|Recupera el valor del parámetro JDBC Blob designado como un objeto Blob en el lenguaje de programación Java.|  
|[getboolean](../../../connect/jdbc/reference/getboolean-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un valor **booleano**.|  
|[getByte](../../../connect/jdbc/reference/getbyte-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un valor **byte**.|  
|[getBytes](../../../connect/jdbc/reference/getbytes-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como una matriz de bytes.|  
|[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto java.io.Reader.|  
|[getClob](../../../connect/jdbc/reference/getclob-method-sqlservercallablestatement.md)|Recupera el valor del parámetro JDBC Blob designado como un objeto Clob en el lenguaje de programación Java.|  
|[getDate](../../../connect/jdbc/reference/getdate-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto java.sql.Date en el lenguaje de programación Java.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)|Recupera el valor de la columna especificada como un objeto [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[getDouble](../../../connect/jdbc/reference/getdouble-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como **double** en el lenguaje de programación Java.|  
|[getFloat](../../../connect/jdbc/reference/getfloat-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como **float** en el lenguaje de programación Java.|  
|[getInt](../../../connect/jdbc/reference/getint-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como **int** en el lenguaje de programación Java.|  
|[getLong](../../../connect/jdbc/reference/getlong-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un valor **long** en el lenguaje de programación Java.|  
|[getNCharacterStream](../../../connect/jdbc/reference/getncharacterstream-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto Reader.|  
|[getNClob](../../../connect/jdbc/reference/getnclob-method-sqlservercallablestatement.md)|Recupera el valor del parámetro JDBC **NCLOB** designado como un objeto **NClob** en el lenguaje de programación Java.|  
|[getNString](../../../connect/jdbc/reference/getnstring-method-sqlservercallablestatement.md)|Recupera el valor del parámetro **NCHAR**, **NVARCHAR** o **LONGNVARCHAR** designado como un objeto String en el lenguaje de programación Java.|  
|[getObject](../../../connect/jdbc/reference/getobject-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto en el lenguaje de programación Java.|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el número de segundos que [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] esperará la ejecución de este objeto CallableStatement.|  
|[getRef](../../../connect/jdbc/reference/getref-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto Ref en el lenguaje de programación Java.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el modo de almacenamiento en búfer de respuesta para este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el resultado actual como un objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera la simultaneidad del conjunto de resultados para los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto CallableStatement.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera la capacidad de alojamiento del conjunto de resultados para los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto CallableStatement.|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el tipo del conjunto de resultados para los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto CallableStatement.|  
|[getShort](../../../connect/jdbc/reference/getshort-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como **short** en el lenguaje de programación Java.|  
|[getString](../../../connect/jdbc/reference/getstring-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto **String** en el lenguaje de programación Java.|  
|[getSQLXML](../../../connect/jdbc/reference/getsqlxml-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto java.sql.SQLXML.|  
|[getTime](../../../connect/jdbc/reference/gettime-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto java.sql.Time en el lenguaje de programación Java.|  
|[getTimestamp](../../../connect/jdbc/reference/gettimestamp-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto java.sql.Timestamp en el lenguaje de programación Java.|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera el resultado actual como un recuento de actualizaciones.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlservercallablestatement.md)|Recupera el valor del parámetro designado como un objeto URL en el lenguaje de programación Java.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Recupera la primera advertencia que notifican las llamadas en este objeto CallableStatement.|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Indica si se ha cerrado este objeto Statement.|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Devuelve un valor que indica si una instrucción se puede agregar al grupo de instrucciones proporcionado por el usuario.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlservercallablestatement.md)|Indica si este objeto de instrucción es un contenedor para la interfaz especificada.|  
|[registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md)|Registra el parámetro OUT.|  
|[setArray](../../../connect/jdbc/reference/setarray-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Establece el número de parámetro designado para el objeto Array determinado.|  
|[setAsciiStream](../../../connect/jdbc/reference/setasciistream-sqlservercallablestatement.md)|Establece el parámetro designado en el flujo de entrada determinado.|  
|[setBigDecimal](../../../connect/jdbc/reference/setbigdecimal-method-sqlservercallablestatement.md)|Establece el número de parámetro designado para el objeto BigDecimal determinado.|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-sqlservercallablestatement.md)|Establece el parámetro designado en el flujo de entrada especificado.|  
|[setBlob](../../../connect/jdbc/reference/setblob-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Establece el parámetro designado para el objeto Blob determinado.|  
|[setboolean](../../../connect/jdbc/reference/setboolean-method-sqlservercallablestatement.md)|Establece el parámetro designado para el valor **Boolean** determinado.|  
|[setByte](../../../connect/jdbc/reference/setbyte-method-sqlservercallablestatement.md)|Establece el parámetro designado para el valor **byte** determinado.|  
|[setBytes](../../../connect/jdbc/reference/setbytes-method-sqlservercallablestatement.md)|Establece el parámetro designado en la matriz determinada de valores de tipo **byte**.|  
|[setCharacterStream](../../../connect/jdbc/reference/setcharacterstream-method-sqlservercallablestatement.md)|Establece el parámetro designado para el objeto Reader determinado.|  
|[setClob](../../../connect/jdbc/reference/setclob-method-sqlservercallablestatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Establece el parámetro designado para el objeto especificado.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el nombre de cursor de SQL para la cadena determinada, el cual utilizarán métodos ulteriores.|  
|[setDate](../../../connect/jdbc/reference/setdate-method-sqlservercallablestatement.md)|Establece el parámetro designado para el valor de fecha determinado.|  
|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlservercallablestatement.md)|Establece el valor de la columna especificada en el valor de la [clase DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[setDouble](../../../connect/jdbc/reference/setdouble-method-sqlservercallablestatement.md)|Establece el parámetro designado en el valor **double** indicado.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el modo de procesamiento de escape.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Ofrece una sugerencia al controlador JDBC sobre la dirección en la que se deberían procesar las filas del conjunto de resultados.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Ofrece una sugerencia al controlador JDBC sobre el número de filas que se deberían capturar en la base de datos cuando se necesitan más filas.|  
|[setFloat](../../../connect/jdbc/reference/setfloat-method-sqlservercallablestatement.md)|Establece el parámetro designado en el valor **float** especificado.|  
|[setInt](../../../connect/jdbc/reference/setint-method-sqlservercallablestatement.md)|Establece el parámetro designado en el valor **int** especificado.|  
|[setLong](../../../connect/jdbc/reference/setlong-method-sqlservercallablestatement.md)|Establece el parámetro designado en el valor **long** especificado.|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el límite para el número máximo de bytes en una columna [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que almacene valores de caracteres o binarios en el número especificado de bytes.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el límite para el número máximo de filas que cualquier objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) puede contener para el número especificado.|  
|[setNCharacterStream](../../../connect/jdbc/reference/setncharacterstream-method-sqlservercallablestatement.md)|Establece el parámetro designado en el objeto Reader especificado.|  
|[setNClob](../../../connect/jdbc/reference/setnclob-method-sqlservercallablestatement.md)|Establece el parámetro designado para el objeto especificado.|  
|[setNString](../../../connect/jdbc/reference/setnstring-method-sqlservercallablestatement.md)|Establece el parámetro designado en el objeto String especificado.|  
|[setNull](../../../connect/jdbc/reference/setnull-method-sqlservercallablestatement.md)|Establece el parámetro designado en un valor NULL, según en tipo de parámetro que se va a establecer.|  
|[setObject](../../../connect/jdbc/reference/setobject-method-sqlservercallablestatement.md)|Establece el valor del parámetro designado utilizando el objeto determinado.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Solicita que una instrucción se agrupe o no. De forma predeterminada, un objeto SQLServerCallableStatement se puede agrupar cuando se crea.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el número de segundos que esperará el controlador antes de que se ejecute un objeto CallableStatement según el número de segundos especificado.|  
|[setRef](../../../connect/jdbc/reference/setref-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Establece el parámetro designado en el objeto Ref especificado.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|(Se hereda de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)). Establece el modo de almacenamiento en búfer de respuesta para este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) en **String full** sin distinción entre mayúsculas y minúsculas o en **adaptive**.|  
|[setShort](../../../connect/jdbc/reference/setshort-method-sqlservercallablestatement.md)|Establece el parámetro designado en el valor **short** especificado.|  
|[setString](../../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md)|Establece el parámetro designado en el valor **String** de Java especificado.|  
|[setSQLXML](../../../connect/jdbc/reference/setsqlxml-method-sqlservercallablestatement.md)|Establece el parámetro designado en el objeto **SQLXML** especificado.|  
|[setTime](../../../connect/jdbc/reference/settime-method-sqlservercallablestatement.md)|Establece el parámetro designado para el valor de hora especificado.|  
|[setTimestamp](../../../connect/jdbc/reference/settimestamp-method-sqlservercallablestatement.md)|Establece el parámetro designado para el valor de marca de tiempo especificado.|  
|[setUnicodeStream](../../../connect/jdbc/reference/setunicodestream-method-sqlserverpreparedstatement.md)|(Se hereda de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)). Establece el número de parámetro designado en el flujo de entrada determinado, que tendrá el número de bytes indicado.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlservercallablestatement.md)|Establece el parámetro designado para el valor de URL especificado.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md)|Devuelve un objeto que implementa la interfaz especificada para permitir el acceso a los métodos específicos de [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
|[wasNull](../../../connect/jdbc/reference/wasnull-method-sqlservercallablestatement.md)|Recupera si la última lectura del parámetro OUT incorporaba el valor de NULL de SQL.|  
  
## <a name="inherited-methods"></a>Métodos heredados  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement|addBatch, clearBatch, clearParameters, close, execute, executeBatch, executeQuery, executeUpdate, getMetaData, getParameterMetaData, setArray, setAsciiStream, setBigDecimal, setBinaryStream, setBlob, setboolean, setByte, setBytes, setCharacterStream, setClob, setDate, setDouble, setFloat, setInt, setLong, setNull, setObject, setRef, setShort, setString, setTime, setTimestamp, setUnicodeStream, setURL|  
|com.microsoft.sqlserver.jdbc.SQLServerStatement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, isPoolable, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setPoolable, setQueryTimeout|  
|class java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait,|  
|java.sql.PreparedStatement|addBatch, clearParameters, execute, executeQuery, executeUpdate, getMetaData, getParameterMetaData, getSQLXML, setArray, setAsciiStream, setBigDecimal, setBinaryStream, setBlob, setboolean, setByte, setBytes, setCharacterStream, setClob, setDate, setDate, setDouble, setFloat, setInt, setLong, setNull, setObject, setRef, setShort, setString, setSQLXML, setTime, setTimestamp, setUnicodeStream, setURL|  
|java.sql.Statement|addBatch, cancel, clearBatch, clearWarnings, close, execute, executeBatch, executeQuery, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setQueryTimeout|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Consulte también  
 [Clase SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
