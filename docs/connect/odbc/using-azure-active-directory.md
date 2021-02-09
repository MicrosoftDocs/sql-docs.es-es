---
title: Uso de Azure Active Directory con el controlador ODBC
description: Microsoft ODBC Driver for SQL Server permite que las aplicaciones ODBC se conecten a una instancia de Azure SQL Database mediante Azure Active Directory.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 52205f03-ff29-4254-bfa8-07cced155c86
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2a6b27c8c14a92ed9f17c56cab1cf2285b2cfec9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198846"
---
# <a name="using-azure-active-directory-with-the-odbc-driver"></a>Uso de Azure Active Directory con el controlador ODBC
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="purpose"></a>Propósito

La versión 13.1 o anteriores de Microsoft ODBC Driver for SQL Server permite que las aplicaciones ODBC se conecten a una instancia de Azure SQL Database mediante una identidad federada en Azure Active Directory con un nombre de usuario y contraseña, un token de acceso de Azure Active Directory, una identidad administrada de Azure Active Directory (versión 17.3 y posteriores) o la autenticación integrada de Windows (versión 17.6 y posteriores en Linux/macOS). Para la versión 13.1 del controlador ODBC, la autenticación de token de acceso de Azure Active Directory es _solo Windows_. La versión 17 del controlador ODBC y versiones posteriores admiten esta autenticación en todas las plataformas (Windows, Linux y macOS). En la versión 17.1 del controlador ODBC para Windows se incluye una nueva autenticación interactiva de Azure Active Directory con el identificador de inicio de sesión. Se agregó un nuevo método de autenticación de identidad administrada de Azure Active Directory en la versión 17.3.1.1 del controlador ODBC para las identidades asignadas por el sistema y por el usuario. Todo esto se logra mediante el uso de nuevas palabras clave de cadena de conexión y DSN con atributos de conexión.

> [!NOTE]
> El controlador ODBC en Linux y macOS con versiones anteriores a la 17.6 solo admite la autenticación de Azure Active Directory directamente en Azure Active Directory. Si va a usar la autenticación de nombre de usuario y contraseña de Azure Active Directory desde un cliente Linux o macOS, y la configuración de Active Directory requiere que el cliente se autentique sobre un punto de conexión de Servicios de federación de Active Directory, se puede producir un error en la autenticación. Esta limitación se ha quitado a partir de la versión 17.6 del controlador.

## <a name="new-andor-modified-dsn-and-connection-string-keywords"></a>Palabras clave de cadena de conexión y DSN nuevas y/o modificadas

Se puede usar la palabra clave `Authentication` al conectarse con un DSN o una cadena de conexión para controlar el modo de autenticación. El valor definido en la cadena de conexión reemplaza al del DNS, si se proporciona. El _valor de atributo anterior_ del parámetro `Authentication` es el valor calculado a partir de la cadena de conexión y los valores de DSN.

