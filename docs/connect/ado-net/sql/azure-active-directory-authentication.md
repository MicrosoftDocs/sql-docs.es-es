---
title: Uso de la autenticación de Azure Active Directory con SqlClient
description: Describe cómo usar los modos de autenticación de Azure Active Directory compatibles para conectarse a orígenes de datos de Azure SQL con SqlClient.
ms.date: 11/20/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: karinazhou
ms.author: v-jizho2
ms.reviewer: v-daenge
ms.openlocfilehash: c57c2d10854ed902a6230eafc3a912cd0508c989
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836018"
---
# <a name="using-azure-active-directory-authentication-with-sqlclient"></a>Uso de la autenticación de Azure Active Directory con SqlClient

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

En este artículo se describe cómo conectarse a orígenes de datos de Azure SQL mediante la autenticación de Azure Active Directory (Azure AD) desde una aplicación .NET con SqlClient.

La autenticación de Azure AD usa identidades en Azure AD para tener acceso a orígenes de datos de Azure SQL como Azure SQL Database, Azure SQL Managed Instance y Azure Synapse Analytics. El espacio de nombres **Microsoft.Data.SqlClient** permite a las aplicaciones cliente especificar credenciales de Azure AD en modos de autenticación diferentes cuando se conectan a Azure SQL Database.

Al establecer la propiedad de conexión `Authentication` en la cadena de conexión, el cliente puede elegir un modo de autenticación de Azure AD preferido según el valor proporcionado:

- La versión más antigua de **Microsoft.Data.SqlClient** admite `Active Directory Password` para .NET Framework, .NET Core y .NET Standard. También admite la autenticación `Active Directory Integrated` y la autenticación `Active Directory Interactive` para .NET Framework. 

- A partir de **Microsoft.Data.SqlClient** 2.0.0, se ha ampliado la compatibilidad con la autenticación `Active Directory Integrated` y la autenticación `Active Directory Interactive` en .NET Framework, .NET Core y .NET Standard. 

  También se agregó un nuevo modo de autenticación `Active Directory Service Principal` en SqlClient 2.0.0. Hace uso del identificador y el secreto de cliente de una identidad de entidad de servicio para realizar la autenticación. 

- Se han agregado más modos de autenticación en **Microsoft. Data. SqlClient** 2.1.0, incluidos `Active Directory Device Code Flow` y `Active Directory Managed Identity` (también conocido como `Active Directory MSI`). Estos nuevos modos permiten a la aplicación adquirir un token de acceso para conectarse al servidor. 

Para obtener más información acerca de la autenticación de Azure AD más allá de lo que describen las siguientes secciones, consulte [Conexión a SQL Database mediante la autenticación de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview).

## <a name="setting-azure-active-directory-authentication"></a>Configuración de la autenticación de Azure Active Directory

Cuando la aplicación se conecta a orígenes de datos de Azure SQL mediante la autenticación de Azure AD, debe proporcionar un modo de autenticación válido. En la tabla siguiente se enumeran los modos de autenticación admitidos. La aplicación especifica un modo mediante el uso de la propiedad de conexión `Authentication` en la cadena de conexión.

| Valor | Descripción  | Marco    | Versión de Microsoft.Data.SqlClient |
|:--|:--|:--|:--:|
| Contraseña de Active Directory | Autentíquese con una identidad de Azure AD mediante un nombre de usuario y una contraseña. | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+  | 1.0+|
| Integrado en Active Directory |Autentíquese con una identidad de Azure AD mediante la autenticación integrada. | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.0.0+<sup>1</sup> |
| Active Directory interactivo | Autentíquese con una identidad de Azure AD mediante la autenticación interactiva. | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.0.0+<sup>1</sup> |
| Entidad de servicio de Active Directory | Autentíquese con una identidad de Azure AD mediante el identificador y el secreto de cliente de una identidad de entidad de servicio. | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.0.0+ |
| Flujo de código de dispositivo de Active Directory | Autentíquese con una identidad de Azure AD mediante el modo de flujo de código de dispositivo. | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.1.0+ |
| Identidad administrada de Active Directory <br>MSI de Active Directory | Autentíquese con una identidad de Azure AD mediante una identidad administrada asignada por el sistema o por el usuario | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.1.0+ |

