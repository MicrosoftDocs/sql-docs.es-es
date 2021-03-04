---
title: Uso de Always Encrypted con los controladores PHP para SQL Server
description: Aprenda a usar Always Encrypted con los controladores PHP para SQL Server con el fin de proteger la información confidencial de la aplicación.
ms.date: 12/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: ''
ms.author: v-daenge
author: David-Engel
manager: v-mabarw
ms.openlocfilehash: 4c16930d75d191c83f3f6eead028bc9913556e3c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837142"
---
# <a name="using-always-encrypted-with-the-php-drivers-for-sql-server"></a>Uso de Always Encrypted con los controladores PHP para SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

## <a name="applicable-to"></a>Aplicable a
 -   Controladores de Microsoft 5.2 para PHP para SQL Server
 
## <a name="introduction"></a>Introducción

En este artículo se proporciona información sobre cómo desarrollar aplicaciones PHP mediante [Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) y los [controladores PHP para SQL Server](../../connect/php/Microsoft-php-driver-for-sql-server.md).

Always Encrypted permite a las aplicaciones cliente cifrar la información confidencial y nunca revelar los datos ni las claves de cifrado en SQL Server o Azure SQL Database. Un controlador habilitado para Always Encrypted, como ODBC Driver for SQL Server, cifra y descifra de manera transparente la información confidencial en la aplicación cliente. El controlador determina automáticamente qué parámetros de consulta corresponden a columnas de bases de datos confidenciales (protegidas mediante Always Encrypted) y cifra los valores de esos parámetros antes de pasar los datos a SQL Server o Azure SQL Database. De forma similar, el controlador descifra de manera transparente los datos que se han recuperado de las columnas de bases de datos cifradas de los resultados de la consulta. Para obtener más información, vea [Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Los controladores PHP para SQL Server usan el controlador ODBC para SQL Server para cifrar la información confidencial.

## <a name="prerequisites"></a>Prerrequisitos

 -   Configure Always Encrypted en su base de datos. Esta configuración implica el aprovisionamiento de las claves Always Encrypted y la configuración del cifrado para las columnas de las bases de datos seleccionadas. Si todavía no tiene una base de datos configurada con Always Encrypted, siga las instrucciones de [Introducción a Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). En concreto, la base de datos debe contener las definiciones de metadatos para una clave maestra de columna (CMK), una clave de cifrado de columna (CEK) y una tabla que contiene una o varias columnas cifradas mediante dicha CEK.
 -   Asegúrese de que la versión 17 o superior del controlador ODBC para SQL Server está instalada en la máquina de desarrollo. Para detalles, consulte el artículo sobre el [controlador ODBC para SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md).

## <a name="enabling-always-encrypted-in-a-php-application"></a>Habilitar Always Encrypted en una aplicación PHP

Establecer el valor de la palabra clave de cadena de conexión `ColumnEncryption` en `Enabled` es la manera más sencilla de habilitar el cifrado de parámetros que tienen como destino las columnas cifradas y el descifrado de los resultados de la consulta. Los siguientes son ejemplos de cómo habilitar Always Encrypted en los controladores SQLSRV y PDO_SQLSRV:

SQLSRV:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled");
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

Habilitar Always Encrypted no es suficiente para que el cifrado o el descifrado se realicen correctamente; también debe asegurarse de que:
 -   La aplicación tiene los permisos de base de datos VIEW ANY COLUMN MASTER KEY DEFINITION y VIEW ANY COLUMN ENCRYPTION KEY DEFINITION, que se requieren para tener acceso a los metadatos sobre las claves Always Encrypted de la base de datos. Para más información, vea [Permiso para la base de datos](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
 -   La aplicación puede acceder a la CMK que protege las CEK de las columnas cifradas consultadas. Este requisito depende del proveedor del almacén de claves que almacena la CMK. Para más información, vea [Trabajar con almacenes de claves maestras de columna](#working-with-column-master-key-stores).

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Recuperar y modificar los datos de las columnas cifradas

Una vez que habilita Always Encrypted en una conexión, puede usar las API de SQLSRV estándar (consulte [Referencia de API del controlador SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)) o las API de PDO_SQLSRV (consulte [Referencia de la API del controlador PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)) para recuperar o modificar datos en columnas de bases de datos cifradas. Suponiendo que la aplicación tiene los permisos de base de datos necesarios y puede acceder a la clave maestra de columna, el controlador cifra todos los parámetros de consulta destinados a las columnas cifradas y descifra los datos recuperados de las columnas cifradas, con un comportamiento transparente en la aplicación, como si las columnas no estuvieran cifradas.

Si Always Encrypted no está habilitado, se produce un error en las consultas con parámetros que tengan como destino las columnas cifradas. Las datos todavía se pueden recuperar de las columnas cifradas, siempre y cuando la consulta no tenga parámetros que tengan como destino las columnas cifradas. Sin embargo, el controlador no intenta descifrar nada, y la aplicación recibe los datos binarios cifrados (como matrices de bytes).

En la tabla siguiente se resume el comportamiento de las consultas, dependiendo de si Always Encrypted está habilitado o no:

|Característica de las consultas|Always Encrypted está habilitado y la aplicación puede tener acceso a las claves y a los metadatos de clave|Always Encrypted está habilitado y la aplicación no puede tener acceso a las claves ni a los metadatos de clave|Always Encrypted está deshabilitado|
|---|---|---|---|
|Parámetros que tienen como destino las columnas cifradas.|Los valores de parámetro se cifran de manera transparente.|Error|Error|
|Recuperación de datos de las columnas cifradas, sin parámetros que tengan como destino las columnas cifradas.|Los resultados de las columnas cifradas se descifran de manera transparente. La aplicación recibe valores de columna de texto no cifrado. |Error|Los resultados de las columnas cifradas no se descifran. La aplicación recibe valores cifrados como matrices de bytes.|
 
En el siguiente ejemplo se ilustra la recuperación y la modificación de datos en las columnas cifradas. En el ejemplo se supone que hay una tabla con el esquema siguiente. Las columnas SSN y BirthDate están cifradas.
```
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="data-insertion-example"></a>Ejemplo de inserción de datos

En los ejemplos siguientes se muestra cómo utilizar los controladores SQLSRV y PDO_SQLSRV para insertar una fila en la tabla Patient. Tenga en cuenta los siguientes puntos:
 -   No existe nada específico al cifrado en el código de ejemplo. El controlador detecta y cifra automáticamente los valores del SSN y los parámetros de fecha de nacimiento, cuyo destino son las columnas cifradas. Este mecanismo hace que el cifrado se realice de manera transparente en la aplicación.
 -   Los valores que se insertan en las columnas de bases de datos, incluidas las columnas cifradas, se pasan como parámetros enlazados. Aunque el uso de parámetros es opcional al enviar valores a las columnas no cifradas (aunque es altamente recomendable porque ayuda a evitar la inyección de código SQL), es necesario para los valores que tienen como destino las columnas cifradas. Si los valores insertados en las columnas SSN o BirthDate se pasaran como literales insertados en la instrucción de consulta, la consulta produciría un error porque el controlador no intenta cifrar o procesar los literales en las consultas. Como resultado, el servidor los rechazará considerándolos incompatibles con las columnas cifradas.
 -   Cuando se insertan valores mediante parámetros de enlace, se debe pasar a la base de datos un tipo SQL que sea idéntico al tipo de datos de la columna de destino o cuya conversión al tipo de datos de la columna de destino es compatible. Este requisito se debe a que Always Encrypted admite algunas conversiones de tipo. Para más detalles, consulte [Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Cada uno de los dos controladores PHP, SQLSRV y PDO_SQLSRV, tiene un mecanismo para ayudar al usuario a determinar el tipo SQL del valor. Por lo tanto, no es necesario que el usuario especifique explícitamente el tipo SQL.
  -   Para el controlador SQLSRV, el usuario tiene dos opciones:
   -   Confiar en el controlador PHP para determinar y establecer el tipo SQL correcto. En este caso, el usuario debe usar `sqlsrv_prepare` y `sqlsrv_execute` para ejecutar una consulta con parámetros.
   -   Establecer explícitamente el tipo SQL.
  -   En el caso del controlador de PDO_SQLSRV, el usuario no tiene la opción de establecer explícitamente el tipo SQL de un parámetro. El controlador PDO_SQLSRV ayuda automáticamente al usuario a determinar el tipo SQL al enlazar un parámetro.
 -   Para que los controladores determinen el tipo SQL, se aplican algunas limitaciones:
  -   Controlador SQLSRV:
   -   Si el usuario quiere que el controlador determine los tipos SQL de las columnas cifradas, el usuario debe usar `sqlsrv_prepare` y `sqlsrv_execute`.
   -   Si se prefiere `sqlsrv_query`, el usuario es responsable de especificar los tipos SQL de todos los parámetros. El tipo SQL especificado debe incluir la longitud de la cadena de los tipos de cadena y la escala y la precisión de los tipos decimales.
  -   Controlador PDO_SQLSRV:
   -   El atributo de instrucción `PDO::SQLSRV_ATTR_DIRECT_QUERY` no se admite en una consulta con parámetros.
   -   El atributo de instrucción `PDO::ATTR_EMULATE_PREPARES` no se admite en una consulta con parámetros.
   
Controlador SQLSRV y [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel;
$birthDate = "1996-10-19";
$params = array($ssn, $firstName, $lastName, $birthDate);
// during sqlsrv_prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = sqlsrv_prepare($conn, $query, $params);
sqlsrv_execute($stmt);
```

Controlador SQLSRV y [sqlsrv_query](../../connect/php/sqlsrv-query.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel";
$birthDate = "1996-10-19";
// need to provide the SQL types for ALL parameters  
// note the SQL types (including the string length) have to be the same at the column definition
$params = array(array(&$ssn, null, null, SQLSRV_SQLTYPE_CHAR(11)),
                array(&$firstName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$lastName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$birthDate, null, null, SQLSRV_SQLTYPE_DATE));
sqlsrv_query($conn, $query, $params);
```

Controlador PDO_SQLSRV y [PDO::prepare](../../connect/php/pdo-prepare.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Able";
$birthDate = "1996-10-19";
// during PDO::prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
$stmt->bindParam(2, $firstName);
$stmt->bindParam(3, $lastName);
$stmt->bindParam(4, $birthDate);
$stmt->execute();
```

### <a name="plaintext-data-retrieval-example"></a>Ejemplo de recuperación de datos de texto no cifrado

En los ejemplos siguientes se muestra el filtrado de datos en función de valores cifrados y la recuperación de datos de texto no cifrado de columnas cifradas mediante los controladores SQLSRV y PDO_SQLSRV. Tenga en cuenta los siguientes puntos:
 -   El valor que se ha usado en la cláusula WHERE para filtrar por la columna SSN necesita pasarse mediante el parámetro bind, de forma que el controlador pueda cifrarlo de manera transparente antes de enviarlo al servidor.
 -   Al ejecutar una consulta con parámetros enlazados, los controladores PHP determinan automáticamente el tipo SQL para el usuario, a menos que el usuario especifique explícitamente el tipo SQL al usar el controlador SQLSRV.
 -   Todos los valores impresos por el programa están en texto sin formato, ya que el controlador descifra los datos que se han recuperado de las columnas SSN y BirthDate de manera transparente.
 
Nota: Las consultas pueden realizar comparaciones de igualdad en las columnas cifradas solo si el cifrado es determinista. Para obtener más información, vea la sección [Selección del cifrado determinista o aleatorio](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

SQLSRV:
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = sqlsrv_prepare($conn, $query, array(&$ssn));
// during sqlsrv_execute, the driver encrypts the ssn value and passes it to the database
sqlsrv_execute($stmt);
// fetch like usual
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV:
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
// during PDOStatement::execute, the driver encrypts the ssn value and passes it to the database
$stmt->execute();
// fetch like usual
$row = $stmt->fetch();
```

### <a name="ciphertext-data-retrieval-example"></a>Ejemplo de recuperación de datos de texto cifrado

Si Always Encrypted no está habilitado, una consulta todavía puede recuperar datos de las columnas cifradas, siempre y cuando la consulta no tenga parámetros que tengan como destino las columnas cifradas.

En los ejemplos siguientes se muestra cómo recuperar datos cifrados binarios de columnas cifradas mediante los controladores SQLSRV y PDO_SQLSRV. Tenga en cuenta los siguientes puntos:
 -   Como Always Encrypted no está habilitado en la cadena de conexión, la consulta devuelve valores cifrados de SSN y BirthDate como matrices de bytes (el programa convierte los valores en cadenas).
 -   Una consulta que recupera datos de columnas cifradas con Always Encrypted deshabilitado puede tener parámetros, siempre y cuando ninguno de ellos tenga como destino una columna cifrada. La siguiente consulta filtra por LastName, que no está cifrado en la base de datos. Si la consulta filtra por SSN o BirthDate, esta producirá un error.
 
SQLSRV:
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = sqlsrv_prepare($conn, $query, array(&$lastName));
sqlsrv_execute($stmt);
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV:
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $lastName);
$stmt->execute();
$row = $stmt->fetch();
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Evitar problemas comunes al consultar columnas cifradas

En esta sección se describen las categorías comunes de errores al consultar columnas cifradas de aplicaciones PHP y algunas instrucciones sobre cómo evitarlos.

#### <a name="unsupported-data-type-conversion-errors"></a>Errores de conversión de tipos de datos no compatibles

Always Encrypted admite algunas conversiones para los tipos de datos cifrados. Vea [Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) para obtener una lista detallada de las conversiones de tipos compatibles. Para evitar los errores de conversión de tipo de datos, haga lo siguiente:
 -   Al usar el controlador SQLSRV con `sqlsrv_prepare` y `sqlsrv_execute`, se determina automáticamente el tipo SQL, junto con el tamaño de la columna y el número de dígitos decimales del parámetro.
 -   Al usar el controlador PDO_SQLSRV para ejecutar una consulta, también se determina automáticamente el tipo SQL con el tamaño de la columna y el número de dígitos decimales del parámetro.
 -   Al usar el controlador SQLSRV con `sqlsrv_query` para ejecutar una consulta:
  -   El tipo SQL del parámetro es exactamente el mismo que el tipo de la columna de destino, o se admite la conversión del tipo SQL al tipo de la columna.
  -   La precisión y la escala de los parámetros que tienen como destino columnas de los tipos de datos `decimal` y `numeric` de SQL Server debe ser la misma que la precisión y la escala configurada para la columna de destino.
  -   La precisión de los parámetros que tienen como destino las columnas de tipos de datos `datetime2`, `datetimeoffset` o `time` de SQL Server no debe ser superior a la precisión de la columna de destino, en las consultas que modifiquen la columna de destino.
 -   No use los atributos `PDO::SQLSRV_ATTR_DIRECT_QUERY` ni `PDO::ATTR_EMULATE_PREPARES` de la instrucción PDO_SQLSRV en una consulta con parámetros.
 
#### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Errores debidos a pasar texto sin formato en lugar de valores cifrados

Cualquier valor que tenga como destino una columna cifrada necesita cifrarse antes de enviarse al servidor. Un intento para insertar, modificar o filtrar mediante un valor de texto sin formato en una columna cifrada produce un error. Para evitar dichos errores, asegúrese de que:
 -   Always Encrypted está habilitado (en la cadena de conexión, establezca la palabra clave `ColumnEncryption` en `Enabled`).
 -   Usa un parámetro de enlace para enviar datos que tengan como destino las columnas cifradas. En el ejemplo siguiente se muestra una consulta que filtra de manera incorrecta mediante un literal o una constante en una columna cifrada (SSN):
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

## <a name="controlling-performance-impact-of-always-encrypted"></a>Controlar el impacto en el rendimiento de Always Encrypted

Como Always Encrypted es una tecnología de cifrado del lado cliente, la mayoría de las sobrecargas de rendimiento se observan en el lado cliente y no en la base de datos. Además del costo de las operaciones de cifrado y descifrado, los demás orígenes de las sobrecargas de rendimiento en el lado cliente son:
 -   Ciclos de ida y vuelta adicionales a la base de datos para recuperar metadatos para los parámetros de consulta.
 -   Llamadas a un almacén de claves maestras de columna para tener acceso a una clave maestra de columna.
 
### <a name="round-trips-to-retrieve-metadata-for-query-parameters"></a>Ciclos de ida y vuelta para recuperar metadatos para los parámetros de consulta

De forma predeterminada, si Always Encrypted está habilitado para una conexión, ODBC Driver llamará a [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) para cada consulta con parámetros, al pasar la instrucción de consulta (sin ningún valor de parámetro) a SQL Server. Este procedimiento almacenado analiza la instrucción de consulta para averiguar si algún parámetro necesita cifrarse y, de ser así, devuelve la información relacionada con el cifrado de cada parámetro que permitirá al controlador realizar el cifrado.

Dado que los controladores PHP permiten que el usuario enlace un parámetro en una instrucción preparada sin proporcionar el tipo SQL, al enlazar un parámetro en una conexión habilitada para Always Encrypted, los controladores PHP llaman a [SQLDescribeParam](../../odbc/reference/syntax/sqldescribeparam-function.md) en el parámetro para obtener el tipo SQL, el tamaño de la columna y los dígitos decimales. Los metadatos se utilizan para llamar a [SQLBindParameter]( ../../odbc/reference/syntax/sqlbindparameter-function.md). Estas llamadas `SQLDescribeParam` adicionales no requieren más viajes de ida y vuelta a la base de datos, ya que el controlador ODBC ya almacenó la información en el lado cliente cuando se llamó a `sys.sp_describe_parameter_encryption`.

Los comportamientos anteriores garantizan un alto nivel de transparencia de la aplicación cliente: la aplicación y el desarrollador de la aplicación no necesitan conocer qué consultas tienen acceso a las columnas cifradas, siempre y cuando los valores que tienen como destino estas columnas se pasen al controlador en parámetros.

A diferencia del controlador ODBC para SQL Server, la habilitación de Always Encrypted en el nivel de instrucción/consulta todavía no se admite en los controladores PHP. 

### <a name="column-encryption-key-caching"></a>Almacenamiento en memoria caché de claves de cifrado de columnas

Para reducir el número de llamadas a un almacén de claves maestras de columna para descifrar las claves de cifrado de columnas (CEK), el controlador almacena en memoria caché las claves de cifrado de columnas de texto no cifrado en la memoria. Después de recibir la CEK cifrada (ECEK) de los metadatos de la base de datos, el controlador primero intenta encontrar la CEK de texto no cifrado correspondiente para el valor de la clave cifrada en la caché. El controlador llama al almacén de claves que contiene la CMK solo si no puede encontrar la CEK de texto no cifrado correspondiente en la caché.

Nota: En el controlador ODBC para SQL Server, las entradas de la memoria caché se desalojan después de un tiempo de expiración de dos horas. Este comportamiento significa que, para una ECEK específica, el controlador se pone en contacto con el almacén de claves solo una vez durante la vigencia de la aplicación o cada dos horas, lo que menos tiempo tarde.

## <a name="working-with-column-master-key-stores"></a>Trabajar con almacenes de claves maestras de columna

Para cifrar o descifrar los datos, el controlador necesita obtener una CEK que esté configurada para la columna de destino. Las CEK se almacenan con formato cifrado (ECEK) en los metadatos de la base de datos. Cada CEK tiene una CMK correspondiente que se usó para cifrarla. Los [metadatos de la base de datos](../../t-sql/statements/create-column-master-key-transact-sql.md) no almacenan la propia CMK; solo contiene el nombre el almacén de claves e información que el almacén de claves puede usar para buscar la CMK.

Para obtener el valor de texto no cifrado de una ECEK, el controlador primero obtiene los metadatos sobre la CEK y su CMK correspondiente y, después, usa esta información para ponerse en contacto con el almacén de claves que contiene la CMK y le pide que descifre la ECEK. El controlador se comunica con un almacén de claves mediante un proveedor de almacén de claves.

Para el controlador 5.3.0 de Microsoft de PHP para SQL Server, solo se admiten el proveedor del Almacén de certificados de Windows y Azure Key Vault. El otro proveedor de almacén de claves compatible con el controlador ODBC (proveedor de almacén de claves personalizado) todavía no es compatible.

### <a name="using-the-windows-certificate-store-provider"></a>Uso del proveedor para el Almacén de certificados de Windows

El controlador ODBC para SQL Server en Windows incluye un proveedor de almacén de claves maestras de columna integrado para el almacén de certificados de Windows, denominado `MSSQL_CERTIFICATE_STORE`. (Este proveedor no está disponible en macOS o Linux). Con este proveedor, la CMK se almacena localmente en el equipo cliente y la aplicación no requiere ninguna configuración adicional para usarla con el controlador. Sin embargo, la aplicación debe tener acceso al certificado y a su clave privada en el almacén. Para obtener más información, vea [Create and Store Column Master Keys (Always Encrypted) (Crear y almacenar claves maestras de columna (Always Encrypted))](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-azure-key-vault"></a>Uso de Azure Key Vault

Azure Key Vault ofrece una manera de almacenar claves de cifrado, contraseñas y otros secretos mediante Azure y se puede usar para almacenar claves para Always Encrypted. El controlador ODBC para SQL Server (versión 17 y versiones posteriores) incluye un proveedor de almacén de claves maestras integrado para Azure Key Vault. Las opciones de conexión siguientes controlan la configuración de Azure Key Vault: `KeyStoreAuthentication`, `KeyStorePrincipalId` y `KeyStoreSecret`. 
 -   `KeyStoreAuthentication` puede tomar uno de los dos valores de cadena posibles: `KeyVaultPassword` y `KeyVaultClientSecret`. Estos valores controlan el tipo de credenciales de autenticación que se usa con las otras dos palabras clave.
 -   `KeyStorePrincipalId` toma una cadena que representa un identificador de la cuenta que busca el acceso a Azure Key Vault. 
     -   Si `KeyStoreAuthentication` está establecido en `KeyVaultPassword`, `KeyStorePrincipalId` debe ser el nombre de un usuario de Azure Active Directory.
     -   Si `KeyStoreAuthentication` se establece en `KeyVaultClientSecret`, `KeyStorePrincipalId` debe ser un identificador de cliente de la aplicación.
 -   `KeyStoreSecret` toma una cadena que representa un secreto de credencial. 
     -   Si `KeyStoreAuthentication` se establece en `KeyVaultPassword`, `KeyStoreSecret` debe ser la contraseña del usuario. 
     -   Si `KeyStoreAuthentication` se establece en `KeyVaultClientSecret`, `KeyStoreSecret` debe ser el secreto de aplicación asociado al identificador de cliente de la aplicación.

Las tres opciones deben estar presentes en la cadena de conexión para utilizar Azure Key Vault. Además, `ColumnEncryption` debe establecerse en `Enabled`. Si `ColumnEncryption` está establecido en `Disabled` pero las opciones de Azure Key Vault están presentes, el script continuará sin errores, pero no se realizará ningún cifrado.

En los ejemplos siguientes se muestra cómo conectarse a SQL Server mediante Azure Key Vault.

SQLSRV:

Al usar una cuenta de Azure Active Directory:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultPassword", "KeyStorePrincipalId"=>$AADUsername, "KeyStoreSecret"=>$AADPassword);
$conn = sqlsrv_connect($server, $connectionInfo);
```
Al usar un secreto y un identificador de cliente de aplicación de Azure:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultClientSecret", "KeyStorePrincipalId"=>$applicationClientID, "KeyStoreSecret"=>$applicationClientSecret);
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV: al usar una cuenta de Azure Active Directory:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultPassword; KeyStorePrincipalId = $AADUsername; KeyStoreSecret = $AADPassword;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```
Al usar un secreto y un identificador de cliente de aplicación de Azure:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultClientSecret; KeyStorePrincipalId = $applicationClientID; KeyStoreSecret = $applicationClientSecret;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

## <a name="limitations-of-the-php-drivers-when-using-always-encrypted"></a>Limitaciones de los controladores PHP al usar Always Encrypted

SQLSRV y PDO_SQLSRV:
 -   Linux/macOS no admiten el proveedor del Almacén de certificados de Windows
 -   Forzar el cifrado de parámetros
 -   Al habilitar Always Encrypted en el nivel de instrucción 
 -   Al usar la característica Always Encrypted y las configuraciones regionales no UTF8 en Linux y macOS (como "en_US.ISO-8859-1"), es posible que la inserción de datos NULL o una cadena vacía en una columna char(n) cifrada no funcione, a menos que se haya instalado la página de códigos 1252 en el sistema
 
Solo SQLSRV:
 -   Al usar `sqlsrv_query` para el parámetro de enlace sin especificar el tipo SQL
 -   Al usar `sqlsrv_prepare` para enlazar parámetros en un lote de instrucciones SQL  
 
PDO_SQLSRV:
 -   El atributo de instrucción `PDO::SQLSRV_ATTR_DIRECT_QUERY` especificado en una consulta con parámetros
 -   El atributo de instrucción `PDO::ATTR_EMULATE_PREPARE` especificado en una consulta con parámetros
 -   Al enlazar parámetros en un lote de instrucciones SQL
 
Los controladores PHP también heredan las limitaciones que impone el controlador ODBC para SQL Server y la base de datos. Consulte [Limitaciones del controlador ODBC al usar Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md) y [Detalles de las características de Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#feature-details).  
  
## <a name="see-also"></a>Consulte también  
[Guía de programación para el controlador SQL para PHP](../../connect/php/programming-guide-for-php-sql-driver.md)

[Referencia de API del controlador SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Referencia de la API del controlador PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)  
  