|Nombre|Valores|Valor predeterminado|Descripción|
|-|-|-|-|
|`Authentication`|(sin establecer), (cadena vacía), `SqlPassword`, `ActiveDirectoryPassword`, `ActiveDirectoryIntegrated`, `ActiveDirectoryInteractive`, `ActiveDirectoryMsi`, `ActiveDirectoryServicePrincipal` |(sin establecer)|Controla el modo de autenticación.<table><tr><th>Value<th>Descripción<tr><td>(sin establecer)<td>Modo de autenticación determinado por otras palabras clave (opciones de conexión heredadas existentes).<tr><td>(cadena vacía)<td>(Solo cadena de conexión) Invalide y anule un valor de `Authentication` establecido en el DSN.<tr><td>`SqlPassword`<td>Autentíquese directamente en una instancia de SQL Server mediante un nombre de usuario y una contraseña.<tr><td>`ActiveDirectoryPassword`<td>Autentíquese con una identidad de Azure Active Directory mediante un nombre de usuario y una contraseña.<tr><td>`ActiveDirectoryIntegrated`<td>_Windows y Linux/Mac con la versión 17.6 y posteriores, solo controlador_. Autentíquese con una identidad de Azure Active Directory mediante la autenticación integrada.<tr><td>`ActiveDirectoryInteractive`<td>_Solo controlador de Windows_. Autentíquese con una identidad de Azure Active Directory mediante la autenticación interactiva.<tr><td>`ActiveDirectoryMsi`<td>Autentíquese con una identidad de Azure Active Directory mediante la autenticación de la identidad administrada. Para la identidad asignada por el usuario, el UID se establece en el identificador de objeto de la identidad del usuario.<tr><td>`ActiveDirectoryServicePrincipal`<td>(17.7 y versiones posteriores) Autentíquese con una identidad de Azure Active Directory mediante la autenticación de la entidad de servicio.</table>|
|`Encrypt`|(sin establecer), `Yes`, `No`|(consulte la descripción)|Controla el cifrado de una conexión. Si el valor de atributo anterior del parámetro `Authentication` no es _ninguno_ en el DSN o en la cadena de conexión, el valor predeterminado es `Yes`. De lo contrario, el valor predeterminado es `No`. Si el atributo `SQL_COPT_SS_AUTHENTICATION` invalida el valor de atributo anterior de `Authentication`, establezca explícitamente el valor de Encryption en el DSN, la cadena de conexión o el atributo de conexión. El valor de atributo anterior de Encryption es `Yes` si el valor se establece en `Yes` en el DSN o en la cadena de conexión.|

## <a name="new-andor-modified-connection-attributes"></a>Atributos de conexión nuevos o modificados

Se han incorporado o modificado los siguientes atributos de conexión anteriores a la conexión para admitir la autenticación de Azure Active Directory. Cuando un atributo de conexión tiene una cadena de conexión o una palabra clave de DSN correspondientes y se establece, el atributo de conexión tiene prioridad.