<sup>1</sup> Antes de **Microsoft.Data.SqlClient** 2.0.0, los modos de autenticación `Active Directory Integrated` y `Active Directory Interactive` se admiten solo en .NET Framework 4.6+.

## <a name="using-active-directory-password-authentication"></a>Uso de la autenticación de contraseña de Active Directory

El modo de autenticación `Active Directory Password` admite la autenticación en orígenes de datos de Azure con Azure AD para usuarios de Azure AD nativos o federados. Cuando se usa este modo, se deben proporcionar las credenciales de usuario en la cadena de conexión. En el ejemplo siguiente se muestra cómo usar la autenticación `Active Directory Password`.

```c#
// Use your own server, database, user ID, and password.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Password; Database=testdb; User Id=user@domain.com; Password=***";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-integrated-authentication"></a>Uso de la autenticación integrada de Active Directory

Para usar el modo de autenticación `Active Directory Integrated`, debe federar la instancia de Active Directory local con Azure AD en la nube. Puede realizar la federación mediante Servicios de federación de Active Directory (AD FS), por ejemplo.

Cuando haya iniciado sesión en un equipo unido a un dominio, puede acceder a los orígenes de datos de Azure SQL sin que se le soliciten las credenciales con este modo. No se puede especificar el nombre de usuario y la contraseña en la cadena de conexión para aplicaciones de .NET Framework. El nombre de usuario es opcional en la cadena de conexión para las aplicaciones de .NET Core y .NET Standard. No se puede establecer la propiedad `Credential` de SqlConnection en este modo. 

El siguiente fragmento de código es un ejemplo de cuándo se está usando la autenticación `Active Directory Integrated`.

```c#
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is optional for .NET Core and .NET Standard.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-interactive-authentication"></a>Uso de autenticación de Active Directory interactivo

La autenticación `Active Directory Interactive` admite la tecnología de autenticación multifactor para conectarse a orígenes de datos de Azure SQL. Si proporciona este modo de autenticación en la cadena de conexión, aparecerá una pantalla de autenticación de Azure y solicitará al usuario que escriba credenciales válidas. No se puede especificar la contraseña en la cadena de conexión. 

No se puede establecer la propiedad `Credential` de SqlConnection en este modo. Con **Microsoft.Data.SqlClient** 2.0.0 y versiones posteriores, se permite el nombre de usuario en la cadena de conexión cuando está en modo interactivo. 

En el ejemplo siguiente se muestra cómo usar la autenticación `Active Directory Interactive`.

```c#
// Use your own server, database, and user ID.
// User ID is optional.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is not provided.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-service-principal-authentication"></a>Uso de la autenticación de entidad de servicio de Active Directory

En el modo de autenticación `Active Directory Service Principal`, la aplicación cliente se puede conectar a orígenes de datos de Azure SQL proporcionando el identificador y el secreto de cliente de una identidad de entidad de servicio. La autenticación de la entidad de servicio implica lo siguiente:
1. Configuración del registro de una aplicación con un secreto.
1. Concesión de permisos a la aplicación en la instancia de Azure SQL Database.
1. Conexión con la credencial correcta. 

En el ejemplo siguiente se muestra cómo usar la autenticación `Active Directory Service Principal`.

```c#
// Use your own server, database, app ID, and secret.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Service Principal; Database=testdb; User Id=AppId; Password=secret";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-device-code-flow-authentication"></a>Uso de la autenticación de flujo de código de dispositivo de Active Directory

