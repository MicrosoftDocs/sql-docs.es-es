---
description: Miembros SQLServerDataSource
title: Miembros SQLServerDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 7e749bc5-d765-4864-be2b-7822d4c20c09
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2e16d07888c7a86ecd1b3296ee234794ae7aac00
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178315"
---
# <a name="sqlserverdatasource-members"></a>Miembros SQLServerDataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  En las siguientes tablas se enumeran los miembros que expone la clase [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).  
  
## <a name="constructors"></a>Constructores  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[SQLServerDataSource ()](../../../connect/jdbc/reference/sqlserverdatasource-constructor.md)|Inicializa una instancia nueva de la clase [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
  
## <a name="fields"></a>Fields  
 Ninguno.  
  
## <a name="inherited-fields"></a>Campos heredados  
 Ninguno.  
  
## <a name="methods"></a>Métodos  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[getApplicationIntent](../../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)|Devuelve el valor de la propiedad de conexión **applicationIntent**.|  
|[getApplicationName](../../../connect/jdbc/reference/getapplicationname-method-sqlserverdatasource.md)|Devuelve el nombre de la aplicación.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)|Intenta establecer una conexión con el origen de datos que representa este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[getDatabaseName](../../../connect/jdbc/reference/getdatabasename-method-sqlserverdatasource.md)|Devuelve el nombre de la base de datos.|  
|[getDisableStatementPooling](../../../connect/jdbc/reference/getdisablestatementpooling-method-sqlserverdatasource.md)|Devuelve el valor de la propiedad de conexión **disableStatementPooling**. Este valor controla si la agrupación de instrucciones está habilitada o no para esta conexión.|  
|[getEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/getenableprepareonfirstpreparedstatementcall-method-sqlserverdatasource.md)|Devuelve el valor de la propiedad de conexión **enablePrepareOnFirstPreparedStatementCall**.|  
|[getEncrypt](../../../connect/jdbc/reference/getencrypt-method-sqlserverdatasource.md)|Devuelve un valor **Boolean** que indica si la propiedad encrypt está habilitada.|  
|[getDescription](../../../connect/jdbc/reference/getdescription-method-sqlserverdatasource.md)|Devuelve una descripción del origen de datos.|  
|[getFailoverPartner](../../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md)|Devuelve el nombre del servidor de conmutación por error que se usa en la configuración de la creación de reflejo de la base de datos.|  
|[getHostNameInCertificate](../../../connect/jdbc/reference/gethostnameincertificate-method-sqlserverdatasource.md)|Devuelve el nombre del host que se usa para validar el certificado de Seguridad de la capa de transporte (TLS) de SQL Server, antes conocida como Capa de sockets seguros (SSL).|  
|[getInstanceName](../../../connect/jdbc/reference/getinstancename-method-sqlserverdatasource.md)|Devuelve el nombre de instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md)|Devuelve un valor **Boolean** que indica si la propiedad lastUpdateCount está habilitada.|  
|[getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)|Devuelve un valor de tipo **int** que indica el número de milisegundos que la base de datos espera antes de notificar un tiempo de espera de bloqueo.|  
|[getLoginTimeout](../../../connect/jdbc/reference/getlogintimeout-method-sqlserverdatasource.md)|Devuelve el número de segundos que este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) esperará mientras intenta efectuar una conexión.|  
|[getLogWriter](../../../connect/jdbc/reference/getlogwriter-method-sqlserverdatasource.md)|Devuelve un flujo de salida de caracteres que se va a utilizar para todos los mensajes de registro y seguimiento.|  
|[getMultiSubnetFailover](../../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)|Devuelve el valor de la propiedad de conexión **multiSubnetFailover**.|  
|[getPacketSize](../../../connect/jdbc/reference/getpacketsize-method-sqlserverdatasource.md)|Devuelve el tamaño del paquete de red actual que se usa para establecer la comunicación con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y se especifica en bytes.|  
|[getPortNumber](../../../connect/jdbc/reference/getportnumber-method-sqlserverdatasource.md)|Devuelve el número de puerto actual que se utiliza para la comunicación con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getReference](../../../connect/jdbc/reference/getreference-method-sqlserverdatasource.md)|Devuelve una referencia a este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverdatasource.md)|Devuelve el método de almacenamiento en búfer de respuesta para este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md)|Devuelve el tipo de cursor predeterminado que se utiliza para todos los conjuntos de resultados creados mediante este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md)|Devuelve un valor de tipo **Boolean** que indica si está habilitado el envío de parámetros de cadena al servidor en formato UNICODE.|  
|[getSendTimeAsDatetime](../../../connect/jdbc/reference/getsendtimeasdatetime-method-sqlserverdatasource.md)|Devuelve el valor de la propiedad de conexión **SendTimeAsDatetime**.|  
|[getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md)|Devuelve el nombre del equipo que ejecuta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/getserverpreparedstatementdiscardthreshold-method-sqlserverdatasource.md)|Devuelve el valor de la propiedad de conexión **serverPreparedStatementDiscardThreshold**.|  
|[getStatementPoolingCacheSize](../../../connect/jdbc/reference/getstatementpoolingcachesize-method-sqlserverdatasource.md)|Devuelve el tamaño de la memoria caché de instrucciones preparadas para esta conexión.|  
|[getTrustManagerClass](../../../connect/jdbc/reference/gettrustmanagerclass-method-sqlserverdatasource.md)|Devuelve el valor de cadena de la propiedad de conexión TrustManagerClass.|  
|[getTrustManagerConstructorArg](../../../connect/jdbc/reference/gettrustmanagerconstructorarg-method-sqlserverdatasource.md)|Devuelve el valor de cadena de la propiedad de conexión TrustManagerConstructorArg.|  
|[getTrustServerCertificate](../../../connect/jdbc/reference/gettrustservercertificate-method-sqlserverdatasource.md)|Devuelve un valor de tipo **Boolean** que indica si la propiedad trustServerCertificate está habilitada.|  
|[getTrustStore](../../../connect/jdbc/reference/gettruststore-method-sqlserverdatasource.md)|Devuelve la ruta de acceso (incluido el nombre de archivo) del archivo trustStore del certificado.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md)|Devuelve la dirección URL que se utiliza para la conexión al origen de datos.|  
|[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md)|Devuelve el nombre de usuario que se utiliza para conectar el origen de datos.|  
|[getUseSQLServerBaseDate](../../../connect/jdbc/reference/getsendtimeasdatetime-method-sqlserverdatasource.md)|Devuelve el valor de la propiedad de conexión useSQLServerBaseDate.|  
|[getWorkstationID](../../../connect/jdbc/reference/getworkstationid-method-sqlserverdatasource.md)|Devuelve el nombre del nombre del equipo cliente que se utiliza para la conexión al origen de datos.|  
|[getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)|Devuelve un valor de tipo **Boolean** que indica si está habilitada la conversión de estados SQL a estados conforme a XOPEN.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverdatasource.md)|Indica si este objeto de origen de datos es un contenedor para la interfaz especificada.|  
|[setApplicationIntent](../../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)|Establece el valor de la propiedad de conexión **applicationIntent**.|  
|[setApplicationName](../../../connect/jdbc/reference/setapplicationname-method-sqlserverdatasource.md)|Establece el nombre de la aplicación.|  
|[setAuthenticationScheme](../../../connect/jdbc/reference/setauthenticationscheme-sqlserverdatasource.md)|Indica el tipo de seguridad integrada que desea que use la aplicación.|  
|[setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)|Establece el nombre de la base de datos a la que se va a efectuar la conexión.|  
|[setDescription](../../../connect/jdbc/reference/setdescription-method-sqlserverdatasource.md)|Establece la descripción del origen de datos.|  
|[setDisableStatementPooling](../../../connect/jdbc/reference/setdisablestatementpooling-method-sqlserverdatasource.md)|Establece la agrupación de instrucciones en true o false.|  
|[setEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/setenableprepareonfirstpreparedstatementcall-method-sqlserverdatasource.md)|Especifica el nuevo valor de la propiedad de conexión **enablePrepareOnFirstPreparedStatementCall**.|  
|[setEncrypt](../../../connect/jdbc/reference/setencrypt-method-sqlserverdatasource.md)|Establece un valor de tipo **Boolean** que indica si la propiedad encrypt está habilitada.|  
|[setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)|Establece el nombre del servidor de conmutación por error que se usa en la configuración de la creación de reflejo de la base de datos.|  
|[setHostNameInCertificate](../../../connect/jdbc/reference/sethostnameincertificate-method-sqlserverdatasource.md)|Establece el nombre del host que se va a usar para validar el certificado de Seguridad de la capa de transporte (TLS) de SQL Server, antes conocida como Capa de sockets seguros (SSL).|  
|[setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)|Establece el nombre de instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setIntegratedSecurity](../../../connect/jdbc/reference/setintegratedsecurity-method-sqlserverdatasource.md)|Establece un valor de tipo **Boolean** que indica si la propiedad integratedSecurity está habilitada.|  
|[setLastUpdateCount](../../../connect/jdbc/reference/setlastupdatecount-method-sqlserverdatasource.md)|Establece un valor de tipo **Boolean** que indica si la propiedad lastUpdateCount está habilitada.|  
|[setLockTimeout](../../../connect/jdbc/reference/setlocktimeout-method-sqlserverdatasource.md)|Devuelve un valor de tipo **int** que indica el número de milisegundos que transcurrirán antes de que la base de datos notifique un tiempo de espera de bloqueo.|  
|[setLoginTimeout](../../../connect/jdbc/reference/setlogintimeout-method-sqlserverdatasource.md)|Establece el número de segundos que este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) esperará mientras intenta efectuar una conexión.|  
|[setLogWriter](../../../connect/jdbc/reference/setlogwriter-method-sqlserverdatasource.md)|Establece un flujo de salida de caracteres que se utilizará para todos los mensajes de registro y seguimiento.|  
|[setMultiSubnetFailover](../../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)|Establece el valor de la propiedad de conexión **multiSubnetFailover**.|  
|[setPacketSize](../../../connect/jdbc/reference/setpacketsize-method-sqlserverdatasource.md)|Establece el tamaño del paquete de red actual que se usa para establecer la comunicación con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y se especifica en bytes.|  
|[setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)|Establece la contraseña que se utiliza para conectarse a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md)|Establece el número de puerto para la comunicación con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverdatasource.md)|Establece el modo de almacenamiento en búfer de respuesta para las conexiones creadas mediante este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[setSelectMethod](../../../connect/jdbc/reference/setselectmethod-method-sqlserverdatasource.md)|Establece el tipo de cursor predeterminado que se utiliza para todos los conjuntos de resultados creados mediante este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[setSendStringParametersAsUnicode](../../../connect/jdbc/reference/setsendstringparametersasunicode-method-sqlserverdatasource.md)|Establece un valor de tipo **Boolean** que indica si está habilitado el envío de parámetros de cadena al servidor en formato UNICODE.|  
|[setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md)|Especifica cómo enviar los valores java.sql.Time al servidor.|  
|[setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)|Establece el nombre del equipo que ejecuta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/setserverpreparedstatementdiscardthreshold-method-sqlserverdatasource.md)|Establece el nuevo valor de la propiedad de conexión **serverPreparedStatementDiscardThreshold**.|  
|[setStatementPoolingCacheSize](../../../connect/jdbc/reference/setstatementpoolingcachesize-method-sqlserverdatasource.md)|Establece el tamaño de la memoria caché de instrucciones preparadas para esta conexión.|  
|[setTrustManagerClass](../../../connect/jdbc/reference/settrustmanagerclass-method-sqlserverdatasource.md)|Establece el valor de cadena de la propiedad de conexión TrustManagerClass.|  
|[setTrustManagerConstructorArg](../../../connect/jdbc/reference/settrustmanagerconstructorarg-method-sqlserverdatasource.md)|Establece el valor de cadena de la propiedad de conexión TrustManagerConstructorArg.|  
|[setTrustServerCertificate](../../../connect/jdbc/reference/settrustservercertificate-method-sqlserverdatasource.md)|Establece un valor de tipo **Boolean** que indica si la propiedad trustServerCertificate está habilitada.|  
|[setTrustStore](../../../connect/jdbc/reference/settruststore-method-sqlserverdatasource.md)|Establece la ruta de acceso (incluido el nombre de archivo) del archivo trustStore del certificado.|  
|[setTrustStorePassword](../../../connect/jdbc/reference/settruststorepassword-method-sqlserverdatasource.md)|Establece la contraseña que se usa para comprobar la integridad de los datos trustStore.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md)|Establece la dirección URL que se utiliza para la conexión al origen de datos.|  
|[setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md)|Establece el nombre de usuario que se utiliza para conectar el origen de datos.|  
|[setWorkstationID](../../../connect/jdbc/reference/setworkstationid-method-sqlserverdatasource.md)|Establece el nombre del nombre del equipo cliente que se utiliza para la conexión al origen de datos.|  
|[setXopenStates](../../../connect/jdbc/reference/setxopenstates-method-sqlserverdatasource.md)|Establece un valor de tipo **Boolean** que indica si está habilitada la conversión de estados SQL a estados conforme a XOPEN.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverdatasource.md)|Devuelve un objeto que implementa la interfaz especificada para permitir el acceso a los métodos específicos de [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Métodos heredados  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Consulte también  
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
