---
description: Miembros SQLServerConnectionPoolDataSource
title: Miembros SQLServerConnectionPoolDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: dac0337e-8088-488c-a25a-801a2190f6ca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 614bcf5295b776cdd61fc9bb7b070fe1e26cbb69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178351"
---
# <a name="sqlserverconnectionpooldatasource-members"></a>Miembros SQLServerConnectionPoolDataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  En las siguientes tablas se enumeran los miembros que expone la clase [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md).  
  
## <a name="constructors"></a>Constructores  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[SQLServerConnectionPoolDataSource ()](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-constructor.md)|Inicializa una nueva instancia de la clase [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md).|  
  
## <a name="fields"></a>Fields  
 Ninguno.  
  
## <a name="inherited-fields"></a>Campos heredados  
 Ninguno.  
  
## <a name="methods"></a>Métodos  
  
|Nombre|Descripción|  
|----------|-----------------|  
|[getApplicationIntent](../../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el valor de la propiedad de conexión **applicationIntent**.|  
|[getApplicationName](../../../connect/jdbc/reference/getapplicationname-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el nombre de la aplicación.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Intenta establecer una conexión con el origen de datos que representa el objeto DataSource.|  
|[getDatabaseName](../../../connect/jdbc/reference/getdatabasename-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el nombre de la base de datos.|  
|[getDescription](../../../connect/jdbc/reference/getdescription-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve una descripción del origen de datos.|  
|[getFailoverPartner](../../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el nombre del servidor de conmutación por error que se utiliza en una configuración de creación de reflejo de la base de datos.|  
|[getInstanceName](../../../connect/jdbc/reference/getinstancename-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el nombre de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve un valor **Boolean** que indica si la propiedad lastUpdateCount está habilitada.|  
|[getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve un valor **int** que indica el número de milisegundos que transcurrirán antes de que la base de datos notifique un tiempo de espera de bloqueo.|  
|[getLoginTimeout](../../../connect/jdbc/reference/getlogintimeout-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el número de segundos que transcurrirán hasta que este objeto DataSource intente efectuar una conexión.|  
|[getLogWriter](../../../connect/jdbc/reference/getlogwriter-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve un flujo de salida de caracteres que se va a utilizar en todos los mensajes de registro y seguimiento.|  
|[getMultiSubnetFailover](../../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el valor de la propiedad de conexión **multiSubnetFailover**.|  
|[getPooledConnection](../../../connect/jdbc/reference/getpooledconnection-method-sqlserverconnectionpooldatasource.md)|Intenta establecer una conexión a bases de datos físicas que se pueda utilizar como una conexión agrupada.|  
|[getPortNumber](../../../connect/jdbc/reference/getportnumber-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el número de puerto actual que se utiliza para comunicar con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getReference](../../../connect/jdbc/reference/getreference-method-sqlserverconnectionpooldatasource.md)|Devuelve una referencia a este objeto DataSource.|  
|[getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el tipo de cursor predeterminado que se utiliza para todos los conjuntos de resultados que se crean con el objeto DataSource.|  
|[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve un valor **Boolean** que indica si está habilitado el envío de parámetros de tipo String al servidor en formato UNICODE.|  
|[getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el nombre del equipo que está ejecutando [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve la dirección URL que se utiliza para conectar al origen de datos.|  
|[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el nombre de usuario que se utiliza para conectar al origen de datos.|  
|[getWorkstationID](../../../connect/jdbc/reference/getworkstationid-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve el nombre del nombre del equipo cliente que se utiliza para conectar al origen de datos.|  
|[getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Devuelve un valor **Boolean** que indica si está habilitada la conversión de estados de SQL a estados conforme a XOPEN.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverconnectionpooldatasource.md)|Indica si este objeto es un contenedor para la interfaz especificada.|  
|[setApplicationIntent](../../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el valor de la propiedad de conexión **applicationIntent**.|  
|[setApplicationName](../../../connect/jdbc/reference/setapplicationname-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el nombre de la aplicación.|  
|[setAuthenticationSceme](../../../connect/jdbc/reference/setauthenticationscheme-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Indica el tipo de seguridad integrada que quiere que use la aplicación.|  
|[setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el nombre de la base de datos con la que se va a efectuar la conexión.|  
|[setDescription](../../../connect/jdbc/reference/setdescription-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece la descripción del origen de datos.|  
|[setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el nombre del servidor de conmutación por error que se utiliza en una configuración de creación de reflejo de la base de datos.|  
|[setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el nombre de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setIntegratedSecurity](../../../connect/jdbc/reference/setintegratedsecurity-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece un valor **Boolean** que indica si la propiedad integratedSecurity está habilitada.|  
|[setLastUpdateCount](../../../connect/jdbc/reference/setlastupdatecount-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece un valor **Boolean** que indica si la propiedad lastUpdateCount está habilitada.|  
|[setLockTimeout](../../../connect/jdbc/reference/setlocktimeout-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece un valor **int** que indica el número de milisegundos que han de transcurrir antes de que la base de datos notifique un tiempo de espera de bloqueo.|  
|[setLoginTimeout](../../../connect/jdbc/reference/setlogintimeout-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el número de segundos que transcurrirán hasta que este objeto DataSource intente efectuar una conexión.|  
|[setLogWriter](../../../connect/jdbc/reference/setlogwriter-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece un flujo de salida de caracteres que se va a utilizar para todos los mensajes de registro y seguimiento.|  
|[setMultiSubnetFailover](../../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el valor de la propiedad de conexión **multiSubnetFailover**.|  
|[setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece la contraseña que se utilizará para conectar a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el número de puerto que se va a utilizar para comunicar con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setSelectMethod](../../../connect/jdbc/reference/setselectmethod-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el tipo de cursor predeterminado que se utiliza para todos los conjuntos de resultados que se crean con el objeto DataSource.|  
|[setSendStringParametersAsUnicode](../../../connect/jdbc/reference/setsendstringparametersasunicode-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece un valor **Boolean** que indica si está habilitado el envío de parámetros de tipo String al servidor en formato UNICODE.|  
|[setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el nombre del equipo que está ejecutando [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece la dirección URL que se utiliza para conectar al origen de datos.|  
|[setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el nombre de usuario que se utiliza para conectar al origen de datos.|  
|[setWorkstationID](../../../connect/jdbc/reference/setworkstationid-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece el nombre del equipo cliente que se utiliza para conectar al origen de datos.|  
|[setXopenStates](../../../connect/jdbc/reference/setxopenstates-method-sqlserverdatasource.md)|(Se hereda de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Establece un valor **Boolean** que indica si está habilitada la conversión de estados de SQL a estados conforme a XOPEN.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverconnectionpooldatasource.md)|Devuelve un objeto que implementa la interfaz especificada para permitir el acceso a los métodos específicos de [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Métodos heredados  
  
|Clase heredada de:|Métodos|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerDataSource|getApplicationName, getConnection, getDatabaseName, getDescription, getFailoverPartner, getInstanceName, getLastUpdateCount, getLockTimeout, getLoginTimeout, getLogWriter, getPortNumber, getSelectMethod, getSendStringParametersAsUnicode, getServerName, getURL, getUser, getWorkstationID, getXopenStates, setApplicationName, setDatabaseName, setDescription, setFailoverPartner, setInstanceName, setIntegratedSecurity, setLastUpdateCount, setLockTimeout, setLoginTimeout, setLogWriter, setPassword, setPortNumber, setSelectMethod, setSendStringParametersAsUnicode, setServerName, setURL, setUser, setWorkstationID, setXopenStates|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
|javax.sql.ConnectionPoolDataSource|getLoginTimeout, getLogWriter, setLoginTimeout, setLogWriter, getPooledConnection|  
  
## <a name="see-also"></a>Consulte también  
 [Clase SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)  
  
  