|Atributo|Tipo|Valores|Valor predeterminado|Descripción|
|-|-|-|-|-|
|`SQL_COPT_SS_AUTHENTICATION`|`SQL_IS_INTEGER`|`SQL_AU_NONE`, `SQL_AU_PASSWORD`, `SQL_AU_AD_INTEGRATED`, `SQL_AU_AD_PASSWORD`, `SQL_AU_AD_INTERACTIVE`, `SQL_AU_AD_MSI`, `SQL_AU_AD_SPA`, `SQL_AU_RESET`|(sin establecer)|Consulte la descripción de la palabra clave `Authentication` anteriormente. `SQL_AU_NONE` se proporciona para reemplazar explícitamente un valor `Authentication` establecido en el DSN o la cadena de conexión, mientras `SQL_AU_RESET` desactiva el atributo si se estableció, lo que permite que el valor de la cadena de conexión o DSN tenga prioridad.|
|`SQL_COPT_SS_ACCESS_TOKEN`|`SQL_IS_POINTER`|Puntero a `ACCESSTOKEN` o NULL|NULL|Si no es NULL, especifica el token de acceso de AzureAD que se va a usar. Es un error especificar un token de acceso y también las palabras clave de cadena de conexión `UID`, `PWD`, `Trusted_Connection` o `Authentication` o sus atributos equivalentes. <br> **NOTA:** La versión 13.1 del controlador ODBC solo lo admite en _Windows_.|
|`SQL_COPT_SS_ENCRYPT`|`SQL_IS_INTEGER`|`SQL_EN_OFF`, `SQL_EN_ON`|(consulte la descripción)|Controla el cifrado de una conexión. `SQL_EN_OFF` y `SQL_EN_ON` deshabilitan y habilitan el cifrado, respectivamente. Si el valor de atributo anterior del valor `Authentication` no es _ninguno_ o `SQL_COPT_SS_ACCESS_TOKEN` está establecido y `Encrypt` no se ha especificado en el DSN o en la cadena de conexión, el valor predeterminado es `SQL_EN_ON`. De lo contrario, el valor predeterminado es `SQL_EN_OFF`. Si el atributo de conexión `SQL_COPT_SS_AUTHENTICATION` se establece en no _ninguno_, establezca explícitamente `SQL_COPT_SS_ENCRYPT` en el valor deseado si no se ha especificado `Encrypt` en el DSN o la cadena de conexión. El valor efectivo de este atributo controla [si se utilizará el cifrado para la conexión.](../../relational-databases/native-client/features/using-encryption-without-validation.md)|
|`SQL_COPT_SS_OLDPWD`|\-|\-|\-|No se admite con Azure Active Directory, ya que los cambios de contraseña en las entidades de seguridad de Azure AD no se pueden realizar a través de una conexión ODBC. <br><br>La expiración de contraseñas para la autenticación de SQL Server se incorporó en SQL Server 2005. Se ha agregado el atributo `SQL_COPT_SS_OLDPWD` para permitir que el cliente especifique la contraseña antigua y la nueva contraseña para la conexión. Cuando se establezca esta propiedad, el proveedor no usará el grupo de conexiones para la primera conexión o para conexiones posteriores, ya que la cadena de conexión contendrá la "contraseña antigua", que ahora ha cambiado.|
|`SQL_COPT_SS_INTEGRATED_SECURITY`|`SQL_IS_INTEGER`|`SQL_IS_OFF`,`SQL_IS_ON`|`SQL_IS_OFF`|_En desuso_; en su lugar, use `SQL_COPT_SS_AUTHENTICATION` establecido en `SQL_AU_AD_INTEGRATED`. <br><br>Fuerza el uso de la autenticación de Windows (Kerberos en Linux y macOS) para la validación de acceso en el inicio de sesión del servidor. Cuando se usa la autenticación de Windows, el controlador omite el identificador de usuario y los valores de contraseña proporcionados como parte del procesamiento de `SQLConnect`, `SQLDriverConnect` o `SQLBrowseConnect`.|

## <a name="ui-additions-for-azure-active-directory-windows-driver-only"></a>Adiciones de interfaz de usuario para Azure Active Directory (solo controlador de Windows)

Las interfaces de conexión e instalación de DSN del controlador se han mejorado con las opciones adicionales necesarias para usar la autenticación con Azure AD.

### <a name="creating-and-editing-dsns-in-the-ui"></a>Creación y edición de nombres de orígenes de datos (DSN) en la interfaz de usuario

Es posible usar las nuevas opciones de autenticación de Azure AD al crear o editar un DSN existente mediante la interfaz de usuario de instalación del controlador:

`Authentication=ActiveDirectoryIntegrated` para la autenticación integrada de Azure Active Directory en Azure SQL Database

![La pantalla de creación y edición de DSN con la autenticación integrada de Azure Active Directory seleccionada.](windows/create-dsn-ad-integrated.png)

`Authentication=ActiveDirectoryPassword` para la autenticación de nombre de usuario/contraseña de Azure Active Directory en Azure SQL Database

![La pantalla de creación y edición de DSN con la autenticación de contraseña de Azure Active Directory seleccionada.](windows/create-dsn-ad-password.png)

`Authentication=ActiveDirectoryInteractive` para la autenticación interactiva de Azure Active Directory en Azure SQL Database

![La pantalla de creación y edición de DSN con la autenticación interactiva de Azure Active Directory seleccionada.](windows/create-dsn-ad-interactive.png)

`Authentication=SqlPassword` para la autenticación de nombre de usuario/contraseña en SQL Server (Azure o de otro modo)

![La pantalla de creación y edición de DSN con la autenticación de SQL Server seleccionada.](windows/create-dsn-ad-sql-server.png)

`Trusted_Connection=Yes` en el caso de autenticación integrada SSPI para Windows

