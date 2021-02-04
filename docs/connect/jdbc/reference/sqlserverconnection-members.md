---
description: Miembros SQLServerConnection
title: Miembros SQLServerConnection | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 3115a533-756b-4c78-aee9-4ba7253c85e0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed9783e0b91dd79fe7b52c80b05fa7693213759c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172834"
---
# <a name="sqlserverconnection-members"></a>Miembros SQLServerConnection
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  En las siguientes tablas se enumeran los miembros que expone la clase [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="constructors"></a>Constructores  
 Ninguno.  
  
## <a name="fields"></a>Campos  
  
|NOMBRE|Descripción|  
|----------|-----------------|  
|[TRANSACTION_SNAPSHOT](../../../connect/jdbc/reference/transaction-snapshot-field-sqlserverconnection.md)|Se utiliza para especificar el nivel de aislamiento de transacción de instantáneas.|  
  
## <a name="inherited-fields"></a>Campos heredados  
  
|Clase heredada de:|Descripción|  
|---------------------------|-----------------|  
|java.sql.Connection|TRANSACTION_NONE, TRANSACTION_READ_COMMITTED, TRANSACTION_READ_UNCOMMITTED, TRANSACTION_REPEATABLE_READ, TRANSACTION_SERIALIZABLE|  
  
## <a name="methods"></a>Métodos  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverconnection.md)|Borra todas las advertencias que se han notificado para este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverconnection.md)|Libera inmediatamente la base de datos para este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) y los recursos de JDBC en lugar de esperar a que se liberen automáticamente.|  
|[closeUnreferencedPreparedStatementHandles](../../../connect/jdbc/reference/closeunreferencedpreparedstatementhandles-method-sqlserverconnection.md)|Fuerza las solicitudes de cancelación de preparación para cualquier instrucción preparada descartada pendiente que se vaya a ejecutar.| 
|[commit](../../../connect/jdbc/reference/commit-method-sqlserverconnection.md)|Realiza todos los cambios efectuados desde la confirmación anterior o realiza una reversión permanente y libera los bloqueos de la base de datos que incorpore este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) en esos momentos.|  
|[createBlob](../../../connect/jdbc/reference/createblob-method-sqlserverconnection.md)|Crea un objeto **java.sql.Blob** sin datos.|  
|[createClob](../../../connect/jdbc/reference/createclob-method-sqlserverconnection.md)|Crea un objeto **java.sql.Clob** sin datos.|  
|[createNClob](../../../connect/jdbc/reference/createnclob-method-sqlserverconnection.md)|Crea un objeto **java.sql.NClob** sin datos.|  
|[createStatement](../../../connect/jdbc/reference/createstatement-method-sqlserverconnection.md)|Crea un objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) para enviar instrucciones SQL a la base de datos.|  
|[createSQLXML](../../../connect/jdbc/reference/createsqlxml-method-sqlserverconnection.md)|Crea un objeto **java.sql.SQLXML** sin datos.|  
|[getAutoCommit](../../../connect/jdbc/reference/getautocommit-method-sqlserverconnection.md)|Recupera el modo de confirmación automática actual para este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getCatalog](../../../connect/jdbc/reference/getcatalog-method-sqlserverconnection.md)|Recupera el nombre del catálogo actual para este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[Método getClientConnectionID &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/getclientconnectionid-method-sqlserverconnection.md)|Obtiene el identificador de conexión del intento de conexión más reciente, independientemente de que dicho intento fuera correcto o erróneo.|  
|[getClientInfo](../../../connect/jdbc/reference/getclientinfo-method-sqlserverconnection.md)|Recupera información con respecto a las propiedades de información del cliente que admita el controlador JDBC.|  
|[getDisableStatementPooling](../../../connect/jdbc/reference/getdisablestatementpooling-method-sqlserverconnection.md)|Devuelve el valor de la propiedad de conexión **disableStatementPooling**. Este valor controla si la agrupación de instrucciones está habilitada o no para esta conexión.|
|[getDiscardedServerPreparedStatementCount](../../../connect/jdbc/reference/getdiscardedserverpreparedstatementcount-method-sqlserverconnection.md)|Devuelve el número de acciones de cancelación de preparación de instrucciones preparadas pendientes.|
|[getEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/getenableprepareonfirstpreparedstatementcall-method-sqlserverconnection.md)|Devuelve el valor de la propiedad de conexión **enablePrepareOnFirstPreparedStatementCall**.|
|[getHoldability](../../../connect/jdbc/reference/getholdability-method-sqlserverconnection.md)|Recupera la capacidad de alojamiento actual de los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que se crean mediante el uso del objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverconnection.md)|Recupera un objeto [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md) que contiene los metadatos sobre la base de datos para la que este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) representa una conexión.|  
|[getServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/getserverpreparedstatementdiscardthreshold-method-sqlserverconnection.md)|Devuelve el valor de la propiedad de conexión **serverPreparedStatementDiscardThreshold**.|  
|[getStatementHandleCacheEntryCount](../../../connect/jdbc/reference/getstatementhandlecacheentrycount-method-sqlserverconnection.md)|Devuelve el número actual de identificadores de instrucción preparada agrupados.|  
|[getStatementPoolingCacheSize](../../../connect/jdbc/reference/getstatementpoolingcachesize-method-sqlserverconnection.md)|Devuelve el tamaño de la memoria caché de instrucciones preparadas para esta conexión.|  
|[getTransactionIsolation](../../../connect/jdbc/reference/gettransactionisolation-method-sqlserverconnection.md)|Recupera el nivel de aislamiento de transacción actual para este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getTypeMap](../../../connect/jdbc/reference/gettypemap-method-sqlserverconnection.md)|Recupera el objeto Map que está asociado a este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverconnection.md)|Recupera la primera advertencia que notifican las llamadas en este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverconnection.md)|Indica si se ha cerrado este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverconnection.md)|Indica si este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) está en modo de solo lectura.|  
|[isStatementPoolingEnabled](../../../connect/jdbc/reference/isstatementpoolingenabled-method-sqlserverconnection.md)|Devuelve si la agrupación de instrucciones está habilitada o no para esta conexión.|  
|[isValid](../../../connect/jdbc/reference/isvalid-method-sqlserverconnection.md)|Indica si este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) se ha cerrado y si sigue siendo válido.|  
|[nativeSQL](../../../connect/jdbc/reference/nativesql-method-sqlserverconnection.md)|Convierte la instrucción SQL establecida en gramática SQL nativa del servidor de la base de datos.|  
|[prepareCall](../../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md)|Crea un objeto [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) para llamar a procedimientos almacenados de la base de datos.|  
|[prepareStatement](../../../connect/jdbc/reference/preparestatement-method-sqlserverconnection.md)|Crea un objeto [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) para enviar las instrucciones SQL parametrizadas a la base de datos.|  
|[releaseSavepoint](../../../connect/jdbc/reference/releasesavepoint-method-sqlserverconnection.md)|Quita el objeto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) especificado de la transacción actual.|  
|[rollback](../../../connect/jdbc/reference/rollback-method-sqlserverconnection.md)|Deshace todos los cambios realizados en la transacción actual y libera cualquier bloque en la base de datos que contenga en esos momentos este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[setAutoCommit](../../../connect/jdbc/reference/setautocommit-method-sqlserverconnection.md)|Establece el modo de confirmación automática para este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) en el estado determinado.|  
|[setCatalog](../../../connect/jdbc/reference/setcatalog-method-sqlserverconnection.md)|Establece el nombre del catálogo especificado para seleccionar un subespacio de la base de datos de este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) donde pueda funcionar.|  
|[setClientInfo](../../../connect/jdbc/reference/setclientinfo-method-sqlserverconnection.md)|Establece el valor de las propiedades de la información del cliente.|  
|[setDisableStatementPooling](../../../connect/jdbc/reference/setdisablestatementpooling-method-sqlserverconnection.md)|Establece la agrupación de instrucciones en true o false.|  
|[setEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/setenableprepareonfirstpreparedstatementcall-method-sqlserverconnection.md)|Especifica el nuevo valor de la propiedad de conexión **enablePrepareOnFirstPreparedStatementCall**.|  
|[setHoldability](../../../connect/jdbc/reference/setholdability-method-sqlserverconnection.md)|Cambia la capacidad de alojamiento de los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que se han creado mediante el uso de este objeto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) según la capacidad de alojamiento determinada.|  
|[setReadOnly](../../../connect/jdbc/reference/setreadonly-method-sqlserverconnection.md)|Establece este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) en modo de solo lectura como sugerencia para que el controlador JDBC pueda habilitar las optimizaciones de la base de datos.|  
|[setSavepoint](../../../connect/jdbc/reference/setsavepoint-method-sqlserverconnection.md)|Crea un punto de retorno sin nombre en la transacción actual y devuelve el nuevo objeto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) que lo representa.|  
|[setServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/setserverpreparedstatementdiscardthreshold-method-sqlserverconnection.md)|Establece el nuevo valor de la propiedad de conexión **serverPreparedStatementDiscardThreshold**.|  
|[setStatementPoolingCacheSize](../../../connect/jdbc/reference/setstatementpoolingcachesize-method-sqlserverconnection.md)|Establece el tamaño de la memoria caché de instrucciones preparadas para esta conexión.|  
|[setTransactionIsolation](../../../connect/jdbc/reference/settransactionisolation-method-sqlserverconnection.md)|Intenta cambiar el nivel de aislamiento de transacción para este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) según el que se haya determinado.|  
|[setTypeMap](../../../connect/jdbc/reference/settypemap-method-sqlserverconnection.md)|Instala el objeto TypeMap determinado como la asignación de tipos para este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
  
## <a name="inherited-methods"></a>Métodos heredados  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.lang.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Consulte también  
 [Clase SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
