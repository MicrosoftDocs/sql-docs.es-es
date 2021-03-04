---
title: Palabras clave de la cadena de conexión y del DSN de ODBC
description: En esta página se enumeran las palabras clave para las cadenas de conexión y DSN, así como los atributos de conexión para SQLSetConnectAttr y SQLGetConnectAttr, disponibles en el controlador ODBC para SQL Server.
ms.custom: ''
ms.date: 02/04/2019
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-chojas
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: d905ac9c7f2ea02bfac04b54e247d833ae337b96
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837390"
---
# <a name="dsn-and-connection-string-keywords-and-attributes"></a>Atributos y palabras clave de cadena de conexión y DSN

En esta página se enumeran las palabras clave para las cadenas de conexión y DSN, así como los atributos de conexión para SQLSetConnectAttr y SQLGetConnectAttr, disponibles en el controlador ODBC para SQL Server.

## <a name="supported-dsnconnection-string-keywords-and-connection-attributes"></a>Atributos de conexión y palabras clave de cadena de conexión y DSN compatibles

En la tabla siguiente se enumeran los atributos y las palabras clave disponibles para cada plataforma (L: Linux; M: macOS; W: Windows). Haga clic en una palabra clave o un atributo para obtener más detalles.

| Palabra clave de cadena de conexión y DSN | Atributo de conexión | Plataforma |
|-|-|-|
| [Addr](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Dirección](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [AnsiNPW](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ANSI_NPW](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssansinpw) | LMW |
| [APP](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [ApplicationIntent](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_APPLICATION_INTENT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssapplicationintent) | LMW |
| [AttachDBFileName](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ATTACHDBFILENAME](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssattachdbfilename) | LMW |
| [Autenticación](../../connect/odbc/dsn-connection-string-attribute.md#authentication---sql_copt_ss_authentication) | [SQL_COPT_SS_AUTHENTICATION](../../connect/odbc/dsn-connection-string-attribute.md#authentication---sql_copt_ss_authentication) | LMW |
| [AutoTranslate](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_TRANSLATE](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstranslate) | LMW |
| [ColumnEncryption](../../connect/odbc/dsn-connection-string-attribute.md#columnencryption---sql_copt_ss_column_encryption) | [SQL_COPT_SS_COLUMN_ENCRYPTION](../../connect/odbc/dsn-connection-string-attribute.md#columnencryption---sql_copt_ss_column_encryption) | LMW |
| [ConnectRetryCount](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | [SQL_COPT_SS_CONNECT_RETRY_COUNT](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | W |
| [ConnectRetryInterval](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | [SQL_COPT_SS_CONNECT_RETRY_INTERVAL](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | W |
| [Base de datos](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_ATTR_CURRENT_CATALOG](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| [Descripción](../../connect/odbc/dsn-connection-string-attribute.md#description) | | LMW |
| [Controlador](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [DSN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Encrypt](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ENCRYPT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssencrypt) | LMW |
| [Failover_Partner](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_FAILOVER_PARTNER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssfailoverpartner) | W |
| [FailoverPartnerSPN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_FAILOVER_PARTNER_SPN](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | W |
| [FileDSN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [KeepAlive](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md) (v17.4+, solo DSN)| | LMW |
| [KeepAliveInterval](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md) (v17.4+, solo DSN) | | LMW |
| [KeystoreAuthentication](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [KeystorePrincipalId](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [KeystoreSecret](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [Lenguaje](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [MARS_Connection](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_MARS_ENABLED](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssmarsenabled) | LMW |
| [MultiSubnetFailover](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_MULTISUBNET_FAILOVER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssmultisubnetfailover) | LMW |
| [Net](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Network](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [PWD](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [QueryLog_On](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfquery) | W |
| [QueryLogFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY_LOG](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfquerylog) | W |
| [QueryLogTIme](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY_INTERVAL](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfqueryinterval) | W |
| [QuotedId](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_QUOTED_IDENT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssquotedident) | LMW |
| [Regional](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [SaveFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Server](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [ServerSPN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_SERVER_SPN](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| [StatsLog_On](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_DATA](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdata) | W |
| [StatsLogFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_DATA_LOG](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdatalog) | W |
| [TransparentNetworkIPResolution](../../connect/odbc/dsn-connection-string-attribute.md#transparentnetworkipresolution---sql_copt_ss_tnir) | [SQL_COPT_SS_TNIR](../../connect/odbc/dsn-connection-string-attribute.md#transparentnetworkipresolution---sql_copt_ss_tnir) | LMW |
| [Trusted_Connection](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_INTEGRATED_SECURITY](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssintegratedsecurity) | LMW |
| [TrustServerCertificate](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_TRUST_SERVER_CERTIFICATE](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstrustservercertificate) | LMW |
| [UID](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [UseFMTONLY](../../connect/odbc/dsn-connection-string-attribute.md#usefmtonly) | | LMW |
| [WSID](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| | [SQL_ATTR_ACCESS_MODE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_ACCESS_MODE) | LMW |
| | [SQL_ATTR_ASYNC_DBC_EVENT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_PCALLBACK](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_PCONTEXT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_ENABLE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_AUTO_IPD](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_AUTOCOMMIT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_AUTOCOMMIT) | LMW |
| | [SQL_ATTR_CONNECTION_DEAD](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_CONNECTION_TIMEOUT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_DBC_INFO_TOKEN](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_LOGIN_TIMEOUT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_LOGIN_TIMEOUT) | LMW |
| | [SQL_ATTR_METADATA_ID](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_ODBC_CURSORS](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_ODBC_CURSORS) | LMW |
| | [SQL_ATTR_PACKET_SIZE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_PACKET_SIZE) | LMW |
| | [SQL_ATTR_QUIET_MODE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_QUIET_MODE) | LMW |
| | [SQL_ATTR_RESET_CONNECTION](../../odbc/reference/develop-driver/upgrading-a-3-5-driver-to-a-3-8-driver.md#connection-pooling) <br> (SQL_COPT_SS_RESET_CONNECTION) | LMW |  
| | [SQL_ATTR_TRACE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_OPT_TRACE) | LMW |
| | [SQL_ATTR_TRACEFILE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_OPT_TRACEFILE) | LMW |
| | [SQL_ATTR_TRANSLATE_LIB](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TRANSLATE_DLL) | LMW |
| | [SQL_ATTR_TRANSLATE_OPTION](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TRANSLATE_OPTION) | LMW |
| | [SQL_ATTR_TXN_ISOLATION](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TXN_ISOLATION) | LMW |
| | [SQL_COPT_SS_ACCESS_TOKEN](dsn-connection-string-attribute.md#sql_copt_ss_access_token) | LMW |
| | [SQL_COPT_SS_ANSI_OEM](dsn-connection-string-attribute.md#sql_copt_ss_ansi_oem)| W |
| | [SQL_COPT_SS_AUTOBEGINTXN](dsn-connection-string-attribute.md#sql_copt_ss_autobegintxn)| LMW |
| | [SQL_COPT_SS_BCP](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbcp) | LMW |
| | [SQL_COPT_SS_BROWSE_CACHE_DATA](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) | LMW |
| | [SQL_COPT_SS_BROWSE_CONNECT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbrowseconnect) | LMW |
| | [SQL_COPT_SS_BROWSE_SERVER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbrowseserver) | LMW |
| | [SQL_COPT_SS_CEKEYSTOREDATA](dsn-connection-string-attribute.md#sql_copt_ss_cekeystoredata) | LMW |
| | [SQL_COPT_SS_CEKEYSTOREPROVIDER](dsn-connection-string-attribute.md#sql_copt_ss_cekeystoreprovider) | LMW |
| | [SQL_COPT_SS_CLIENT_CONNECTION_ID](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) | LMW |
| | [SQL_COPT_SS_CONCAT_NULL](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssconcatnull) | LMW |
| | [SQL_COPT_SS_CONNECTION_DEAD](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssconnectiondead) | LMW |
| | [SQL_COPT_SS_ENLIST_IN_DTC](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssenlistindtc) | W |
| | [SQL_COPT_SS_ENLIST_IN_XA](dsn-connection-string-attribute.md#sql_copt_ss_enlist_in_xa) | LMW |
| | [SQL_COPT_SS_FALLBACK_CONNECT](dsn-connection-string-attribute.md#sql_copt_ss_fallback_connect) | LMW |
| | [SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| | [SQL_COPT_SS_MUTUALLY_AUTHENTICATED](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| | [SQL_COPT_SS_OLDPWD](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssoldpwd) | LMW |
| | [SQL_COPT_SS_PERF_DATA_LOG_NOW](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdatalognow) | W |
| | [SQL_COPT_SS_PRESERVE_CURSORS](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsspreservecursors) | LMW |
| | [SQL_COPT_SS_SPID](../../connect/odbc/dsn-connection-string-attribute.md#sql_copt_ss_spid) (v17.5+) | LMW |
| | [SQL_COPT_SS_TXN_ISOLATION](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstxnisolation) | LMW |
| | [SQL_COPT_SS_USER_DATA](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssuserdata) | LMW |
| | [SQL_COPT_SS_WARN_ON_CP_ERROR](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsswarnoncperror) | LMW |
| [ClientCertificate](../../connect/odbc/dsn-connection-string-attribute.md#clientcertificate) | | LMW | 
| [ClientKey](../../connect/odbc/dsn-connection-string-attribute.md#clientkey) | | LMW | 


Estos son algunos de los atributos y las palabras clave de cadena de conexión que no se documentan en [Usar palabras clave de cadena de conexión con SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md), [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) y [Función SQLSetConnectAttr](../../odbc/reference/syntax/sqlsetconnectattr-function.md).

### <a name="description"></a>Descripción

Se usa para describir el origen de datos.

### <a name="sql_copt_ss_ansi_oem"></a>SQL_COPT_SS_ANSI_OEM

Controla la conversión de datos de ANSI a OEM. 

| Valor del atributo | Descripción |
|-|-|
| SQL_AO_OFF | (Valor predeterminado) No se realiza la traducción. |
| SQL_AO_ON | Se realiza la traducción. |

### <a name="sql_copt_ss_autobegintxn"></a>SQL_COPT_SS_AUTOBEGINTXN

Versión 17.6 y posterior Mientras que la confirmación automática está desactivada, controla la transacción BEGIN TRANSACTION automática después de ROLLBACK o COMMIT.

| Valor del atributo | Descripción |
|-|-|
| SQL_AUTOBEGINTXN_ON | (Predeterminado) Transacción BEGIN TRANSACTION automática después de ROLLBACK o COMMIT. |
| SQL_AUTOBEGINTXN_OFF | Ninguna transacción BEGIN TRANSACTION automática después de ROLLBACK o COMMIT. |

### <a name="sql_copt_ss_fallback_connect"></a>SQL_COPT_SS_FALLBACK_CONNECT

Controla el uso de conexiones de reserva de SQL Server. Ya no se admite.

| Valor del atributo | Descripción |
|-|-|
| SQL_FB_OFF | (Valor predeterminado) Las conexiones de reserva están deshabilitadas. |
| SQL_FB_ON | Las conexiones de reserva están habilitadas. |



## <a name="new-connection-string-keywords-and-connection-attributes"></a>Nuevos atributos de conexión y palabras clave de cadena de conexión

###  <a name="authentication---sql_copt_ss_authentication"></a>Autenticación: SQL_COPT_SS_AUTHENTICATION

Establece el modo de autenticación que se utilizará al conectarse a SQL Server. Para más información, vea [Uso de Azure Active Directory](using-azure-active-directory.md).

| Valor de palabra clave | Valor del atributo | Descripción |
|-|-|-|
| |SQL_AU_NONE|(Valor predeterminado) Sin establecer. La combinación de otros atributos determina el modo de autenticación.|
|SqlPassword|SQL_AU_PASSWORD|Autenticación de SQL Server con nombre de usuario y contraseña.|
|ActiveDirectoryIntegrated|SQL_AU_AD_INTEGRATED|Autenticación integrada de Azure Active Directory.|
|ActiveDirectoryPassword|SQL_AU_AD_PASSWORD|Autenticación de contraseña de Azure Active Directory.|
|ActiveDirectoryInteractive|SQL_AU_AD_INTERACTIVE|Autenticación interactiva de Azure Active Directory.|
|ActiveDirectoryMsi|SQL_AU_AD_MSI|Autenticación de identidad administrada de Azure Active Directory. Para la identidad asignada por el usuario, el UID se establece en el identificador de objeto de la identidad del usuario. |
| |SQL_AU_RESET|Anular. Invalida cualquier DSN o configuración de cadena de conexión.|

> [!NOTE]
> Al utilizar un atributo o palabra clave `Authentication`, especifique explícitamente la opción `Encrypt` en el valor deseado de la cadena de conexión, DSN o atributo de conexión. Para más información, consulte [Usar palabras clave de cadena de conexión con SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).

### <a name="columnencryption---sql_copt_ss_column_encryption"></a>ColumnEncryption: SQL_COPT_SS_COLUMN_ENCRYPTION

Controla el cifrado transparente de columnas (Always Encrypted). Para más información, vea [Uso de Always Encrypted (ODBC)](using-always-encrypted-with-the-odbc-driver.md).

| Valor de palabra clave | Valor del atributo | Descripción |
|-|-|-|
|habilitado|SQL_CE_ENABLED|Habilita Always Encrypted.|
|Disabled|SQL_CE_DISABLED|(Valor predeterminado) Deshabilita Always Encrypted.|
| |SQL_CE_RESULTSETONLY|Habilita solamente el descifrado (los resultados y valores devueltos).|

### <a name="transparentnetworkipresolution---sql_copt_ss_tnir"></a>TransparentNetworkIPResolution: SQL_COPT_SS_TNIR

Controla la característica de resolución de IP de red transparente, que interactúa con MultiSubnetFailover para permitir intentos de reconexión más rápidos. Para más información, vea [Uso de resolución de IP de red transparente](using-transparent-network-ip-resolution.md).

| Valor de palabra clave | Valor del atributo| Descripción |
|-|-|-|
|habilitado|SQL_IS_ON|(Predeterminado) Habilita la resolución de IP de red transparente.|
|Disabled|SQL_IS_OFF|Deshabilita la resolución de IP de red transparente.|

### <a name="usefmtonly"></a>UseFMTONLY

Controla el uso de SET FMTONLY para los metadatos al conectarse a SQL Server 2012 y versiones más recientes.

| Valor de palabra clave | Descripción |
|-|-|
|No|(Valor predeterminado) Si está disponible, use sp_describe_first_result_set para los metadatos. |
|Sí| Use SET FMTONLY para los metadatos. |


## <a name="clientcertificate"></a>ClientCertificate

Especifica el certificado que se usará para la autenticación. Las opciones son: 

| Valor de la opción | Descripción |
|-|-|
| sha1:`<hash_value>` | El controlador ODBC utiliza el hash SHA1 para localizar un certificado en el Almacén de certificados de Windows |
| subject:`<subject>` | El controlador ODBC utiliza subject para localizar un certificado en el Almacén de certificados de Windows |
| file:`<file_location>`[,password:`<password>`] | El controlador ODBC utiliza un archivo de certificado. |

En caso de que el certificado esté en formato PFX y la clave privada dentro del certificado PFX esté protegida por contraseña, se requiere la palabra clave de contraseña. Para los certificados en formatos PEM y DER, se requiere el atributo ClientKey


## <a name="clientkey"></a>ClientKey

Especifica una ubicación de archivo de la clave privada para los certificados PEM o DER especificados por el atributo ClientCertificate. Formato: 

| Valor de la opción | Descripción |
|-|-|
| file:`<file_location>`[,password:`<password>`] | Especifica la ubicación del archivo de clave privada. |

En caso de que el archivo de clave privada esté protegido por contraseña, se requiere la palabra clave de contraseña. Si la contraseña contiene algún carácter ",", se agrega automáticamente un carácter "," adicional inmediatamente después de cada uno de ellos. Por ejemplo, si la contraseña es "a, b, c", la contraseña con escape presente en la cadena de conexión es "a,, b,, c". 
    

### <a name="sql_copt_ss_access_token"></a>SQL_COPT_SS_ACCESS_TOKEN

Permite el uso de un token de acceso de Azure Active Directory para la autenticación. Para más información, vea [Uso de Azure Active Directory](using-azure-active-directory.md).

| Valor del atributo | Descripción |
|-|-|
| NULL | (Valor predeterminado) No se proporciona ningún token de acceso. |
| ACCESSTOKEN* | Puntero a un token de acceso. |

### <a name="sql_copt_ss_cekeystoredata"></a>SQL_COPT_SS_CEKEYSTOREDATA

Se comunica con una biblioteca de proveedor de almacén de claves cargada. Controla el cifrado transparente de columnas (Always Encrypted). Este atributo no tiene ningún valor predeterminado. Para más información, vea [Proveedores de almacén de claves personalizados](custom-keystore-providers.md).

| Valor del atributo | Descripción |
|-|-|
| CEKEYSTOREDATA * | Estructura de datos de comunicación para la biblioteca de proveedor de almacén de claves |

### <a name="sql_copt_ss_cekeystoreprovider"></a>SQL_COPT_SS_CEKEYSTOREPROVIDER

Carga una biblioteca de proveedor de almacén de claves para Always Encrypted o recupera los nombres de las bibliotecas de proveedor de almacén de claves cargadas. Para más información, vea [Proveedores de almacén de claves personalizados](custom-keystore-providers.md). Este atributo no tiene ningún valor predeterminado.

| Valor del atributo | Descripción |
|-|-|
| char * | Ruta de acceso a una biblioteca de proveedor de almacén de claves |

### <a name="sql_copt_ss_enlist_in_xa"></a>SQL_COPT_SS_ENLIST_IN_XA

Para habilitar transacciones XA con un procesador de transacciones (TP) compatible con XA, la aplicación debe llamar a **SQLSetConnectAttr** con SQL_COPT_SS_ENLIST_IN_XA establecido y un puntero a un objeto `XACALLPARAM`. Esta opción es compatible con Windows, Linux (17.3 y versiones posteriores) y macOS.
```
SQLSetConnectAttr(hdbc, SQL_COPT_SS_ENLIST_IN_XA, param, SQL_IS_POINTER);  // XACALLPARAM *param
``` 
 Para asociar una transacción XA con una conexión ODBC solamente, proporcione TRUE o FALSE con SQL_COPT_SS_ENLIST_IN_XA, en lugar del puntero al llamar a **SQLSetConnectAttr**. Esto es válido solamente en Windows y no se puede usar para especificar operaciones de XA a través de una aplicación cliente. 
 ```
SQLSetConnectAttr(hdbc, SQL_COPT_SS_ENLIST_IN_XA, (SQLPOINTER)TRUE, 0);
``` 

|Value|Descripción|Plataformas|  
|-----------|-----------------|-----------------|  
|Objeto XACALLPARAM*|El puntero al objeto `XACALLPARAM`.|Windows, Linux y macOS|
|TRUE|Asocia la transacción XA con la conexión ODBC. Todas las actividades de base de datos relacionadas se realizarán bajo la protección de la transacción XA.|Windows|  
|FALSE|Desasocia la transacción XA con la conexión ODBC.|Windows|

 Para más información sobre las transacciones XA, vea [Uso de las transacciones XA](../../connect/odbc/use-xa-with-dtc.md).

### <a name="sql_copt_ss_spid"></a>SQL_COPT_SS_SPID

Recupera el identificador de proceso de servidor de la conexión. Esto es equivalente a la variable [@@SPID](../../t-sql/functions/spid-transact-sql.md) de T-SQL, con la salvedad de que no incurre en un recorrido de ida y vuelta adicional al servidor.

| Valor del atributo | Descripción |
|-|-|
| DWORD | SPID |