![La pantalla de creación y edición de DSN con la autenticación integrada de Windows seleccionada.](windows/create-dsn-win-sspi.png)

`Authentication=ActiveDirectoryMsi` para la autenticación de identidad administrada de Azure Active Directory

![La pantalla de creación y edición de DSN con la autenticación de Managed Service Identity seleccionada.](windows/create-dsn-ad-msi.png)

`Authentication=ActiveDirectoryServicePrincipal` para la autenticación de la entidad de servicio de Azure Active Directory

![Pantalla de creación y edición de DSN con la autenticación de la entidad de servicio de Azure Active Directory seleccionada.](windows/create-dsn-ad-spa.png)

Las siete opciones corresponden a `Trusted_Connection=Yes` (autenticación integrada existente solo con SSPI para Windows heredado) y `Authentication=` `ActiveDirectoryIntegrated`, `SqlPassword`, `ActiveDirectoryPassword`, `ActiveDirectoryInteractive`, `ActiveDirectoryMsi` y `ActiveDirectoryServicePrincipal`, respectivamente.

### <a name="sqldriverconnect-prompt-windows-driver-only"></a>Solicitud de SQLDriverConnect (solo controlador de Windows)

El cuadro de diálogo de solicitud que muestra SQLDriverConnect cuando solicita información necesaria para completar la conexión contiene cuatro nuevas opciones para la autenticación de Azure AD:

![Cuadro de diálogo de inicio de sesión de SQL Server que se muestra en SQLDriverConnect.](windows/server-login.png)

Estas opciones se corresponden con las mismas seis disponibles en la interfaz de usuario de instalación de DSN anterior.

### <a name="example-connection-strings"></a>Ejemplos de cadena de conexión
1. Autenticación de SQL Server: sintaxis heredada. No se valida el certificado de servidor y el cifrado solo se utiliza si el servidor lo exige. El nombre de usuario/contraseña se pasa en la cadena de conexión.
`server=Server;database=Database;UID=UserName;PWD=Password;`
2. Autenticación de SQL: nueva sintaxis. El cliente solicita el cifrado (el valor predeterminado de `Encrypt` es `true`) y se valida el certificado de servidor, independientemente de la configuración de cifrado (a menos que `TrustServerCertificate` se establezca en `true`). El nombre de usuario/contraseña se pasa en la cadena de conexión.
 `server=Server;database=Database;UID=UserName;PWD=Password;Authentication=SqlPassword;`
3. Autenticación integrada de Windows (Kerberos en Linux y macOS) mediante SSPI (para una IaaS de SQL Server o SQL): sintaxis actual. No se valida el certificado de servidor, a menos que se use el cifrado. 
`server=Server;database=Database;Trusted_Connection=yes;`
4. (_Solo controlador de Windows_). Autenticación integrada de Windows mediante SSPI (si la base de datos de destino se encuentra en una IaaS de SQL Server o SQL): nueva sintaxis. El cliente solicita el cifrado (el valor predeterminado de `Encrypt` es `true`) y se valida el certificado de servidor, independientemente de la configuración de cifrado (a menos que `TrustServerCertificate` se establezca en `true`). 
`server=Server;database=Database;Authentication=ActiveDirectoryIntegrated;`
5. Autenticación de nombre de usuario/contraseña de Azure Active Directory (si la base de datos de destino está en Azure SQL Database). Se valida el certificado de servidor, independientemente de la configuración de cifrado (a menos que `TrustServerCertificate` se establezca en `true`). El nombre de usuario/contraseña se pasa en la cadena de conexión. 
`server=Server;database=Database;UID=UserName;PWD=Password;Authentication=ActiveDirectoryPassword;`
6. (_Windows y Linux/macOS con la versión 17.6 y posteriores, solo controlador_). Autenticación integrada de Windows mediante ADAL o Kerberos, que implica el canje de credenciales de cuenta de Windows para un token de acceso emitido por Azure AD, suponiendo que la base de datos de destino esté en Azure SQL Database. Se valida el certificado de servidor, independientemente de la configuración de cifrado (a menos que `TrustServerCertificate` se establezca en `true`). En Linux/macOS, es necesario disponer de un vale Kerberos adecuado. Consulte la sección siguiente sobre cuentas federadas y [Uso de la autenticación integrada](linux-mac/using-integrated-authentication.md) para obtener más información.
`server=Server;database=Database;Authentication=ActiveDirectoryIntegrated;`
7. (_Solo controlador de Windows_). La autenticación interactiva de Azure AD usa la tecnología de Azure Active Directory Multifactor Authentication para configurar la conexión. En este modo, al proporcionar el identificador de inicio de sesión, se desencadena un cuadro de diálogo de autenticación de Azure que permite al usuario escribir la contraseña para completar la conexión. El nombre de usuario se pasa en la cadena de conexión.
`server=Server;database=Database;UID=UserName;Authentication=ActiveDirectoryInteractive;`

