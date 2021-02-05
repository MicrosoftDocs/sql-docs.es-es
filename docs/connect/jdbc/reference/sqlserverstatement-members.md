---
description: Miembros SQLServerStatement
title: Miembros SQLServerStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 828cbaa9-ea7a-4986-95c3-5ba0d7d01d83
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3eecadb565935db91ce97c211b46a73536087dda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180000"
---
# <a name="sqlserverstatement-members"></a>Miembros SQLServerStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  En las siguientes tablas se enumeran los miembros que expone la clase [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
## <a name="constructors"></a>Constructores  
 Ninguno.  
  
## <a name="fields"></a>Fields  
 Ninguno.  
  
## <a name="inherited-fields"></a>Campos heredados  
  
|NOMBRE|Descripción|  
|----------|-----------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="methods"></a>Métodos  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverstatement.md)|Agrega el comando SQL determinado a la lista de comandos actual para este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|Cancela la instrucción de SQL que se esté ejecutando en esos momentos en este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverstatement.md)|Vacía la lista actual de comandos SQL para este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|Borra todas las advertencias que se notifican en este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverstatement.md)|Libera inmediatamente la base de datos de este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) y los recursos de JDBC en lugar de esperar a que se liberen automáticamente.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md)|Ejecuta la instrucción SQL determinada, que puede devolver varios resultados.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverstatement.md)|Envía un lote de comandos a la base de datos para que se ejecute. Si todos los comandos se ejecutan correctamente, devuelve una matriz de recuentos de actualizaciones.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverstatement.md)|Ejecuta la instrucción SQL determinada y devuelve un objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) único.|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverstatement.md)|Ejecuta la instrucción SQL determinada, que puede ser una instrucción INSERT, UPDATE, MERGE o DELETE; o una instrucción SQL que no devuelve nada, como una instrucción DDL de SQL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|Recupera el objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) que generó este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|Recupera la dirección para capturar las filas de las tablas de base de datos que es el valor predeterminado para los conjuntos de resultados que se generaron a partir de este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|Recupera el número de filas del conjunto de resultados que sea el tamaño de captura predeterminado para los objetos del conjunto de resultados que generó este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|Recupera cualquier clave generada automáticamente que se cree como resultado de ejecutar este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|Recupera el número máximo de bytes que se pueden devolver para valores de columna de caracteres y binarios en un objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto [ISQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|Recupera el número máximo de filas que puede contener un objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|Se desplaza al próximo resultado de este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|Recupera el número de segundos que [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] esperará la ejecución de este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|Recupera el modo de almacenamiento en búfer de respuesta para este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|Recupera el resultado actual como un objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|Recupera la simultaneidad del conjunto de resultados para los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|Recupera la capacidad de alojamiento del conjunto de resultados para los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|Recupera el tipo del conjunto de resultados para los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que haya generado este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|Recupera el resultado actual como un recuento de actualizaciones.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|Recupera la primera advertencia notificada por las llamadas en este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|Indica si se ha cerrado este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|Devuelve un valor que indica si una instrucción se puede agregar al grupo de instrucciones proporcionado por el usuario.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md)|Indica si este objeto de instrucción es un contenedor para la interfaz especificada.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|Establece el nombre de cursor de SQL para la cadena determinada, el cual utilizarán métodos ulteriores.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|Establece el modo de procesamiento de escape.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|Ofrece una sugerencia al controlador JDBC sobre la dirección en la que se deberían procesar las filas del conjunto de resultados.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|Ofrece una sugerencia al controlador JDBC sobre el número de filas que se deberían capturar en la base de datos cuando se necesitan más filas.|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|Establece el límite para el número máximo de bytes en una columna [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que almacena valores de caracteres o binarios en el número establecido de bytes.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|Establece el límite del número máximo de filas que cualquier objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) puede contener para el número determinado.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|Solicita que una instrucción se agrupe o no.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|Establece el número de segundos que esperará el controlador antes de que se ejecute un objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) según el número de segundos determinado.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|Establece el modo de almacenamiento en búfer de respuesta para este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) en **String full** sin distinción entre mayúsculas y minúsculas o en **adaptive**.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md)|Devuelve un objeto que implementa la interfaz especificada para permitir el acceso a los métodos específicos de [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Métodos heredados  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Consulte también  
 [Clase SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
