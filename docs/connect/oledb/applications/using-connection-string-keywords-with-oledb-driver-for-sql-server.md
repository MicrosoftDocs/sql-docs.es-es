---
title: Uso de palabras clave de cadena de conexión con OLE DB Driver
description: Algunas API de Microsoft OLE DB Driver for SQL Server usan cadenas de conexión, que son una lista de palabras clave y valores que identifican atributos de conexión concretos.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: vanto
ms.technology: connectivity
ms.topic: conceptual
f1_keywords:
- sql13.swb.connecttoserver.options.registeredservers.f1
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], connection string keywords
- MSOLEDBSQL, connection string keywords
- connection strings [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, connection string keywords
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7b0a42cbec8c1feb253140c1534b8f9f2a3142eb
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506399"
---
# <a name="using-connection-string-keywords-with-ole-db-driver-for-sql-server"></a>Uso de palabras clave de cadena de conexión con el controlador OLE DB para SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Algunas API de OLE DB Driver for SQL Server usan cadenas de conexión para especificar atributos de conexión. Las cadenas de conexión son una lista de palabras clave y valores asociados; cada palabra clave identifica un atributo de conexión determinado.  
  
> [!NOTE]
> El controlador OLE DB para SQL Server permite la ambigüedad en las cadenas de conexión para mantener la compatibilidad con versiones anteriores (por ejemplo, algunas palabras clave pueden especificarse más de una vez y pueden permitirse palabras clave en conflicto cuya resolución se base en la posición o en la precedencia). En futuras versiones de OLE DB Driver for SQL Server no se permitirán ambigüedades en las cadenas de conexión. Al modificar aplicaciones, es recomendable usar el controlador OLE DB para SQL Server a fin de eliminar cualquier dependencia sobre la ambigüedad de las cadenas de conexión.  
  
 En las siguientes secciones se describen las palabras clave que pueden utilizarse con OLE DB Driver for SQL Server y los Objetos de datos ActiveX (ADO) cuando se utiliza OLE DB Driver for SQL Server como proveedor de datos.  

## <a name="ole-db-driver-connection-string-keywords"></a>Palabras clave de cadena de conexión de OLE DB Driver  

 Las aplicaciones OLE DB pueden inicializar los objetos de origen de datos de dos formas:  
  
-   **IDBInitialize::Initialize**  
  
-   **IDataInitialize::GetDataSource**  
  
 En el primer caso, se puede usar una cadena de proveedor para inicializar las propiedades de conexión estableciendo la propiedad DBPROP_INIT_PROVIDERSTRING en el conjunto de propiedades DBPROPSET_DBINIT. En el segundo caso, se puede pasar una cadena de inicialización al método **IDataInitialize::GetDataSource** para inicializar las propiedades de conexión. Ambos métodos inicializan las mismas propiedades de conexión OLE DB, pero se utilizan conjuntos diferentes de palabras clave. El conjunto de palabras clave usado por **IDataInitialize::GetDataSource** es, como mínimo, la descripción de propiedades del grupo de propiedades de inicialización.  
  
 En cualquier configuración de cadena de proveedor que tenga una propiedad OLE DB correspondiente establecida en algún valor predeterminado o establecida explícitamente en un valor, el valor de la propiedad OLE DB invalidará la configuración de la cadena de proveedor.  
  
 Las propiedades booleanas establecidas en las cadenas de proveedor a través de los valores DBPROP_INIT_PROVIDERSTRING se establecen con los valores `yes` y `no`. Las propiedades booleanas establecidas en las cadenas de inicialización mediante **IDataInitialize::GetDataSource** se establecen con los valores `true` y `false`.  
  
 Las aplicaciones que usan **IDataInitialize::GetDataSource** también pueden usar las palabras clave empleadas por **IDBInitialize::Initialize**, pero solo para las propiedades que no tienen un valor predeterminado. Si una aplicación usa tanto la palabra clave **IDataInitialize::GetDataSource** como la palabra clave **IDBInitialize::Initialize** en la cadena de inicialización, se usa el valor de palabra clave **IDataInitialize::GetDataSource**. Se recomienda que las aplicaciones no usen palabras clave **IDBInitialize::Initialize** en cadenas de conexión **IDataInitialize:GetDataSource**, puesto que es posible que este comportamiento no se mantenga en versiones futuras.  
  
> [!NOTE]  
>  Una cadena de conexión pasada a través de **IDataInitialize::GetDataSource** se convierte en propiedades y se aplica por medio de **IDBProperties::SetProperties**. Si los servicios del componente encuentran la descripción de propiedad en **IDBProperties::GetPropertyInfo**, esta propiedad se aplica como propiedad independiente. De lo contrario, se aplicará a través de la propiedad DBPROP_PROVIDERSTRING. Por ejemplo, si se especifica la cadena de conexión **Data Source=server1;Server=server2**, **Data Source** se establecerá como propiedad, pero **Server** irá a una cadena de proveedor.  
  
 Si especifica varias instancias de la misma propiedad específica del proveedor, se utilizará el primer valor de la primera propiedad.  

## <a name="using-idbinitializeinitialize"></a>Uso de IDBInitialize::Initialize

 Las cadenas de conexión usadas por las aplicaciones OLE DB que emplean DBPROP_INIT_PROVIDERSTRING con **IDBInitialize::Initialize** tienen la siguiente sintaxis:  
  
 - `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 - `empty-string ::=`  
  
 - `attribute ::= attribute-keyword=[{]attribute-value[}]`  
  
 - `attribute-value ::= character-string`  
  
 - `attribute-keyword ::= identifier`  
  
 Los valores de atributo pueden incluirse opcionalmente entre llaves y es una práctica recomendable. Esto evita que se produzcan problemas cuando los valores de atributo contienen caracteres no alfanuméricos. Se supone que la primera llave de cierre termina el valor, de modo que los valores no pueden contener caracteres de llave de cierre.  
  
 Un carácter de espacio después del signo `=` de una palabra clave de cadena de conexión se interpreta como un literal, incluso aunque el valor vaya entre comillas.  
  
 En la tabla siguiente se describen las palabras clave que pueden utilizarse con DBPROP_INIT_PROVIDERSTRING.  
  
|Palabra clave|Propiedad de inicialización|Descripción|  
|-------------|-----------------------------|-----------------|  
|**Addr**|SSPROP_INIT_NETWORKADDRESS|Sinónimo de **Address**.|  
|**Dirección**|SSPROP_INIT_NETWORKADDRESS|Dirección de red del servidor en el que se ejecuta una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **Address** suele ser el nombre de red del servidor, pero también puede ser otros nombres, como una canalización, una dirección IP o un puerto TCP/IP y una dirección de socket.<br /><br /> Si especifica una dirección IP, asegúrese de que los protocolos TCP/IP o de canalizaciones con nombre estén habilitados en el Administrador de configuración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> El valor de **Address** tiene prioridad sobre el valor que se pasa a **Server** en las cadenas de conexión cuando se usa OLE DB Driver for SQL Server. Tenga también en cuenta que `Address=;` se conecta al servidor especificado en la palabra clave **Server**, mientras que `Address= ;, Address=.;`, `Address=localhost;` y `Address=(local);` establecen una conexión al servidor local.<br /><br /> La sintaxis completa de la palabra clave **Address** es la siguiente:<br /><br /> [_protocol_ **:** ]_Address_[ **,** _port |pipe\pipename_]<br /><br /> _protocolo_ puede ser **tcp** (TCP/IP), **lpc** (memoria compartida) o **np** (canalizaciones con nombre). Para más información sobre protocolos, consulte [Configuración de protocolos de cliente](../../../database-engine/configure-windows/configure-client-protocols.md).<br /><br /> Si no se especifica un objeto _protocol_ ni la palabra clave **Network**, OLE DB Driver for SQL Server utilizará el orden de protocolo especificado en Configuration Manager de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> *port* es el puerto al que se va a conectar en el servidor especificado. De forma predeterminada, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa el puerto 1433.|   
|**APP**|SSPROP_INIT_APPNAME|Cadena que identifica la aplicación.|  
|**ApplicationIntent**|SSPROP_INIT_APPLICATIONINTENT|Declara el tipo de carga de trabajo de la aplicación al conectarse a un servidor. Los valores posibles son `ReadOnly` y `ReadWrite`.<br /><br /> El valor predeterminado es `ReadWrite`. Para más información sobre la compatibilidad de OLE DB Driver for SQL Server con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consulte [Compatibilidad de OLE DB Driver for SQL Server con alta disponibilidad y recuperación ante desastres](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**AttachDBFileName**|SSPROP_INIT_FILENAME|Nombre del archivo principal (incluido el nombre de la ruta de acceso completa) de una base de datos adjuntable. Para usar **AttachDBFileName**, debe especificar también el nombre de la base de datos con la palabra clave Database de la cadena del proveedor. Si la base de datos se ha adjuntado previamente, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no vuelve a adjuntarla (utiliza la base de datos adjuntada como valor predeterminado para la conexión).|  
|**Authentication**<a href="#table1_1"><sup id="table1_authmode">**1**</sup></a>|SSPROP_AUTH_MODE|Especifica la autenticación de SQL o Active Directory que se usa. Los valores válidos son:<br/><ul><li>`(not set)`: modo de autenticación determinado por otras palabras clave.</li><li>`ActiveDirectoryPassword:`Autenticación de identificador de usuario y contraseña con una identidad de Azure Active Directory.</li><li>`ActiveDirectoryIntegrated:` autenticación integrada con una identidad de Azure Active Directory.</li><br/>**NOTA:** La palabra clave `ActiveDirectoryIntegrated` también puede usarse para la autenticación de Windows en SQL Server. Reemplaza las palabras clave de autenticación `Integrated Security` (o `Trusted_Connection`). Se **recomienda** que las aplicaciones que usen palabras clave `Integrated Security` (o `Trusted_Connection`) o sus propiedades correspondientes establezcan el valor de la palabra clave `Authentication` (o su propiedad correspondiente) en `ActiveDirectoryIntegrated` para habilitar el nuevo comportamiento de cifrado y validación de certificados.<br/><br/><li>`ActiveDirectoryInteractive:` autenticación interactiva con una identidad de Azure Active Directory. Este método admite Azure Multi-Factor Authentication (MFA). </li><li>`ActiveDirectoryMSI:` autentiación de [identidad administrada (MSI)](/azure/active-directory/managed-identities-azure-resources/overview). En el caso de una identidad asignada por el usuario, el identificador de usuario se establece en el identificador de objeto de la identidad del usuario.</li><li>`ActiveDirectoryServicePrincipal:` Autenticación con una entidad de servicio de Azure Active Directory. El identificador de usuario debe establecerse en el identificador de la aplicación (cliente). La contraseña debe establecerse en el secreto de la aplicación (cliente).</li><li>`SqlPassword:` para la autenticación con identificador de usuario y contraseña.</li><br/>**NOTA:** Se **recomienda** que las aplicaciones que usan la autenticación de `SQL Server` establezcan el valor de la palabra clave `Authentication` (o su propiedad correspondiente) en `SqlPassword` para habilitar el [nuevo comportamiento de cifrado y validación de certificados](../features/using-azure-active-directory.md#encryption-and-certificate-validation).</ul>|
|**Auto Translate**|SSPROP_INIT_AUTOTRANSLATE|Sinónimo de **AutoTranslate**.|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|Configura la traducción de caracteres OEM/ANSI. Los valores reconocidos son `yes` y `no`.|  
|**Base de datos**|DBPROP_INIT_CATALOG|Nombre de la base de datos.|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|Especifica el modo de administración de tipos de datos que debe utilizarse. Los valores reconocidos son `0` para los tipos de datos del proveedor y `80` para los tipos de datos de SQL Server 2000.|  
|**Encrypt**<a href="#table1_1"><sup>**1**</sup></a>|SSPROP_INIT_ENCRYPT|Especifica si los datos deben cifrarse antes de enviarse a través de la red. Los valores posibles son `yes` y `no`. El valor predeterminado es `no`.|  
|**FailoverPartner**|SSPROP_INIT_FAILOVERPARTNER|Nombre del servidor de conmutación por error utilizado para la creación de reflejo de la base de datos.|  
|**FailoverPartnerSPN**|SSPROP_INIT_FAILOVERPARTNERSPN|SPN del asociado de conmutación por error. El valor predeterminado es una cadena vacía. Una cadena vacía hace que OLE DB Driver for SQL Server utilice el SPN predeterminado generado por el proveedor.|  
|**Lenguaje**|SSPROPT_INIT_CURRENTLANGUAGE|Lenguaje [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**MarsConn**|SSPROP_INIT_MARSCONNECTION|Habilita o deshabilita conjuntos de resultados activos múltiples (MARS) en la conexión si el servidor es [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o posterior. Los valores posibles son `yes` y `no`. El valor predeterminado es `no`.|  
|**MultiSubnetFailover**|SSPROP_INIT_MULTISUBNETFAILOVER|Especifique siempre **MultiSubnetFailover=Yes** al conectarse a la escucha de grupo de disponibilidad de un grupo de disponibilidad de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o a una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **MultiSubnetFailover=Yes** configura el controlador OLE DB para SQL Server para proporcionar una detección del servidor activo y una conexión con él más rápidas. Los valores posibles son `Yes` y `No`. El valor predeterminado es `No`. Por ejemplo:<br /><br /> `MultiSubnetFailover=Yes`<br /><br /> Para más información sobre la compatibilidad de OLE DB Driver for SQL Server con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consulte [Compatibilidad de OLE DB Driver for SQL Server con alta disponibilidad y recuperación ante desastres](../../oledb/features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**Net**|SSPROP_INIT_NETWORKLIBRARY|Sinónimo de **Network**.|  
|**Network**|SSPROP_INIT_NETWORKLIBRARY|Biblioteca de red que se utiliza para establecer una conexión a una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en la organización.|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|Sinónimo de **Network**.|  
|**PacketSize**|SSPROP_INIT_PACKETSIZE|Tamaño de paquete de flujo TDS. El valor predeterminado es 0 (el servidor determinará el valor real).|  
|**PersistSensitive**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|Acepta las cadenas `yes` y `no` como valores. Al usar `no`, no se permite que el objeto de origen de datos conserve ninguna información confidencial de autenticación.|  
|**PWD**|DBPROP_AUTH_PASSWORD|Contraseña de inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Server**|DBPROP_INIT_DATASOURCE|Nombre de una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. El valor debe ser el nombre de un servidor de la red, una dirección IP o el nombre de un alias del Administrador de configuración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Si no se especifica, se establece una conexión a la instancia predeterminada en el equipo local.<br /><br /> La palabra clave **Address** reemplaza a la palabra clave **Server**.<br /><br /> Puede conectarse a la instancia predeterminada en el servidor local especificando uno de los valores siguientes:<br /><br /> **Server=;**<br /><br /> **Server=.;**<br /><br /> **Server=(local);**<br /><br /> **Server=(local);**<br /><br /> **Server=(localhost);**<br /><br /> **Server=(localdb)\\** *nombreDeInstancia* **;**<br /><br /> Para más información sobre la compatibilidad con LocalDB, consulte [Compatibilidad de OLE DB Driver for SQL Server con LocalDB](../../oledb/features/oledb-driver-for-sql-server-support-for-localdb.md).<br /><br /> Para especificar una instancia con nombre de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], anexe **\\** _InstanceName_.<br /><br /> Cuando no se especifica ningún servidor, se establece una conexión a la instancia predeterminada en el equipo local.<br /><br /> Si especifica una dirección IP, asegúrese de que los protocolos TCP/IP o de canalizaciones con nombre estén habilitados en el Administrador de configuración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> La sintaxis completa de la palabra clave **Server** es la siguiente:<br /><br /> **Server=** [_protocol_ **:** ]*Server*[ **,** _port_]<br /><br /> _protocolo_ puede ser **tcp** (TCP/IP), **lpc** (memoria compartida) o **np** (canalizaciones con nombre).<br /><br /> A continuación se muestra un ejemplo de la especificación de una canalización con nombre:<br /><br /> `np:\\.\pipe\MSSQL$MYINST01\sql\query`<br /><br /> La línea anterior especifica el protocolo de canalización con nombre (`np`), una canalización con nombre en el equipo local (`\\.\pipe`), el nombre de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (`MSSQL$MYINST01`) y el nombre predeterminado de la canalización con nombre (`sql/query`).<br /><br /> Si no se especifica *un objeto protocol* ni la palabra clave **Network**, OLE DB Driver for SQL Server utilizará el orden de protocolo especificado en Configuration Manager de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> *port* es el puerto al que se va a conectar en el servidor especificado. De forma predeterminada, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa el puerto 1433.<br /><br /> Se omiten los espacios al principio del valor pasado a  **Server** en las cadenas de conexión cuando se utiliza OLE DB Driver for SQL Server.|   
|**ServerSPN**|SSPROP_INIT_SERVERSPN|SPN del servidor. El valor predeterminado es una cadena vacía. Una cadena vacía hace que OLE DB Driver for SQL Server utilice el SPN predeterminado generado por el proveedor.|  
|**Tiempo de espera**|DBPROP_INIT_TIMEOUT|Cantidad de tiempo (en segundos) que hay que esperar a que se complete la inicialización del origen de datos.|  
|**TransparentNetworkIPResolution**|SSPROP_INIT_TNIR|Afecta la secuencia de conexión cuando la primera dirección IP resuelta del nombre de host no responde y hay varias direcciones IP asociadas con el nombre de host. TNIR interactúa con MultiSubnetFailover para proporcionar otras secuencias de conexión. Los valores posibles son `Yes` y `No`. El valor predeterminado es `Yes`. Para más información, vea [Uso de resolución de IP de red transparente](../../oledb/features/using-transparent-network-ip-resolution.md).|  
|**Trusted_Connection**|DBPROP_AUTH_INTEGRATED|Si es `yes`, indica a OLE DB Driver for SQL Server que utilice la autenticación de Windows para la validación del inicio de sesión. De lo contrario, OLE DB Driver for SQL Server usará un nombre de usuario y una contraseña de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para la validación del inicio de sesión, y deben especificarse las palabras clave PWD y UID.|  
|**TrustServerCertificate**<a href="#table1_1"><sup>**1**</sup></a>|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|Acepta las cadenas `yes` y `no` como valores. El valor predeterminado es `no`, que significa que se validará el certificado del servidor.|  
|**UID**|DBPROP_AUTH_USERID|Nombre de inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**UseFMTONLY**|SSPROP_INIT_USEFMTONLY|Controla cómo se recuperan los metadatos al conectarse a [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] y versiones más recientes. Los valores posibles son `yes` y `no`. El valor predeterminado es `no`.<br /><br />De forma predeterminada, OLE DB Driver for SQL Server usa los procedimientos almacenados [sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) y [sp_describe_undeclared_parameters](../../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md) para recuperar metadatos. Estos procedimientos almacenados tienen algunas limitaciones (por ejemplo, generarán un error al trabajar con tablas temporales). Al establecer **UseFMTONLY** en `yes`, se indica al controlador que utilice [SET FMTONLY](../../../t-sql/statements/set-fmtonly-transact-sql.md) en su lugar para la recuperación de metadatos.|  
|**UseProcForPrepare**|SSPROP_INIT_USEPROCFORPREP|Esta palabra clave ha quedado en desuso y OLE DB Driver for SQL Server omite su valor.|  
|**WSID**|SSPROP_INIT_WSID|Identificador de la estación de trabajo.|  
  
<b id="table1_1">[1]:</b> para mejorar la seguridad, el comportamiento de cifrado y validación de certificados se modifica al usar las propiedades de inicialización de token de acceso o autenticación, o sus palabras clave de cadena de conexión correspondientes. Para obtener más información, vea [Cifrado y validación de certificados](../features/using-azure-active-directory.md#encryption-and-certificate-validation).

## <a name="using-idatainitializegetdatasource"></a>Uso de IDataInitialize::GetDataSource

 Las cadenas de conexión empleadas por las aplicaciones OLE DB que usan **IDataInitialize::GetDataSource** tienen la siguiente sintaxis:  
  
 - `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 - `empty-string ::=`  
  
 - `attribute ::= attribute-keyword=[quote]attribute-value[quote]`  
  
 - `attribute-value ::= character-string`  
  
 - `attribute-keyword ::= identifier`  
  
 - `quote ::= " | '`  
  
 El uso de la propiedad debe cumplir la sintaxis permitida en su ámbito. Por ejemplo, **WSID** usa llaves ( **{}** ) para los caracteres de comillas y **Application Name** usa caracteres de comillas simples ( **'** ) o dobles ( **"** ). Solo se pueden entrecomillar las propiedades de cadena. Al intentar entrecomillar un entero o una propiedad enumerada se producirá un error `Connection String does not conform to OLE DB specification`.  
  
 Los valores de atributo pueden incluirse opcionalmente entre comillas simples o dobles, y es una práctica recomendada. Esto evita que se produzcan problemas cuando los valores contienen caracteres no alfanuméricos. El carácter de comillas que se utilice también puede aparecer en los valores, siempre y cuando aparezca entre comillas dobles.  
  
 Un carácter de espacio después del signo = de una palabra clave de cadena de conexión se interpreta como un literal, aun cuando el valor esté entre comillas.  
  
 Si una cadena de conexión tiene más de una de las propiedades enumeradas en la tabla siguiente, se utilizará el valor de la última propiedad.  
  
 En la tabla siguiente se describen las palabras clave que pueden usarse con **IDataInitialize::GetDataSource**:  
  
|Palabra clave|Propiedad de inicialización|Descripción|  
|-------------|-----------------------------|-----------------|  
|**Token de acceso**<a href="#table2_1"><sup id="table2_accesstoken">**1**</sup></a>|SSPROP_AUTH_ACCESS_TOKEN|token de acceso usado para autenticarse en Azure Active Directory. <br/><br/>**NOTA:** Es un error especificar esta palabra clave y también las palabras clave de cadena de conexión `UID`, `PWD`, `Trusted_Connection` o `Authentication` o sus propiedades o palabras clave correspondientes.|
|**Nombre de la aplicación**|SSPROP_INIT_APPNAME|Cadena que identifica la aplicación.|  
|**Application Intent**|SSPROP_INIT_APPLICATIONINTENT|Declara el tipo de carga de trabajo de la aplicación al conectarse a un servidor. Los valores posibles son `ReadOnly` y `ReadWrite`.<br /><br /> El valor predeterminado es `ReadWrite`. Para más información sobre la compatibilidad de OLE DB Driver for SQL Server con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consulte [Compatibilidad de OLE DB Driver for SQL Server con alta disponibilidad y recuperación ante desastres](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**Authentication**<a href="#table2_1"><sup>**1**</sup></a>|SSPROP_AUTH_MODE|Especifica la autenticación de SQL o Active Directory que se usa. Los valores válidos son:<br/><ul><li>`(not set)`: modo de autenticación determinado por otras palabras clave.</li><li>`ActiveDirectoryPassword:`Autenticación de identificador de usuario y contraseña con una identidad de Azure Active Directory.</li><li>`ActiveDirectoryIntegrated:` autenticación integrada con una identidad de Azure Active Directory.</li><br/>**NOTA:** La palabra clave `ActiveDirectoryIntegrated` también puede usarse para la autenticación de Windows en SQL Server. Reemplaza las palabras clave de autenticación `Integrated Security` (o `Trusted_Connection`). Se **recomienda** que las aplicaciones que usen palabras clave `Integrated Security` (o `Trusted_Connection`) o sus propiedades correspondientes establezcan el valor de la palabra clave `Authentication` (o su propiedad correspondiente) en `ActiveDirectoryIntegrated` para habilitar el nuevo comportamiento de cifrado y validación de certificados.<br/><br/><li>`ActiveDirectoryInteractive:` autenticación interactiva con una identidad de Azure Active Directory. Este método admite Azure Multi-Factor Authentication (MFA). </li><li>`ActiveDirectoryMSI:` autentiación de [identidad administrada (MSI)](/azure/active-directory/managed-identities-azure-resources/overview). En el caso de una identidad asignada por el usuario, el identificador de usuario se establece en el identificador de objeto de la identidad del usuario.</li><li>`ActiveDirectoryServicePrincipal:` Autenticación con una entidad de servicio de Azure Active Directory. El identificador de usuario debe establecerse en el identificador de la aplicación (cliente). La contraseña debe establecerse en el secreto de la aplicación (cliente).</li><li>`SqlPassword:` para la autenticación con identificador de usuario y contraseña.</li><br/>**NOTA:** Se **recomienda** que las aplicaciones que usan la autenticación de `SQL Server` establezcan el valor de la palabra clave `Authentication` (o su propiedad correspondiente) en `SqlPassword` para habilitar el [nuevo comportamiento de cifrado y validación de certificados](../features/using-azure-active-directory.md#encryption-and-certificate-validation).</ul>|
|**Auto Translate**|SSPROP_INIT_AUTOTRANSLATE|Sinónimo de **AutoTranslate**.|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|Configura la traducción de caracteres OEM/ANSI. Los valores reconocidos son `true` y `false`.|  
|**Connect Timeout**|DBPROP_INIT_TIMEOUT|Cantidad de tiempo (en segundos) que hay que esperar a que se complete la inicialización del origen de datos.|  
|**Current Language**|SSPROPT_INIT_CURRENTLANGUAGE|Nombre del lenguaje [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Data Source** (Origen de datos)|DBPROP_INIT_DATASOURCE|Nombre de una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en la organización.<br /><br /> Si no se especifica, se establece una conexión a la instancia predeterminada en el equipo local.<br /><br /> Para obtener más información sobre la sintaxis válida para las direcciones, vea la descripción de la palabra clave **Server** en este tema.|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|Especifica el modo de administración de tipos de datos que debe utilizarse. Los valores reconocidos son `0` para los tipos de datos del proveedor y `80` para los tipos de datos de [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)].|  
|**Failover Partner**|SSPROP_INIT_FAILOVERPARTNER|Nombre del servidor de conmutación por error utilizado para la creación de reflejo de la base de datos.|  
|**Failover Partner SPN**|SSPROP_INIT_FAILOVERPARTNERSPN|SPN del asociado de conmutación por error. El valor predeterminado es una cadena vacía. Una cadena vacía hace que OLE DB Driver for SQL Server utilice el SPN predeterminado generado por el proveedor.|  
|**Catálogo original**|DBPROP_INIT_CATALOG|Nombre de la base de datos.|  
|**Nombre de archivo inicial**|SSPROP_INIT_FILENAME|Nombre del archivo principal (incluido el nombre de la ruta de acceso completa) de una base de datos adjuntable. Para usar **AttachDBFileName**, debe especificar también el nombre de la base de datos con la palabra clave DATABASE de la cadena del proveedor. Si la base de datos se ha adjuntado previamente, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no vuelve a adjuntarla (utiliza la base de datos adjuntada como valor predeterminado para la conexión).|  
|**Seguridad integrada**|DBPROP_AUTH_INTEGRATED|Acepta el valor `SSPI` para la autenticación de Windows.|  
|**MARS Connection**|SSPROP_INIT_MARSCONNECTION|Habilita o deshabilita conjuntos de resultados activos múltiples (MARS) en la conexión. Los valores reconocidos son `true` y `false`. El valor predeterminado es `false`.|  
|**MultiSubnetFailover**|SSPROP_INIT_MULTISUBNETFAILOVER|Especifique siempre **MultiSubnetFailover=True** al conectarse a la escucha de grupo de disponibilidad de un grupo de disponibilidad de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o a una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **MultiSubnetFailover=True** configura el controlador OLE DB para SQL Server para proporcionar una detección del servidor activo y una conexión con él más rápidas. Los valores posibles son `True` y `False`. El valor predeterminado es `False`. Por ejemplo:<br /><br /> `MultiSubnetFailover=True`<br /><br /> Para más información sobre la compatibilidad de OLE DB Driver for SQL Server con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consulte [Compatibilidad de OLE DB Driver for SQL Server con alta disponibilidad y recuperación ante desastres](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**Network Address**|SSPROP_INIT_NETWORKADDRESS|Dirección de red de una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en la organización.<br /><br /> Para obtener más información sobre la sintaxis válida para las direcciones, vea la descripción de la palabra clave **Address** en este tema.|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|Biblioteca de red que se utiliza para establecer una conexión a una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en la organización.|  
|**Packet Size**|SSPROP_INIT_PACKETSIZE|Tamaño de paquete de flujo TDS. El valor predeterminado es 0 (el servidor determinará el valor real).|  
|**Contraseña**|DBPROP_AUTH_PASSWORD|Contraseña de inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Persist Security Info**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|Acepta las cadenas `true` y `false` como valores. Si es `false`, no se permite que el objeto de origen de datos conserve ninguna información confidencial de autenticación.|  
|**Proveedor**||En el caso de OLE DB Driver for SQL Server, debe ser MSOLEDBSQL.|  
|**Server SPN**|SSPROP_INIT_SERVERSPN|SPN del servidor. El valor predeterminado es una cadena vacía. Una cadena vacía hace que OLE DB Driver for SQL Server utilice el SPN predeterminado generado por el proveedor.|  
|**TransparentNetworkIPResolution**|SSPROP_INIT_TNIR|Afecta la secuencia de conexión cuando la primera dirección IP resuelta del nombre de host no responde y hay varias direcciones IP asociadas con el nombre de host. TNIR interactúa con MultiSubnetFailover para proporcionar otras secuencias de conexión. Los valores posibles son `True` y `False`. El valor predeterminado es `True`. Para más información, vea [Uso de resolución de IP de red transparente](../../oledb/features/using-transparent-network-ip-resolution.md).|  
|**Certificado de servidor de confianza**<a href="#table2_1"><sup>**1**</sup></a>|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|Acepta las cadenas `true` y `false` como valores. El valor predeterminado es `false`, que significa que se validará el certificado del servidor.|  
|**Utilizar cifrado para los datos**<a href="#table2_1"><sup>**1**</sup></a>|SSPROP_INIT_ENCRYPT|Especifica si los datos deben cifrarse antes de enviarse a través de la red. Los valores posibles son `true` y `false`. El valor predeterminado es `false`.|  
|**Use FMTONLY**|SSPROP_INIT_USEFMTONLY|Controla cómo se recuperan los metadatos al conectarse a [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] y versiones más recientes. Los valores posibles son `true` y `false`. El valor predeterminado es `false`.<br /><br />De forma predeterminada, OLE DB Driver for SQL Server usa los procedimientos almacenados [sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) y [sp_describe_undeclared_parameters](../../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md) para recuperar metadatos. Estos procedimientos almacenados tienen algunas limitaciones (por ejemplo, generarán un error al trabajar con tablas temporales). Al establecer **Use FMTONLY** en `true`, se indica al controlador que utilice [SET FMTONLY](../../../t-sql/statements/set-fmtonly-transact-sql.md) en su lugar para la recuperación de metadatos.|  
|**Id. de usuario**|DBPROP_AUTH_USERID|Nombre de inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Workstation ID**|SSPROP_INIT_WSID|Identificador de la estación de trabajo.|  
  
<b id="table2_1">[1]:</b> para mejorar la seguridad, el comportamiento de cifrado y validación de certificados se modifica cuando se usan las propiedades de inicialización de token de acceso y autenticación o sus palabras clave de cadena de conexión correspondientes. Para más información, consulte [Cifrado y validación de certificados](../features/using-azure-active-directory.md#encryption-and-certificate-validation).

 > [!NOTE]
 > En la cadena de conexión, la propiedad `Old Password` establece SSPROP_AUTH_OLD_PASSWORD, que es la contraseña actual (posiblemente expirada) que no está disponible a través de una propiedad de cadena del proveedor.  
  
## <a name="activex-data-objects-ado-connection-string-keywords"></a>Palabras clave de la cadena de conexión Objetos de datos ActiveX (ADO)  

 Las aplicaciones ADO establecen la propiedad **ConnectionString** de los objetos **ADODBConnection** o proporcionan una cadena de conexión como parámetro al método **Open** de los objetos **ADODBConnection**.  
  
 Las aplicaciones ADO también pueden usar las palabras clave que emplea el método **IDBInitialize::Initialize** de OLE DB, pero solo para las propiedades que no tienen un valor predeterminado. Si una aplicación usa tanto palabras clave de ADO como palabras clave **IDBInitialize::Initialize** en la cadena de inicialización, se usa el valor de palabra clave de ADO. Se recomienda que las aplicaciones utilicen solamente palabras clave de cadena de conexión ADO.  
  
 Las cadenas de conexión utilizadas por ADO presentan la sintaxis siguiente:  
  
 - `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 - `empty-string ::=`  
  
 - `attribute ::= attribute-keyword=["]attribute-value["]`  
  
 - `attribute-value ::= character-string`  
  
 - `attribute-keyword ::= identifier`  
  
 Los valores de atributo pueden incluirse opcionalmente entre comillas dobles y es una práctica recomendable. Esto evita que se produzcan problemas cuando los valores contienen caracteres no alfanuméricos. Los valores de atributo no pueden incluir comillas dobles.  
  
 En la tabla siguiente se describen las palabras clave que pueden utilizarse con una cadena de conexión ADO.  
  
|Palabra clave|Propiedad de inicialización|Descripción|  
|-------------|-----------------------------|-----------------|  
|**Token de acceso**<a href="#table3_1"><sup id="table3_accesstoken">**1**</sup></a>|SSPROP_AUTH_ACCESS_TOKEN|token de acceso usado para autenticarse en Azure Active Directory.<br/><br/>**NOTA:** Es un error especificar esta palabra clave y también las palabras clave de cadena de conexión `UID`, `PWD`, `Trusted_Connection` o `Authentication` o sus propiedades o palabras clave correspondientes.|
|**Application Intent**|SSPROP_INIT_APPLICATIONINTENT|Declara el tipo de carga de trabajo de la aplicación al conectarse a un servidor. Los valores posibles son `ReadOnly` y `ReadWrite`.<br /><br /> El valor predeterminado es `ReadWrite`. Para más información sobre la compatibilidad de OLE DB Driver for SQL Server con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consulte [Compatibilidad de OLE DB Driver for SQL Server con alta disponibilidad y recuperación ante desastres](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**Nombre de la aplicación**|SSPROP_INIT_APPNAME|Cadena que identifica la aplicación.|  
|**Authentication**<a href="#table3_1"><sup>**1**</sup></a>|SSPROP_AUTH_MODE|Especifica la autenticación de SQL o Active Directory que se usa. Los valores válidos son:<br/><ul><li>`(not set)`: modo de autenticación determinado por otras palabras clave.</li><li>`ActiveDirectoryPassword:`Autenticación de identificador de usuario y contraseña con una identidad de Azure Active Directory.</li><li>`ActiveDirectoryIntegrated:` autenticación integrada con una identidad de Azure Active Directory.</li><br/>**NOTA:** La palabra clave `ActiveDirectoryIntegrated` también puede usarse para la autenticación de Windows en SQL Server. Reemplaza las palabras clave de autenticación `Integrated Security` (o `Trusted_Connection`). Se **recomienda** que las aplicaciones que usen palabras clave `Integrated Security` (o `Trusted_Connection`) o sus propiedades correspondientes establezcan el valor de la palabra clave `Authentication` (o su propiedad correspondiente) en `ActiveDirectoryIntegrated` para habilitar el nuevo comportamiento de cifrado y validación de certificados.<br/><br/><li>`ActiveDirectoryInteractive:` autenticación interactiva con una identidad de Azure Active Directory. Este método admite Azure Multi-Factor Authentication (MFA). </li><li>`ActiveDirectoryMSI:` autentiación de [identidad administrada (MSI)](/azure/active-directory/managed-identities-azure-resources/overview). En el caso de una identidad asignada por el usuario, el identificador de usuario se establece en el identificador de objeto de la identidad del usuario.</li><li>`ActiveDirectoryServicePrincipal:` Autenticación con una entidad de servicio de Azure Active Directory. El identificador de usuario debe establecerse en el identificador de la aplicación (cliente). La contraseña debe establecerse en el secreto de la aplicación (cliente).</li><li>`SqlPassword:` para la autenticación con identificador de usuario y contraseña.</li><br/>**NOTA:** Se **recomienda** que las aplicaciones que usan la autenticación de `SQL Server` establezcan el valor de la palabra clave `Authentication` (o su propiedad correspondiente) en `SqlPassword` para habilitar el [nuevo comportamiento de cifrado y validación de certificados](../features/using-azure-active-directory.md#encryption-and-certificate-validation).</ul>|
|**Auto Translate**|SSPROP_INIT_AUTOTRANSLATE|Sinónimo de **AutoTranslate**.|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|Configura la traducción de caracteres OEM/ANSI. Los valores reconocidos son `true` y `false`.|  
|**Connect Timeout**|DBPROP_INIT_TIMEOUT|Cantidad de tiempo (en segundos) que hay que esperar a que se complete la inicialización del origen de datos.|  
|**Current Language**|SSPROPT_INIT_CURRENTLANGUAGE|Nombre del lenguaje [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Data Source** (Origen de datos)|DBPROP_INIT_DATASOURCE|Nombre de una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en la organización.<br /><br /> Si no se especifica, se establece una conexión a la instancia predeterminada en el equipo local.<br /><br /> Para obtener más información sobre la sintaxis válida para las direcciones, vea la descripción de la palabra clave **Server** en este tema.|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|Especifica el modo de administración de tipos de datos que se utilizará. Los valores reconocidos son `0` para los tipos de datos del proveedor y `80` para los tipos de datos de SQL Server 2000.|  
|**Failover Partner**|SSPROP_INIT_FAILOVERPARTNER|Nombre del servidor de conmutación por error utilizado para la creación de reflejo de la base de datos.|  
|**Failover Partner SPN**|SSPROP_INIT_FAILOVERPARTNERSPN|SPN del asociado de conmutación por error. El valor predeterminado es una cadena vacía. Una cadena vacía hace que OLE DB Driver for SQL Server utilice el SPN predeterminado generado por el proveedor.|  
|**Catálogo original**|DBPROP_INIT_CATALOG|Nombre de la base de datos.|  
|**Nombre de archivo inicial**|SSPROP_INIT_FILENAME|Nombre del archivo principal (incluido el nombre de la ruta de acceso completa) de una base de datos adjuntable. Para usar **AttachDBFileName**, debe especificar también el nombre de la base de datos con la palabra clave **DATABASE** de la cadena del proveedor. Si la base de datos se ha adjuntado previamente, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no vuelve a adjuntarla, sino que utiliza la base de datos adjuntada como valor predeterminado para la conexión.|  
|**Seguridad integrada**|DBPROP_AUTH_INTEGRATED|Acepta el valor `SSPI` para la autenticación de Windows.|  
|**MARS Connection**|SSPROP_INIT_MARSCONNECTION|Habilita o deshabilita conjuntos de resultados activos múltiples (MARS) en la conexión si el servidor es [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o posterior. Los valores reconocidos son `true` y `false`; el valor predeterminado es `false`.|  
|**MultiSubnetFailover**|SSPROP_INIT_MULTISUBNETFAILOVER|Especifique siempre **MultiSubnetFailover=True** al conectarse a la escucha de grupo de disponibilidad de un grupo de disponibilidad de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o a una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **MultiSubnetFailover=True** configura el controlador OLE DB para SQL Server para proporcionar una detección del servidor activo y una conexión con él más rápidas. Los valores posibles son `True` y `False`. El valor predeterminado es `False`. Por ejemplo:<br /><br /> `MultiSubnetFailover=True`<br /><br /> Para más información sobre la compatibilidad de OLE DB Driver for SQL Server con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consulte [Compatibilidad de OLE DB Driver for SQL Server con alta disponibilidad y recuperación ante desastres](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**Network Address**|SSPROP_INIT_NETWORKADDRESS|Dirección de red de una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en la organización.<br /><br /> Para obtener más información sobre la sintaxis válida para las direcciones, vea la descripción de la palabra clave **Address** en este tema.|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|Biblioteca de red que se utiliza para establecer una conexión a una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en la organización.|  
|**Packet Size**|SSPROP_INIT_PACKETSIZE|Tamaño de paquete de flujo TDS. El valor predeterminado es 0 (el servidor determinará el valor real).|  
|**Contraseña**|DBPROP_AUTH_PASSWORD|Contraseña de inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Persist Security Info**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|Acepta las cadenas `true` y `false` como valores. Si es `false`, no se permite que el objeto de origen de datos conserve ninguna información confidencial de autenticación.|  
|**Proveedor**||En el caso de OLE DB Driver for SQL Server, el valor es `MSOLEDBSQL`.|  
|**Server SPN**|SSPROP_INIT_SERVERSPN|SPN del servidor. El valor predeterminado es una cadena vacía. Una cadena vacía hace que OLE DB Driver for SQL Server utilice el SPN predeterminado generado por el proveedor.|  
|**TransparentNetworkIPResolution**|SSPROP_INIT_TNIR|Afecta la secuencia de conexión cuando la primera dirección IP resuelta del nombre de host no responde y hay varias direcciones IP asociadas con el nombre de host. TNIR interactúa con MultiSubnetFailover para proporcionar otras secuencias de conexión. Los valores posibles son `True` y `False`. El valor predeterminado es `True`. Para más información, vea [Uso de resolución de IP de red transparente](../../oledb/features/using-transparent-network-ip-resolution.md).|  
|**Certificado de servidor de confianza**<a href="#table3_1"><sup>**1**</sup></a>|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|Acepta las cadenas `true` y `false` como valores. El valor predeterminado es `false`, que significa que se validará el certificado del servidor.|  
|**Utilizar cifrado para los datos**<a href="#table3_1"><sup>**1**</sup></a>|SSPROP_INIT_ENCRYPT|Especifica si los datos deben cifrarse antes de enviarse a través de la red. Los valores posibles son `true` y `false`. El valor predeterminado es `false`.|  
|**Use FMTONLY**|SSPROP_INIT_USEFMTONLY|Controla cómo se recuperan los metadatos al conectarse a [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] y versiones más recientes. Los valores posibles son `true` y `false`. El valor predeterminado es `false`.<br /><br />De forma predeterminada, OLE DB Driver for SQL Server usa los procedimientos almacenados [sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) y [sp_describe_undeclared_parameters](../../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md) para recuperar metadatos. Estos procedimientos almacenados tienen algunas limitaciones (por ejemplo, generarán un error al trabajar con tablas temporales). Al establecer **Use FMTONLY** en `true`, se indica al controlador que utilice [SET FMTONLY](../../../t-sql/statements/set-fmtonly-transact-sql.md) en su lugar para la recuperación de metadatos.|  
|**Id. de usuario**|DBPROP_AUTH_USERID|Nombre de inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Workstation ID**|SSPROP_INIT_WSID|Identificador de la estación de trabajo.|  
  
<b id="table3_1">[1]:</b> para mejorar la seguridad, el comportamiento de cifrado y validación de certificados se modifica cuando se usan las propiedades de inicialización de token de acceso y autenticación o sus palabras clave de cadena de conexión correspondientes. Para más información, consulte [Cifrado y validación de certificados](../features/using-azure-active-directory.md#encryption-and-certificate-validation).

 > [!NOTE] 
 > En la cadena de conexión, la propiedad "Contraseña anterior" establece SSPROP_AUTH_OLD_PASSWORD, que es la contraseña actual (posiblemente expirada) que no está disponible a través de una propiedad de cadena del proveedor.  
  
## <a name="see-also"></a>Consulte también  

 [Compilación de aplicaciones con el controlador OLE DB para SQL Server](building-applications-with-oledb-driver-for-sql-server.md)