![Interfaz de usuario de autenticación de Microsoft Azure al usar la autenticación interactiva de Active Directory.](windows/WindowsAzureAuth.png)

8. La autenticación de identidad administrada de Azure Active Directory usa una identidad asignada por el sistema o por el usuario para la autenticación con el fin de configurar la conexión. Para la identidad asignada por el usuario, el UID se establece en el identificador de objeto de la identidad del usuario.<br>
Para la identidad asignada por el sistema,<br>
`server=Server;database=Database;Authentication=ActiveDirectoryMsi;`<br>
Para la identidad asignada por el usuario cuyo identificador de objeto es igual a myObjectId,<br>
`server=Server;database=Database;UID=myObjectId;Authentication=ActiveDirectoryMsi;`

9. Para la autenticación de la entidad de servicio de Azure Active Directory se usa `server=Server;databse=Database;UID=clientId;PWD=clientSecret;Authentication=ActiveDirectoryServicePrincipal;`.

> [!NOTE]
>- Al usar las nuevas opciones de Active Directory con el controlador ODBC de Windows **_anterior a la_* _ versión 17.4.2, asegúrese de que se ha instalado la [Biblioteca de autenticación de Active Directory para SQL Server](https://go.microsoft.com/fwlink/?LinkID=513072). Al usar los controladores de Linux y macOS, asegúrese de que se ha instalado `libcurl`. En el caso de la versión de controlador 17.2 y versiones posteriores, esta no es una dependencia explícita, ya que no es necesaria para los otros métodos de autenticación u operaciones de ODBC.
>- Cuando la configuración de Azure Active Directory incluye directivas de acceso condicional y el cliente es Windows 10 o Server 2016 o posteriores, puede producirse un error en la autenticación integrada o de nombre de usuario/contraseña. Las directivas de acceso condicional requieren el uso del administrador de cuentas de Windows (WAM), que es compatible con la versión 17.6 o posteriores del controlador para Windows. Para usar WAM, cree una cadena o un valor DWORD denominado `ADALuseWAM` en `HKLM\Software\ODBC\ODBCINST.INI\ODBC Driver 17 for SQL Server`, `HKCU\Software\ODBC\ODBC.INI\<your-user-DSN-name>` o `HKLM\Software\ODBC\ODBC.INI\<your-system-DSN-name>` para la configuración global, de DSN de usuario o de ámbito DSN del sistema, respectivamente, y establézcalo en un valor de 1. Tenga en cuenta que la autenticación con WAM no admite la ejecución de la aplicación como un usuario diferente con `runas`. Los escenarios que requieren directivas de acceso condicional no se admiten en Linux o macOS.
>- Para conectarse con un nombre de usuario y una contraseña de la cuenta de SQL Server, ahora puede usar la nueva opción `SqlPassword`, que se recomienda especialmente para Azure SQL, ya que esta opción habilita los valores predeterminados de conexión más seguros.
>- Para conectarse con un nombre de usuario y una contraseña de la cuenta de Azure Active Directory, especifique `Authentication=ActiveDirectoryPassword` en la cadena de conexión y las palabras clave `UID` y `PWD` con el nombre de usuario y la contraseña, respectivamente.
>- Para conectarse mediante la autenticación integrada de Windows o de Active Directory (Windows y Linux/macOS con la versión 17.6 y posteriores, solo controlador), especifique `Authentication=ActiveDirectoryIntegrated` en la cadena de conexión. El controlador elegirá automáticamente el modo de autenticación correcto. No se debe especificar `UID` y `PWD`.
>- Para conectarse con autenticación interactiva de Active Directory (solo controlador de Windows), se debe especificar `UID`.

## <a name="authenticating-with-an-access-token"></a>Autenticación con token de acceso

El atributo anterior a la conexión `SQL_COPT_SS_ACCESS_TOKEN` permite el uso de un token de acceso obtenido de Azure AD para la autenticación en lugar de un nombre de usuario y una contraseña, y también omite la negociación y la obtención de un token de acceso por parte del controlador. Para usar un token de acceso, establezca el atributo de conexión `SQL_COPT_SS_ACCESS_TOKEN` en un puntero a una estructura `ACCESSTOKEN`:

~~~
typedef struct AccessToken
{
    DWORD dataSize;
    BYTE data[];
} ACCESSTOKEN;
~~~

`ACCESSTOKEN` es una estructura de longitud variable formada por un elemento _length_ de 4 bytes seguido de _length_ bytes de datos opacos que forman el token de acceso. Debido a cómo SQL Server controla los tokens de acceso, uno obtenido a través de una respuesta JSON de [OAuth 2.0](/azure/active-directory/develop/active-directory-authentication-scenarios) debe expandirse para que cada byte vaya seguido de un byte 0 de relleno, similar a una cadena UCS-2 que solo contiene caracteres ASCII; sin embargo, el token es un valor opaco y la longitud especificada, en bytes, no debe incluir ningún terminador NULL. Dadas sus considerables restricciones de longitud y formato, este método de autenticación solo está disponible mediante programación a través del atributo de conexión `SQL_COPT_SS_ACCESS_TOKEN`; no hay ninguna palabra clave de cadena de conexión o DSN correspondiente. La cadena de conexión no debe contener las palabras clave `UID`, `PWD`, `Authentication` o `Trusted_Connection`.

> [!NOTE]
> La versión 13.1 del controlador ODBC solo admite esta autenticación en _Windows_.

## <a name="azure-active-directory-authentication-sample-code"></a>Código de ejemplo de autenticación de Azure Active Directory

En el ejemplo siguiente se muestra el código necesario para conectarse a SQL Server mediante Azure Active Directory con palabras clave de conexión. Tenga en cuenta que no es necesario cambiar el propio código de aplicación; la cadena de conexión, o el DSN si se usa uno, es la única modificación necesaria para usar Azure AD para la autenticación:
~~~
    ...
    SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myuser;PWD=myPass;Authentication=ActiveDirectoryPassword"
    ...
    SQLDriverConnect(hDbc, NULL, connString, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);    
    ...
~~~
En el ejemplo siguiente se muestra el código necesario para conectarse a SQL Server mediante Azure Active Directory con autenticación de token de acceso. En este caso, es necesario modificar el código de aplicación para procesar el token de acceso y establecer el atributo de conexión asociado.
~~~
    SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server}"
    SQLCHAR accessToken[] = "eyJ0eXAiOi..."; // In the format extracted from an OAuth JSON response
    ...
    DWORD dataSize = 2 _ strlen(accessToken);
    ACCESSTOKEN *pAccToken = malloc(sizeof(ACCESSTOKEN) + dataSize);
    pAccToken->dataSize = dataSize;
    // Expand access token with padding bytes
    for(int i = 0, j = 0; i < dataSize; i += 2, j++) {
        pAccToken->data[i] = accessToken[j];
        pAccToken->data[i+1] = 0;
    }
    ...
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_ACCESS_TOKEN, (SQLPOINTER)pAccToken, SQL_IS_POINTER);
    SQLDriverConnect(hDbc, NULL, connString, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);        
    ...
    free(pAccToken);
