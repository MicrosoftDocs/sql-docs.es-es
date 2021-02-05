---
description: Miembros SQLServerResultSet
title: Miembros SQLServerResultSet | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2a438d5d-2d6a-46a0-a2ae-f35fbae4a472
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bf8e9b71b638a3836ebefb71e3d574183372267e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180105"
---
# <a name="sqlserverresultset-members"></a>Miembros SQLServerResultSet
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  En las siguientes tablas se enumeran los miembros que expone la clase [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="constructors"></a>Constructores  
 Ninguno.  
  
## <a name="fields"></a>Campos  
  
|NOMBRE|Descripción|  
|----------|-----------------|  
|[CONCUR_SS_OPTIMISTIC_CC](../../../connect/jdbc/reference/concur-ss-optimistic-cc-field-sqlserverresultset.md)|Se utiliza para especificar un tipo de simultaneidad optimista de lectura y escritura de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sin bloqueos de fila.|  
|[CONCUR_SS_OPTIMISTIC_CCVAL](../../../connect/jdbc/reference/concur-ss-optimistic-ccval-field-sqlserverresultset.md)|Se utiliza para especificar un tipo de simultaneidad optimista de lectura y escritura de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sin bloqueos de fila.|  
|[CONCUR_SS_SCROLL_LOCKS](../../../connect/jdbc/reference/concur-ss-scroll-locks-field-sqlserverresultset.md)|Se utiliza para especificar un tipo de simultaneidad optimista de lectura y escritura de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con bloqueos de fila.|  
|[TYPE_SS_DIRECT_FORWARD_ONLY](../../../connect/jdbc/reference/type-ss-direct-forward-only-field-sqlserverresultset.md)|Se usa para especificar un tipo de cursor de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de solo avance y solo lectura.|  
|[TYPE_SS_SCROLL_DYNAMIC](../../../connect/jdbc/reference/type-ss-scroll-dynamic-field-sqlserverresultset.md)|Se utiliza para especificar un tipo de cursor dinámico de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[TYPE_SS_SCROLL_KEYSET](../../../connect/jdbc/reference/type-ss-scroll-keyset-field-sqlserverresultset.md)|Se usa para especificar un tipo de cursor de conjunto de claves de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[TYPE_SS_SCROLL_STATIC](../../../connect/jdbc/reference/type-ss-scroll-static-field-sqlserverresultset.md)|Se utiliza para especificar un tipo de cursor estático de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[TYPE_SS_SERVER_CURSOR_FORWARD_ONLY](../../../connect/jdbc/reference/type-ss-server-cursor-forward-only-field-sqlserverresultset.md)|Se usa para especificar un tipo de cursor de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de solo avance y solo lectura.|  
  
## <a name="inherited-fields"></a>Campos heredados  
  
|Clase heredada de:|Descripción|  
|---------------------------|-----------------|  
|java.sql.ResultSet|CLOSE_CURSORS_AT_COMMIT, CONCUR_READ_ONLY, CONCUR_UPDATABLE, FETCH_FORWARD, FETCH_REVERSE, FETCH_UNKNOWN, HOLD_CURSORS_OVER_COMMIT, TYPE_FORWARD_ONLY, TYPE_SCROLL_INSENSITIVE, TYPE_SCROLL_SENSITIVE|  
  
## <a name="methods"></a>Métodos  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[absolute](../../../connect/jdbc/reference/absolute-method-sqlserverresultset.md)|Mueve el cursor a la fila especificada en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[afterLast](../../../connect/jdbc/reference/afterlast-method-sqlserverresultset.md)|Mueve el cursor a la posición tras la última fila de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[beforeFirst](../../../connect/jdbc/reference/beforefirst-method-sqlserverresultset.md)|Mueve el cursor a la posición antes de la primera fila de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[cancelRowUpdates](../../../connect/jdbc/reference/cancelrowupdates-method-sqlserverresultset.md)|Cancela las actualizaciones realizadas en la fila actual en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverresultset.md)|Borra todas las advertencias notificadas en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverresultset.md)|Libera inmediatamente la base de datos de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) y los recursos de JDBC en lugar de esperar a que esto se realice cuando se cierre de forma automática.|  
|[deleteRow](../../../connect/jdbc/reference/deleterow-method-sqlserverresultset.md)|Elimina la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) y de la base de datos subyacente.|  
|[finalize](../../../connect/jdbc/reference/finalize-method-sqlserverresultset.md)|Cierra explícitamente este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[findColumn](../../../connect/jdbc/reference/findcolumn-method-sqlserverresultset.md)|Recupera el índice de la primera columna coincidente para el nombre de la columna especificado en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[first](../../../connect/jdbc/reference/first-method-sqlserverresultset.md)|Mueve el cursor a la primera fila de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getArray](../../../connect/jdbc/reference/getarray-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto Array.|  
|[getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un flujo de caracteres ASCII.|  
|[getBigDecimal](../../../connect/jdbc/reference/getbigdecimal-method-sqlserverresultset.md)|Recupera el valor del índice de la columna designado en la fila actual del objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como java.math.BigDecimal.|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un flujo binario de bytes sin interpretar.|  
|[getBlob](../../../connect/jdbc/reference/getblob-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto Blob en el lenguaje de programación Java.|  
|[getBoolean](../../../connect/jdbc/reference/getboolean-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo **boolean** en el lenguaje de programación Java.|  
|[getByte](../../../connect/jdbc/reference/getbyte-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo **byte** en el lenguaje de programación Java.|  
|[getBytes](../../../connect/jdbc/reference/getbytes-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como una matriz de tipo **byte** en el lenguaje de programación Java.|  
|[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto java.io.Reader.|  
|[getClob](../../../connect/jdbc/reference/getclob-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto Clob en el lenguaje de programación Java.|  
|[getConcurrency](../../../connect/jdbc/reference/getconcurrency-method-sqlserverresultset.md)|Recupera el modo de simultaneidad de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getCursorName](../../../connect/jdbc/reference/getcursorname-method-sqlserverresultset.md)|Recupera el nombre del cursor de SQL que utiliza el objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getDate](../../../connect/jdbc/reference/getdate-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto java.sql.Date en el lenguaje de programación Java.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-sqlserverresultset.md)|Recupera el valor de la columna especificada como un objeto [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[getDouble](../../../connect/jdbc/reference/getdouble-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo **double** en el lenguaje de programación Java.|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverresultset.md)|Recupera la dirección de la captura para este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverresultset.md)|Recupera el tamaño de la captura para este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getFloat](../../../connect/jdbc/reference/getfloat-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo **float** en el lenguaje de programación Java.|  
|[getHoldability](../../../connect/jdbc/reference/getholdability-method-sqlserverresultset.md)|Recupera la capacidad de alojamiento de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getInt](../../../connect/jdbc/reference/getint-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo **int** en el lenguaje de programación Java.|  
|[getLong](../../../connect/jdbc/reference/getlong-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo **long** en el lenguaje de programación Java.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverresultset.md)|Recupera el número, tipos y propiedades de las columnas de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getNCharacterStream](../../../connect/jdbc/reference/getncharacterstream-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual del objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto Reader.|  
|[getNClob](../../../connect/jdbc/reference/getnclob-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual del objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto **NClob** en el lenguaje de programación Java.|  
|[getNString](../../../connect/jdbc/reference/getnstring-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual del objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo String en el lenguaje de programación Java.|  
|[getObject](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md)|Obtiene el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto en el lenguaje de programación Java.|  
|[getRef](../../../connect/jdbc/reference/getref-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo Ref en el lenguaje de programación Java.|  
|[getRow](../../../connect/jdbc/reference/getrow-method-sqlserverresultset.md)|Recupera el número de fila actual.|  
|[getShort](../../../connect/jdbc/reference/getshort-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo **short** en el lenguaje de programación Java.|  
|[getStatement](../../../connect/jdbc/reference/getstatement-method-sqlserverresultset.md)|Recupera el objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) que generó este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getString](../../../connect/jdbc/reference/getstring-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor **String** en el lenguaje de programación Java.|  
|[getSQLXML](../../../connect/jdbc/reference/getsqlxml-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual del objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto **SQLXML**.|  
|[getTime](../../../connect/jdbc/reference/gettime-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto java.sql.Time en el lenguaje de programación Java.|  
|[getTimestamp](../../../connect/jdbc/reference/gettimestamp-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto java.sql.Timestamp en el lenguaje de programación Java.|  
|[getType](../../../connect/jdbc/reference/gettype-method-sqlserverresultset.md)|Recupera el tipo de cursor de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getUnicodeStream](../../../connect/jdbc/reference/getunicodestream-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un flujo de caracteres Unicode.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverresultset.md)|Recupera el valor de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto URL.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverresultset.md)|Recupera la primera advertencia que notifican las llamadas en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[insertRow](../../../connect/jdbc/reference/insertrow-method-sqlserverresultset.md)|Inserta el contenido de la fila de inserción en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) y en la base de datos.|  
|[isAfterLast](../../../connect/jdbc/reference/isafterlast-method-sqlserverresultset.md)|Recupera si el cursor está tras la última fila en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isBeforeFirst](../../../connect/jdbc/reference/isbeforefirst-method-sqlserverresultset.md)|Recupera si el cursor está antes de la primera fila en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverresultset.md)|Indica si se ha cerrado este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isFirst](../../../connect/jdbc/reference/isfirst-method-sqlserverresultset.md)|Recupera si el cursor está en la primera fila de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isLast](../../../connect/jdbc/reference/islast-method-sqlserverresultset.md)|Recupera si el cursor está en la última fila de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[last](../../../connect/jdbc/reference/last-method-sqlserverresultset.md)|Mueve el cursor a la última fila en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[moveToCurrentRow](../../../connect/jdbc/reference/movetocurrentrow-method-sqlserverresultset.md)|Mueve el cursor a la posición que se recuerda, que suele ser la fila actual.|  
|[moveToInsertRow](../../../connect/jdbc/reference/movetoinsertrow-method-sqlserverresultset.md)|Mueve el cursor a la fila de inserción.|  
|[siguiente](../../../connect/jdbc/reference/next-method-sqlserverresultset.md)|Mueve el cursor una fila hacia abajo desde su posición actual.|  
|[previous](../../../connect/jdbc/reference/previous-method-sqlserverresultset.md)|Mueve el cursor a la fila anterior en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[refreshRow](../../../connect/jdbc/reference/refreshrow-method-sqlserverresultset.md)|Actualiza la fila actual con el valor más reciente en la base de datos.|  
|[relative](../../../connect/jdbc/reference/relative-method-sqlserverresultset.md)|Mueve el cursor tantas filas como se hayan indicado, con respecto a la fila actual, tanto en una posición positiva como en una negativa.|  
|[rowDeleted](../../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md)|Recupera si se ha eliminado una fila.|  
|[rowInserted](../../../connect/jdbc/reference/rowinserted-method-sqlserverresultset.md)|Recupera si la fila actual ha tenido una inserción.|  
|[rowUpdated](../../../connect/jdbc/reference/rowupdated-method-sqlserverresultset.md)|Recupera si se ha actualizado la fila actual.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverresultset.md)|Ofrece una sugerencia sobre la dirección en la que se procesarán las filas de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverresultset.md)|Ofrece al controlador JDBC una sugerencia sobre el número de filas que se deberían capturar en la base de datos cuando se necesitan más filas para este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[updateArray](../../../connect/jdbc/reference/updatearray-method-sqlserverresultset.md)|Actualiza la columna designada con un objeto Array.|  
|[updateAsciiStream](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de flujo ASCII.|  
|[updateBigDecimal](../../../connect/jdbc/reference/updatebigdecimal-method-sqlserverresultset.md)|Actualiza la columna designada con un objeto BigDecimal.|  
|[updateBinaryStream](../../../connect/jdbc/reference/updatebinarystream-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de flujo binario.|  
|[updateBlob](../../../connect/jdbc/reference/updateblob-method-sqlserverresultset.md)|Actualiza la columna designada con un valor java.sql.Blob.|  
|[updateBoolean](../../../connect/jdbc/reference/updateboolean-method-sqlserverresultset.md)|Actualiza la columna designada con un valor **boolean**.|  
|[updateByte](../../../connect/jdbc/reference/updatebyte-method-sqlserverresultset.md)|Actualiza la columna designada con un valor **byte**.|  
|[updateBytes](../../../connect/jdbc/reference/updatebytes-method-sqlserverresultset.md)|Actualiza la columna designada con una matriz de valores **byte**.|  
|[updateCharacterStream](../../../connect/jdbc/reference/updatecharacterstream-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de flujo de caracteres.|  
|[updateClob](../../../connect/jdbc/reference/updateclob-method-sqlserverresultset.md)|Actualiza la columna designada con un valor java.sql.Clob.|  
|[updateDate](../../../connect/jdbc/reference/updatedate-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de fecha.|  
|[updateDateTimeOffset](../../../connect/jdbc/reference/updatedatetimeoffset-sqlserverresultset.md)|Actualiza una columna [Clase DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[updateDouble](../../../connect/jdbc/reference/updatedouble-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de tipo **double**.|  
|[updateFloat](../../../connect/jdbc/reference/updatefloat-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de **float**.|  
|[updateInt](../../../connect/jdbc/reference/updateint-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de tipo **int**.|  
|[updateLong](../../../connect/jdbc/reference/updatelong-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de tipo **long**.|  
|[updateNCharacterStream](../../../connect/jdbc/reference/updatencharacterstream-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de flujo de caracteres.|  
|[updateNClob](../../../connect/jdbc/reference/updatenclob-method-sqlserverresultset.md)|Actualiza la columna designada con el valor de objeto especificado.|  
|[updateNString](../../../connect/jdbc/reference/updatenstring-method-sqlserverresultset.md)|Actualiza la columna designada con un valor **string**.|  
|[updateNull](../../../connect/jdbc/reference/updatenull-method-sqlserverresultset.md)|Actualiza la columna designada con un valor NULL.|  
|[updateObject](../../../connect/jdbc/reference/updateobject-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de tipo **Object**.|  
|[updateRef](../../../connect/jdbc/reference/updateref-method-sqlserverresultset.md)|Actualiza la columna designada con un valor java.sql.Ref.|  
|[updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md)|Actualiza la base de datos subyacente con el nuevo contenido de la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[updateShort](../../../connect/jdbc/reference/updateshort-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de tipo **short**.|  
|[updateString](../../../connect/jdbc/reference/updatestring-method-sqlserverresultset.md)|Actualiza la columna designada con un valor **string**.|  
|[updateSQLXML](../../../connect/jdbc/reference/updatesqlxml-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de tipo **SQLXML**.|  
|[updateTime](../../../connect/jdbc/reference/updatetime-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de hora.|  
|[updateTimestamp](../../../connect/jdbc/reference/updatetimestamp-method-sqlserverresultset.md)|Actualiza la columna designada con un valor de marca de tiempo.|  
|[wasNull](../../../connect/jdbc/reference/wasnull-method-sqlserverresultset.md)|Comprueba si la última lectura del valor dio como resultado un valor NULL.|  
  
## <a name="inherited-methods"></a>Métodos heredados  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Consulte también  
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