Con la [Biblioteca de autenticación de Microsoft](/azure/active-directory/develop/msal-overview) para .NET (MSAL.NET), la autenticación `Active Directory Device Code Flow` permite a la aplicación cliente conectarse a orígenes de datos de Azure SQL desde dispositivos y sistemas operativos que no tienen un explorador web interactivo. La autenticación interactiva se realizará en otro dispositivo. Para obtener más información sobre la autenticación de flujo de código de dispositivo, consulte [Flujo de concesión de autorización de dispositivo de OAuth 2.0](/azure/active-directory/develop/v2-oauth2-device-code). 

Cuando este modo está en uso, no se puede establecer la propiedad `Credential` de `SqlConnection`. Además, el nombre de usuario y la contraseña no se deben especificar en la cadena de conexión. 

El siguiente fragmento de código es un ejemplo del uso de la autenticación `Active Directory Device Code Flow`.

```c#
// Use your own server and database.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Device Code Flow; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-managed-identity-authentication"></a>Uso de la autenticación de identidad administrada de Azure Active Directory

*Identidades administradas* para recursos de Azure es el nombre con el que ahora se conoce al servicio Managed Service Identity (MSI). Cuando una aplicación cliente usa un recurso de Azure para tener acceso a un servicio de Azure que admite la autenticación de Azure AD, puede usar identidades administradas para autenticar proporcionando una identidad para el recurso de Azure en Azure AD. Después, puede usar esa identidad para obtener los tokens de acceso. Este método de autenticación puede eliminar la necesidad de administrar las credenciales y los secretos.

Hay dos tipos de identidades administradas:

- La _identidad administrada asignada por el sistema_ se crea en una instancia de servicio de Azure AD. Está vinculada al ciclo de vida de esa instancia de servicio. 
- La _identidad administrada asignada por el usuario_ se crea como recurso de Azure independiente. Se puede asignar a una o varias instancias de un servicio de Azure. 

Para más información sobre las identidades administradas, consulte [¿Qué son las identidades administradas para recursos de Azure?](/azure/active-directory/managed-identities-azure-resources/overview)

A partir de **Microsoft.Data.SqlClient** 2.1.0, el controlador admite la autenticación en Azure SQL Database, Azure Synapse Analytics y Azure SQL Managed Instance mediante la adquisición de tokens de acceso a través de la identidad administrada. Para usar esta autenticación, especifique `Active Directory Managed Identity` o `Active Directory MSI` en la cadena de conexión; no se requiere ninguna contraseña. 

Tampoco puede establecer la propiedad `Credential` de `SqlConnection` en este modo. Para una identidad administrada asignada por el usuario, se debe proporcionar un nombre de usuario. 

En el ejemplo siguiente se muestra cómo usar la autenticación `Active Directory Managed Identity` con una identidad administrada asignada por el sistema.

```c#
// For system-assigned managed identity
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```

En el ejemplo siguiente se muestra la autenticación `Active Directory Managed Identity` con una identidad administrada asignada por el usuario.

```c#
// For user-assigned managed identity
// Use your own server, database, and user ID.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="customizing-active-directory-authentication"></a>Personalización de la autenticación de Active Directory

Además de usar la autenticación de Active Directory integrada en el controlador, **Microsoft. Data. SqlClient** 2.1.0 y versiones posteriores proporcionan a las aplicaciones la opción de personalizar la autenticación de Active Directory. La personalización se basa en la clase `ActiveDirectoryAuthenticationProvider`, que se deriva de la clase abstracta [`SqlAuthenticationProvider`](/dotnet/api/system.data.sqlclient.sqlauthenticationprovider). 

Durante la autenticación de Active Directory, la aplicación cliente puede definir su propia clase `ActiveDirectoryAuthencationProvider` con uno de estas dos opciones:

- Usar un método de devolución de llamada personalizado.
- Pasar un identificador de cliente de aplicación a la biblioteca MSAL a través del controlador SqlClient para obtener los tokens de acceso.

