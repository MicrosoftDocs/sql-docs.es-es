---
title: Uso de Always Encrypted con el controlador ODBC
description: Obtenga información sobre cómo desarrollar aplicaciones ODBC con Always Encrypted y Microsoft ODBC Driver for SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 02e306b8-9dde-4846-8d64-c528e2ffe479
ms.reviewer: v-daenge
ms.author: v-chojas
author: v-chojas
ms.openlocfilehash: 7da6c0d9864f514d757d0872fd32ce1e0219f01d
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464783"
---
# <a name="using-always-encrypted-with-the-odbc-driver-for-sql-server"></a>Uso de Always Encrypted con ODBC Driver for SQL Server

[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

### <a name="applicable-to"></a>Aplicable a

- Controlador ODBC 13.1 para SQL Server
- Controlador ODBC 17 para SQL Server

### <a name="introduction"></a>Introducción

En este artículo se proporciona información sobre cómo desarrollar aplicaciones ODBC mediante [Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) o [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md) y el [controlador ODBC para SQL Server](microsoft-odbc-driver-for-sql-server.md).

Always Encrypted permite a las aplicaciones cliente cifrar la información confidencial y nunca revelar los datos ni las claves de cifrado en SQL Server o Azure SQL Database. Un controlador habilitado para Always Encrypted, como ODBC Driver for SQL Server, consigue esta seguridad al cifrar y descifrar de manera transparente la información confidencial en la aplicación cliente. El controlador determina automáticamente qué parámetros de consulta corresponden a columnas de bases de datos confidenciales (protegidas mediante Always Encrypted) y cifra los valores de esos parámetros antes de pasar los datos a SQL Server o Azure SQL Database. De forma similar, el controlador descifra de manera transparente los datos que se han recuperado de las columnas de bases de datos cifradas de los resultados de la consulta. Always Encrypted *con enclaves seguros* amplía esta característica para ofrecer una funcionalidad más completa en los datos confidenciales mientras mantiene la confidencialidad de estos.

Para más información, consulte [Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) y [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

### <a name="prerequisites"></a>Prerrequisitos

Configure Always Encrypted en su base de datos. Este proceso implica el aprovisionamiento de las claves Always Encrypted y la configuración del cifrado para las columnas de las bases de datos seleccionadas. Si todavía no tiene una base de datos configurada con Always Encrypted, siga las instrucciones de [Introducción a Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). En concreto, la base de datos debe contener las definiciones de metadatos para una clave maestra de columna (CMK), una clave de cifrado de columna (CEK) y una tabla que contiene una o varias columnas cifradas mediante dicha CEK.

Si usa Always Encrypted con enclaves seguros, vea [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md) para obtener los requisitos previos adicionales.

### <a name="enabling-always-encrypted-in-an-odbc-application"></a>Habilitar Always Encrypted en una aplicación ODBC

La forma más fácil de habilitar el cifrado de parámetros y el descifrado de la columna cifrada del conjunto de resultados consiste en establecer el valor de la palabra clave de la cadena de conexión `ColumnEncryption` en **Habilitado**. A continuación se muestra un ejemplo de una cadena de conexión que habilita Always Encrypted:

```cpp
SQLWCHAR *connString = L"Driver={ODBC Driver 17 for SQL Server};Server={myServer};Trusted_Connection=yes;ColumnEncryption=Enabled;";
```

Always Encrypted también puede habilitarse en la configuración de DSN, con la misma clave y el mismo valor (que se sobrescribirán con la configuración de la cadena de conexión, si existe), o mediante programación con el atributo anterior a la conexión `SQL_COPT_SS_COLUMN_ENCRYPTION`. Con una configuración de este tipo, se sobrescribe el valor definido en la cadena de conexión o en DSN:

```cpp
 SQLSetConnectAttr(hdbc, SQL_COPT_SS_COLUMN_ENCRYPTION, (SQLPOINTER)SQL_COLUMN_ENCRYPTION_ENABLE, 0);
```

Una vez habilitado para la conexión, el comportamiento de Always Encrypted puede ajustarse para consultas individuales. Para más información, consulte [Controlar el impacto en el rendimiento de Always Encrypted](#controlling-the-performance-impact-of-always-encrypted).

Habilitar Always Encrypted no es suficiente para que el cifrado o el descifrado se realicen correctamente; también debe asegurarse de que:

- La aplicación tiene los permisos de base de datos *VIEW ANY COLUMN MASTER KEY DEFINITION* y *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* , que se requieren para tener acceso a los metadatos sobre las claves Always Encrypted de la base de datos. Para más información, vea [Permisos para la base de datos](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).

- La aplicación puede acceder a la CMK que protege las CEK de las columnas cifradas consultadas. Este comportamiento depende del proveedor del almacén de claves que almacena la CMK. Para más información, vea [Trabajar con almacenes de claves maestras de columna](#working-with-column-master-key-stores).

### <a name="enabling-always-encrypted-with-secure-enclaves"></a>Habilitación de Always Encrypted con enclaves seguros

> [!NOTE]
> En Linux y macOS, se requiere la versión 1.0.1 o posterior de OpenSSL para usar Always Encrypted con enclaves seguros.

A partir de la versión 17.4, el controlador admite Always Encrypted con enclaves seguros. Para habilitar el uso del enclave al conectarse a una base de datos, establezca la clave DSN `ColumnEncryption`, la palabra clave de cadena de conexión o el atributo de conexión en el valor `<attestation protocol>\<attestation URL>`, donde:

- `<attestation protocol>`: especifica un protocolo utilizado para la atestación de enclave.
  - Si utiliza [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] y el Servicio de protección de host (HGS), `<attestation protocol>` debe ser `VBS-HGS`.
  - Si utiliza [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y Microsoft Azure Attestation, `<attestation protocol>` debe ser `SGX-AAS`.

- `<attestation URL>`: especifica una dirección URL de atestación (un punto de conexión de servicio de atestación). Debe obtener una dirección URL de atestación para su entorno por parte del administrador del servicio de atestación.

  - Si usa [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] y el Servicio de protección de host (HGS), vea [Determinación y uso compartido de la dirección URL de atestación de HGS](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Si usa [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y Microsoft Azure Attestation, vea [Determinación de la dirección URL de atestación de la directiva de atestación](../../relational-databases/security/encryption/always-encrypted-enclaves.md#secure-enclave-attestation).

Ejemplos de cadenas de conexión que permiten cálculos de enclave para una conexión de base de datos:

- [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)]:
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.myDomain;Database=myDataBase;Trusted_Connection=Yes;ColumnEncryption=VBS-HGS,http://myHGSServer.myDomain/Attestation
   ```

- [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]:
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.database.windows.net;Database=myDataBase;Uid=myUsername;Pwd=myPassword;Encrypt=yes;ColumnEncryption=SGX-AAS,https://myAttestationProvider.uks.attest.azure.net/attest/SgxEnclave
   ```

Si el servidor y el servicio de atestación están configurados correctamente, así como CMK y CEK habilitados para enclaves para las columnas deseadas, ahora debería poder ejecutar consultas que utilizan el enclave, como el cifrado en contexto y cálculos enriquecidos, además de la funcionalidad existente proporcionada por Always Encrypted. Para más información, consulte [Configuración y uso de Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/configure-always-encrypted-enclaves.md).

### <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Recuperar y modificar los datos de las columnas cifradas

Una vez que habilite Always Encrypted en una conexión, puede usar las API de ODBC estándar. Las API de ODBC pueden recuperar o modificar datos en columnas de bases de datos cifradas. Los elementos de documentación siguientes pueden resultar de ayuda:

- [Código de ejemplo de ODBC](cpp-code-example-app-connect-access-sql-db.md)
- [Referencia del programador de ODBC](../../odbc/reference/odbc-programmer-s-reference.md)

La aplicación debe tener los permisos de base de datos necesarios y debe poder acceder a la clave maestra de columna. A continuación, el controlador cifra los parámetros de consulta que tienen como destino las columnas cifradas. El controlador también descifra los datos recuperados de las columnas cifradas. El controlador realiza todo este cifrado y descifrado sin ayuda del código fuente. Para el programa, es como si las columnas no estuvieran cifradas.

Si Always Encrypted no está habilitado, se producirá un error en las consultas con parámetros que tengan como destino las columnas cifradas. Las datos todavía se pueden recuperar de las columnas cifradas, siempre y cuando la consulta no tenga parámetros que tengan como destino las columnas cifradas. Sin embargo, el controlador no intentará descifrar nada, y la aplicación recibirá los datos binarios cifrados (como matrices de bytes).

En la tabla siguiente se resume el comportamiento de las consultas, dependiendo de si Always Encrypted está habilitado o no:

|Característica de las consultas | Always Encrypted está habilitado y la aplicación puede tener acceso a las claves y a los metadatos de clave|Always Encrypted está habilitado y la aplicación no puede tener acceso a las claves ni a los metadatos de clave | Always Encrypted está deshabilitado|
|:---|:---|:---|:---|
| Parámetros que tienen como destino las columnas cifradas. | Los valores de parámetro se cifran de manera transparente. | Error | Error|
| Recuperación de datos de las columnas cifradas, sin parámetros que tengan como destino las columnas cifradas.| Los resultados de las columnas cifradas se descifran de manera transparente. La aplicación recibe valores de columna de texto no cifrado. | Error | Los resultados de las columnas cifradas no se descifran. La aplicación recibe valores cifrados como matrices de bytes.

En el siguiente ejemplo se ilustra la recuperación y la modificación de datos en las columnas cifradas. En el ejemplo se supone que hay una tabla con el esquema siguiente. Las columnas SSN y BirthDate están cifradas.

```tsql
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
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] )
 GO
```

#### <a name="data-insertion-example"></a>Ejemplo de inserción de datos

En este ejemplo se inserta una fila en la tabla Patients. Tenga en cuenta los siguientes detalles:

- No existe nada específico al cifrado en el código de ejemplo. El controlador detecta y cifra automáticamente los valores del SSN y los parámetros de fecha, cuyo destino son las columnas cifradas. Este comportamiento hace que el cifrado se realice de manera transparente en la aplicación.

- Los valores que se insertan en las columnas de bases de datos, incluidas las columnas cifradas, se pasan como parámetros enlazados (consulte [Función SQLBindParameter](../../odbc/reference/syntax/sqlbindparameter-function.md)). Aunque el uso de parámetros es opcional al enviar valores a las columnas no cifradas (aunque es altamente recomendable porque ayuda a evitar la inyección de código SQL), es necesario para los valores que tienen como destino las columnas cifradas. Si los valores insertados en las columnas SSN o BirthDate se pasaran como literales insertados en la instrucción de consulta, la consulta produciría un error porque el controlador no intenta cifrar o procesar los literales en las consultas. Como resultado, el servidor los rechazará considerándolos incompatibles con las columnas cifradas.

- El tipo SQL del parámetro insertado en la columna SSN se establece en SQL_CHAR, que se asigna al tipo de datos de SQL Server **char** (`rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);`). Si el tipo del parámetro se ha establecido en SQL_WCHAR, que se asigna a **nchar**, la consulta produciría un error, ya que Always Encrypted no admite conversiones del lado servidor de valores nchar cifrados a valores char cifrados. Consulte el [apéndice D sobre tipos de datos de la referencia para programadores de ODBC](../../odbc/reference/appendixes/appendix-d-data-types.md) para información sobre las asignaciones de tipos de datos.

```cpp
    SQL_DATE_STRUCT date;
    SQLLEN cbdate;   // size of date structure  

    SQLCHAR SSN[12];
    strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

    SQLWCHAR* firstName = L"Catherine";
    SQLWCHAR* lastName = L"Abel";
    SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

    // Initialize the date structure  
    date.day = 10;
    date.month = 9;
    date.year = 1996;

    // Size of structures   
    cbdate = sizeof(SQL_DATE_STRUCT);

    SQLRETURN rc = 0;

    string queryText = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?) ";

    rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

    //SSN
    rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);
    //FirstName
    rc = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)firstName, 0, &cbFirstName);
    //LastName
    rc = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);
    //BirthDate
    rc = SQLBindParameter(hstmt, 4, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TYPE_DATE, 10, 0, (SQLPOINTER)&date, 0, &cbdate);

    rc = SQLExecute(hstmt);
```

#### <a name="plaintext-data-retrieval-example"></a>Ejemplo de recuperación de datos de texto no cifrado

En el siguiente ejemplo se muestra el filtrado de datos basándose en valores cifrados, así como la recuperación de datos de texto sin formato de las columnas cifradas. Tenga en cuenta los siguientes detalles:

- El valor que se ha usado en la cláusula WHERE para filtrar por la columna SSN necesita pasarse con SQLBindParameter, de forma que el controlador pueda cifrarlo de manera transparente antes de enviarlo al servidor.

- Todos los valores impresos por el programa estarán en texto sin formato, ya que el controlador descifrará los datos que se han recuperado de las columnas SSN y BirthDate de manera transparente.

> [!NOTE]
> Las consultas pueden realizar comparaciones de igualdad en las columnas cifradas solo si el cifrado es determinista o si el enclave seguro está habilitado. Para obtener más información, vea la sección [Selección del cifrado determinista o aleatorio](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```cpp
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[SSN] = ? ";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//SSN
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="ciphertext-data-retrieval-example"></a>Ejemplo de recuperación de datos de texto cifrado

Si Always Encrypted no está habilitado, una consulta todavía puede recuperar datos de las columnas cifradas, siempre y cuando la consulta no tenga parámetros que tengan como destino las columnas cifradas.

En el siguiente ejemplo se ilustra la recuperación de datos binarios cifrados de las columnas cifradas. Tenga en cuenta los siguientes detalles:

- Como Always Encrypted no está habilitado en la cadena de conexión, la consulta devolverá valores cifrados de SSN y BirthDate como matrices de bytes (el programa convierte los valores en cadenas).
- Una consulta que recupera datos de columnas cifradas con Always Encrypted deshabilitado puede tener parámetros, siempre y cuando ninguno de ellos tenga como destino una columna cifrada. La consulta anterior filtra por LastName, que no está cifrado en la base de datos. Si la consulta ha filtrado por SSN o BirthDate, esta producirá un error.

```cpp
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[LastName] = ?";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//LastName
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="moneysmallmoney-encryption"></a>Cifrado de tipos de datos money y smallmoney

A partir de la versión 17.7 del controlador, es posible usar Always Encrypted con los tipos de datos money y smallmoney. Para ello, son necesarios algunos pasos adicionales.
Al insertar en una columna cifrada de tipos de datos money o smallmoney, use uno de los siguientes tipos de C:

```cpp
SQL_C_CHAR
SQL_C_WCHAR
SQL_C_SHORT
SQL_C_LONG
SQL_C_FLOAT
SQL_C_DOUBLE
SQL_C_BIT
SQL_C_TINYINT
SQL_C_SBIGINT
SQL_C_NUMERIC
```

Use también un tipo SQL `SQL_NUMERIC` o `SQL_DOUBLE` (es posible que se pierda precisión al usar este tipo).

##### <a name="binding-the-variable"></a>Enlace de la variable

Cada vez que se enlaza una variable de tipos de datos money o smallmoney en una columna cifrada, se deben establecer los siguientes campos descriptores:

```cpp
// n is the descriptor record of the MONEY/SMALLMONEY parameter
// the type is assumed to be SMALLMONEY if isSmallMoney is true and MONEY otherwise

SQLHANDLE ipd = 0;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, (SQLPOINTER)&ipd, SQL_IS_POINTER, NULL);
SQLSetDescField(ipd, n, SQL_CA_SS_SERVER_TYPE, isSmallMoney ? (SQLPOINTER)SQL_SS_TYPE_SMALLMONEY :
                                                              (SQLPOINTER)SQL_SS_TYPE_MONEY, SQL_IS_INTEGER);

// If the variable is bound as SQL_NUMERIC, additional descriptor fields have to be set
// var is SQL_NUMERIC_STRUCT containing the value to be inserted

SQLHDESC   hdesc = NULL;
SQLGetStmtAttr(hStmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);
SQLSetDescField(hdesc, n, SQL_DESC_PRECISION, (SQLPOINTER)(var.precision), 0);
SQLSetDescField(hdesc, n, SQL_DESC_SCALE, (SQLPOINTER)(var.scale), 0);
SQLSetDescField(hdesc, n, SQL_DESC_DATA_PTR, &var, 0);
```

#### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Evitar problemas comunes al consultar columnas cifradas

En esta sección se describen las categorías comunes de errores al consultar columnas cifradas de aplicaciones ODBC y algunas instrucciones sobre cómo evitarlos.

##### <a name="unsupported-data-type-conversion-errors"></a>Errores de conversión de tipos de datos no compatibles

Always Encrypted admite algunas conversiones para los tipos de datos cifrados. Vea [Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) para obtener una lista detallada de las conversiones de tipos compatibles. Para evitar errores de conversión del tipo de datos, asegúrese de observar los siguientes puntos cuando se usa SQLBindParameter con parámetros que tienen como destino columnas cifradas:

- El tipo SQL del parámetro es exactamente el mismo que el tipo de la columna de destino, o se admite la conversión del tipo SQL al tipo de la columna.

- La precisión y la escala de los parámetros que tienen como destino columnas de los tipos de datos `decimal` y `numeric` de SQL Server debe ser la misma que la precisión y la escala configurada para la columna de destino.

- La precisión de los parámetros que tienen como destino las columnas de tipos de datos `datetime2`, `datetimeoffset` o `time` de SQL Server no debe ser superior a la precisión de la columna de destino, en las consultas que modifiquen la columna de destino.  

##### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Errores debidos a pasar texto sin formato en lugar de valores cifrados

Cualquier valor que tenga como destino una columna cifrada necesita cifrarse antes de enviarse al servidor. Un intento para insertar, modificar o filtrar mediante un valor de texto sin formato en una columna cifrada producirá un error. Para evitar dichos errores, asegúrese de que:

- Always Encrypted está habilitado (en el DSN, la cadena de conexión, antes de establecer la conexión mediante la configuración del atributo de conexión `SQL_COPT_SS_COLUMN_ENCRYPTION` para una conexión específica o el atributo de la instrucción `SQL_SOPT_SS_COLUMN_ENCRYPTION` para una instrucción concreta).

- Usa SQLBindParameter para enviar datos que tengan como destino las columnas cifradas. En el ejemplo siguiente se muestra una consulta que filtra de manera incorrecta mediante un literal o constante por una columna cifrada (SSN), en lugar de pasar el literal como un argumento a SQLBindParameter.

```cpp
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

### <a name="precautions-when-using-sqlsetpos-and-sqlmoreresults"></a>Precauciones al usar SQLSetPos y SQLMoreResults

#### <a name="sqlsetpos"></a>SQLSetPos

La API `SQLSetPos` permite que una aplicación actualice las filas de un conjunto de resultados con los búferes que se enlazaron con SQLBindCol y en los que se capturaron previamente los datos de las filas. Debido al comportamiento de relleno asimétrico de los tipos de longitud fija cifrados, es posible alterar de forma inesperada los datos de estas columnas mientras se realizan las actualizaciones de otras columnas de la fila. Con AE, los valores de caracteres longitud fija se rellenarán si el valor es menor que el tamaño de búfer.

Para mitigar este comportamiento, use la marca `SQL_COLUMN_IGNORE` para ignorar las columnas que no se actualizarán como parte de `SQLBulkOperations` y al usar `SQLSetPos` para las actualizaciones basadas en el cursor.  Todas las columnas no modificadas directamente por la aplicación deben ignorarse, tanto por el rendimiento como para evitar el truncamiento de las columnas enlazadas a un búfer *más pequeño* con respecto a su tamaño real (DB). Para más información, vea la [referencia sobre las funciones SQLSetPos](../../odbc/reference/syntax/sqlsetpos-function.md).

#### <a name="sqlmoreresults--sqldescribecol"></a>SQLMoreResults & SQLDescribeCol

Los programas de la aplicación pueden llamar a [SQLDescribeCol](../../odbc/reference/syntax/sqldescribecol-function.md) para devolver los metadatos sobre las columnas en instrucciones preparadas.  Al habilitar Always Encrypted, llamar a `SQLMoreResults` *antes* que a `SQLDescribeCol` da lugar a que se llame a [sp_describe_first_result_set](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md), que no devuelve correctamente los metadatos de texto no cifrado para las columnas cifradas. Para evitar este problema, llame a `SQLDescribeCol` en instrucciones preparadas *antes* de llamar a `SQLMoreResults`.

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Controlar el impacto en el rendimiento de Always Encrypted

Como Always Encrypted es una tecnología de cifrado del lado cliente, la mayoría de las sobrecargas de rendimiento se observan en el lado cliente y no en la base de datos. Además del costo de las operaciones de cifrado y descifrado, los demás orígenes de las sobrecargas de rendimiento en el lado cliente son:

- Ciclos de ida y vuelta adicionales a la base de datos para recuperar metadatos para los parámetros de consulta.

- Llamadas a un almacén de claves maestras de columna para tener acceso a una clave maestra de columna.

En esta sección se describen las optimizaciones integradas de rendimiento en ODBC Driver for SQL Server y la forma en que puede controlar el impacto de los dos factores anteriores en el rendimiento.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Controlar los ciclos de ida y vuelta para recuperar metadatos para los parámetros de consulta

De forma predeterminada, si Always Encrypted está habilitado para una conexión, el controlador llamará a [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) para cada consulta con parámetros, al pasar la instrucción de consulta (sin ningún valor de parámetro) a SQL Server. Este procedimiento almacenado analiza la instrucción de consulta para averiguar si algún parámetro necesita cifrarse y, de ser así, devuelve la información relacionada con el cifrado de cada parámetro que permitirá al controlador realizar el cifrado. El comportamiento anterior garantiza un alto nivel de transparencia de la aplicación cliente: la aplicación y el desarrollador de la aplicación no necesitan conocer qué consultas tienen acceso a las columnas cifradas, siempre y cuando los valores que tienen como destino estas columnas se pasen al controlador en parámetros.

A partir de la versión 17.6, el controlador también almacena en caché los metadatos de cifrado para las instrucciones preparadas, lo cual mejora el rendimiento al permitir que las llamadas posteriores a `SQLExecute` no requieran un recorrido de ida y vuelta adicional para recuperar los metadatos de cifrado.

### <a name="per-statement-always-encrypted-behavior"></a>Comportamiento de Always Encrypted por cada instrucción

Para controlar el impacto en el rendimiento a la hora de recuperar metadatos de cifrado para las consultas con parámetros, puede alterar el comportamiento de Always Encrypted para las consultas individuales si se ha habilitado en la conexión. De esta manera, puede estar seguro de que `sys.sp_describe_parameter_encryption` se invoca solo para las consultas que sabe que incluyen parámetros que tienen como destino las columnas cifradas. Pero tenga en cuenta que haciendo esto reduce la transparencia del cifrado: si cifra columnas adicionales en la base de datos, puede que necesite cambiar el código de la aplicación para alinearlo con los cambios de esquema.

Para controlar el comportamiento de Always Encrypted en una instrucción, lame a SQLSetStmtAttr para establecer el atributo de la instrucción `SQL_SOPT_SS_COLUMN_ENCRYPTION` en uno de los siguientes valores:

|Value|Descripción|
|-|-|
|`SQL_CE_DISABLED` (0)|Always Encrypted está deshabilitado para la instrucción|
|`SQL_CE_RESULTSETONLY` (1)|Solo descifrado. Los conjuntos de resultados y los valores devueltos están descifrados, y los parámetros no se cifran.|
|`SQL_CE_ENABLED` (3) | Always Encrypted está habilitado y se usa tanto para los parámetros como para los resultados.|

Los nuevos identificadores de instrucciones creados a partir de una conexión con Always Encrypted habilitado tienen el valor predeterminado de SQL_CE_ENABLED. Los identificadores creados a partir de una conexión con esta opción deshabilitada tienen el valor predeterminado de SQL_CE_DISABLED y, en este caso, no es posible habilitar Always Encrypted en ellos.

Si la mayoría de las consultas de una aplicación cliente acceden a columnas cifradas, se recomiendan los siguientes puntos:

- Establezca la palabra clave de cadena de conexión `ColumnEncryption` en `Enabled`.

- Establezca el atributo `SQL_SOPT_SS_COLUMN_ENCRYPTION` en `SQL_CE_DISABLED` en las instrucciones que no acceden a ninguna columna cifrada. Este valor deshabilitará la llamada a `sys.sp_describe_parameter_encryption` y los intentos para descifrar los valores del conjunto de resultados.

- Establezca el atributo `SQL_SOPT_SS_COLUMN_ENCRYPTION` en `SQL_CE_RESULTSETONLY` en las instrucciones que no tienen ningún parámetro que requiera cifrado, pero que recuperan datos de las columnas cifradas. Este valor deshabilitará la llamada a `sys.sp_describe_parameter_encryption` y el cifrado de parámetros. Los resultados que contienen columnas cifradas seguirán descifrándose.

- Use instrucciones preparadas para las consultas que se ejecutarán más de una vez. Prepare la consulta con `SQLPrepare` y guarde el identificador de la instrucción, para reutilizarlo con `SQLExecute` cada vez que se ejecute. Este es el método preferido para el rendimiento, incluso aunque no haya columnas cifradas, y permite que el controlador aproveche los metadatos almacenados en caché.

## <a name="always-encrypted-security-settings"></a>Configuración de seguridad de Always Encrypted

### <a name="force-column-encryption"></a>Cifrado forzado de columnas

Para forzar el cifrado de un parámetro, establezca el campo del descriptor de parámetros de implementación (IPD) `SQL_CA_SS_FORCE_ENCRYPT` mediante una llamada a la función SQLSetDescField. Un valor distinto de cero causa que el controlador devuelva un error cuando no se devuelve ningún metadato de cifrado para el parámetro asociado.

```cpp
SQLHDESC ipd;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, &ipd, 0, 0);
SQLSetDescField(ipd, paramNum, SQL_CA_SS_FORCE_ENCRYPT, (SQLPOINTER)TRUE, SQL_IS_SMALLINT);   
```

Si SQL Server indica al controlador que no hace falta cifrar el parámetro, las consultas que utilice este último generarán un error. Este comportamiento confiere una protección adicional frente a ataques contra la seguridad que utilice un servidor SQL Server comprometido que proporcione metadatos de cifrado incorrectos al cliente, lo cual podría provocar la divulgación de datos.

### <a name="column-encryption-key-caching"></a>Almacenamiento en memoria caché de claves de cifrado de columnas

Para reducir el número de llamadas a un almacén de claves maestras de columna para descifrar las claves de cifrado de columnas, el controlador almacena en memoria caché las claves de cifrado de columnas de texto no cifrado en la memoria. La caché de la CEK es global para el controlador y no está asociada con ninguna conexión. Después de recibir la ECEK de los metadatos de la base de datos, el controlador primer intenta encontrar la CEK de texto no cifrado correspondiente para el valor de la clave cifrada en la caché. El controlador llama al almacén de claves que contiene la CMK solo si no puede encontrar la CEK de texto no cifrado correspondiente en la caché.

> [!NOTE]
> En el controlador ODBC para SQL Server, las entradas de la memoria caché se desalojan después de un tiempo de expiración de dos horas. Este comportamiento significa que, para una ECEK específica, el controlador se pone en contacto con el almacén de claves solo una vez durante la vigencia de la aplicación o cada dos horas, lo que menos tiempo tarde.

A partir del controlador ODBC 17.1 para SQL Server, el tiempo de expiración de la caché para la CEK puede ajustarse con el atributo de conexión `SQL_COPT_SS_CEKCACHETTL`, que especifica el número de segundos que una CEK permanecerá en la caché. Debido a la naturaleza global de la caché, este atributo puede ajustarse a partir de cualquier identificador de conexión válido para el controlador. Si el TTL de la caché se reduce, las CEK existentes que no excedan el nuevo TTL también se desalojan. Si el valor es 0, no se almacena ninguna CEK en la caché.

### <a name="trusted-key-paths"></a>Rutas de acceso de claves de confianza

A partir del controlador ODBC 17.1 para SQL Server, el atributo de conexión `SQL_COPT_SS_TRUSTEDCMKPATHS` permite a una aplicación requerir que las operaciones de Always Encrypted solo usen una lista específica de CMK, identificadas por sus rutas de acceso de claves. De forma predeterminada, este atributo es NULL, lo que significa que el controlador acepta cualquier ruta de acceso de claves. Para usar esta característica, establezca `SQL_COPT_SS_TRUSTEDCMKPATHS` para que apunte a una cadena de caracteres anchos terminada en NULL y delimitada por valores NULL en la que se enumeren las rutas de acceso de claves permitidas. La memoria a la que apunta este atributo debe seguir siendo válida durante las operaciones de cifrado o descifrado con el identificador de conexión en el que se establece, en el que se basará el controlador para comprobar si la ruta de acceso de la CMK especificada por los metadatos del servidor no distingue mayúsculas y minúsculas en esta lista. Si la ruta de acceso a la CMK no está en la lista, se producirá un error con la operación. La aplicación puede cambiar el contenido de la memoria a la que apunta este atributo, para cambiar su lista de CMK de confianza, sin necesidad de volver a configurar el atributo.

## <a name="working-with-column-master-key-stores"></a>Trabajar con almacenes de claves maestras de columna

Para cifrar o descifrar los datos, el controlador necesita obtener una CEK que esté configurada para la columna de destino. Las CEK se almacenan con formato cifrado (ECEK) en los metadatos de la base de datos. Cada CEK tiene una CMK correspondiente que se usó para cifrarla. Los [metadatos de la base de datos](../../t-sql/statements/create-column-master-key-transact-sql.md) no almacenan la propia CMK; solo contienen el nombre el almacén de claves e información que el almacén de claves puede usar para buscar la CMK.

Para obtener el valor de texto no cifrado de una ECEK, el controlador primero obtiene los metadatos sobre la CEK y su CMK correspondiente y, después, usa esta información para ponerse en contacto con el almacén de claves que contiene la CMK y le pide que descifre la ECEK. El controlador se comunica con un almacén de claves mediante un proveedor de almacén de claves.

### <a name="built-in-keystore-providers"></a>Proveedores de almacén de claves integrados

El controlador ODBC para SQL Server incluye los siguientes proveedores de almacén de claves integrados:

| Nombre | Descripción | Nombre del proveedor (metadatos) |Disponibilidad|
|:---|:---|:---|:---|
|Azure Key Vault |Almacena las CMK en una instancia de Azure Key Vault | `AZURE_KEY_VAULT` |Windows, macOS, Linux|
|Almacén de certificados de Windows|Almacena las CMK localmente en el almacén de claves de Windows| `MSSQL_CERTIFICATE_STORE`|Windows|

- Usted (o su DBA) necesita asegurarse de que el nombre de proveedor, configurado en los metadatos de clave maestra de columna, sea correcto y que la ruta de acceso de la clave maestra de columna cumpla con un formato de ruta de acceso de la clave para un proveedor determinado. Se recomienda que configure las claves mediante herramientas como SQL Server Management Studio, que genera automáticamente las rutas de acceso a la clave y los nombres de proveedor válidos al emitir la instrucción [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) (Transact-SQL).

- Asegúrese de que la aplicación puede tener acceso a la clave del almacén de claves. Este proceso puede suponer conceder a la aplicación el acceso a la clave o al almacén de claves, dependiendo de este, o realizar cualquier otro paso de configuración específico del almacén de claves. Por ejemplo, para tener acceso a una instancia de Azure Key Vault, debe proporcionar las credenciales correctas para el almacén de claves.

### <a name="using-the-azure-key-vault-provider"></a>Usar el proveedor de Azure Key Vault

Azure Key Vault (AKV) es una opción adecuada para almacenar y administrar claves maestras de columna para Always Encrypted (especialmente si sus aplicaciones se hospedan en Azure). El controlador ODBC para SQL Server en Linux, macOS y Windows incluye un proveedor de almacén de claves maestras de columna integrado para Azure Key Vault. Para obtener más información sobre cómo configurar una instancia de Azure Key Vault para Always Encrypted, vea la [guía detallada sobre Azure Key Vault](/archive/blogs/kv/azure-key-vault-step-by-step), la [introducción a Key Vault](/azure/key-vault/general/overview) y el artículo sobre cómo [crear claves maestras de columna en Azure Key Vault](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md#creating-column-master-keys-in-azure-key-vault).

> [!NOTE]
> El controlador ODBC solo admite la autenticación de AKV directamente en Azure Active Directory. Si va a usar la autenticación de Azure Active Directory para AKV y la configuración de Active Directory requiere autenticación sobre un punto de conexión de Servicios de federación de Active Directory, se puede producir un error en la autenticación.
> En Linux y macOS, para la versión 17.2 y versiones posteriores, `libcurl` debe utilizar este proveedor, pero no es una dependencia explícita, ya que otras operaciones con el controlador no la necesitan. Si detecta un error con respecto a `libcurl`, asegúrese de que está instalado.

El controlador admite la autenticación en Azure Key Vault mediante los siguientes tipos de credencial:

- Nombre de usuario/contraseña: con este método, las credenciales son el nombre de un usuario de Azure Active Directory y su contraseña.

- Id. de cliente/secreto: con este método, las credenciales son un identificador de cliente y un secreto de la aplicación.

- Identidad administrada (17.5.2 o posterior): asignada por el sistema o por el usuario; para más información, vea [Identidades administradas para los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/).

- Autenticación interactiva de Azure Key Vault (17.7 o versiones posteriores de los controladores de Windows): con este método, las credenciales se autentican mediante Azure Active Directory con el identificador de inicio de sesión.

Para permitir que el controlador use las CMK almacenadas en AKV para el cifrado de columnas, utilice las siguientes palabras clave solo para la cadena de conexión:

|Tipo de credencial|<code>KeyStoreAuthentication</code>|<code>KeyStorePrincipalId</code>|<code>KeyStoreSecret</code>|
|-|-|-|-|
|Nombre de usuario y contraseña| `KeyVaultPassword`|Nombre principal del usuario|Contraseña|
|Id. de cliente/secreto| `KeyVaultClientSecret`|Id. de cliente|Secreto|
|Identidad administrada|`KeyVaultManagedIdentity`|Identificador de objeto (opcional, solo para las asignadas por el usuario)|(no especificado)|
|Actualización interactiva de AKV|`KeyVaultInteractive`|(sin establecer)|(sin establecer)|

#### <a name="example-connection-strings"></a>Ejemplos de cadena de conexión

Las cadenas de conexión siguientes muestran cómo autenticarse en Azure Key Vault con los dos tipos de credenciales:

**ClientID o secreto**:

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<clientId>;KeyStoreSecret=<secret>
```

**Nombre de usuario y contraseña**:

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultPassword;KeyStorePrincipalId=<username>;KeyStoreSecret=<password>
```

**Identidad administrada (asignada por el sistema)**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity
```

**Identidad administrada (asignada por el usuario)**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity;KeyStorePrincipalId=<objectID>
```

**Actualización interactiva de AKV**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultInteractive;UID=<userID>;PWD=<password>
```

Ningún otro cambio de la aplicación ODBC deben usar AKV para el almacenamiento de CMK.

> [!NOTE]
> El controlador contiene una lista de puntos de conexión de AKV en los que confía. A partir de la versión 17.5.2 del controlador, esta lista es configurable: establezca la propiedad `AKVTrustedEndpoints` en el controlador o la clave del Registro ODBCINST.INI u ODBC.INI del DSN (Windows), o bien la sección de archivo `odbcinst.ini` u `odbc.ini` (Linux/macOS) en una lista delimitada por punto y coma. La configuración en el DSN tiene prioridad sobre la configuración en el controlador. Si el valor comienza con un punto y coma, extiende la lista predeterminada; en caso contrario, la reemplaza. La lista predeterminada (a partir de 17.5) es `vault.azure.net;vault.azure.cn;vault.usgovcloudapi.net;vault.microsoftazure.de`. A partir de la versión 17.7, la lista también incluye `managedhsm.azure.net;managedhsm.azure.cn;managedhsm.usgovcloudapi.net;managedhsm.microsoftazure.de`.

### <a name="using-the-windows-certificate-store-provider"></a>Uso del proveedor para el Almacén de certificados de Windows

El controlador ODBC para SQL Server en Windows incluye un proveedor de almacén de claves maestras de columna integrado para el almacén de certificados de Windows, denominado `MSSQL_CERTIFICATE_STORE`. (Este proveedor no está disponible en macOS o Linux). Con este proveedor, la CMK se almacena localmente en el equipo cliente, y la aplicación no requiere ninguna configuración adicional para usarla con el controlador. Sin embargo, la aplicación debe tener acceso al certificado y a su clave privada en el almacén. Para obtener más información, vea [Create and Store Column Master Keys (Always Encrypted) (Crear y almacenar claves maestras de columna (Always Encrypted))](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-custom-keystore-providers"></a>Usar proveedores de almacén de claves personalizados

El controlador ODBC para SQL Server también admite proveedores de almacén de claves personalizados de terceros mediante la interfaz CEKeystoreProvider. Esta característica permite a una aplicación cargar, consultar y configurar los proveedores de almacén de claves, de modo que el controlador pueda usarlos para acceder a las columnas cifradas. Las aplicaciones pueden interactuar directamente con un proveedor de almacén de claves con el fin de cifrar las CEK para el almacenamiento en SQL Server y realizar tareas más allá de obtener acceso a las columnas cifradas con ODBC; para más información, vea [Proveedores de almacén de claves personalizados](custom-keystore-providers.md).

Se usan dos atributos de conexión para interactuar con los proveedores de almacén de claves personalizados. Son las siguientes:

- `SQL_COPT_SS_CEKEYSTOREPROVIDER`

- `SQL_COPT_SS_CEKEYSTOREDATA`

El primero se usa para cargar y enumerar los proveedores de almacén de claves cargados, mientras que el segundo permite las comunicaciones entre la aplicación y el proveedor. Estos atributos de conexión pueden utilizarse en cualquier momento, antes o después de establecer una conexión, puesto que la interacción con el proveedor de la aplicación no requiere comunicación con SQL Server. Sin embargo, como aún no se ha cargado el controlador, la configuración y obtención de estos atributos antes de la conexión causará que el Administrador de controladores los procese, y puede no ofrecer los resultados esperados.

#### <a name="loading-a-keystore-provider"></a>Cargar un proveedor de almacén de claves

La configuración del atributo de conexión `SQL_COPT_SS_CEKEYSTOREPROVIDER` permite a una aplicación cliente cargar una biblioteca de proveedores, en la que se encuentran disponibles los proveedores de almacén de claves para su uso.

```cpp
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| Argumento | Descripción |
|:---|:---|
|`ConnectionHandle`|[Entrada] Identificador de conexión. Debe ser un identificador de conexión válido, pero se puede acceder a los proveedores cargados con un identificador de conexión desde cualquier otro en el mismo proceso.|
|`Attribute`|[Entrada] Atributo que se debe establecer: la constante `SQL_COPT_SS_CEKEYSTOREPROVIDER`.|
|`ValuePtr`|[Entrada] Puntero a una cadena de caracteres terminada en NULL que especifica el nombre de archivo de la biblioteca de proveedores. Para SQLSetConnectAttrA, este valor es una cadena ANSI (multibyte). Para SQLSetConnectAttrW, este valor es una cadena Unicode (wchar_t).|
|`StringLength`|[Entrada] La longitud de la cadena ValuePtr o SQL_NTS.|

El controlador intenta cargar la biblioteca identificada por el parámetro ValuePtr con el mecanismo de carga de la biblioteca dinámica definido por la plataforma (`dlopen()` en Linux y macOS y `LoadLibrary()` en Windows), y agrega cualquier proveedor definido en ella a la lista de proveedores conocidos del controlador. Pueden producirse los errores siguientes:

| Error | Descripción |
|:--|:--|
|`CE203`|No se pudo cargar la biblioteca dinámica.|
|`CE203`|El símbolo exportado "CEKeyStoreProvider" no se encontró en la biblioteca.|
|`CE203`|Ya se han cargado uno o varios proveedores en la biblioteca.|

`SQLSetConnectAttr` devuelve el error habitual o los valores de éxito, y se encuentra disponible información adicional para todos los errores ocurridos con el mecanismo de diagnóstico de ODBC estándar.

> [!NOTE]
> El programador de la aplicación debe asegurarse de que todos los proveedores personalizados se carguen antes de que cualquier consulta que los necesite se envíe a través de cualquier conexión. Si no lo hace, se producirá el siguiente error:

| Error | Descripción |
|:--|:--|
|`CE200`|No se encontró el proveedor de almacén de claves %1. Asegúrese de que se haya cargado la biblioteca adecuada de proveedores de almacenes de claves.|

> [!NOTE]
> Los implementadores de proveedores de almacén de claves deben evitar el uso de `MSSQL` en el nombre de sus proveedores personalizados. Este término se reserva exclusivamente para uso de Microsoft y puede provocar conflictos con futuros proveedores integrados. El uso de este término en el nombre de un proveedor personalizado puede producir una advertencia de ODBC.

#### <a name="getting-the-list-of-loaded-providers"></a>Obtener la lista de proveedores cargados

Obtener este atributo de conexión permite que una aplicación cliente determine los proveedores de almacén de claves cargados actualmente en el controlador, incluidos los proveedores integrados. Este proceso solo puede realizarse después de la conexión.

```cpp
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| Argumento | Descripción |
|:---|:---|
|`ConnectionHandle`|[Entrada] Identificador de conexión. Debe ser un identificador de conexión válido, pero se puede acceder a los proveedores cargados con un identificador de conexión desde cualquier otro en el mismo proceso.|
|`Attribute`|[Entrada] Atributo que se debe recuperar: la constante `SQL_COPT_SS_CEKEYSTOREPROVIDER`.|
|`ValuePtr`|[Salida] Un puntero a la memoria en la que se va a devolver el siguiente nombre de proveedor cargado.|
|`BufferLength`|[Entrada] La longitud del búfer ValuePtr.|
|`StringLengthPtr`|[Salida] Un puntero a un búfer en el que se va a devolver el número total de bytes (excluido el carácter de terminación NULL) disponibles para devolver en \*ValuePtr. Si ValuePtr es un puntero nulo, no se devuelve ninguna longitud. Si el valor del atributo es una cadena de caracteres y el número de bytes disponible para devolver es mayor que BufferLength menos la longitud del carácter con terminación NULL, los datos de \*ValuePtr se truncan en BufferLength menos la longitud del carácter de terminación NULL y termina en NULL en el controlador.|

Para permitir la recuperación de toda la lista, cada operación Get devuelve el nombre del proveedor actual e incrementa un contador interno al siguiente. Una vez que este contador alcanza el final de la lista, se devuelve una cadena vacía ("") y se restablece el contador; las operaciones Get sucesivas continúan de nuevo desde el principio de la lista.

### <a name="communicating-with-keystore-providers"></a>Comunicación con proveedores de almacén de claves

El atributo de conexión `SQL_COPT_SS_CEKEYSTOREDATA` permite que una aplicación cliente se comunique con los proveedores de almacén de claves cargados para configurar parámetros adicionales, material para claves, etc. La comunicación entre una aplicación cliente y un proveedor sigue un protocolo simple de solicitud-respuesta, en función de las solicitudes Get y Set que usan este atributo de conexión. La comunicación la inicia solo la aplicación cliente.

> [!NOTE]
> Debido a la naturaleza de la respuesta de CEKeyStoreProvider a las llamadas de ODBC para (SQLGet/SetConnectAttr), la interfaz de ODBC solo permite configurar los datos con la resolución del contexto de conexión.

La aplicación se comunica con los proveedores de almacén de claves mediante el controlador a través de la estructura CEKeystoreData:

```cpp
typedef struct CEKeystoreData {
wchar_t *name;
unsigned int dataSize;
char data[];
} CEKEYSTOREDATA;
```

| Argumento | Descripción |
|:---|:---|
|`name`|[Entrada] Una vez realizada la solicitud Set, se corresponde con el nombre el proveedor al que se envían los datos. Se omite tras la solicitud Get. Cadena de caracteres anchos terminada en NULL.|
|`dataSize`|[Entrada] El tamaño de la matriz de datos siguiendo la estructura.|
|`data`|[Entrada] Una vez realizada la solicitud Set, se corresponde con los datos que se deben enviar al proveedor. Estos datos pueden ser arbitrarios; el controlador no intenta interpretarlos. Tras la solicitud Get, el búfer recibe los datos leídos del proveedor.|

#### <a name="writing-data-to-a-provider"></a>Escribir datos en un proveedor

Una llamada `SQLSetConnectAttr` con el atributo `SQL_COPT_SS_CEKEYSTOREDATA` escribe un “paquete” de datos en el proveedor de almacén de claves especificado.

```cpp
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| Argumento | Descripción |
|:---|:---|
|`ConnectionHandle`| [Entrada] Identificador de conexión. Debe ser un identificador de conexión válido, pero se puede acceder a los proveedores cargados con un identificador de conexión desde cualquier otro en el mismo proceso.|
|`Attribute`|[Entrada] Atributo que se debe establecer: la constante `SQL_COPT_SS_CEKEYSTOREDATA`.|
|`ValuePtr`|[Entrada] Puntero a una estructura CEKeystoreData. El campo de nombre de la estructura identifica el proveedor al que se destinan los datos.|
|`StringLength`|[Entrada] Constante SQL_IS_POINTER|

Puede obtener información detallada y adicional del error en [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md).

> [!NOTE]
> El proveedor puede usar el identificador de conexión para asociar los datos escritos en una conexión específica, si así lo desea. Esta característica es útil para implementar la configuración de cada conexión. También puede ignorar el contexto de conexión y tratar los datos de forma idéntica, con independencia de la conexión utilizada para enviar los datos. Para obtener más información, consulte [Asociación de contexto](custom-keystore-providers.md#context-association).

#### <a name="reading-data-from-a-provider"></a>Lectura de datos de un proveedor

Una llamada a `SQLGetConnectAttr` utilizando el atributo `SQL_COPT_SS_CEKEYSTOREDATA` lee un "paquete" de datos del proveedor *en el que se escribió por última vez*. Si no hay ninguno, se produce un error en la secuencia de función. Los implementadores del proveedor de almacén de claves recomiendan admitir “escrituras ficticias” de 0 bytes como una manera de seleccionar el proveedor para las operaciones de lectura sin provocar otros efectos secundarios, si tiene sentido hacerlo.

```cpp
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| Argumento | Descripción |
|:---|:---|
|`ConnectionHandle`|[Entrada] Identificador de conexión. Debe ser un identificador de conexión válido, pero se puede acceder a los proveedores cargados con un identificador de conexión desde cualquier otro en el mismo proceso.|
|`Attribute`|[Entrada] Atributo que se debe recuperar: la constante `SQL_COPT_SS_CEKEYSTOREDATA`.|
|`ValuePtr`|[Salida] Un puntero a una estructura CEKeystoreData en la que se colocan los datos leídos del proveedor.|
|`BufferLength`|[Entrada] Constante SQL_IS_POINTER|
|`StringLengthPtr`|[Salida] Un puntero a un búfer en el que se va a devolver BufferLength. Si *ValuePtr es un puntero nulo, no se devuelve ninguna longitud.|

El autor de la llamada debe asegurarse de que un búfer con la longitud suficiente siguiendo la estructura de CEKEYSTOREDATA se asigna para que el proveedor escriba en él. Cuando se devuelve, su campo dataSize se actualiza con la longitud real de los datos leídos del proveedor. Puede obtener información detallada y adicional del error en [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md).

Esta interfaz no impone ningún requisito adicional sobre el formato de los datos transferidos entre una aplicación y un proveedor de almacén de claves. Cada proveedor puede definir su propio formato de datos/protocolo, según sus necesidades.

Para obtener un ejemplo de la implementación de su propio proveedor de almacén de claves, vea [Proveedores de almacén de claves personalizados](custom-keystore-providers.md)

## <a name="limitations-of-the-odbc-driver-when-using-always-encrypted"></a>Limitaciones del controlador ODBC al usar Always Encrypted

### <a name="asynchronous-operations"></a>Operaciones asincrónicas

Aunque el controlador ODBC permitirá usar [operaciones asincrónicas](../../relational-databases/native-client/odbc/creating-a-driver-application-asynchronous-mode-and-sqlcancel.md) con Always Encrypted, hay un impacto en el rendimiento de las operaciones cuando Always Encrypted está habilitado. La llamada a `sys.sp_describe_parameter_encryption` para determinar los metadatos de cifrado de la instrucción está bloqueada y hará que el controlador espere a que el servidor devuelva los metadatos antes de devolver `SQL_STILL_EXECUTING`.

### <a name="retrieve-data-in-parts-with-sqlgetdata"></a>Recuperaciones de datos por partes en SQLGetData

En las versiones anteriores al controlador ODBC 17 para SQL Server, las columnas binarias y los caracteres cifrados no se pueden recuperar en partes con SQLGetData. Solo se puede realizar una llamada a SQLGetData, con un búfer de la longitud suficiente para contener los datos de la columna completa.

### <a name="send-data-in-parts-with-sqlputdata"></a>Enviar datos por partes con SQLPutData

En las versiones anteriores al controlador ODBC 17.3 para SQL Server, los datos para inserción o comparación no se pueden enviar por partes con SQLPutData. Solo se puede realizar una llamada a SQLPutData, con un búfer que contiene todos los datos. Para insertar datos largos en columnas cifradas, use la API de copia masiva, descrita en la siguiente sección, con un archivo de datos de entrada.

### <a name="encrypted-money-and-smallmoney"></a>Valores money y smallmoney cifrados

Los parámetros no pueden tener como destino las columnas cifradas **money** o **smallmoney**, ya que no hay un tipo de datos ODBC específico que se asigne a dichos tipos, lo que resulta en errores de conflicto de tipo de operando.

## <a name="bulk-copy-of-encrypted-columns"></a>Copia masiva de columnas cifradas

El uso de la [funciones de copia masiva de SQL](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md) y de la utilidad **bcp** es compatible con Always Encrypted desde el controlador ODBC 17 para SQL Server. Tanto el texto no cifrado (cifrado al insertarse y descifrado al recuperarse) como el texto cifrado (valor textual transferido) pueden insertarse y recuperarse con las API de copia masiva (bcp_&#42;) y la utilidad **bcp**.

- Para recuperar el texto cifrado en formato varbinary(max) (por ejemplo, para la carga masiva en una base de datos diferente), realice la conexión sin la opción `ColumnEncryption`, o establézcala en `Disabled` y realice una operación BCP OUT.

- Para insertar y recuperar texto no cifrado y dejar que el controlador realice el cifrado y descifrado de forma transparente y según sea necesario, es suficiente con establecer `ColumnEncryption` en `Enabled`. De lo contrario, no se modifica la funcionalidad de la API de BCP.

- Para insertar texto cifrado en formato varbinary(max) (por ejemplo, como el que se recuperó anteriormente), establezca la opción `BCPMODIFYENCRYPTED` en TRUE y realice una operación BCP IN. Para que se puedan descifrar los datos resultantes, asegúrese de que la CEK de la columna de destino sea la misma que la del texto cifrado de la que se obtuvo originalmente.

Al usar la utilidad **bcp**: para controlar la configuración de `ColumnEncryption`, use la opción -D y especifique un DSN que contenga el valor deseado. Para insertar texto cifrado, asegúrese de que la configuración `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` del usuario está habilitada.

En la tabla siguiente se proporciona un resumen de las acciones al trabajar en una columna cifrada:

|<code>ColumnEncryption</code>|Dirección BCP|Descripción|
|----------------|-------------|-----------|
|`Disabled`|OUT (al cliente)|Recupera el texto cifrado. El tipo de datos observado es **varbinary(max)** .|
|`Enabled`|OUT (al cliente)|Recupera el texto no cifrado. El controlador descifrará los datos de la columna.|
|`Disabled`|IN (en el servidor)|Inserta el texto cifrado. Este valor está pensado para mover de manera opaca los datos cifrados sin necesidad de que se descifren. La operación generará un error si la opción `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` no está establecida en el usuario o si BCPMODIFYENCRYPTED no está establecido en el identificador de conexión. Para obtener más información, consulte más adelante.|
|`Enabled`|IN (en el servidor)|Inserta el texto no cifrado. El controlador cifrará los datos de la columna.|

### <a name="the-bcpmodifyencrypted-option"></a>La opción BCPMODIFYENCRYPTED

Para evitar datos dañados, el servidor normalmente no permite insertar texto cifrado directamente en una columna cifrada y, por tanto, se producirá un error si intenta hacerlo; sin embargo, para la carga masiva de datos cifrados mediante la API de BCP, la definición de la opción `BCPMODIFYENCRYPTED` [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) en TRUE permite insertar el texto cifrado directamente y reduce el riesgo de dañar los datos cifrados al configurar la opción `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` en la cuenta de usuario. Sin embargo, las claves deben coincidir con los datos y resulta una buena idea realizar algunas comprobaciones de solo lectura de los datos insertados después de la inserción masiva y antes de su uso.

Para obtener más información, vea [Migración de datos confidenciales protegidos mediante Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="always-encrypted-api-summary"></a>Resumen de la API de Always Encrypted

### <a name="connection-string-keywords"></a>Palabras clave de cadena de conexión

|Nombre|Descripción|  
|----------|-----------------|  
|`ColumnEncryption`|Los valores aceptados son `Enabled`/`Disabled`.<br>`Enabled`: habilita o deshabilita la funcionalidad de Always Encrypted para la conexión.<br>`Disabled`: deshabilita la funcionalidad de Always Encrypted para la conexión.<br>*Protocolo de atestación*, *dirección URL de atestación* (versión 17.4 y posterior): habilita Always Encrypted con enclave seguro mediante el protocolo de atestación especificado y la dirección URL de atestación. <br><br>El valor predeterminado es `Disabled`.|
|`KeyStoreAuthentication` | Valores válidos: `KeyVaultPassword`, `KeyVaultClientSecret` y `KeyVaultInteractive`. |
|`KeyStorePrincipalId` | Cuando `KeyStoreAuthentication` = `KeyVaultPassword`, establezca este valor en un nombre principal de usuario de Azure Active Directory válido. <br>Cuando `KeyStoreAuthetication` = `KeyVaultClientSecret`, establezca este valor en un identificador de cliente de aplicación de Azure Active Directory válido. |
|`KeyStoreSecret` | Cuando `KeyStoreAuthentication` = `KeyVaultPassword`, establezca este valor en una contraseña para el nombre de usuario correspondiente. <br>Cuando `KeyStoreAuthentication` = `KeyVaultClientSecret`, establezca este valor en el secreto de la aplicación asociado con un identificador de cliente de aplicación de Azure Active Directory válido. |

### <a name="connection-attributes"></a>Atributos de conexión

|Nombre|Tipo|Descripción|  
|----------|-------|----------|  
|`SQL_COPT_SS_COLUMN_ENCRYPTION`|Antes de conectar|`SQL_COLUMN_ENCRYPTION_DISABLE` (0): Deshabilitar Always Encrypted <br>`SQL_COLUMN_ENCRYPTION_ENABLE` (1): Habilitar Always Encrypted<br> puntero a la cadena *protocolo de atestación*,*dirección URL de atestación*: (versión 17.4 y posteriores) habilitar con enclave seguro|
|`SQL_COPT_SS_CEKEYSTOREPROVIDER`|Después de conectar|[Set]: Intentar cargar CEKeystoreProvider<br>[Get]: Devolver un nombre CEKeystoreProvider|
|`SQL_COPT_SS_CEKEYSTOREDATA`|Después de conectar|[Set]: Escribir datos en CEKeystoreProvider<br>[Get]: Leer datos de CEKeystoreProvider|
|`SQL_COPT_SS_CEKCACHETTL`|Después de conectar|[Set]: Establecer el TTL de la caché de CEK<br>[Get]: Obtener el TTL de la caché de CEK|
|`SQL_COPT_SS_TRUSTEDCMKPATHS`|Después de conectar|[Set]: Establecer el puntero de rutas de acceso de CMK de confianza<br>[Get]: Obtener el puntero de rutas de acceso de CMK de confianza actuales|

### <a name="statement-attributes"></a>Atributos de instrucción

|Nombre|Descripción|  
|----------|-----------------|  
|`SQL_SOPT_SS_COLUMN_ENCRYPTION`|`SQL_CE_DISABLED` (0): Always Encrypted está deshabilitado para la instrucción <br>`SQL_CE_RESULTSETONLY` (1): Solo descifrado. Los conjuntos de resultados y los valores devueltos están descifrados, y los parámetros no se cifran. <br>`SQL_CE_ENABLED` (3): Always Encrypted está habilitado y se usa tanto para los parámetros como para los resultados.|

### <a name="descriptor-fields"></a>Campos de descriptor

|Campo de IPD|Tamaño/Tipo|Valor predeterminado|Descripción|
|-|-|-|-|  
|`SQL_CA_SS_FORCE_ENCRYPT` (1236)|WORD (2 bytes)|0|Cuando 0 (valor predeterminado): la decisión para cifrar este parámetro la determina la disponibilidad de los metadatos de cifrado.<br><br>Cuando es distinto de cero: si los metadatos de cifrado están disponibles para este parámetro, se cifran. De lo contrario, la solicitud produce un error [CE300] [Microsoft][Controlador ODBC 17 para SQL Server]El cifrado obligatorio se específico para un parámetro, pero el servidor no proporcionó ningún metadato de cifrado.|

### <a name="bcp_control-options"></a>Opciones de bcp_control

|Nombre de la opción|Valor predeterminado|Descripción|
|-|-|-|
|`BCPMODIFYENCRYPTED` (21)|FALSE|Cuando es TRUE, permite que los valores varbinary(max) se inserten en una columna cifrada. Cuando es FALSE, impide la inserción, a menos que se proporcionen los metadatos de cifrado y el tipo correcto.|

## <a name="troubleshooting"></a>Solución de problemas

Si encuentra dificultades al usar Always Encrypted, empiece por comprobar los puntos siguientes:

- El CEK que cifra la columna deseada está presente y es accesible en el servidor.

- La CMK que cifra el CEK tiene metadatos accesibles en el servidor y también es accesible desde el cliente.

- `ColumnEncryption` está habilitado en el DSN, la cadena de conexión o el atributo de conexión y, si usa el enclave seguro, tiene el formato correcto.

Además, cuando se usa el enclave seguro, los errores de atestación identifican el paso en el proceso de atestación en el que se produjo el error, según la tabla siguiente:

|Paso|Descripción|
|----|-----------|
|0 a 99| Respuesta de atestación no válida o error de comprobación de firma. |
|100 a 199| Error al recuperar certificados de la dirección URL de atestación. Asegúrese de que `<attestation URL>/v2.0/signingCertificates` sea válido y esté accesible. |
|200 a 299| Formato inesperado o incorrecto de la identidad del enclave. |
|300 a 399| Error al establecer un canal seguro con el enclave. |

## <a name="see-also"></a>Consulte también

- [Always Encrypted (motor de base de datos)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md)
