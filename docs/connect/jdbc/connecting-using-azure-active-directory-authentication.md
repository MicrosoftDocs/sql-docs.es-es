---
title: Conexión mediante autenticación de Azure Active Directory
description: Obtenga información sobre cómo desarrollar aplicaciones Java que usan la característica de autenticación de Azure Active Directory con Microsoft JDBC Driver para SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9c9d97be-de1d-412f-901d-5d9860c3df8c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cbcf01d51ef4abe344b66529d72476a7d1385bbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168725"
---
# <a name="connecting-using-azure-active-directory-authentication"></a>Conexión mediante autenticación de Azure Active Directory

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

En este artículo se proporciona información sobre cómo desarrollar aplicaciones Java que usan la característica de autenticación de Azure Active Directory con Microsoft JDBC Driver para SQL Server.

Puede usar la autenticación de Azure Active Directory (Azure AD), que es un mecanismo que permite conectar con Azure SQL Database v12 mediante identidades en Azure Active Directory. Use la autenticación de Azure Active Directory para administrar identidades de usuarios de base de datos de forma centralizada y como alternativa a la autenticación de SQL Server. El controlador JDBC permite especificar las credenciales de Azure Active Directory en la cadena de conexión de JDBC para conectarse a Azure SQL Database. Para obtener información sobre cómo configurar la autenticación de Azure Active Directory, visite [Conectar a la SQL Database mediante la autenticación de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview). 

Las propiedades de conexión para admitir la autenticación de Azure Active Directory en Microsoft JDBC Driver para SQL Server son:
*   **autenticación**:  use esta propiedad para indicar qué método de autenticación de SQL se va a utilizar para la conexión. Los valores posibles son: 
    * **ActiveDirectoryMSI**
        * Compatible desde la versión **7.2** del controlador, `authentication=ActiveDirectoryMSI` se puede usar para conectarse a Azure SQL Database o Synapse Analytics desde dentro de un recurso de Azure con la compatibilidad con "Identidad" habilitada. Opcionalmente, **msiClientId** también se puede especificar en las propiedades de conexión/origen de datos junto con su modo de autenticación, que debe contener el identificador de cliente de una identidad administrada que se va a usar para adquirir el **accessToken** para establecer la conexión.
    * **ActiveDirectoryIntegrated**
        * Compatible desde la versión **6.0** del controlador, `authentication=ActiveDirectoryIntegrated` se puede usar para conectarse con Azure SQL Database o Synapse Analytics mediante la autenticación integrada. Para usar este modo de autenticación, debe federar los Servicios de federación de Active Directory (ADFS) locales con Azure Active Directory en la nube. Una vez que se haya configurado, puede conectarse si agrega la biblioteca nativa "mssql-jdbc_auth-\<version>-\<arch>.dll" a la ruta de acceso de la clase de aplicación del sistema operativo Windows, o bien si configura un vale Kerberos para la compatibilidad con la autenticación multiplataforma. Podrá acceder a Azure SQL Database o Azure Synapse Analytics sin que se le soliciten credenciales al iniciar sesión en una máquina unida al dominio.

    * **ActiveDirectoryPassword**
        * Compatible desde la versión **6.0** del controlador, `authentication=ActiveDirectoryPassword` se puede usar para conectarse con Azure SQL Database o Synapse Analytics mediante un nombre de usuario de Azure AD y una contraseña.
    * **ActiveDirectoryInteractive**
        * Compatible desde la versión **9.2** del controlador, `authentication=ActiveDirectoryInteractive` se puede usar para conectarse con Azure SQL Database o Synapse Analytics mediante un flujo de autenticación interactiva (autenticación multifactor).
    * **ActiveDirectoryServicePrincipal**
        * Compatible desde la versión **9.2** del controlador, `authentication=ActiveDirectoryServicePrincipal` se puede usar para conectarse con Azure SQL Database o Synapse Analytics mediante el identificador de cliente y el secreto de una identidad de entidad de servicio.

    * **SqlPassword**
        * Use `authentication=SqlPassword` para conectarse a un servidor SQL Server mediante las propiedades userName/user y password.
    * **NotSpecified**
        * Use `authentication=NotSpecified` o déjelo como valor predeterminado cuando no se necesite ninguno de estos métodos de autenticación.