En el ejemplo siguiente se muestra cómo usar una devolución de llamada personalizada cuando la autenticación `Active Directory Device Code Flow` está en uso. 

[!code-csharp [AADAuthenticationCustomDeviceFlowCallback#1](~/../sqlclient/doc/samples/AADAuthenticationCustomDeviceFlowCallback.cs#1)]

Con una clase `ActiveDirectoryAuthenticationProvider` personalizada, se puede pasar un identificador de cliente de aplicación definido por el usuario a SqlClient cuando se usa un modo de autenticación de Active Directory compatible. Entre los modos de autenticación compatibles con Active Directory se incluyen `Active Directory Password`, `Active Directory Integrated`, `Active Directory Interactive`, `Active Directory Service Principal` y `Active Directory Device Code Flow`.

El identificador de cliente de la aplicación también se puede configurar a través de `SqlAuthenticationProviderConfigurationSection` o `SqlClientAuthenticationProviderConfigurationSection`. La propiedad de configuración `applicationClientId` se aplica a .NET Framework 4.6 + y .NET Core 2.1+.

El siguiente fragmento de código es un ejemplo del uso de una clase `ActiveDirectoryAuthenticationProvider` personalizada con un identificador de cliente de aplicación definido por el usuario cuando se está usando la autenticación `Active Directory Interactive`.

[!code-csharp [ApplicationClientIdAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/ApplicationClientIdAzureAuthenticationProvider.cs#1)]

En el ejemplo siguiente se muestra cómo establecer un identificador de cliente de aplicación a través de una sección de configuración.

```xml
<configuration>
  <configSections>
    <section name="SqlClientAuthenticationProviders"
             type="Microsoft.Data.SqlClient.SqlClientAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
  </configSections>
  <SqlClientAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>

<!--or-->

<configuration>
  <configSections>
    <section name="SqlAuthenticationProviders"
             type="Microsoft.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
  </configSections>
  <SqlAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>
```

## <a name="support-for-a-custom-sql-authentication-provider"></a>Compatibilidad con un proveedor de autenticación de SQL personalizado

Dada la mayor flexibilidad, la aplicación cliente también puede usar su propio proveedor para la autenticación de Active Directory en lugar de usar la clase `ActiveDirectoryAuthenticationProvider`. El proveedor de autenticación personalizado debe ser una subclase de `SqlAuthenticationProvider` con métodos invalidados. 

En el ejemplo siguiente se muestra cómo usar un nuevo proveedor de autenticación para la autenticación `Active Directory Device Code Flow`.

[!code-csharp [CustomDeviceCodeFlowAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/CustomDeviceCodeFlowAzureAuthenticationProvider.cs#1)]

Además de mejorar la experiencia de autenticación de `Active Directory Interactive`, **Microsoft.Data.SqlClient** 2.1.0 y versiones posteriores proporcionan las siguientes API para que las aplicaciones cliente personalicen la autenticación interactiva y la autenticación del flujo de código de dispositivo.

```c#
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    // Sets a reference to the current System.Windows.Forms.IWin32Window that triggers the browser to be shown. 
    // Used to center the browser pop-up onto this window.
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    // Sets a reference to the ViewController (if using Xamarin.iOS), Activity (if using Xamarin.Android) IWin32Window, or IntPtr (if using .NET Framework). 
    // Used for invoking the browser for Active Directory Interactive authentication.
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Sets a callback method that's invoked with a custom web UI instance that will let the user sign in with Azure AD, present consent if needed, and get back the authorization code. 
    // Applicable when working with Active Directory Interactive authentication.
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Clears cached user tokens from the token provider.
    public static void ClearUserTokenCache();
}
```


## <a name="see-also"></a>Vea también
- [Objetos de aplicación y de entidad de servicio de Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals)
- [Flujos de autenticación](/azure/active-directory/develop/msal-authentication-flows)