~~~
A continuación se muestra una cadena de conexión de ejemplo para su uso con la autenticación interactiva de Azure Active Directory. Tenga en cuenta que no contiene el campo PWD, ya que la contraseña se escribiría con la pantalla de autenticación de Azure.
~~~
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myuser;Authentication=ActiveDirectoryInteractive"
~~~
A continuación se muestra una cadena de conexión de ejemplo para su uso con la autenticación de identidad administrada de Azure Active Directory. Tenga en cuenta que el UID se establece en el identificador de objeto de la identidad del usuario para la identidad asignada por el usuario.
~~~
// For system-assigned identity,
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};Authentication=ActiveDirectoryMsi"
...
// For user-assigned identity with object ID equals to myObjectId
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myObjectId;Authentication=ActiveDirectoryMsi"
~~~

## <a name="considerations-for-using-adfs-federated-accounts-on-linuxmacos"></a>Consideraciones sobre el uso de cuentas federadas de ADFS en Linux/macOS

A partir de la versión 17.6, los controladores para Linux y macOS admiten la autenticación mediante cuentas federadas de ADFS en Azure Active Directory con nombre de usuario y contraseña (`ActiveDirectoryPassword`) o Kerberos (`ActiveDirectoryIntegrated`). Hay algunas limitaciones que dependen de la plataforma al usar el modo integrado.