*   **AccessToken**: use esta propiedad de conexión para conectarse a una base de datos SQL mediante un token de acceso. Solo se puede establecer accessToken mediante el parámetro Properties del método getConnection() en la clase DriverManager. No se puede usar en la dirección URL de conexión.  

Para obtener más información, consulte la propiedad de autenticación en la página [Establecer las propiedades de conexión](setting-the-connection-properties.md).  


## <a name="client-setup-requirements"></a>Requisitos de configuración del cliente
En el caso de la autenticación **ActiveDirectoryMSI**, deben instalarse los siguientes componentes en el equipo cliente:
* Java 8 o posterior
* Microsoft JDBC Driver 7.2 (o superior) para SQL Server
* El entorno de cliente debe ser un recurso de Azure y debe tener habilitada la compatibilidad con la característica de "identidad".
* Un usuario de la base de datos independiente que represente la identidad administrada asignada por el sistema de su recurso de Azure o la identidad administrada asignada por el usuario, o bien uno de los grupos al que pertenece su identidad administrada, debe existir en la base de datos de destino y debe contar con el permiso CONNECT.

En el caso de otros modos de autenticación, deben instalarse los siguientes componentes en el equipo cliente:
* Java 7 o posterior
* Microsoft JDBC Driver 6.0 (o superior) para SQL Server
* Si usa el modo de autenticación basado en token de acceso, necesitará [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) y sus dependencias para ejecutar los ejemplos de este artículo. Para más información, consulte la sección [Conexión mediante el token de acceso](#connecting-using-access-token).
* Si usa el modo de autenticación **ActiveDirectoryPassword**, necesitará [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) y sus dependencias. Para obtener más información, consulte la sección [Conexión con el modo de autenticación ActiveDirectoryPassword](#connecting-using-activedirectorypassword-authentication-mode).
* Si usa el modo **ActiveDirectoryIntegrated**, necesitará [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) y sus dependencias. Para obtener más información, consulte la sección [Conexión con el modo de autenticación ActiveDirectoryIntegrated](#connecting-using-activedirectoryintegrated-authentication-mode).
* Si usa el modo de autenticación **ActiveDirectoryInteractive**, necesitará [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) y sus dependencias. Para obtener más información, consulte la sección [Conexión con el modo de autenticación ActiveDirectoryInteractive](#connecting-using-activedirectoryinteractive-authentication-mode).
* Si usa el modo de autenticación **ActiveDirectoryServicePrincipal**, necesitará [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) y sus dependencias. Para obtener más información, consulte la sección [Conexión con el modo de autenticación ActiveDirectoryServicePrincipal]()#connecting-using-activedirectoryserviceprincipal-authentication-mode section.


## <a name="connecting-using-activedirectorymsi-authentication-mode"></a>Conexión con el modo de autenticación ActiveDirectoryMSI
En el siguiente ejemplo se muestra cómo usar el modo `authentication=ActiveDirectoryMSI`. Ejecute este ejemplo desde dentro de un recurso de Azure, p. ej., una máquina virtual de Azure, App Service o Function App que se federa con Azure Active Directory.

Reemplace el nombre del servidor o la base de datos por su nombre del servidor o la base de datos en las siguientes líneas antes de ejecutar el ejemplo:

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
//Optional
ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used
```

El ejemplo para usar el modo de autenticación ActiveDirectoryMSI:

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AAD_MSI {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryMSI");
        // Optional
        ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

Al ejecutar este ejemplo en una máquina virtual de Azure, se captura un token de acceso de la _identidad administrada asignada por el sistema_ o la _identidad administrada asignada por el usuario_ (si se especifica **msiClientId**) y se establece una conexión mediante el token de acceso capturado. Si se establece una conexión, debería ver el mensaje siguiente:

```bash
You have successfully logged on as: <your Managed Identity username>
```

## <a name="connecting-using-activedirectoryintegrated-authentication-mode"></a>Conexión con el modo de autenticación ActiveDirectoryIntegrated
Con la versión 6.4, el controlador JDBC de Microsoft agrega compatibilidad con la autenticación ActiveDirectoryIntegrated mediante un vale Kerberos en varias plataformas (Windows, Linux y macOS).
Para más información, vea [Establecimiento del vale Kerberos en Windows, Linux y macOS](#set-kerberos-ticket-on-windows-linux-and-macos) para más detalles. Como alternativa, en Windows también se puede usar mssql-jdbc_auth-\<version>-\<arch>.dll para la autenticación ActiveDirectoryIntegrated con el controlador JDBC.

> [!NOTE]
>  Si usa una versión anterior del controlador, compruebe este [vínculo](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md) para las dependencias correspondientes necesarias para usar este modo de autenticación. 

En el siguiente ejemplo se muestra cómo usar el modo `authentication=ActiveDirectoryIntegrated`. Ejecute este ejemplo en una máquina unida al dominio que se federa con Azure Active Directory. En la base de datos debe haber un usuario de base de datos independiente que represente a su usuario de Azure AD o a uno de los grupos a los que pertenece, y debe tener el permiso CONNECT. 

Antes de compilar y ejecutar el ejemplo, en el equipo cliente (donde desea ejecutar el ejemplo), descargue la [biblioteca azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) y sus dependencias, e inclúyalas en la ruta de acceso de compilación de Java.

Reemplace el nombre del servidor o la base de datos por su nombre del servidor o la base de datos en las siguientes líneas antes de ejecutar el ejemplo:

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
```

El ejemplo para usar el modo de autenticación ActiveDirectoryIntegrated:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADIntegrated {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryIntegrated");

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

Al ejecutar este ejemplo en un equipo cliente, su vale Kerberos se usa automáticamente y no es necesaria ninguna contraseña. Si se establece una conexión, debería ver el mensaje siguiente:

```
You have successfully logged on as: <your domain user name>
```

### <a name="set-kerberos-ticket-on-windows-linux-and-macos"></a>Establecimiento del vale Kerberos en Windows, Linux y macOS

Debe configurar un vale de Kerberos que vincule el usuario actual a una cuenta de dominio de Windows. A continuación se incluye un resumen de los pasos clave.

#### <a name="windows"></a>Windows
JDK incluye `kinit`, que puede usar para obtener un TGT del Centro de distribución de claves (KDC) en una máquina unida al dominio que se federa con Azure Active Directory.

##### <a name="step-1-ticket-granting-ticket-retrieval"></a>Paso 1: Recuperación de un vale de concesión de vales
- **Ejecutar en**: Windows
- **Acción**:
  - Use el comando `kinit username@DOMAIN.COMPANY.COM` para obtener un TGT de KDC y, a continuación, le solicitará la contraseña del dominio.
  - Use `klist` para ver los vales disponibles. Si el comando kinit se ha ejecutado correctamente, debería ver un vale en krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM.

> [!NOTE]
>  Es posible que tenga que especificar un archivo `.ini` con `-Djava.security.krb5.conf` para que la aplicación busque KDC.

#### <a name="linux-and-macos"></a>Linux y macOS

##### <a name="requirements"></a>Requisitos
Acceso a un equipo Windows unido a dominio para consultar el controlador de dominio de Kerberos.

##### <a name="step-1-find-kerberos-kdc"></a>Paso 1: Búsqueda de KDC de Kerberos
- **Ejecutar en**: Línea de comandos de Windows
- **Acción**: `nltest /dsgetdc:DOMAIN.COMPANY.COM` (donde "DOMAIN.COMPANY.COM" se asigna al nombre del dominio)
- **Salida de ejemplo**
  ```
  DC: \\co1-red-dc-33.domain.company.com
  Address: \\2111:4444:2111:33:1111:ecff:ffff:3333
  ...
  The command completed successfully
  ```
- **Información para extraer** El nombre del controlador de dominio, en este caso `co1-red-dc-33.domain.company.com`

##### <a name="step-2-configuring-kdc-in-krb5conf"></a>Paso 2: Configuración de KDC en krb5.conf
- **Ejecutar en**: Linux/macOS
- **Acción**: Edite el archivo /etc/krb5.conf en el editor que elija. Configure las claves siguientes
  ```
  [libdefaults]
    default_realm = DOMAIN.COMPANY.COM
   
  [realms]
  DOMAIN.COMPANY.COM = {
     kdc = co1-red-dc-28.domain.company.com
  }
  ```
  Luego, guarde el archivo krb5.conf y salga.

> [!NOTE]
>  El dominio debe estar en MAYÚSCULAS.

##### <a name="step-3-testing-the-ticket-granting-ticket-retrieval"></a>Paso 3: Prueba de la recuperación de un vale de concesión de vales
- **Ejecutar en**: Linux/macOS
- **Acción**:
  - Use el comando `kinit username@DOMAIN.COMPANY.COM` para obtener un TGT de KDC y, a continuación, le solicitará la contraseña del dominio.
  - Use `klist` para ver los vales disponibles. Si el comando kinit se ha ejecutado correctamente, debería ver un vale en krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM.

## <a name="connecting-using-activedirectorypassword-authentication-mode"></a>Conexión con el modo de autenticación ActiveDirectoryPassword
En el siguiente ejemplo se muestra cómo usar el modo `authentication=ActiveDirectoryPassword`.

Antes de compilar y ejecutar el ejemplo:
1.  En el equipo cliente (donde desea ejecutar el ejemplo), descargue la [biblioteca azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) y sus dependencias, e inclúyalas en la ruta de acceso de compilación de Java.
2.  Busque las siguientes líneas de código y reemplace el nombre del servidor o la base de datos por su nombre del servidor o la base de datos.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Busque las siguientes líneas de código y reemplace el nombre del usuario por el nombre del usuario de Azure AD que desea que aparezca cuando se conecte.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ds.setPassword("password");     // replace with your password
    ```

El ejemplo para usar el modo de autenticación ActiveDirectoryPassword:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADUserPassword {
    
    public static void main(String[] args) throws Exception{
        
        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        ds.setPassword("password"); // Replace with your password
        ds.setAuthentication("ActiveDirectoryPassword");
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Si se establece una conexión, debería ver el mensaje siguiente como resultado:
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Debe existir una base de datos de usuario independiente, así como un usuario de la base de datos independiente que represente al usuario de Azure AD especificado, o bien uno de los grupos al que pertenece el usuario de Azure AD especificado. Asimismo, debe contar con el permiso CONNECT (salvo para un grupo o un administrador del servidor de Azure Active Directory).

## <a name="connecting-using-activedirectoryinteractive-authentication-mode"></a>Conexión con el modo de autenticación ActiveDirectoryInteractive 
En el siguiente ejemplo se muestra cómo usar el modo `authentication=ActiveDirectoryInteractive`.

Antes de compilar y ejecutar el ejemplo:
1.  En el equipo cliente (donde desea ejecutar el ejemplo), descargue la [biblioteca azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) y sus dependencias, e inclúyalas en la ruta de acceso de compilación de Java.
2.  Busque las siguientes líneas de código y reemplace el nombre del servidor o la base de datos por su nombre del servidor o la base de datos.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Busque las siguientes líneas de código y reemplace el nombre del usuario por el nombre del usuario de Azure AD que desea que aparezca cuando se conecte.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ```

El ejemplo para usar el modo de autenticación ActiveDirectoryInteractive:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADInteractive {
    public static void main(String[] args) throws Exception{
        
        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
    ds.setAuthentication("ActiveDirectoryInteractive");
      
    // Optional
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Al ejecutar el programa, se mostrará un explorador para autenticar al usuario. Lo que ve el usuario depende de cómo se haya configurado su instancia de Azure AD. Puede incluir o no solicitudes de autenticación multifactor, como un nombre de usuario, una contraseña, un PIN o una segunda autenticación de dispositivo mediante un teléfono, por ejemplo. Si se realizan varias solicitudes de autenticación interactiva en el mismo programa, es posible que las solicitudes posteriores no se envíen al usuario si la biblioteca de autenticación puede volver a usar un token de autenticación previamente almacenado en caché.

Para información sobre cómo configurar Azure AD para exigir Multi-Factor Authentication, consulte [Introducción a Multi-Factor Authentication de Azure AD en la nube](/azure/active-directory/authentication/howto-mfa-getstarted).

Para obtener capturas de pantalla de estos cuadros de diálogo, consulte [Configuración de la autenticación multifactor para SQL Server Management Studio y Azure AD](/azure/azure-sql/database/authentication-mfa-ssms-configure).

Si la autenticación del usuario se completa correctamente, debería ver el siguiente mensaje en el explorador:
```
Authentication complete. You can close the browser and return to the application.
```
Tenga en cuenta que esto solo indica que la autenticación del usuario se ha realizado correctamente, y no implica necesariamente una conexión correcta con el servidor. Al volver a la aplicación, si se ha establecido la conexión con el servidor, debería ver el siguiente mensaje como salida:
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Debe existir una base de datos de usuario independiente, así como un usuario de la base de datos independiente que represente al usuario de Azure AD especificado, o bien uno de los grupos al que pertenece el usuario de Azure AD especificado. Asimismo, debe contar con el permiso CONNECT (salvo para un grupo o un administrador del servidor de Azure Active Directory).

## <a name="connecting-using-activedirectoryserviceprincipal-authentication-mode"></a>Conexión con el modo de autenticación ActiveDirectoryServicePrincipal
En el siguiente ejemplo se muestra cómo usar el modo `authentication=ActiveDirectoryServicePrincipal`.

Antes de compilar y ejecutar el ejemplo:
1.  En el equipo cliente (donde desea ejecutar el ejemplo), descargue la [biblioteca azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) y sus dependencias, e inclúyalas en la ruta de acceso de compilación de Java.
2.  Busque las siguientes líneas de código y reemplace el nombre del servidor o la base de datos por su nombre del servidor o la base de datos.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Busque las siguientes líneas de código y reemplace el nombre del usuario por el nombre del usuario de Azure AD que desea que aparezca cuando se conecte.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ```

El ejemplo para usar el modo de autenticación ActiveDirectoryInteractive:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADServicePrincipal {
    public static void main(String[] args) throws Exception{
        String principalId = "1846943b-ad04-4808-aa13-4702d908b5c1"; // Replace with your AAD secure principal ID.
        String principalSecret = "..."; // Replace with your AAD principal secret.

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
    ds.setAuthentication("ActiveDirectoryServicePrincipal");
    ds.setAADSecurePrincipalId(principalId);
    ds.setAADSecurePrincipalSecret(principalSecret);
      
    // Optional
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Si se establece una conexión, debería ver el siguiente mensaje como resultado:
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Debe existir una base de datos de usuario independiente, así como un usuario de la base de datos independiente que represente al usuario de Azure AD especificado, o bien uno de los grupos al que pertenece el usuario de Azure AD especificado. Asimismo, debe contar con el permiso CONNECT (salvo para un grupo o un administrador del servidor de Azure Active Directory).

## <a name="connecting-using-access-token"></a>Conexión mediante el token de acceso
Las aplicaciones o servicios pueden recuperar un token de acceso de Azure Active Directory y usarlo para conectarse a Azure SQL Database o Synapse Analytics.

> [!NOTE] 
> Solo se puede establecer **accessToken** mediante el parámetro Properties del método getConnection() en la clase DriverManager. No se puede usar en la cadena de conexión.

En el siguiente ejemplo se incluye una aplicación Java sencilla que se conecta a Azure SQL Database o Synapse Analytics mediante la autenticación basada en tokens de acceso. Antes de compilar y ejecutar el ejemplo, realice los pasos siguientes:
1.  Cree una cuenta de aplicación en Azure Active Directory para su servicio.
    1. Inicie sesión en Azure Portal.
    2. Haga clic en Azure Active Directory en la barra de navegación de la izquierda.
    3. Haga clic en la pestaña "Registros de aplicaciones".
    4. En el cajón, haga clic en "Nuevo registro de aplicaciones".
    5. Escriba mytokentest como nombre descriptivo para la aplicación y seleccione "Aplicación web/API".
    6. No necesitamos URL DE INICIO DE SESIÓN. Solo tiene que proporcionar algo: "https://mytokentest".
    7. Haga clic en "Crear" en la parte inferior.
    9. Mientras sigue en Azure Portal, haga clic en la pestaña "Configuración" de su aplicación y abra la pestaña "Propiedades".
    10. Busque el valor "Identificador de la aplicación" (conocido también como identificador de cliente) y cópielo; lo necesitará más adelante cuando configure la aplicación (por ejemplo, 1846943b-ad04-4808-aa13-4702d908b5c1). Consulte la instantánea siguiente.
    11. En la sección "Claves", cree una clave rellenando el campo de nombre, seleccionando la duración de la clave y guardando la configuración (deje el campo de valor vacío). Tras guardarla, el campo de valor debe rellenarse automáticamente. Copie el valor generado. Este es el secreto del cliente.
    12. Haga clic en Azure Active Directory en el panel izquierdo. En "Registros de aplicaciones", busque la pestaña "Puntos finales". Copie la dirección URL en "PUNTO DE CONEXIÓN DE TOKEN DE OATH 2.0". Esta es su dirección URL de STS.
    
    ![Punto de conexión de registro de aplicación de Azure Portal - URL de STS](media/jdbc_aad_token.png)  
2. Inicie sesión en la base de datos de usuario de Azure SQL Server como administrador de Azure Active Directory y use un comando de T-SQL para aprovisionar un usuario de la base de datos independiente para su entidad de seguridad de aplicación. Para más información, vea [Conexión a SQL Database o a Azure Synapse Analytics mediante autenticación de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview) para más detalles sobre cómo crear un administrador de Azure Active Directory y un usuario de la base de datos independiente.

    ```
    CREATE USER [mytokentest] FROM EXTERNAL PROVIDER
    ```

3.  En el equipo cliente en el que quiere ejecutar el ejemplo, descargue la biblioteca [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) y sus dependencias, e inclúyalas en la ruta de acceso de compilación de Java. Tenga en cuenta que la biblioteca microsoft-authentication-library-for-java solo es necesaria para ejecutar este ejemplo específico. En el ejemplo se usan las API de esta biblioteca para recuperar el token de acceso de Azure AD. Si ya tiene un token de acceso, puede omitir este paso. Tenga en cuenta que también debe quitar la sección del ejemplo que recupera el token de acceso.

En el siguiente ejemplo, reemplace la dirección URL de STS, el identificador de cliente, el secreto de cliente, el servidor y el nombre de base de datos por sus valores.

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

// The azure-activedirectory-library-for-java is needed to retrieve the access token from the AD.
import com.microsoft.aad.msal4j.ClientCredentialFactory;
import com.microsoft.aad.msal4j.ClientCredentialParameters;
import com.microsoft.aad.msal4j.ConfidentialClientApplication;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.IClientCredential;

public class AADTokenBased {

    public static void main(String[] args) throws Exception {

        // Retrieve the access token from the AD.
        String spn = "https://database.windows.net/";
        String stsurl = "https://login.microsoftonline.com/..."; // Replace with your STS URL.
        String clientId = "1846943b-ad04-4808-aa13-4702d908b5c1"; // Replace with your client ID.
        String clientSecret = "..."; // Replace with your client secret.

    String scope = spn +  "/.default";
    Set<String> scopes = new HashSet<>();
        scopes.add(scope);
    
    ExecutorService executorService = Executors.newSingleThreadExecutor();
    IClientCredential credential = ClientCredentialFactory.createFromSecret(clientSecret);
    ConfidentialClientApplication clientApplication = ConfidentialClientApplication
            .builder(clientId, credential).executorService(executorService).authority(stsurl).build();
    CompletableFuture<IAuthenticationResult> future = clientApplication
            .acquireToken(ClientCredentialParameters.builder(scopes).build());
            
    IAuthenticationResult authenticationResult = future.get();
    String accessToken = authenticationResult.accessToken();

        System.out.println("Access Token: " + accessToken);

        // Connect with the access token.
        SQLServerDataSource ds = new SQLServerDataSource();

        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name.
        ds.setDatabaseName("demo"); // Replace with your database name.
        ds.setAccessToken(accessToken);

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
``` 

Si la conexión se realiza correctamente, debería ver el siguiente mensaje como resultado:

```bash
Access Token: <your access token>
You have successfully logged on as: <your client ID>    
```
