---
title: Usar Always Encrypted con el controlador JDBC
description: Obtenga información sobre cómo usar Always Encrypted en la aplicación Java con JDBC Driver para SQL Server para cifrar datos confidenciales en el servidor.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 271c0438-8af1-45e5-b96a-4b1cabe32707
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a89deba9ed124587899bc915fa86ef74074ed761
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195278"
---
# <a name="using-always-encrypted-with-the-jdbc-driver"></a>Usar Always Encrypted con el controlador JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

En esta página se proporciona información sobre cómo desarrollar aplicaciones de Java mediante [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) y Microsoft JDBC Driver 6.0 (o posterior) para SQL Server.

Always Encrypted permite a los clientes cifrar la información confidencial y nunca revelar los datos ni las claves de cifrado en SQL Server o Azure SQL Database. Un controlador habilitado para Always Encrypted, como Microsoft JDBC Driver 6.0 (o superior) for SQL Server, consigue este comportamiento al cifrar y descifrar de manera transparente la información confidencial en la aplicación cliente. El controlador determina automáticamente qué parámetros de consulta corresponden a columnas de bases de datos de Always Encrypted y cifra los valores de esos parámetros antes de enviarlos a SQL Server o Azure SQL Database. De forma similar, el controlador descifra de manera transparente los datos que se han recuperado de las columnas de bases de datos cifradas de los resultados de la consulta. Para más información, consulte [Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) y [Referencia de la API de Always Encrypted para el controlador JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).