Cuando se autentica con un usuario cuyo sufijo UPN es diferente del dominio kerberos, es decir, se usa un sufijo UPN alternativo, es necesario usar la opción de entidad de seguridad empresarial (use la opción `-E` con `kinit` y proporcione el nombre de la entidad de seguridad con el formato `user@federated-domain`) al obtener vales Kerberos. Esto permite que el controlador determine correctamente el dominio federado y el dominio kerberos.

Puede comprobar que un vale Kerberos adecuado está disponible mediante la inspección de la salida del comando `klist`. Si el dominio federado es el mismo que el dominio kerberos y el sufijo UPN, el nombre de la entidad de seguridad tendrá el formato `user@realm`. Si es diferente, el nombre de la entidad de seguridad debe tener el formato `user@federated-domain@realm`.

### <a name="linux"></a>Linux

En SuSE 11, la versión 1.6.x de la biblioteca de Kerberos predeterminada no admite la opción de entidad de seguridad empresarial necesaria para usar sufijos UPN alternativos. Para usar sufijos UPN alternativos con la autenticación integrada de Azure AD, actualice la biblioteca de Kerberos a la versión 1.7 o posterior.

En Alpine Linux, la opción `libcurl` predeterminada no admite la autenticación SPNEGO/Kerberos necesaria para la autenticación integrada de Azure AD.

### <a name="macos"></a>macOS

La biblioteca de Kerberos del sistema `kinit` admite la entidad de seguridad empresarial con la opción `--enterprise`, pero también realiza implícitamente la canonización de nombres, lo que impide el uso de sufijos UPN alternativos. Para usar sufijos UPN alternativos con la autenticación integrada de Azure AD, instale una biblioteca de Kerberos más reciente a través de `brew install krb5` y use su `kinit` con la opción `-E`, como se describió anteriormente.

## <a name="see-also"></a>Consulte también

[Compatibilidad de la autenticación basada en tokens para Azure SQL Database mediante la autenticación de Azure AD](/archive/blogs/sqlsecurity/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth)

[Uso de la autenticación integrada](linux-mac/using-integrated-authentication.md)