## <a name="prerequisites"></a>Prerrequisitos
- Asegúrese de que Microsoft JDBC Driver 6.0 (o posterior) para SQL Server está instalado en la máquina de desarrollo. 
- Descargue e instale los archivos de Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy.  Asegúrese de leer el archivo Léame incluido en el archivo zip para obtener instrucciones de instalación y detalles relevantes sobre los posibles problemas de importación o exportación.  

    - Si usa mssql-jdbc-X.X.X.jre7.jar o sqljdbc41.jar, los archivos de directiva pueden descargarse desde [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 7 Download](https://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html).

    - Si usa mssql-jdbc-X.X.X.jre8.jar o sqljdbc42.jar, los archivos de directiva pueden descargarse desde [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 8 Download](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html).

    - Si usa mssql-jdbc-X.X.X.jre9.jar, no es necesario descargar ningún archivo de directiva. La directiva de jurisdicción de Java 9 se establece de manera predeterminada en el nivel de cifrado ilimitado.

## <a name="working-with-column-master-key-stores"></a>Trabajar con almacenes de claves maestras de columna
Parra cifrar o descifrar los datos de las columnas cifradas, SQL Server mantiene las claves de cifrado de columnas. Las claves de cifrado de columnas se almacenan con formato cifrado en los metadatos de la base de datos. Cada clave de cifrado de columnas tiene una clave maestra de columna correspondiente que se ha usado para cifrar la clave de cifrado de columnas. Los metadatos de la base de datos no contienen las claves maestras de columnas. Esas claves solo las conserva el cliente. Sin embargo, los metadatos de la base de datos contienen información sobre dónde se almacenan las claves maestras de columnas en relación con el cliente. Por ejemplo, los metadatos de la base de datos pueden indicar que el almacén de claves que contiene una clave maestra de columna es el Almacén de certificados de Windows y el certificado específico que se usa para cifrar y descifrar está ubicado en una ruta de acceso específica dentro del Almacén de certificados de Windows. Si el cliente tiene acceso a ese certificado en el Almacén de certificados de Windows, puede obtener el certificado. De ese modo, el certificado se puede usar para descifrar la clave de cifrado de columna. A continuación, la clave de cifrado se puede usar para descifrar o cifrar los datos de las columnas cifradas que usan esa clave de cifrado de columna.

Microsoft JDBC Driver para SQL Server se comunica con un almacén de claves mediante un proveedor de almacenamiento de claves maestras de columna, que es una instancia de una clase derivada de **SQLServerColumnEncryptionKeyStoreProvider**.

### <a name="using-built-in-column-master-key-store-providers"></a>Usar proveedores integrados de almacenamiento de claves maestras de columna
Microsoft JDBC Driver para SQL Server incluye estos proveedores de almacenamiento de claves maestras de columna integrados. Algunos de estos proveedores se registran previamente con los nombres de proveedores específicos (que se usan para buscar el proveedor) y algunos requieren credenciales adicionales o registro explícito.

| Clase                                                 | Descripción                                        | Nombre del proveedor (búsqueda)  | ¿Está registrado previamente? | Plataforma |
| :---------------------------------------------------- | :------------------------------------------------- | :---------------------- | :----------------- | :------- |
| **SQLServerColumnEncryptionAzureKeyVaultProvider**    | Proveedor para un almacén de claves para Azure Key Vault. | AZURE_KEY_VAULT         | _No_ antes de la versión 7.4.1 del controlador JDBC, pero _sí_ a partir de la versión 7.4.1 del controlador JDBC. | Windows, Linux, macOS |
| **SQLServerColumnEncryptionCertificateStoreProvider** | Un proveedor para el Almacén de certificados de Windows.      | MSSQL_CERTIFICATE_STORE | _Sí_                | Windows |
| **SQLServerColumnEncryptionJavaKeyStoreProvider**     | Proveedor del almacén de claves de Java.                  | MSSQL_JAVA_KEYSTORE     | _Sí_                | Windows, Linux, macOS |
|||||

En el caso de proveedores de almacén de claves registrados previamente, no es necesario realizar ningún cambio en el código de aplicación para usar estos proveedores, pero tenga en cuenta los elementos siguientes:

- Usted (o su DBA) necesita asegurarse de que el nombre de proveedor, configurado en los metadatos de clave maestra de columna, sea correcto y que la ruta de acceso de la clave maestra de columna cumpla con un formato de ruta de acceso de la clave que sea válido para un proveedor determinado. Se recomienda que configure las claves mediante herramientas como SQL Server Management Studio, que genera automáticamente las rutas de acceso a la clave y los nombres de proveedor válidos al emitir la instrucción CREATE COLUMN MASTER KEY (Transact-SQL).
- Asegúrese de que la aplicación puede tener acceso a la clave del almacén de claves. Esta tarea puede suponer conceder a la aplicación el acceso a la clave o al almacén de claves, dependiendo de este, o bien realizar cualquier otro paso de configuración específico del almacén de claves. Por ejemplo, para usar SQLServerColumnEncryptionJavaKeyStoreProvider, debe proporcionar la ubicación y la contraseña del almacén de claves en las propiedades de conexión. 

Todos estos proveedores de almacén de claves se describen con más detalle en las secciones siguientes. Solo es necesario implementar un proveedor de almacén de claves para usar Always Encrypted.

### <a name="using-azure-key-vault-provider"></a>Usar el proveedor de Azure Key Vault

Azure Key Vault es una opción adecuada para almacenar y administrar claves maestras de columna para Always Encrypted (especialmente si sus aplicaciones se hospedan en Azure). Microsoft JDBC Driver para SQL Server incluye un proveedor integrado, SQLServerColumnEncryptionAzureKeyVaultProvider, para las aplicaciones que tienen claves almacenadas en Azure Key Vault. El nombre de este proveedor es AZURE_KEY_VAULT. Para usar el proveedor de almacén de Azure Key Vault, un desarrollador de aplicaciones debe crear el almacén y las claves en Azure Key Vault y crear un registro de aplicación en Azure Active Directory. La aplicación registrada debe contar con los permisos Get, Decrypt, Encrypt, Unwrap Key, Wrap Key y Verify en las directivas de acceso definidas para el almacén de claves creado para usarlo con Always Encrypted. Para más información sobre cómo configurar el almacén de claves y crear una clave maestra de columna, consulte [Azure Key Vault: paso a paso](/archive/blogs/kv/azure-key-vault-step-by-step) y [Creación y almacenamiento de claves maestras de columnas en Azure Key Vault](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md#creating-column-master-keys-in-azure-key-vault).

Al usar el proveedor Azure Key Vault, el controlador JDBC valida la ruta de acceso de la clave maestra de columna con la lista de puntos de conexión de confianza. A partir de la versión 8.2.2 del controlador, esta lista es configurable: cree el archivo "mssql-jdbc.properties" en el directorio de trabajo de la aplicación y establezca la propiedad `AKVTrustedEndpoints` en una lista delimitada por signos de punto y coma. Si el valor comienza con un punto y coma, extiende la lista predeterminada; en caso contrario, la reemplaza.

En los ejemplos de esta página, si creó una clave maestra de columna y una clave de cifrado de columna basadas en Azure Key Vault mediante SQL Server Management Studio, el script T-SQL para volver a crearlas puede ser similar a este ejemplo con sus propios **KEY_PATH** y **ENCRYPTED_VALUE** específicos:

```sql
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',
    KEY_PATH = N'https://<MyKeyValutName>.vault.azure.net:443/keys/Always-Encrypted-Auto1/c61f01860f37302457fa512bb7e7f4e8'
);

CREATE COLUMN ENCRYPTION KEY [MyCEK]
WITH VALUES
(
    COLUMN_MASTER_KEY = [MyCMK],
    ALGORITHM = 'RSA_OAEP',
    ENCRYPTED_VALUE = 0x01BA000001680074507400700073003A002F002F006400610076006...
);
```

Una aplicación que usa el controlador JDBC puede usar Azure Key Vault. La sintaxis o las instrucciones correspondientes a este uso de Azure Key Vault se modificaron a partir de la versión 7.4.1 del controlador JDBC.

#### <a name="jdbc-driver-741-or-later"></a>Controlador JDBC 7.4.1 o versión posterior

En esta sección se hace referencia a la versión 7.4.1 o posterior del controlador JDBC.

Una aplicación cliente que usa el controlador JDBC se puede configurar para usar Azure Key Vault al mencionar `keyVaultProviderClientId=<ClientId>;keyVaultProviderClientKey=<ClientKey>` en la cadena de conexión de JDBC.

Este es un ejemplo de cómo proporcionar esta información de configuración en una cadena de conexión de JDBC.

```java
String connectionUrl = "jdbc:sqlserver://<server>:<port>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;keyVaultProviderClientId=<ClientId>;keyVaultProviderClientKey=<ClientKey>";
```
El controlador JDBC crea automáticamente una instancia de un objeto **SQLServerColumnEncryptionAzureKeyVaultProvider** cuando estas credenciales están presentes entre las propiedades de la conexión.

> [!IMPORTANT]
> Las propiedades de conexión **keyVaultProviderClientId** y **keyVaultProviderClientKey** han quedado en desuso a partir de la versión 8.4.1. Se recomienda a los usuarios que, en su lugar, usen **keyStoreAuthentication**, **KeyStorePrincipalId** y **KeyStoreSecret**.

#### <a name="jdbc-driver-version-prior-to-741"></a>Versión del controlador JDBC anterior a 7.4.1

En esta sección se hace referencia a las versiones del controlador JDBC anteriores a la versión 7.4.1.

Una aplicación cliente que usa el controlador JDBC debe crear una instancia de un objeto **SQLServerColumnEncryptionAzureKeyVaultProvider** y, luego, registrar el objeto con el controlador.

```java
SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider = new SQLServerColumnEncryptionAzureKeyVaultProvider(clientID, clientKey);
```

**clientID** es el identificador de aplicación de un registro de aplicación en una instancia de Azure Active Directory. **clientKey** es una contraseña de clave registrada en esa aplicación, lo que proporciona acceso de API a Azure Key Vault.

Una vez que la aplicación crea una instancia de SQLServerColumnEncryptionAzureKeyVaultProvider, la aplicación debe registrar la instancia con el controlador a través del método SQLServerConnection.registerColumnEncryptionKeyStoreProviders(). Se recomienda encarecidamente que la instancia se registre con el nombre de búsqueda predeterminado, AZURE_KEY_VAULT, que se puede obtener al llamar a la API SQLServerColumnEncryptionAzureKeyVaultProvider.getName(). Usar el nombre predeterminado le permitirá usar herramientas como SQL Server Management Studio o PowerShell para aprovisionar o administrar claves de Always Encrypted (las herramientas usan el nombre predeterminado para generar el objeto de metadatos para la clave maestra de columna). En el ejemplo siguiente se muestra cómo registrar el proveedor de Azure Key Vault. Para más información sobre el método SQLServerConnection.registerColumnEncryptionKeyStoreProviders(), consulte [Referencia de la API de Always Encrypted para el controlador JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).

```java
Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
keyStoreMap.put(akvProvider.getName(), akvProvider);
SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
```

> [!IMPORTANT]
>  Si usa el proveedor de almacén de claves de Azure Key Vault, la implementación de Azure Key Vault del controlador JDBC tiene dependencias de estas bibliotecas (desde GitHub) que se deben incluir en la aplicación:
>
>  [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java)
>
>  [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java)
>
> Para ver un ejemplo de cómo incluir estas dependencias en un proyecto de Maven, consulte el artículo sobre cómo [descargar las dependencias de MSAL4J y AKV con Apache Maven](https://github.com/microsoft/mssql-jdbc/wiki/Download-MSAL4J-And-AKV-Dependencies-with-Apache-Maven).

### <a name="using-azure-key-vault-authentication-with-managed-identities"></a>Uso de la autenticación de Azure Key Vault con identidades administradas

A partir de JDBC Driver **8.4.1**, el controlador agregó compatibilidad con la autenticación en Azure Key Vault mediante identidades administradas.

Si la aplicación se hospeda en Azure, el usuario puede usar [identidades administradas](/azure/active-directory/managed-identities-azure-resources/overview) para autenticarse en Azure Key Vault, lo que elimina la necesidad de proporcionar y exponer las credenciales en el código. 

#### <a name="connection-properties-for-key-vault-authentication-with-managed-identities"></a>Propiedades de conexión para la autenticación en Key Vault con identidades administradas

Para JDBC Driver 8.4.1 y versiones posteriores, el controlador incorporó las siguientes propiedades de conexión:

| ConnectionProperty    | Emparejamiento de valores posibles 1 | Emparejamiento de valores posibles 2 | Emparejamiento de valores posibles 3 |
| ---|---|---|----|
| keyStoreAuthentication| KeyVaultClientSecret   |KeyVaultManagedIdentity |JavaKeyStorePassword |  
| keyStorePrincipalId   | \<Azure AD Application Client ID\>    | \<Azure AD Application object ID\> (opcional)| N/D |
| keyStoreSecret        | \<Azure AD Application Client Secret\>|N/D|\<secret/password for the Java Key Store\> |

En los siguientes ejemplos se muestra cómo se usan las propiedades de conexión en una cadena de conexión.

#### <a name="use-managed-identity-to-authenticate-to-akv"></a>Uso de la identidad administrada para autenticarse en AKV
```
"jdbc:sqlserver://<server>:<port>;columnEncryptionSetting=Enabled;keyStoreAuthentication=KeyVaultManagedIdentity;"
```
#### <a name="use-managed-identity-and-the-principal-id-to-authenticate-to-akv"></a>Uso de la identidad administrada y el identificador de la entidad de seguridad para autenticarse en AKV
```
"jdbc:sqlserver://<server>:<port>;columnEncryptionSetting=Enabled;keyStoreAuthentication=KeyVaultManagedIdentity;keyStorePrincipal=<principalId>"
```
#### <a name="use-clientid-and-clientsecret-to-authentication-to-akv"></a>Uso de clientId y clientSecret para autenticarse en AKV
```
"jdbc:sqlserver://<server>:<port>;columnEncryptionSetting=Enabled;keyStoreAuthentication=KeyVaultClientSecret;keyStorePrincipalId=<clientId>;keyStoreSecret=<clientSecret>"
```
Se recomienda a los usuarios que usen estas propiedades de conexión para especificar el tipo de autenticación que se usa para los almacenes de claves, en lugar de usar `SQLServerColumnEncryptionAzureKeyVaultProvider`.

Tenga en cuenta que las propiedades de conexión agregadas anteriormente `keyVaultProviderClientId` y `keyVaultProviderClientKey` están en desuso y que se reemplazaron por las propiedades de conexión descritas anteriormente.

Para obtener información sobre cómo configurar las identidades administradas, vea [Configuración de identidades administradas de recursos de Azure en una VM de Azure mediante Azure Portal](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm).

### <a name="using-windows-certificate-store-provider"></a>Uso del proveedor para el Almacén de certificados de Windows
SQLServerColumnEncryptionCertificateStoreProvider, que se puede utilizar para almacenar claves maestras de columna en el almacén de certificados de Windows. Use el Asistente de Always Encrypted para SQL Server Management Studio (SSMS) u otras herramientas compatibles para crear las definiciones de clave maestra de columna y de clave de cifrado de columna en la base de datos. Se puede usar el mismo asistente para generar un certificado autofirmado en el Almacén de certificados de Windows que se puede utilizar como clave maestra de columna para los datos de Always Encrypted. Para más información sobre la sintaxis T-SQL de la clave maestra de columna y de la clave de cifrado de columna, consulte[CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) y [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md), respectivamente.

El nombre del SQLServerColumnEncryptionCertificateStoreProvider es MSSQL_CERTIFICATE_STORE y se puede consultar mediante la API getName() del objeto de proveedor. Lo registra automáticamente el controlador y se puede usar sin problemas sin ningún cambio en la aplicación.

En los ejemplos de esta página, si creó una clave maestra de columna y una clave de cifrado de columna basadas en el Almacén de certificados de Windows mediante SQL Server Management Studio, el script T-SQL para volver a crearlas puede ser similar a este ejemplo con sus propios **KEY_PATH** y **ENCRYPTED_VALUE** específicos:

```sql
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',
    KEY_PATH = N'CurrentUser/My/A2A91F59C461B559E4D962DA9D2BC6131B32CB91'
);

CREATE COLUMN ENCRYPTION KEY [MyCEK]
WITH VALUES
(
    COLUMN_MASTER_KEY = [MyCMK],
    ALGORITHM = 'RSA_OAEP',
    ENCRYPTED_VALUE = 0x016E000001630075007200720065006E0074007500730065007200...
);
```

> [!IMPORTANT]
> Si bien los otros proveedores de almacén de claves mencionados en este artículo están disponibles en todas las plataformas compatibles con el controlador, la implementación de SQLServerColumnEncryptionCertificateStoreProvider del controlador JDBC solo está disponible en los sistemas operativos de Windows. Tiene una dependencia del archivo mssql-jdbc_auth-\<version>-\<arch>.dll que está disponible en el paquete de controladores. Para usar este proveedor, copie el archivo mssql-jdbc_auth-\<version>-\<arch>.dll en un directorio de la ruta del sistema de Windows del equipo en el que esté instalado el controlador JDBC. También puede establecer la propiedad del sistema java.libary.path para especificar el directorio de mssql-jdbc_auth-\<version>-\<arch>.dll. Si ejecuta una máquina virtual Java (JVM) de 32 bits, use el archivo mssql-jdbc_auth-\<version>-x86.dll de la carpeta x86, incluso si la versión del sistema operativo es x64. Si ejecuta una JVM de 64 bits en un procesador x64, use el archivo mssql-jdbc_auth-\<version>-x64.dll de la carpeta x64. Por ejemplo, si está usando JVM de 32 bits y el controlador JDBC está instalado en el directorio predeterminado, puede especificar la ubicación de la DLL con el siguiente argumento de máquina virtual (VM) si la aplicación de Java se ha iniciado: `-Djava.library.path=C:\Microsoft JDBC Driver <version> for SQL Server\sqljdbc_<version>\enu\auth\x86`

### <a name="using-java-key-store-provider"></a>Uso del proveedor del almacén de claves de Java
El controlador JDBC incluye una clave integrada para la implementación del proveedor del almacén de claves para el almacén de claves de Java. Si la propiedad **keyStoreAuthentication** de la cadena de conexión está presente en la cadena de conexión y está establecida en "JavaKeyStorePassword", el controlador crea automáticamente una instancia del proveedor y lo registra para el almacén de claves de Java. El nombre del proveedor del almacén de claves de Java es MSSQL_JAVA_KEYSTORE. Este nombre también se puede consultar mediante la API SQLServerColumnEncryptionJavaKeyStoreProvider.getName(). 

Hay tres propiedades de la cadena de conexión que permiten que una aplicación cliente especifique las credenciales que el controlador necesita para autenticarse en el almacén de claves de Java. El controlador inicializa el proveedor en función de los valores de estas tres propiedades en la cadena de conexión.

**keyStoreAuthentication:** identifica el almacén de claves de Java que se va a usar. Con Microsoft JDBC Driver 6.0 y versiones posteriores para SQL Server, solo puede autenticarse en el almacén de claves de Java a través de esta propiedad. En el almacén de claves de Java, el valor de esta propiedad debe ser `JavaKeyStorePassword`.

**keyStoreLocation:** la ruta de acceso al archivo del almacén de claves de Java que almacena la clave maestra de columna. La ruta de acceso incluye el nombre de archivo del almacén de claves.

**keyStoreSecret:** el secreto o la contraseña que se usará para el almacén de claves, así como para la clave. Para usar el almacén de claves de Java, el almacén de claves y la contraseña de la clave deben ser los mismos.

Este es un ejemplo de cómo proporcionar estas credenciales en la cadena de conexión:

```java
String connectionUrl = "jdbc:sqlserver://<server>:<port>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;keyStoreAuthentication=JavaKeyStorePassword;keyStoreLocation=<path_to_the_keystore_file>;keyStoreSecret=<keystore_key_password>";
```

También puede obtener o establecer esta configuración mediante el objeto SQLServerDataSource. Para más información, consulte [Referencia de la API de Always Encrypted para el controlador JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).

El controlador JDBC crea automáticamente instancias de SQLServerColumnEncryptionJavaKeyStoreProvider cuando estas credenciales están presentes en las propiedades de conexión.

### <a name="creating-a-column-master-key-for-the-java-key-store"></a>Creación de una clave maestra de columna para el almacén de claves de Java
SQLServerColumnEncryptionJavaKeyStoreProvider se puede usar con los tipos de almacén de claves JKS o PKCS12. Para crear o importar una clave para usarla con este proveedor, use la utilidad [keytool](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html) de Java. La clave debe tener la misma contraseña que el almacén de claves mismo. Este es un ejemplo de cómo usar la utilidad keytool para crear una clave pública y su clave privada asociada:

```console
keytool -genkeypair -keyalg RSA -alias AlwaysEncryptedKey -keystore keystore.jks -storepass mypassword -validity 360 -keysize 2048 -storetype jks
```

Este comando crea una clave pública y la encapsula en un certificado X.509 autofirmado, que se almacena en el almacén de claves `keystore.jks` junto con su clave privada asociada. Esta entrada en el almacén de claves se identifica mediante el alias `AlwaysEncryptedKey`.

Este es un ejemplo de lo mismo pero usando un tipo de almacén PKCS12:

```console
keytool -genkeypair -keyalg RSA -alias AlwaysEncryptedKey -keystore keystore.pfx -storepass mypassword -validity 360 -keysize 2048 -storetype pkcs12 -keypass mypassword
```

Si el almacén de claves es del tipo PKCS12, la utilidad keytool no pide una contraseña de clave y la contraseña de clave se debe proporcionar con la opción `-keypass`, porque **SQLServerColumnEncryptionJavaKeyStoreProvider** requiere que el almacén de claves y la clave tengan la misma contraseña.

También puede exportar un certificado del Almacén de certificados de Windows en formato .pfx y usarlo con **SQLServerColumnEncryptionJavaKeyStoreProvider**. El certificado exportado también se puede importar al almacén de claves de Java como tipo de almacén de claves JKS.

Después de crear la entrada keytool, cree los metadatos de la clave maestra de columna en la base de datos, que necesita el nombre del proveedor de almacén de claves y la ruta de acceso de la clave. Para más información sobre cómo crear los metadatos de claves maestras de columna, consulte [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md). En el caso de SQLServerColumnEncryptionJavaKeyStoreProvider, la ruta de acceso de la clave es simplemente el alias de la clave y el nombre de SQLServerColumnEncryptionJavaKeyStoreProvider es `MSSQL_JAVA_KEYSTORE`. También puede consultar este nombre mediante la API pública getName() de la clase SQLServerColumnEncryptionJavaKeyStoreProvider. 

La sintaxis T-SQL para crear la clave maestra de columna es:

```sql
CREATE COLUMN MASTER KEY [<CMK_name>]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_JAVA_KEYSTORE',
    KEY_PATH = N'<key_alias>'
);
```

Para "AlwaysEncryptedKey" que se creó anteriormente, la definición de la clave maestra de columna sería:

```sql
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_JAVA_KEYSTORE',
    KEY_PATH = N'AlwaysEncryptedKey'
);
```

> [!NOTE]
> La funcionalidad integrada de SQL Server Management Studio no puede crear las definiciones de la columna maestra de columna para el almacén de claves de Java. Los comandos T-SQL se deben usar mediante programación.

### <a name="creating-a-column-encryption-key-for-the-java-key-store"></a>Creación de una clave de cifrado de columna para el almacén de claves de Java
SQL Server Management Studio ni ninguna otra herramienta se puede usar para crear claves de cifrado de columna mediante claves maestras de columna en el almacén de claves de Java. La aplicación cliente debe crear la clave de cifrado de columna mediante programación con la clase SQLServerColumnEncryptionJavaKeyStoreProvider. Para más información, consulte [Usar proveedores de almacén de claves maestras de columna para el aprovisionamiento de claves mediante programación](#using-column-master-key-store-providers-for-programmatic-key-provisioning).

### <a name="implementing-a-custom-column-master-key-store-provider"></a>Implementar un proveedor personalizado de almacén de claves maestras de columna
Si quiere almacenar claves maestras de columna en un almacén de claves que no sea compatible con un proveedor existente, puede implementar un proveedor personalizado mediante la extensión de la clase SQLServerColumnEncryptionKeyStoreProvider y registrando el proveedor mediante el método SQLServerConnection.registerColumnEncryptionKeyStoreProviders().

```java
public class MyCustomKeyStore extends SQLServerColumnEncryptionKeyStoreProvider{  
    private String name = "MY_CUSTOM_KEYSTORE";

    public void setName(String name)
    {
        this.name = name;
    }

    public String getName()
    {
        return name;
    }

    public byte[] encryptColumnEncryptionKey(String masterKeyPath, String encryptionAlgorithm, byte[] plainTextColumnEncryptionKey)
    {
        // Logic for encrypting the column encryption key
    }

    public byte[] decryptColumnEncryptionKey(String masterKeyPath, String encryptionAlgorithm, byte[] encryptedColumnEncryptionKey)
    {
        // Logic for decrypting the column encryption key
    }
}
```

Registrar el proveedor:

```java
SQLServerColumnEncryptionKeyStoreProvider storeProvider = new MyCustomKeyStore();
Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
keyStoreMap.put(storeProvider.getName(), storeProvider);
SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
```

## <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>Usar proveedores de almacén de claves maestras de columna para el aprovisionamiento de claves mediante programación
Al tener acceso a las columnas cifradas, Microsoft JDBC Driver for SQL Server busca y llama de manera transparente al proveedor de almacenamiento de claves maestras de columna adecuado para descifrar las claves de cifrado de columnas. Normalmente, el código de aplicación normal no llama directamente a los proveedores de almacenamiento de claves maestras de columna. Sin embargo, puede crear instancias de un proveedor y llamarlo mediante programación para aprovisionar y administrar las claves de Always Encrypted. Este paso puede realizarse para generar una clave de cifrado de columna cifrada y descifrar una clave de cifrado de columnas como parte de la rotación de claves maestras de columna, por ejemplo. Para obtener más información, vea [Overview of Key Management for Always Encrypted (Información general de administración de claves de Always Encrypted)](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

Si usa un proveedor de claves personalizado, puede que sea necesario implementar sus propias herramientas de administración de claves. Al usar claves almacenadas en el Almacén de certificados de Windows o en Azure Key Vault, puede usar herramientas existentes como SQL Server Management Studio o PowerShell para administrar y aprovisionar claves. Cuando se usan claves almacenadas en el almacén de claves de Java, es necesario aprovisionar claves mediante programación. En el ejemplo siguiente se muestra el uso de la clase SQLServerColumnEncryptionJavaKeyStoreProvider para cifrar la clave con una clave almacenada en el almacén de claves de Java.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerColumnEncryptionJavaKeyStoreProvider;
import com.microsoft.sqlserver.jdbc.SQLServerColumnEncryptionKeyStoreProvider;
import com.microsoft.sqlserver.jdbc.SQLServerException;

/**
 * This program demonstrates how to create a column encryption key programmatically for the Java Key Store.
 */
public class AlwaysEncrypted {
    // Alias of the key stored in the keystore.
    private static String keyAlias = "<provide key alias>";

    // Name by which the column master key will be known in the database.
    private static String columnMasterKeyName = "MyCMK";

    // Name by which the column encryption key will be known in the database.
    private static String columnEncryptionKey = "MyCEK";

    // The location of the keystore.
    private static String keyStoreLocation = "C:\\Dev\\Always Encrypted\\keystore.jks";

    // The password of the keystore and the key.
    private static char[] keyStoreSecret = "********".toCharArray();

    /**
     * Name of the encryption algorithm used to encrypt the value of the column encryption key. The algorithm for the system providers must be
     * RSA_OAEP.
     */
    private static String algorithm = "RSA_OAEP";

    public static void main(String[] args) {
        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;";

        try (Connection connection = DriverManager.getConnection(connectionUrl);
                Statement statement = connection.createStatement();) {

            // Instantiate the Java Key Store provider.
            SQLServerColumnEncryptionKeyStoreProvider storeProvider = new SQLServerColumnEncryptionJavaKeyStoreProvider(keyStoreLocation,
                    keyStoreSecret);

            byte[] encryptedCEK = getEncryptedCEK(storeProvider);

            /**
             * Create column encryption key For more details on the syntax, see:
             * https://docs.microsoft.com/sql/t-sql/statements/create-column-encryption-key-transact-sql Encrypted column encryption key first needs
             * to be converted into varbinary_literal from bytes, for which byteArrayToHex() is used.
             */
            String createCEKSQL = "CREATE COLUMN ENCRYPTION KEY "
                    + columnEncryptionKey
                    + " WITH VALUES ( "
                    + " COLUMN_MASTER_KEY = "
                    + columnMasterKeyName
                    + " , ALGORITHM =  '"
                    + algorithm
                    + "' , ENCRYPTED_VALUE =  0x"
                    + byteArrayToHex(encryptedCEK)
                    + " ) ";
            statement.executeUpdate(createCEKSQL);
            System.out.println("Column encryption key created with name : " + columnEncryptionKey);
        }
        // Handle any errors that may have occurred.
        catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static byte[] getEncryptedCEK(SQLServerColumnEncryptionKeyStoreProvider storeProvider) throws SQLServerException {
        String plainTextKey = "You need to give your plain text";

        // plainTextKey has to be 32 bytes with current algorithm supported
        byte[] plainCEK = plainTextKey.getBytes();

        // This will give us encrypted column encryption key in bytes
        byte[] encryptedCEK = storeProvider.encryptColumnEncryptionKey(keyAlias, algorithm, plainCEK);

        return encryptedCEK;
    }

    public static String byteArrayToHex(byte[] a) {
        StringBuilder sb = new StringBuilder(a.length * 2);
        for (byte b : a)
            sb.append(String.format("%02x", b).toUpperCase());
        return sb.toString();
    }
}
```

## <a name="enabling-always-encrypted-for-application-queries"></a>Habilitar Always Encrypted para consultas de la aplicación
Establecer el valor de la palabra clave de cadena de conexión **columnEncryptionSetting** en **Enabled** es la manera más sencilla de habilitar el cifrado de parámetros y el descifrado de los resultados de la consulta que tienen como destino las columnas cifradas.

La cadena de conexión siguiente es un ejemplo de cómo habilitar Always Encrypted en el controlador JDBC:

```java
String connectionUrl = "jdbc:sqlserver://<server>:<port>;user=<user>;password=<password>;databaseName=<database>;columnEncryptionSetting=Enabled;";
SQLServerConnection connection = (SQLServerConnection) DriverManager.getConnection(connectionUrl);
```

El código siguiente es un ejemplo equivalente que usa el objeto SQLServerDataSource:

```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName("<server>");
ds.setPortNumber(<port>);
ds.setUser("<user>");
ds.setPassword("<password>");
ds.setDatabaseName("<database>");
ds.setColumnEncryptionSetting("Enabled");
SQLServerConnection con = (SQLServerConnection) ds.getConnection();
```

Always Encrypted también puede habilitarse para las consultas individuales. Para obtener más información, vea [Controlar el impacto en el rendimiento de Always Encrypted](#controlling-the-performance-impact-of-always-encrypted). Habilitar Always Encrypted no es suficiente para que el cifrado o el descifrado se realicen correctamente. También necesita asegurarse de que:
- La aplicación tiene los permisos de base de datos *VIEW ANY COLUMN MASTER KEY DEFINITION* y *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* , que se requieren para tener acceso a los metadatos sobre las claves Always Encrypted de la base de datos. Para obtener más información, vea [Permisos en Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
- La aplicación puede tener acceso a la clave maestra de columna que protege las claves de cifrado de columnas, las cuales cifran las columnas de bases de datos consultadas. Para usar el proveedor del almacén de claves de Java, debe proporcionar credenciales adicionales en la cadena de conexión. Para más información, consulte [Uso del proveedor del almacén de claves de Java](#using-java-key-store-provider).

### <a name="configuring-how-javasqltime-values-are-sent-to-the-server"></a>Configurar el modo en que los valores java.sql.Time se envían al servidor
Puede configurar la forma de enviar el valor java.sql.Time al servidor usando la propiedad de conexión **sendTimeAsDatetime**. Cuando se establece en false, el valor de tiempo se envía como un tipo Time de SQL Server. Cuando se establece en true, el valor de tiempo se envía como un tipo DateTime. Si una columna de hora está cifrada, la propiedad **sendTimeAsDatetime** debe ser false, porque las columnas cifradas no admiten la conversión de time a datetime. Tenga en cuenta también que esta propiedad es true de forma predeterminada, por lo que cuando use columnas de hora cifradas tendrá que establecerla en false. En caso contrario, el controlador inicia una excepción. A partir de la versión 6.0 del controlador, la clase SQLServerConnection tiene dos métodos para configurar el valor de esta propiedad mediante programación:
 
* public void setSendTimeAsDatetime(boolean sendTimeAsDateTimeValue)
* public boolean getSendTimeAsDatetime()

Para más información sobre esta propiedad, consulte [Configurar el modo en que los valores java.sql.Time se envían al servidor](configuring-how-java-sql-time-values-are-sent-to-the-server.md).

### <a name="configuring-how-string-values-are-sent-to-the-server"></a>Configuración del envío de los valores de cadena al servidor
La propiedad de conexión **sendStringParametersAsUnicode** se utiliza para configurar el modo en que se envían los valores de cadena a SQL Server. Si se establece en true, se envían parámetros String al servidor en formato Unicode. Si se establece en false, los parámetros de cadena se envían en formato no Unicode, como ASCII o MBCS, en lugar de Unicode. El valor predeterminado de esta propiedad es true. Cuando Always Encrypted está habilitado y hay una columna char/varchar/varchar(max) cifrada, el valor de **sendStringParametersAsUnicode** se debe establecer en false. Si esta propiedad se establece en true, el controlador generará una excepción al descifrar los datos desde una columna char/varchar/varchar(max) cifrada que tenga caracteres Unicode. Para más información sobre esta propiedad, consulte [Establecimiento de las propiedades de conexión](setting-the-connection-properties.md).
  
## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Recuperar y modificar los datos de las columnas cifradas
Una vez que habilite Always Encrypted para las consultas de aplicación, puede usar las API de JDBC estándar para recuperar o modificar los datos de las columnas de base de datos cifradas. Si la aplicación tiene los permisos de base de datos necesarios y puede acceder a la clave maestra de columna, el controlador cifrará los parámetros de consulta que tengan como destino las columnas cifradas y descifrará los datos recuperados de las columnas cifradas.

Si Always Encrypted no está habilitado, se producirá un error en las consultas con parámetros que tengan como destino las columnas cifradas. Las consultas todavía pueden recuperar datos de las columnas cifradas, siempre y cuando la consulta no tenga parámetros que tengan como destino las columnas cifradas. Pero el controlador no intentará descifrar ningún valor que se haya recuperado de las columnas cifradas y la aplicación recibirá datos binarios cifrados (como matrices de bytes).

En la tabla siguiente se resume el comportamiento de las consultas, dependiendo de si Always Encrypted está habilitado o no:

| Característica de las consultas                                                                           | Always Encrypted está habilitado y la aplicación puede tener acceso a las claves y a los metadatos de clave                                                                                                                        | Always Encrypted está habilitado y la aplicación no puede tener acceso a las claves ni a los metadatos de clave | Always Encrypted está deshabilitado                                                                                        |
| :--------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------ |
| Consultas con parámetros que tienen como destino las columnas cifradas.                                           | Los valores de parámetro se cifran de manera transparente.                                                                                                                                                           | Error                                                                             | Error                                                                                                               |
| Consultas que recuperan datos de las columnas cifradas, sin parámetros que tengan como destino las columnas cifradas. | Los resultados de las columnas cifradas se descifran de manera transparente. La aplicación recibe valores de texto sin formato de los tipos de datos JDBC que corresponden a los tipos de SQL Server configurados para las columnas cifradas. | Error                                                                             | Los resultados de las columnas cifradas no se descifran. La aplicación recibe valores cifrados como matrices de bytes (byte[]). |

### <a name="inserting-and-retrieving-encrypted-data-examples"></a>Ejemplos de inserción y recuperación de datos cifrados

En el siguiente ejemplo se ilustra la recuperación y la modificación de datos en las columnas cifradas. En los ejemplos se supone que la tabla de destino tiene el esquema siguiente y las columnas SSN y BirthDate cifradas. Si configuró una clave maestra de columna denominada "MyCMK" y una clave de cifrado de columna denominada "MyCEK" (como se describe en las secciones anteriores sobre los proveedores de almacén de claves), puede crear la tabla con este script:

```sql
CREATE TABLE [dbo].[Patients]([PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = MyCEK) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = MyCEK) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY]);
 GO
```

Para cada ejemplo de código de Java, deberá insertar código específico del almacén de claves en la ubicación indicada.

Si utiliza un proveedor del almacén de claves de Azure Key Vault:

```java
    String clientID = "<Azure Application ID>";
    String clientKey = "<Azure Application API Key Password>";
    SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider = new SQLServerColumnEncryptionAzureKeyVaultProvider(clientID, clientKey);
    Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
    keyStoreMap.put(akvProvider.getName(), akvProvider);
    SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
    String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;";
```

Si utiliza un proveedor del almacén de certificados de Windows:

```java
    String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;";
```

Si utiliza un proveedor del almacén de claves de Java:

```java
    String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;keyStoreAuthentication=JavaKeyStorePassword;keyStoreLocation=<path to jks or pfx file>;keyStoreSecret=<keystore secret/password>";
```

### <a name="inserting-data-example"></a>Ejemplo de inserción de datos

En este ejemplo se inserta una fila en la tabla Patients. Tenga en cuenta los siguientes aspectos:

- No existe nada específico al cifrado en el código de ejemplo. Microsoft JDBC Driver para SQL Server detecta y cifra automáticamente los parámetros que tienen como destino las columnas cifradas. Este comportamiento hace que el cifrado se realice de manera transparente en la aplicación.
- Los valores que se insertan en las columnas de bases de datos, incluidas las columnas cifradas, se pasan como parámetros mediante SQLServerPreparedStatement. Aunque el uso de parámetros es opcional al enviar valores a las columnas no cifradas (aunque es altamente recomendable porque ayuda a evitar la inyección de código SQL), es necesario para los valores que tienen como destino las columnas cifradas. Si los valores insertados en las columnas cifradas se pasaron como literales insertados en la instrucción de consulta, la consulta podría generar un error porque el controlador no sería capaz de determinar los valores de las columnas cifradas de destino y no cifraría los valores. Como resultado, el servidor los rechazará considerándolos incompatibles con las columnas cifradas.
- Todos los valores impresos por el programa estarán en texto sin formato, ya que Microsoft JDBC Driver for SQL Server descifrará los datos que se han recuperado de las columnas cifradas.
- Si está realizando una búsqueda con una cláusula WHERE, el valor utilizado en la cláusula WHERE se debe pasar como un parámetro para que el controlador pueda cifrarlo de manera transparente antes de enviarlo a la base de datos. En el ejemplo siguiente, el número del seguro social (SNN) se pasa como parámetro, pero el apellido (LastName) se pasa como literal, ya que LastName no está cifrado.
- El método Setter usado para el parámetro que tiene como destino la columna SSN es setString (), que se asigna al tipo de datos char/varchar de SQL Server. Si el método establecedor que se ha usado para este parámetro es setNString(), que se asigna a nchar o nvarchar, la consulta producirá un error, ya que Always Encrypted no admite conversiones de valores nchar o nvarchar cifrados a valores char o varchar cifrados.

```java
// <Insert keystore-specific code here>
try (Connection sourceConnection = DriverManager.getConnection(connectionUrl);
        PreparedStatement insertStatement = sourceConnection.prepareStatement("INSERT INTO [dbo].[Patients] VALUES (?, ?, ?, ?)")) {
    insertStatement.setString(1, "795-73-9838");
    insertStatement.setString(2, "Catherine");
    insertStatement.setString(3, "Abel");
    insertStatement.setDate(4, Date.valueOf("1996-09-10"));
    insertStatement.executeUpdate();
    System.out.println("1 record inserted.\n");
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="retrieving-plaintext-data-example"></a>Ejemplo de la recuperación de datos de texto sin formato

En el siguiente ejemplo se muestra el filtrado de datos en función de valores cifrados, así como la recuperación de datos de texto sin formato de las columnas cifradas. Tenga en cuenta los siguientes aspectos:

- El valor que se ha usado en la cláusula WHERE para filtrar por la columna SSN necesita pasarse como un parámetro, de forma que Microsoft JDBC Driver for SQL Server pueda cifrarlo de manera transparente antes de enviarlo a la base de datos.
- Todos los valores impresos por el programa estarán en texto sin formato, ya que Microsoft JDBC Driver for SQL Server descifrará los datos que se han recuperado de las columnas SSN y BirthDate de manera transparente.

> [!NOTE]
> Si las columnas están cifradas mediante el cifrado determinista, las consultas pueden realizar comparaciones de igualdad en ellas. Para obtener más información, vea [Selección del cifrado determinista o aleatorio de Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```java
// <Insert keystore-specific code here>
try (Connection connection = DriverManager.getConnection(connectionUrl);
        PreparedStatement selectStatement = connection
                .prepareStatement("\"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN = ?;\"");) {
    selectStatement.setString(1, "795-73-9838");
    ResultSet rs = selectStatement.executeQuery();
    while (rs.next()) {
        System.out.println("SSN: " + rs.getString("SSN") + ", FirstName: " + rs.getString("FirstName") + ", LastName:"
                + rs.getString("LastName") + ", Date of Birth: " + rs.getString("BirthDate"));
    }
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="retrieving-encrypted-data-example"></a>Ejemplo de la recuperación de los datos cifrados

Si Always Encrypted no está habilitado, una consulta todavía puede recuperar datos de las columnas cifradas, siempre y cuando la consulta no tenga parámetros que tengan como destino las columnas cifradas.

En el siguiente ejemplo se ilustra la recuperación de datos binarios cifrados de las columnas cifradas. Tenga en cuenta los siguientes aspectos:

- Como Always Encrypted no está habilitado en la cadena de conexión, la consulta devolverá valores cifrados de SSN y BirthDate como matrices de bytes (el programa convierte los valores en cadenas).
- Una consulta que recupera datos de columnas cifradas con Always Encrypted deshabilitado puede tener parámetros, siempre y cuando ninguno de ellos tenga como destino una columna cifrada. La siguiente consulta filtra por LastName, que no está cifrado en la base de datos. Si la consulta ha filtrado por SSN o BirthDate, esta producirá un error.

```java
try (Connection sourceConnection = DriverManager.getConnection(connectionUrl);
        PreparedStatement selectStatement = sourceConnection
                .prepareStatement("SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE LastName = ?;");) {

    selectStatement.setString(1, "Abel");
    ResultSet rs = selectStatement.executeQuery();
    while (rs.next()) {
        System.out.println("SSN: " + rs.getString("SSN") + ", FirstName: " + rs.getString("FirstName") + ", LastName:"
                + rs.getString("LastName") + ", Date of Birth: " + rs.getString("BirthDate"));
    }
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Evitar problemas comunes al consultar columnas cifradas

En esta sección se describen las categorías comunes de errores al consultar columnas cifradas de aplicaciones Java y algunas instrucciones sobre cómo evitarlos.

### <a name="unsupported-data-type-conversion-errors"></a>Errores de conversión de tipos de datos no compatibles

Always Encrypted admite algunas conversiones para los tipos de datos cifrados. Vea [Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) para obtener una lista detallada de las conversiones de tipos compatibles. Esto es lo que puede hacer para evitar los errores de conversión de tipos de datos. Asegúrese de lo siguiente:

- Se usan los métodos Setter adecuados al pasar valores para los parámetros que tienen como destino las columnas cifradas. Asegúrese de que el tipo de datos de SQL Server del parámetro sea exactamente el mismo que el tipo de la columna de destino o una conversión compatible del tipo de datos de SQL Server del parámetro al tipo de destino de la columna. Los métodos de API se han agregado a las clases SQLServerPreparedStatement, SQLServerCallableStatement y SQLServerResultSet para pasar los parámetros correspondientes a tipos de datos de SQL Server específicos. Por ejemplo, si una columna no está cifrada, puede usar el método setTimestamp() para pasar un parámetro a una columna datetime2 o datetime. Pero cuando una columna está cifrada, tendrá que usar el método exacto que representa el tipo de la columna en la base de datos. Por ejemplo, use setTimestamp() para pasar valores a una columna datetime2 cifrada y use setDateTime() para pasar los valores a una columna datetime cifrada. Consulte [Referencia de la API de Always Encrypted para el controlador JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md) para ver una lista completa de las API nuevas.
- la precisión y la escala de los parámetros que tienen como destino columnas de los tipos de datos numéricos y decimales de SQL Server debe ser la misma que la precisión y la escala configurada para la columna de destino. Los métodos de API se han agregado a las clases SQLServerPreparedStatement, SQLServerCallableStatement y SQLServerResultSet para aceptar la precisión y la escala, junto con los valores de los datos de los parámetros o columnas que representan los tipos de datos decimales y numéricos. Consulte [Referencia de la API de Always Encrypted para el controlador JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md) para ver una lista completa de las API nuevas o sobrecargadas.  
- La precisión o escala de fracciones de segundos de los parámetros que tienen como destino las columnas de tipos de datos datetime2, datetimeoffset o time de SQL Server no debe ser superior a la precisión o escala de fracciones de segundos de la columna de destino, en las consultas que modifiquen valores de la columna de destino. Los métodos de API se han agregado a las clases SQLServerPreparedStatement, SQLServerCallableStatement y SQLServerResultSet para aceptar la precisión y la escala de fracciones de segundos, junto con los valores de los datos de los parámetros que representan estos tipos de datos. Para una lista completa de API nuevas o sobrecargadas, consulte [Referencia de la API de Always Encrypted para el controlador JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).

### <a name="errors-due-to-incorrect-connection-properties"></a>Errores debidos a propiedades de conexión incorrectas

En esta sección se describe cómo configurar correctamente la conexión para usar los datos de Always Encrypted. Como los tipos de datos cifrados admiten conversiones limitadas, los valores **sendTimeAsDatetime** y **sendStringParametersAsUnicode** de la conexión se deben configurar correctamente al usar columnas cifradas. Asegúrese de lo siguiente:

- El valor [sendTimeAsDatetime](setting-the-connection-properties.md) de la conexión está establecido en false al insertar los datos en columnas de hora cifradas. Para obtener más información, vea [Configurar el modo en que los valores java.sql.Time se envían al servidor](configuring-how-java-sql-time-values-are-sent-to-the-server.md).
- El valor [sendStringParametersAsUnicode](setting-the-connection-properties.md) de la conexión está establecido en true (o se deja como valor predeterminado) al insertar los datos en columnas char/varchar/varchar(max) cifradas.

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Errores debidos a pasar texto sin formato en lugar de valores cifrados

Cualquier valor que tenga como destino una columna cifrada necesita cifrarse dentro de la aplicación. Un intento para insertar, modificar o filtrar mediante un valor de texto sin formato en una columna cifrada producirá un error similar a este:

```java
com.microsoft.sqlserver.jdbc.SQLServerException: Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'MyCEK', column_encryption_key_database_name = 'ae') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

Para evitar dichos errores, asegúrese de que:

- Always Encrypted está habilitado para las consultas de la aplicación que tienen como destino columnas cifradas (para la cadena de conexión o para una consulta específica).
- Usa instrucciones y parámetros preparados para enviar datos que tienen como destino columnas cifradas. En el ejemplo siguiente se muestra una consulta que filtra de manera incorrecta mediante un literal o constante en una columna cifrada (SSN), en lugar de pasar el literal dentro de un parámetro. Esta consulta generará un error:

```java
ResultSet rs = connection.createStatement().executeQuery("SELECT * FROM Customers WHERE SSN='795-73-9838'");
```

## <a name="force-encryption-on-input-parameters"></a>Forzar cifrado en los parámetros de entrada

La característica Forzar cifrado fuerza el cifrado de un parámetro cuando se usa Always Encrypted. Si se usa Forzar cifrado y SQL Server indica al controlador que no hace falta cifrar el parámetro, la consulta que utilice este último generará un error. Esta propiedad confiere una protección adicional frente a ataques contra la seguridad que utilice un servidor SQL Server comprometido que proporcione metadatos de cifrado incorrectos al cliente, lo cual podría provocar la divulgación de datos. Los métodos set* de las clases SQLServerPreparedStatement y SQLServerCallableStatement y los métodos update\* de la clase SQLServerResultSet se sobrecargan para aceptar un argumento booleano para especificar la configuración de Forzar cifrado. Si el valor de este argumento es false, el controlador no forzará el cifrado en los parámetros. Si la característica Forzar cifrado está establecida en true, el parámetro de consulta solo se envía si la columna de destino está cifrada y Always Encrypted está habilitado en la conexión o en la instrucción. El uso de esta propiedad proporciona una capa adicional de seguridad, lo que garantiza que el controlador no envía datos de manera errónea a SQL Server como texto no cifrado cuando se espera que esté cifrado.

Para más información sobre los métodos SQLServerPreparedStatement y SQLServerCallableStatement que se sobrecargan con la configuración de Forzar cifrado, consulte [Referencia de la API de Always Encrypted para el controlador JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).  

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Controlar el impacto en el rendimiento de Always Encrypted

Como Always Encrypted es una tecnología de cifrado del lado cliente, la mayoría de las sobrecargas de rendimiento se observan en el lado cliente y no en la base de datos. Además del costo de las operaciones de cifrado y descifrado, los demás orígenes de las sobrecargas de rendimiento en el lado cliente son:

- Ciclos de ida y vuelta adicionales a la base de datos para recuperar metadatos para los parámetros de consulta.
- Llamadas a un almacén de claves maestras de columna para tener acceso a una clave maestra de columna.

En esta sección se describen las optimizaciones integradas de rendimiento en Microsoft JDBC Driver for SQL Server y la forma en que puede controlar el impacto de los dos factores anteriores en el rendimiento.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Controlar los ciclos de ida y vuelta para recuperar metadatos para los parámetros de consulta

De forma predeterminada, si Always Encrypted está habilitado para una conexión, el controlador llamará a [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) para cada consulta con parámetros, al pasar la instrucción de consulta (sin ningún valor de parámetro) a SQL Server. [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) analiza la instrucción de consulta para averiguar si algún parámetro necesita cifrarse y, de ser así, devuelve la información relacionada con el cifrado que permitirá al controlador cifrar los valores de parámetro para cada uno de ellos. Este comportamiento garantiza un alto nivel de transparencia para la aplicación cliente. Siempre que la aplicación usa parámetros para pasar valores que tienen como destino columnas cifradas al controlador, la aplicación (y el desarrollador de la aplicación) no necesita saber qué consultas acceden a las columnas cifradas.

### <a name="setting-always-encrypted-at-the-query-level"></a>Configurar Always Encrypted en el nivel de consulta

Para controlar el impacto en el rendimiento a la hora de recuperar metadatos de cifrado para las consultas con parámetros, puede habilitar Always Encrypted para las consultas individuales en lugar de configurarlo para la conexión. De esta manera, puede estar seguro de que sys.sp_describe_parameter_encryption se invoca solo para las consultas que sabe que incluyen parámetros que tienen como destino las columnas cifradas. Pero tenga en cuenta que haciendo esto reduce la transparencia del cifrado: si cambia las propiedades de cifrado de las columnas de la base de datos, puede que necesite cambiar el código de la aplicación para alinearlo con los cambios de esquema.

Para controlar el comportamiento de Always Encrypted de las consultas individuales, debe configurar objetos de instrucción individuales pasando una enumeración, SQLServerStatementColumnEncryptionSetting, que especifica cómo se enviarán y recibirán los datos al leer y escribir columnas cifradas para esa instrucción específica. A continuación se exponen unas directrices prácticas:

- Si la mayoría de las consultas que una aplicación cliente envía a través de una conexión de base de datos acceden a columnas cifradas, use las instrucciones siguientes:

    - Establezca la palabra clave de la cadena de conexión **columnEncryptionSetting** en **Enabled**.
    - Establezca SQLServerStatementColumnEncryptionSetting.Disabled para consultas individuales que no tengan acceso a ninguna columna cifrada. Esta opción deshabilitará la llamada a sys.sp_describe_parameter_encryption así como un intento de descifrar cualquier valor del conjunto de resultados.
    - Establezca SQLServerStatementColumnEncryptionSetting.ResultSet para consultas individuales que no cuenten con ningún parámetro que requiera cifrado, pero que recuperen datos de columnas cifradas. Esta opción deshabilitará la llamada a sys.sp_describe_parameter_encryption y el cifrado de parámetros. La consulta podrá descifrar los resultados de las columnas de cifrado.

- Si la mayoría de las consultas que una aplicación cliente envía a través de una conexión de base de datos no acceden a columnas cifradas, use las instrucciones siguientes:

    - Establezca la palabra clave de la cadena de conexión **columnEncryptionSetting** en **Disabled**.
    - Establezca SQLServerStatementColumnEncryptionSetting.Enabled para consultas individuales que no cuenten con ningún parámetro que se tenga que cifrar. Esta opción habilitará la llamada a sys.sp_describe_parameter_encryption así como el descifrado de cualquier resultado de la consulta que se recupere de las columnas cifradas.
    - Establezca SQLServerStatementColumnEncryptionSetting.ResultSet para consultas que no cuenten con ningún parámetro que requiera cifrado, pero que recuperen datos de columnas cifradas. Esta opción deshabilitará la llamada a sys.sp_describe_parameter_encryption y el cifrado de parámetros. La consulta podrá descifrar los resultados de las columnas de cifrado.

La configuración de SQLServerStatementColumnEncryptionSetting no se puede usar para omitir el cifrado y obtener acceso a los datos de texto no cifrado. Para más información sobre cómo configurar el cifrado de columnas en una instrucción, consulte [Referencia de la API de Always Encrypted para el controlador JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).  

En el ejemplo siguiente, Always Encrypted está deshabilitado para la conexión de base de datos. La consulta que ejecuta la aplicación tiene un parámetro que tiene como destino la columna LastName que no está cifrada. La consulta recupera datos de las columnas SSN y BirthDate que están cifradas. En este caso, no es necesario llamar a sys.sp_describe_parameter_encryption para recuperar los metadatos de cifrado. Pero el descifrado de los resultados de la consulta necesita habilitarse, de forma que la aplicación pueda recibir valores de texto sin formato de las dos columnas cifradas. El parámetro SQLServerStatementColumnEncryptionSetting.ResultSet se usa para garantizar esto.

```java
// Assumes the same table definition as in Section "Retrieving and modifying data in encrypted columns"
// where only SSN and BirthDate columns are encrypted in the database.
String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<database>;user=<user>;password=<password>;" 
        + "keyStoreAuthentication=JavaKeyStorePassword;"
        + "keyStoreLocation=<keyStoreLocation>" 
        + "keyStoreSecret=<keyStoreSecret>;";

String filterRecord = "SELECT FirstName, LastName, SSN, BirthDate FROM " + tableName + " WHERE LastName = ?";

try (SQLServerConnection connection = (SQLServerConnection) DriverManager.getConnection(connectionUrl);
        PreparedStatement selectStatement = connection.prepareStatement(filterRecord, ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY,
                connection.getHoldability(), SQLServerStatementColumnEncryptionSetting.ResultSetOnly);) {

    selectStatement.setString(1, "Abel");
    ResultSet rs = selectStatement.executeQuery();
    while (rs.next()) {
        System.out.println("First name: " + rs.getString("FirstName"));
        System.out.println("Last name: " + rs.getString("LastName"));
        System.out.println("SSN: " + rs.getString("SSN"));
        System.out.println("Date of Birth: " + rs.getDate("BirthDate"));
    }
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="column-encryption-key-caching"></a>Almacenamiento en memoria caché de claves de cifrado de columnas

Para reducir el número de llamadas a un almacén de claves maestras de columna para descifrar las claves de cifrado de columnas, Microsoft JDBC Driver for SQL Server almacena en memoria caché las claves de cifrado de columnas de texto sin formato en la memoria. Después de recibir el valor cifrado de la clave de cifrado de columnas de los metadatos de la base de datos, el controlador primero intenta encontrar la clave de cifrado de columnas de texto sin formato, que corresponde al valor de clave cifrado. El controlador llama al almacén de claves que contiene la clave maestra de columna, solo si no puede encontrar el valor cifrado de la clave de cifrado de columnas en la memoria caché.

Puede configurar un valor de período de vida para las entradas de clave de cifrado de columnas en la caché mediante la API, setColumnEncryptionKeyCacheTtl(), en la clase SQLServerConnection. El valor predeterminado del período de vida de las entradas de clave de cifrado de columnas en la caché es de dos horas. Para desactivar el almacenamiento en caché, use un valor de 0. Para establecer un valor de período de vida, use la API siguiente:

```java
SQLServerConnection.setColumnEncryptionKeyCacheTtl (int columnEncryptionKeyCacheTTL, TimeUnit unit)
```

Por ejemplo, para establecer un valor de período de vida de 10 minutos, use:

```java
SQLServerConnection.setColumnEncryptionKeyCacheTtl (10, TimeUnit.MINUTES)
```

Solo se admiten los valores DAYS (días), HOURS (horas), MINUTES (minutos) o SECONDS (segundos) como unidad de tiempo.  

## <a name="copying-encrypted-data-using-sqlserverbulkcopy"></a>Copia de datos cifrados con SQLServerBulkCopy

Con SQLServerBulkCopy, puede copiar datos que ya están cifrados y almacenados en una tabla a otra, sin descifrarlos. Para ello:

- Asegúrese de que la configuración de cifrado de la tabla de destino es idéntica a la configuración de la tabla de origen. En particular, ambas tablas debe tener las mismas columnas cifradas y estas deben cifrarse mediante los mismos tipos de cifrado y las mismas claves de cifrado. Si alguna de las columnas de destino se cifra de una manera diferente a la de su columna de origen correspondiente, no podrá descifrar los datos de la tabla de destino después de la operación de copia. Los datos estarán dañados.
- Configure ambas conexiones de base de datos, a la tabla de origen y a la tabla de destino, sin tener habilitado Always Encrypted.
- Establezca la opción allowEncryptedValueModifications. Para más información, consulte [Uso de la copia masiva con el controlador JDBC](using-bulk-copy-with-the-jdbc-driver.md).

> [!NOTE]
> Tenga cuidado al especificar AllowEncryptedValueModifications ya que esta opción puede provocar daños en la base de datos dado que Microsoft JDBC Driver for SQL Server no comprueba si los datos están realmente cifrados, o si se han cifrado correctamente mediante la misma clave, algoritmo y tipo de cifrado que la columna de destino.

## <a name="see-also"></a>Consulte también

[Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
