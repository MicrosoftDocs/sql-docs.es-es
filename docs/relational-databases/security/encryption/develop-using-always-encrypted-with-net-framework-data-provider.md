---
title: Always Encrypted con el proveedor de datos .NET Framework
description: Obtenga información sobre cómo desarrollar aplicaciones .NET con la característica Always Encrypted para SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: security, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 827e509e-3c4f-4820-aa37-cebf0f7bbf80
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 26b598865668ce5fb10e34eb9558fe018a7c60a8
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075897"
---
# <a name="using-always-encrypted-with-the-net-framework-data-provider-for-sql-server"></a>Uso de Always Encrypted con el proveedor de datos .NET Framework para SQL Server
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

En este artículo se ofrece información sobre cómo desarrollar aplicaciones .NET con [Always Encrypted](always-encrypted-database-engine.md) o [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md) y el [proveedor de datos .NET Framework para SQL Server](/dotnet/framework/data/adonet/sql/).

Always Encrypted permite a las aplicaciones cliente cifrar la información confidencial y nunca revelar los datos ni las claves de cifrado en SQL Server o Azure SQL Database. Un controlador habilitado para Always Encrypted, como el proveedor de datos .NET Framework para SQL Server, consigue esto al cifrar y descifrar de manera transparente la información confidencial en la aplicación cliente. El controlador determina automáticamente qué parámetros de consulta corresponden a columnas de bases de datos confidenciales (protegidas mediante Always Encrypted) y cifra los valores de esos parámetros antes de pasar los datos a SQL Server o Azure SQL Database. De forma similar, el controlador descifra de manera transparente los datos que se han recuperado de las columnas de bases de datos cifradas de los resultados de la consulta. Para obtener más información, vea [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md).

## <a name="prerequisites"></a>Prerrequisitos

- Configure Always Encrypted en su base de datos. Esto implica el aprovisionamiento de las claves Always Encrypted y la configuración del cifrado para las columnas de las bases de datos seleccionadas. Si todavía no tiene una base de datos configurada con Always Encrypted, siga las instrucciones de [Introducción a Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted).
- Si usa Always Encrypted con enclaves seguros, vea [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md) para obtener los requisitos previos adicionales.
- Asegúrese de que la versión 4.6.1 o posterior de .NET Framework esté instalada en la máquina de desarrollo. Para obtener más información, vea [.NET Framework 4.6](/dotnet/framework/). También debe asegurarse de que la versión 4.6 o posterior de .NET Framework esté configurada como la versión de .NET Framework de destino en su equipo de desarrollo. Si usa Visual Studio, consulte [Cómo: Usar como destino una versión de .NET Framework](/visualstudio/ide/how-to-target-a-version-of-the-dotnet-framework). 

> [!NOTE]
> El nivel de compatibilidad con Always Encrypted en determinadas versiones de .NET Framework varía. Vea la siguiente sección de referencia de la API de Always Encrypted para obtener más información.

## <a name="enabling-always-encrypted-for-application-queries"></a>Habilitar Always Encrypted para consultas de la aplicación
Establecer el valor de la palabra clave de cadena de conexión Valor de cifrado de columnas en **enabled** es la manera más sencilla de habilitar el cifrado de parámetros y el descifrado de los resultados de la consulta que tienen como destino las columnas cifradas.

A continuación se muestra un ejemplo de una cadena de conexión que habilita Always Encrypted:

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
SqlConnection connection = new SqlConnection(connectionString);
```

El ejemplo siguiente es uno equivalente que usa la propiedad SqlConnectionStringBuilder.ColumnEncryptionSetting.

```cs
SqlConnectionStringBuilder strbldr = new SqlConnectionStringBuilder();
strbldr.DataSource = "server63";
strbldr.InitialCatalog = "Clinic";
strbldr.IntegratedSecurity = true;
strbldr.ColumnEncryptionSetting = SqlConnectionColumnEncryptionSetting.Enabled;
SqlConnection connection = new SqlConnection(strbldr.ConnectionString);
```

Always Encrypted también puede habilitarse para las consultas individuales. Vea la siguiente sección **Controlar el impacto en el rendimiento de Always Encrypted** .
Habilitar Always Encrypted no es suficiente para que el cifrado o el descifrado se realicen correctamente. También necesita asegurarse de que:
- La aplicación tiene los permisos de base de datos *VIEW ANY COLUMN MASTER KEY DEFINITION* y *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* , que se requieren para tener acceso a los metadatos sobre las claves Always Encrypted de la base de datos. Para obtener más información, vea la [sección Permisos en Always Encrypted (motor de base de datos)](./always-encrypted-database-engine.md#database-permissions).
- La aplicación puede tener acceso a la clave maestra de columna que protege las claves de cifrado de columnas al cifrar las columnas de bases de datos consultadas.

## <a name="enabling-always-encrypted-with-secure-enclaves"></a>Habilitación de Always Encrypted con enclaves seguros

A partir de la versión 4.7.2 de .NET Framework, el controlador admite [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md). 

Para obtener información general sobre el rol de controlador de cliente en los cálculos de enclave y la atestación de enclave, consulte [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md). 

Para configurar la aplicación:

1. Habilite Always Encrypted para las consultas de la aplicación, como se ha explicado en la sección anterior.
2. Integre el paquete [Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders) de NuGet en la aplicación. NuGet es una biblioteca de proveedores de enclaves que implementa la lógica del lado cliente para protocolos de atestación y para establecer un canal seguro con un enclave seguro.  
3. Actualice la configuración de la aplicación (por ejemplo, en web.config o app.config) para definir la asignación entre un tipo de enclave, configurado para la base de datos, y un proveedor de enclave. 
    1. Si usa [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y el Servicio de protección de host (HGS), tendrá que asignar el tipo de enclave de VBS a la clase Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider del paquete NuGet.
    2. Si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] y Microsoft Azure Attestation, tendrá que asignar el tipo de enclave de SGX a la clase Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.AzureAttestationEnclaveProvider del paquete NuGet.

    Para obtener instrucciones detalladas sobre cómo editar la configuración de la aplicación, vea [Tutorial: Desarrollo de una aplicación de .NET Framework mediante Always Encrypted con enclaves seguros](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md).

4. Establezca la palabra clave `Enclave Attestation URL` de la cadena de conexión de base de datos en una dirección URL de atestación (un punto de conexión de servicio de atestación). Debe obtener una dirección URL de atestación para su entorno por parte del administrador del servicio de atestación.
   1. Si usa [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y el Servicio de protección de host (HGS), vea [Determinación y uso compartido de la dirección URL de atestación de HGS](always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
   2. Si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] y Microsoft Azure Attestation, vea [Determinación de la dirección URL de atestación de la directiva de atestación](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation).

Para seguir un tutorial paso a paso, consulte [Tutorial: Desarrollo de una aplicación de .NET Framework mediante Always Encrypted con enclaves seguros](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Recuperar y modificar los datos de las columnas cifradas

Una vez que habilite Always Encrypted para las consultas de la aplicación, puede usar las API estándar de ADO.NET (vea [Recuperación y modificación de datos en ADO.NET](/dotnet/framework/data/adonet/retrieving-and-modifying-data)) o las API del [proveedor de datos .NET Framework para SQL Server](/dotnet/framework/data/adonet/sql/) , que se definen en [System.Data.SqlClient Namespace](/dotnet/api/system.data.sqlclient), para recuperar o modificar los datos de las columnas de bases de datos cifradas. Suponiendo que su aplicación tenga los permisos necesarios de base de datos y pueda tener acceso a la clave maestra de columna, el proveedor de datos .NET Framework para SQL Server cifrará cualquier parámetro de consulta que tenga como destino las columnas cifradas, y descifrará los datos que se recuperen de las columnas cifradas al devolver valores de texto sin formato de tipos .NET. Estos corresponden a los tipos de datos de SQL Server establecidos para las columnas del esquema de la base de datos.
Si Always Encrypted no está habilitado, se producirá un error en las consultas con parámetros que tengan como destino las columnas cifradas. Las consultas todavía pueden recuperar datos de las columnas cifradas, siempre y cuando la consulta no tenga parámetros que tengan como destino las columnas cifradas. Sin embargo, el proveedor de datos .NET Framework para SQL Server no intentará descifrar ningún valor que se haya recuperado de las columnas cifradas y la aplicación recibirá datos binarios cifrados (como matrices de bytes).

En la tabla siguiente se resume el comportamiento de las consultas, dependiendo de si Always Encrypted está habilitado o no:

|Característica de las consultas | Always Encrypted está habilitado y la aplicación puede tener acceso a las claves y a los metadatos de clave|Always Encrypted está habilitado y la aplicación no puede tener acceso a las claves ni a los metadatos de clave | Always Encrypted está deshabilitado|
|:---|:---|:---|:---|
| Consultas con parámetros que tienen como destino las columnas cifradas. | Los valores de parámetro se cifran de manera transparente. | Error | Error|
| Consultas que recuperan datos de las columnas cifradas, sin parámetros que tengan como destino las columnas cifradas.| Los resultados de las columnas cifradas se descifran de manera transparente. La aplicación recibe valores de texto sin formato de los tipos de datos .NET que corresponden a los tipos de SQL Server configurados para las columnas cifradas. | Error | Los resultados de las columnas cifradas no se descifran. La aplicación recibe valores cifrados como matrices de bytes (byte[]). 

En el siguiente ejemplo se ilustra la recuperación y la modificación de datos en las columnas cifradas. Los ejemplos suponen la tabla de destino con el siguiente esquema. Las columnas SSN y BirthDate están cifradas.


```sql
CREATE TABLE [dbo].[Patients]([PatientId] [int] IDENTITY(1,1), 
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

### <a name="inserting-data-example"></a>Ejemplo de inserción de datos

En este ejemplo se inserta una fila en la tabla Patients. Tenga en cuenta lo siguiente:
- No existe nada específico al cifrado en el código de ejemplo. El proveedor de datos .NET Framework para SQL Server detecta y cifra automáticamente los parámetros *paramSSN* y *paramBirthdate* que tienen como destino las columnas cifradas. Esto hace que el cifrado se realice de manera transparente en la aplicación. 
- Los valores que se insertan en las columnas de bases de datos, incluidas las columnas cifradas, se pasan como objetos [SqlParameter](/dotnet/api/system.data.sqlclient.sqlparameter) . Si bien el uso de **SqlParameter** es opcional al enviar valores a las columnas no cifradas (aunque es altamente recomendable porque ayuda a evitar la inyección de código SQL), es necesario para los valores que tienen como destino las columnas cifradas. Si los valores que se han insertado en las columnas SSN o BirthDate se pasan como literales incrustados en la instrucción de consulta, la consulta producirá un error porque el proveedor de datos .NET Framework para SQL Server no podrá determinar los valores de las columnas cifradas de destino, por lo que no los cifrará. Como resultado, el servidor los rechazará considerándolos incompatibles con las columnas cifradas.
- El tipo de datos del parámetro que tiene como destino la columna SSN se establece como una cadena ANSI (no Unicode), que se asigna al tipo de datos char o varchar de SQL Server. Si el tipo del parámetro se ha establecido como una cadena Unicode (cadena) que se asigna a nchar o nvarchar, la consulta producirá un error, ya que Always Encrypted no admite conversiones de valores cifrados nchar o nvarchar a valores cifrados char o varchar. Vea [Asignar tipos de datos de SQL Server](/dotnet/framework/data/adonet/sql-server-data-type-mappings) para obtener información sobre las asignaciones de tipos de datos.
- El tipo de datos del parámetro que se ha insertado en la columna BirthDate se establece explícitamente en el tipo de datos de SQL Server de destino mediante la propiedad [SqlParameter.SqlDbType](/dotnet/api/system.data.sqlclient.sqlparameter.sqldbtype), en lugar de basarse en la asignación implícita de tipos .NET para los tipos de datos de SQL Server que se han aplicado al usar la propiedad [SqlParameter.DbType](/dotnet/api/system.data.sqlclient.sqlparameter.dbtype). De forma predeterminada, la [estructura DateTime](/dotnet/api/system.datetime) se asigna al tipo de datos datetime de SQL Server. Como el tipo de datos de la columna BirthDate es de fecha y Always Encrypted no admite la conversión de valores cifrados de datetime en valores de fecha cifrados, se producirá un error al usar la asignación predeterminada. 

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
using (SqlConnection connection = new SqlConnection(strbldr.ConnectionString))
{
   using (SqlCommand cmd = connection.CreateCommand())
   {
      cmd.CommandText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

      SqlParameter paramSSN = cmd.CreateParameter();
      paramSSN.ParameterName = @"@SSN";
      paramSSN.DbType = DbType.AnsiStringFixedLength;
      paramSSN.Direction = ParameterDirection.Input;
      paramSSN.Value = "795-73-9838";
      paramSSN.Size = 11;
      cmd.Parameters.Add(paramSSN);

      SqlParameter paramFirstName = cmd.CreateParameter();
      paramFirstName.ParameterName = @"@FirstName";
      paramFirstName.DbType = DbType.String;
      paramFirstName.Direction = ParameterDirection.Input;
      paramFirstName.Value = "Catherine";
      paramFirstName.Size = 50;
      cmd.Parameters.Add(paramFirstName);

      SqlParameter paramLastName = cmd.CreateParameter();
      paramLastName.ParameterName = @"@LastName";
      paramLastName.DbType = DbType.String;
      paramLastName.Direction = ParameterDirection.Input;
      paramLastName.Value = "Abel";
      paramLastName.Size = 50;
      cmd.Parameters.Add(paramLastName);

      SqlParameter paramBirthdate = cmd.CreateParameter();
      paramBirthdate.ParameterName = @"@BirthDate";
      paramBirthdate.SqlDbType = SqlDbType.Date;
      paramBirthdate.Direction = ParameterDirection.Input;
      paramBirthdate.Value = new DateTime(1996, 09, 10);
      cmd.Parameters.Add(paramBirthdate);

      cmd.ExecuteNonQuery();
   } 
}
```

### <a name="retrieving-plaintext-data-example"></a>Ejemplo de la recuperación de datos de texto sin formato

En el siguiente ejemplo se muestra el filtrado de datos basándose en valores cifrados, así como la recuperación de datos de texto sin formato de las columnas cifradas. Tenga en cuenta lo siguiente:
- El valor que se ha usado en la cláusula WHERE para filtrar en la columna SSN necesita pasarse mediante SqlParameter, de forma que el proveedor de datos .NET Framework para SQL Server pueda cifrarlo de manera transparente antes de enviarlo a la base de datos.
- Todos los valores impresos por el programa estarán en texto sin formato, ya que el proveedor de datos .NET Framework para SQL Server descifrará los datos que se han recuperado de las columnas SSN y BirthDate de manera transparente.


> [!NOTE]
> Las consultas pueden realizar comparaciones de igualdad en las columnas si se cifran mediante el cifrado determinista. Para obtener más información, consulte [Selección del cifrado determinista o aleatorio](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
    
using (SqlConnection connection = new SqlConnection(strbldr.ConnectionString))
 {
    using (SqlCommand cmd = connection.CreateCommand())
 {

 cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN=@SSN";
 SqlParameter paramSSN = cmd.CreateParameter();
 paramSSN.ParameterName = @"@SSN";
 paramSSN.DbType = DbType.AnsiStringFixedLength;
 paramSSN.Direction = ParameterDirection.Input;
 paramSSN.Value = "795-73-9838";
 paramSSN.Size = 11;
 cmd.Parameters.Add(paramSSN);
 using (SqlDataReader reader = cmd.ExecuteReader())
 {
   if (reader.HasRows)
 {
 while (reader.Read())
 {
    Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
 }
```

### <a name="retrieving-encrypted-data-example"></a>Ejemplo de la recuperación de los datos cifrados

Si Always Encrypted no está habilitado, una consulta todavía puede recuperar datos de las columnas cifradas, siempre y cuando la consulta no tenga parámetros que tengan como destino las columnas cifradas.

En el ejemplo siguiente se ilustra la recuperación de datos binarios de las columnas cifradas. Tenga en cuenta lo siguiente:

- Como Always Encrypted no está habilitado en la cadena de conexión, la consulta devolverá valores cifrados de SSN y BirthDate como matrices de bytes (el programa convierte los valores en cadenas).
- Una consulta que recupera datos de columnas cifradas con Always Encrypted deshabilitado puede tener parámetros, siempre y cuando ninguno de ellos tenga como destino una columna cifrada. La consulta anterior filtra por LastName, que no está cifrado en la base de datos. Si la consulta ha filtrado por SSN o BirthDate, esta producirá un error.


```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
                
using (SqlConnection connection = new SqlConnection(connectionString))
{
   connection.Open();
   using (SqlCommand cmd = connection.CreateCommand())
   {
      cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName";
      SqlParameter paramLastName = cmd.CreateParameter();
      paramLastName.ParameterName = @"@LastName";
      paramLastName.DbType = DbType.String;
      paramLastName.Direction = ParameterDirection.Input;
      paramLastName.Value = "Abel";
      paramLastName.Size = 50;
      cmd.Parameters.Add(paramLastName);
      using (SqlDataReader reader = cmd.ExecuteReader())
      {
         if (reader.HasRows)
         {
            while (reader.Read())
         {
         Console.WriteLine(@"{0}, {1}, {2}, {3}", BitConverter.ToString((byte[])reader[0]), reader[1], reader[2], BitConverter.ToString((byte[])reader[3]));
      }
   }
}
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Evitar problemas comunes al consultar columnas cifradas

En esta sección se describen las categorías comunes de errores al consultar columnas cifradas de aplicaciones .NET y algunas instrucciones sobre cómo evitarlos.

### <a name="unsupported-data-type-conversion-errors"></a>Errores de conversión de tipos de datos no compatibles

Always Encrypted admite algunas conversiones para los tipos de datos cifrados. Consulte [Always Encrypted](always-encrypted-database-engine.md) para obtener una lista detallada de las conversiones de los tipos admitidos. Para evitar los errores de conversión de tipo de datos, haga lo siguiente:

- Establezca los tipos de parámetros que tienen como destino las columnas cifradas, de forma que el tipo de datos de SQL Server del parámetro sea exactamente el mismo que el tipo de la columna de destino o una conversión compatible del tipo de datos de SQL Server del parámetro al tipo de destino de la columna. Puede aplicar la asignación deseada de los tipos de datos .NET a los tipos de datos de SQL Server mediante la propiedad SqlParameter.SqlDbType.
- Compruebe la precisión y la escala de los parámetros que tienen como destino columnas de los tipos de datos numéricos y decimales de SQL Server debe ser la misma que la precisión y la escala configurada para la columna de destino.  
- Compruebe la precisión de los parámetros que tienen como destino las columnas de tipos de datos datetime2, datetimeoffset o time de SQL Server no debe ser superior a la precisión de la columna de destino, en las consultas que modifiquen valores de la columna de destino.  

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Errores debidos a pasar texto sin formato en lugar de valores cifrados

Cualquier valor que tenga como destino una columna cifrada necesita cifrarse dentro de la aplicación. Un intento para insertar, modificar o filtrar mediante un valor de texto sin formato en una columna cifrada producirá un error similar a este:


```cs
System.Data.SqlClient.SqlException (0x80131904): Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', column_encryption_key_database_name = 'Clinic') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

Para evitar dichos errores, asegúrese de que:
- Always Encrypted esté habilitado para las consultas de la aplicación que tengan como destino las columnas cifradas (para la cadena de conexión o en el objeto [SqlCommand](/dotnet/api/system.data.sqlclient.sqlcommand) para una consulta específica).
- Usa SqlParameter para enviar datos que tengan como destino las columnas cifradas. En el ejemplo siguiente se muestra una consulta que filtra de manera incorrecta mediante un literal o constante en una columna cifrada (SSN), en lugar de pasar el literal dentro de un objeto SqlParameter. 


```cs
using (SqlCommand cmd = connection.CreateCommand())
{
   cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
cmd.ExecuteNonQuery();
}
```

## <a name="working-with-column-master-key-stores"></a>Trabajar con almacenes de claves maestras de columna

Para cifrar un valor de parámetro o para descifrar datos en los resultados de la consulta, el proveedor de datos .NET Framework para SQL Server necesita obtener una clave de cifrado de columnas que esté configurada para la columna de destino. Las claves de cifrado de columnas se almacenan con formato cifrado en los metadatos de la base de datos. Cada clave de cifrado de columnas tiene una clave maestra de columna correspondiente que se ha usado para cifrar la clave de cifrado de columnas. Los metadatos de la base de datos no almacenan las claves maestras de columna; solo contienen la información sobre un almacén de claves que contiene una clave maestra de columna determinada y la ubicación de la clave en el almacén de claves.

Para obtener un valor de texto sin formato de una clave de cifrado de columnas, el proveedor de datos .NET Framework para SQL Server obtiene los metadatos de la clave de cifrado de columnas y su clave maestra de columna correspondiente y, después, usa la información de los metadatos para ponerse en contacto con el almacén de claves que contiene la clave maestra de columna y para descifrar la clave cifrada de cifrado de columnas. El proveedor de datos .NET Framework para SQL Server se comunica con un almacén de claves mediante un proveedor de almacenamiento de claves maestras de columna, que es una instancia de una clase derivada de la clase [SqlColumnEncryptionKeyStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptionkeystoreprovider).


El proceso para obtener una clave de cifrado de columna:

1.  Si Always Encrypted está habilitado para una consulta, el proveedor de datos .NET Framework para SQL Server llama a **sys.sp_describe_parameter_encryption** de forma transparente para recuperar los metadatos de cifrado de los parámetros que tienen como destino las columnas cifradas, siempre que la consulta tenga parámetros. En el caso de los datos cifrados que se incluyen en los resultados de una consulta, SQL Server adjunta automáticamente los metadatos de cifrado. La información sobre la clave maestra de columna incluye lo siguiente:
    - El nombre de un proveedor de almacén de claves que encapsula el almacén de claves que contiene la clave maestra de columna. 
    - La ruta de acceso a la clave que especifica la ubicación de la clave maestra de columna en el almacén de claves.
    
    La información sobre la clave de cifrado de columna incluye lo siguiente:

    - El valor cifrado de una clave de cifrado de columna.
    - El nombre del algoritmo que se usó para cifrar la clave de cifrado de columna.
2.  El proveedor de datos .NET Framework para SQL Server usa el nombre del proveedor de almacén de claves maestras de columna para buscar el objeto de proveedor (una instancia de una clase derivada de la clase SqlColumnEncryptionKeyStoreProvider) en una estructura de datos interna.
3.  Para descifrar la clave de cifrado de columna, el proveedor de datos .NET Framework para SQL Server llama al método SqlColumnEncryptionKeyStoreProvider.DecryptColumnEncryptionKey y pasa la ruta de acceso a la clave maestra de columna, el valor cifrado de la clave de cifrado de columna y el nombre del algoritmo de cifrado, que usa para generar la clave de cifrado de columna cifrada.




### <a name="using-built-in-column-master-key-store-providers"></a>Usar proveedores integrados de almacenamiento de claves maestras de columna

El proveedor de datos .NET Framework para SQL Server incluye los siguientes proveedores integrados de almacenamiento de claves maestras de columna, que se registran previamente con los nombres específicos del proveedor (se usan para buscar el proveedor).


| Clase | Descripción | Nombre del proveedor (búsqueda) |
|:---|:---|:---|
|Clase SqlColumnEncryptionCertificateStoreProvider| Un proveedor para el Almacén de certificados de Windows. | MSSQL_CERTIFICATE_STORE |
|[Clase SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider) <br><br>**Nota:** Este proveedor está disponible en la versión 4.6.1 y posteriores de .NET Framework. |Un proveedor de un almacén de claves que es compatible con [Microsoft Cryptography API: API Next Generation (CNG)](/windows/win32/seccng/cng-portal). Normalmente, un almacén de este tipo es un módulo de seguridad de hardware (un dispositivo físico que protege y administra las claves digitales y proporciona un procesamiento criptográfico).  | MSSQL_CNG_STORE|
| [Clase SqlColumnEncryptionCspProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncspprovider)<br><br>**Nota:** Este proveedor está disponible en la versión 4.6.1 o posteriores de .NET Framework.| Un proveedor de un almacén de claves que es compatible con [Microsoft Cryptography API (CAPI)](/previous-versions/visualstudio/aa266944(v=vs.60)). Normalmente, un almacén de este tipo es un módulo de seguridad de hardware (un dispositivo físico que protege y administra las claves digitales y proporciona un procesamiento criptográfico).| MSSQL_CSP_PROVIDER |
  
No necesita realizar ningún cambio de código en la aplicación para usar estos proveedores pero tenga en cuenta lo siguiente:

- Usted (o su DBA) necesita asegurarse de que el nombre de proveedor, configurado en los metadatos de clave maestra de columna, sea correcto y que la ruta de acceso de la clave maestra de columna cumpla con un formato de ruta de acceso de la clave que sea válido para un proveedor determinado. Se recomienda que configure las claves mediante herramientas como SQL Server Management Studio, que genera automáticamente las rutas de acceso a la clave y los nombres de proveedor válidos al emitir la instrucción [CREATE COLUMN MASTER KEY](../../../t-sql/statements/create-column-master-key-transact-sql.md) (Transact-SQL). Para más información, consulte [Configurar Always Encrypted con SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md) y [Configurar Always Encrypted con PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).
- Asegúrese de que la aplicación pueda acceder a la clave del almacén de claves. Esto puede suponer conceder a la aplicación el acceso a la clave o al almacén de claves, dependiendo de este, o realizar cualquier otro paso de configuración específico del almacén de claves. Por ejemplo, para tener acceso a un almacén de claves que implemente CNG o CAPI (por ejemplo, un módulo de seguridad de hardware), necesita asegurarse de que esté instalada una biblioteca que implemente CNG o CAP para el almacén en su equipo de la aplicación. Para más información, vea [Creación y almacenamiento de claves maestras de columna para Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-azure-key-vault-provider"></a>Usar el proveedor de Azure Key Vault

El Almacén de claves de Azure es una opción adecuada para almacenar y administrar claves maestras de columna para Always Encrypted (especialmente si sus aplicaciones se hospedan en Azure). El proveedor de datos .NET Framework para SQL Server no incluye un proveedor integrado de almacén de claves maestras de columna para Azure Key Vault, pero se encuentra disponible como un paquete NuGet que puede integrar fácilmente en la aplicación. Para obtener más información, vea [Always Encrypted: protección de información confidencial en Base de datos SQL de Azure con cifrado de datos y almacenamiento de las claves de cifrado en el Almacén de claves de Azure](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure).

### <a name="implementing-a-custom-column-master-key-store-provider"></a>Implementar un proveedor personalizado de almacén de claves maestras de columna

Si quiere almacenar claves maestras de columna en un almacén de claves que no sea compatible con un proveedor existente, puede implementar un proveedor personalizado mediante la extensión de la clase [SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider) y registrando el proveedor mediante el método [SqlConnection.RegisterColumnEncryptionKeyStoreProviders](/dotnet/api/system.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders) .


```cs
public class MyCustomKeyStoreProvider : SqlColumnEncryptionKeyStoreProvider
    {
        public override byte[] EncryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] columnEncryptionKey)
        {
            // Logic for encrypting a column encrypted key.
        }
        public override byte[] DecryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] EncryptedColumnEncryptionKey)
        {
            // Logic for decrypting a column encrypted key.
        }
    }  
    class Program
    {
        static void Main(string[] args)
        {
            Dictionary\<string, SqlColumnEncryptionKeyStoreProvider> providers =
               new Dictionary\<string, SqlColumnEncryptionKeyStoreProvider>();
            providers.Add("MY_CUSTOM_STORE", customProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
            providers.Add(SqlColumnEncryptionCertificateStoreProvider.ProviderName, customProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers); 
       // ...
        }

    }
```
 
### <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>Usar proveedores de almacén de claves maestras de columna para el aprovisionamiento de claves mediante programación

Al tener acceso a las columnas cifradas, el proveedor de datos .NET Framework para SQL Server busca y llama de manera transparente al proveedor de almacenamiento de claves maestras de columna adecuado para descifrar las claves de cifrado de columnas. Normalmente, el código de aplicación normal no llama directamente a los proveedores de almacenamiento de claves maestras de columna. Pero puede crear una instancia y llamar a un proveedor explícitamente para aprovisionar y administrar claves Always Encrypted mediante programación: para generar una clave de cifrado de columnas cifrada y descifrar una clave de cifrado de columnas (por ejemplo, como parte de la rotación de claves maestras de columna). Para obtener más información, consulte [Información general sobre la administración de claves de Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).
Puede que la implementación de sus propias herramientas de administración de claves solo sea necesaria si usa un proveedor de almacenamiento de claves personalizado. Al usar claves almacenadas en los almacenes de claves, para las que existen los proveedores integrados, o en el Almacén de claves de Azure, puede usar herramientas existentes como SQL Server Management Studio o PowerShell para administrar y aprovisionar claves.
En el ejemplo siguiente se ilustra la generación de una clave de cifrado de columna y se usa la clase [SqlColumnEncryptionCertificateStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider) para cifrar la clave con un certificado.


```cs
using System.Security.Cryptography;
static void Main(string[] args)
{
    byte[] EncryptedColumnEncryptionKey = GetEncryptedColumnEncryptonKey(); 
    Console.WriteLine("0x" + BitConverter.ToString(EncryptedColumnEncryptionKey).Replace("-", "")); 
    Console.ReadKey();
}

static byte[]  GetEncryptedColumnEncryptonKey()
{
    int cekLength = 32;
    String certificateStoreLocation = "CurrentUser";
    String certificateThumbprint = "698C7F8E21B2158E9AED4978ADB147CF66574180";
    // Generate the plaintext column encryption key.
    byte[] columnEncryptionKey = new byte[cekLength];
    RNGCryptoServiceProvider rngCsp = new RNGCryptoServiceProvider();
    rngCsp.GetBytes(columnEncryptionKey);

    // Encrypt the column encryption key with a certificate.
    string keyPath = String.Format(@"{0}/My/{1}", certificateStoreLocation, certificateThumbprint);
    SqlColumnEncryptionCertificateStoreProvider provider = new SqlColumnEncryptionCertificateStoreProvider();
    return provider.EncryptColumnEncryptionKey(keyPath, @"RSA_OAEP", columnEncryptionKey); 
}
```


## <a name="controlling-performance-impact-of-always-encrypted"></a>Controlar el impacto en el rendimiento de Always Encrypted

Como Always Encrypted es una tecnología de cifrado del lado cliente, la mayoría de las sobrecargas de rendimiento se observan en el lado cliente y no en la base de datos. Además del costo de las operaciones de cifrado y descifrado, los demás orígenes de las sobrecargas de rendimiento en el lado cliente son:
- Ciclos de ida y vuelta adicionales a la base de datos para recuperar metadatos para los parámetros de consulta.
- Llamadas a un almacén de claves maestras de columna para tener acceso a una clave maestra de columna.

En esta sección se describen las optimizaciones integradas de rendimiento en el proveedor .NET Framework para SQL Server y la forma en que puede controlar el impacto de los dos factores anteriores en el rendimiento.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Controlar los ciclos de ida y vuelta para recuperar metadatos para los parámetros de consulta

De forma predeterminada, si Always Encrypted está habilitado para una conexión, el proveedor de datos .NET Framework para SQL Server llamará a [sys.sp_describe_parameter_encryption](../../system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) para cada consulta con parámetros, al pasar la instrucción de consulta (sin ningún valor de parámetro) a SQL Server. **sys.sp_describe_parameter_encryption** analiza la instrucción de consulta para averiguar si algún parámetro necesita cifrarse y, de ser así, devuelve la información relacionada con el cifrado que permitirá al proveedor de datos .NET Framework para SQL Server cifrar los valores de parámetro para cada uno de ellos. El comportamiento anterior garantiza un alto nivel de transparencia para la aplicación cliente: La aplicación (y el desarrollador de aplicaciones) no necesita conocer qué consultas tienen acceso a las columnas cifradas, siempre y cuando los valores que tienen como destino las columnas cifradas se pasen al proveedor de datos .NET Framework para SQL Server en objetos SqlParameter.


### <a name="query-metadata-caching"></a>Almacenamiento en caché de metadatos de consulta

En .NET Framework 4.6.2 y versiones posteriores, el proveedor de datos NET Framework Data Provider para SQL Server almacena en caché los resultados de **sys.sp_describe_parameter_encryption** para cada instrucción de consulta. Por lo tanto, si la misma instrucción de consulta se ejecuta varias veces, el controlador llama a **sys.sp_describe_parameter_encryption** solo una vez. El almacenamiento en caché de metadatos de cifrado para instrucciones de consulta disminuye notablemente el costo que tiene en el rendimiento capturar metadatos desde la base de datos. El almacenamiento en caché está habilitado de manera predeterminada. Puede deshabilitar el almacenamiento en caché de los metadatos de los parámetros estableciendo la [propiedad SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled) en "false", pero esta opción no se recomienda salvo en algunos casos muy puntuales, como el que se describe a continuación:

Considere una base de datos con dos esquemas distintos: s1 y s2. Cada esquema contiene una tabla con el mismo nombre: t. Las definiciones de las tablas s1.t y s2.t son idénticas, salvo las propiedades relacionadas con el cifrado: una columna denominada c, no se cifra en s1.t y está cifrada en s2.t. La base de datos tiene dos usuarios: u1 y u2. El esquema predeterminado del usuario u1 es s1. El esquema predeterminado del usuario u2 es s2. Una aplicación .NET abre dos conexiones con la base de datos, suplantando al usuario u1 en una conexión y al usuario u2 en la otra. La aplicación envía una consulta con un parámetro que tiene como destino la columna c a través de la conexión para el usuario u1 (la consulta no especifica el esquema, por lo que se supone que se trata del esquema predeterminado del usuario). A continuación, la aplicación envía la misma consulta a través de la conexión para el usuario u2. Si el almacenamiento en caché de metadatos de consulta está habilitado, después de la primera consulta, la memoria caché se rellenará con metadatos, lo que indica que la columna c, es decir, los destinos de parámetro de consulta, no está cifrada. Como la segunda consulta tiene exactamente la misma instrucción de consulta, se usará la información almacenada en caché. Como resultado, el controlador enviará la consulta sin cifrar el parámetro (lo que no es correcto, porque la columna de destino, s2.t.c, está cifrada), con lo que el valor del texto no cifrado del parámetro se filtra al servidor. El servidor detectará la incompatibilidad y forzará al controlador a actualizar la memoria caché, para que la aplicación reenvíe la consulta con el valor de parámetro cifrado correctamente. En tal caso, el almacenamiento en caché debe estar deshabilitado para evitar que los valores confidenciales se filtren al servidor. 




### <a name="setting-always-encrypted-at-the-query-level"></a>Configurar Always Encrypted en el nivel de consulta

Para controlar el impacto en el rendimiento a la hora de recuperar metadatos de cifrado para las consultas con parámetros, puede habilitar Always Encrypted para las consultas individuales en lugar de configurarlo para la conexión. De esta manera, puede estar seguro de que **sys.sp_describe_parameter_encryption** se invoca solo para las consultas que sabe que incluyen parámetros que tienen como destino las columnas cifradas. Pero tenga en cuenta que haciendo esto reduce la transparencia del cifrado: si cambia las propiedades de cifrado de las columnas de la base de datos, puede que necesite cambiar el código de la aplicación para alinearlo con los cambios de esquema.

> [!NOTE]
> Establecer Always Encrypted en el nivel de consulta representa pocos beneficios para el rendimiento en .NET 4.6.2 y versiones posteriores, que implementan el almacenamiento en caché de metadatos de cifrado de parámetros.

Para controlar el comportamiento de Always Encrypted con las consultas individuales, necesita usar este constructor de [SqlCommand](/dotnet/api/system.data.sqlclient.sqlcommand) y [SqlCommandColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlcommandcolumnencryptionsetting). A continuación se exponen unas directrices prácticas:
- Si la mayoría de las consultas que una aplicación cliente envía a través de una conexión de base de datos acceden a columnas cifradas:
    - Establezca la palabra clave de la cadena de conexión **Column Encryption Setting** en *Enabled*.
    - Establezca **SqlCommandColumnEncryptionSetting.Disabled** para consultas individuales que no tengan acceso a ninguna columna cifrada. Esto deshabilitará la llamada a sys.sp_describe_parameter_encryption así como un intento de descifrar cualquier valor del conjunto de resultados.
    - Establezca **SqlCommandColumnEncryptionSetting.ResultSet** para consultas individuales que no cuenten con ningún parámetro que requiera cifrado, pero que recuperen datos de columnas cifradas. Esto deshabilitará la llamada a sys.sp_describe_parameter_encryption y el cifrado de parámetros. La consulta podrá descifrar los resultados de las columnas de cifrado.
- Si la mayoría de las consultas que una aplicación cliente envía a través de una conexión de base de datos no acceden a columnas cifradas:
    - Establezca la palabra clave de la cadena de conexión **Column Encryption Setting** en **Disabled**.
    - Establezca **SqlCommandColumnEncryptionSetting.Enabled** para consultas individuales que no cuenten con ningún parámetro que se tenga que cifrar. Esto habilitará la llamada a sys.sp_describe_parameter_encryption así como el descifrado de cualquier resultado de la consulta que se recupere de las columnas cifradas.
    - Establezca **SqlCommandColumnEncryptionSetting.ResultSet** para consultas que no cuenten con ningún parámetro que requiera cifrado, pero que recuperen datos de columnas cifradas. Esto deshabilitará la llamada a sys.sp_describe_parameter_encryption y el cifrado de parámetros. La consulta podrá descifrar los resultados de las columnas de cifrado.

En el ejemplo siguiente, Always Encrypted está deshabilitado para la conexión de base de datos. La consulta, los problemas de la aplicación, tiene un parámetro que tiene como destino la columna LastName que no está cifrada. La consulta recupera datos de las columnas SSN y BirthDate que están cifradas. En este caso, no es necesario llamar a sys.sp_describe_parameter_encryption para recuperar los metadatos de cifrado. Sin embargo, el descifrado de los resultados de la consulta debe habilitarse, de forma que la aplicación pueda recibir valores de texto sin formato de las dos columnas cifradas. La configuración SqlCommandColumnEncryptionSetting.ResultSet se usa para garantizar esto.



```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
using (SqlConnection connection = new SqlConnection(connectionString))
{
    connection.Open();
    using (SqlCommand cmd = new SqlCommand(@"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName",
connection, null, SqlCommandColumnEncryptionSetting.ResultSetOnly))
    {
        SqlParameter paramLastName = cmd.CreateParameter();
        paramLastName.ParameterName = @"@LastName";
        paramLastName.DbType = DbType.String;
        paramLastName.Direction = ParameterDirection.Input;
        paramLastName.Value = "Abel";
        paramLastName.Size = 50;
        cmd.Parameters.Add(paramLastName);
        using (SqlDataReader reader = cmd.ExecuteReader())
            {
               if (reader.HasRows)
               {
                  while (reader.Read())
                  {
                     Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
                  }
               }
            }
  } 
}
```


### <a name="column-encryption-key-caching"></a>Almacenamiento en memoria caché de claves de cifrado de columnas

Para reducir el número de llamadas a un almacén de claves maestras de columna para descifrar las claves de cifrado de columnas, el proveedor de datos .NET Framework para SQL Server almacena en memoria caché las claves de cifrado de columnas de texto sin formato en la memoria. Después de recibir el valor cifrado de la clave de cifrado de columnas de los metadatos de la base de datos, el controlador primero intenta encontrar la clave de cifrado de columnas de texto sin formato, que corresponde al valor de clave cifrado. El controlador llama al almacén de claves que contiene la clave maestra de columna, solo si no puede encontrar el valor cifrado de la clave de cifrado de columnas en la memoria.caché.

> [!NOTE]
> En .NET Framework 4.6 y 4.6.1, las entradas de las claves de cifrado de columnas en la memoria caché nunca se expulsan. Esto significa que para una clave de cifrado de columnas determinada, el controlador se pone en contacto con el almacén de claves solo una vez durante la vigencia de la aplicación.

En .NET Framework 4.6.2 y versiones posteriores, las entradas de caché se expulsan después de un intervalo de período de vida configurable por motivos de seguridad. El valor de período de vida predeterminado es de 2 horas. Si tiene requisitos de seguridad más estrictos con respecto al período durante el cual se pueden mantener en caché las claves de cifrado de columna en el texto no cifrado de la aplicación, puede cambiarlos con la [propiedad SqlConnection.ColumnEncryptionKeyCacheTtl](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionkeycachettl). 


## <a name="enabling-additional-protection-for-a-compromised-sql-server"></a>Habilitar la protección adicional de un SQL Server comprometido

De manera predeterminada, el *proveedor de datos de .NET Framework para SQL Server* se basa en el sistema de base de datos (SQL Server o Azure SQL Database) para proporcionar metadatos sobre qué columnas de la base de datos están cifradas y cómo lo están. Los metadatos de cifrado permiten que el proveedor de datos .NET Framework para SQL Server cifre los parámetros de consulta y descifre los resultados de las consultas sin intervención de la aplicación, lo que disminuye considerablemente el número de cambios que se requieren en la aplicación. Sin embargo, si el proceso de SQL Server se ve comprometido y un atacante altera los metadatos que SQL Server envía al proveedor de datos .NET Framework para SQL Server, el atacante podría tener la posibilidad de robar información confidencial. En esta sección se describen las API que ayudan a proporcionar un nivel de protección adicional contra este tipo de ataque a costa de una menor transparencia. 

### <a name="forcing-parameter-encryption"></a>Forzar el cifrado de parámetros 

Antes de que el proveedor de datos .NET Framework para SQL Server envíe una consulta con parámetros a SQL Server, le indica a SQL Server (a través de una llamada a [sys.sp_describe_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md)) que analice la instrucción de consulta y proporcione información sobre los parámetros que se deben cifrar en la consulta. Una instancia de SQL Server comprometida podría confundir al proveedor de datos .NET Framework para SQL Server al enviar los metadatos que indican que el parámetro no tiene como destino una columna cifrada, a pesar de que la columna está cifrada en la base de datos. Como resultado de lo anterior, el proveedor de datos .NET Framework para SQL Server no cifraría el valor de parámetro y lo enviaría como texto no cifrado a la instancia de SQL Server comprometida.

Para evitar este tipo de ataque, una aplicación puede establecer la [propiedad SqlParameter.ForceColumnEncryption](/dotnet/api/system.data.sqlclient.sqlparameter.forcecolumnencryption) del parámetro en true. Esto hará que el proveedor de datos .NET Framework para SQL Server genere una excepción si los metadatos que recibió del servidor indican que no es necesario que el parámetro esté cifrado.

A pesar de que usar la **propiedad SqlParameter.ForceColumnEncryption** permite mejorar la seguridad, también disminuye la transparencia del cifrado en la aplicación cliente. Si actualiza el esquema de base de datos para cambiar el conjunto de columnas cifradas, es posible que también tenga que hacer cambios en la aplicación.

El ejemplo de código siguiente muestra cómo usar la **propiedad SqlParameter.ForceColumnEncryption** para evitar que los números del seguro social se envíen en texto no cifrado a la base de datos. 



```cs
SqlCommand cmd = _sqlconn.CreateCommand(); 

// Use parameterized queries to access Always Encrypted data. 
 
cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = @SSN;"; 

SqlParameter paramSSN = cmd.CreateParameter(); 
paramSSN.ParameterName = @"@SSN"; 
paramSSN.DbType = DbType.AnsiStringFixedLength; 
paramSSN.Direction = ParameterDirection.Input; 
paramSSN.Value = ssn; 
paramSSN.Size = 11; 
paramSSN.ForceColumnEncryption = true; 
cmd.Parameters.Add(paramSSN); 

SqlDataReader reader = cmd.ExecuteReader();
```
 

### <a name="configuring-trusted-column-master-key-paths"></a>Configurar rutas de acceso a claves maestras de columna de confianza 

Los metadatos de cifrado que devuelve SQL Server para los parámetros de consulta que tienen como destino columnas cifradas y para los resultados que se recuperan de las columnas cifradas incluyen la ruta de acceso a clave de la clave maestra de columna que identifica el almacén de claves y la ubicación de la clave en el mismo. Si la instancia de SQL Server se ve comprometida, podría enviar la ruta de acceso a clave si dirige el proveedor de datos .NET Framework para SQL Server a la ubicación que un atacante controla. Esto puede generar la filtración de credenciales del almacén de claves, en caso de que el almacén de claves requiera la autenticación de la aplicación. 

Para evitar dichos ataques, la aplicación puede especificar la lista de rutas de acceso a claves de confianza en un servidor determinado a través de la [propiedad SqlConnection.ColumnEncryptionTrustedMasterKeyPaths](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths). Si el proveedor de datos .NET Framework para SQL recibe una ruta de acceso a clave que no corresponde a la lista de rutas de acceso a claves de confianza, generará una excepción. 

A pesar de que el hecho de establecer rutas de acceso a claves de confianza mejora la seguridad de la aplicación, deberá cambiar el código o la configuración de la aplicación cada vez que rote la clave maestra de columna (es decir, cada vez que cambie la ruta de acceso a la clave maestra de columna). 

En el ejemplo siguiente se ilustra cómo configurar las rutas de acceso a claves maestras de columna de confianza:


```cs
// Configure trusted key paths to protect against fake key paths sent by a compromised SQL Server instance 
// First, create a list of trusted key paths for your server 
List<string> trustedKeyPathList = new List<string>(); 
trustedKeyPathList.Add("CurrentUser/my/425CFBB9DDDD081BB0061534CE6AB06CB5283F5Ea"); 

// Register the trusted key path list for your server 

SqlConnection.ColumnEncryptionTrustedMasterKeyPaths.Add(serverName, trustedKeyPathList);
```
 



## <a name="copying-encrypted-data-using-sqlbulkcopy"></a>Copiar datos cifrados con SqlBulkCopy

Con SqlBulkCopy, puede copiar datos que ya están cifrados y almacenados en una tabla a otra, sin descifrarlos. Para ello:

- Asegúrese de que la configuración de cifrado de la tabla de destino es idéntica a la configuración de la tabla de origen. En particular, ambas tablas debe tener las mismas columnas cifradas y estas deben cifrarse mediante los mismos tipos de cifrado y las mismas claves de cifrado. Nota: Si alguna de las columnas de destino se cifra de una manera diferente a la de su columna de origen correspondiente, no podrá descifrar los datos de la tabla de destino después de la operación de copia. Los datos estarán dañados.
- Configure ambas conexiones de base de datos, a la tabla de origen y a la tabla de destino, sin tener habilitado Always Encrypted. 
- Establezca la opción AllowEncryptedValueModifications (vea [SqlBulkCopyOptions](/dotnet/api/system.data.sqlclient.sqlbulkcopyoptions)). Nota: Tenga cuidado al especificar AllowEncryptedValueModifications ya que puede provocar daños en la base de datos ya que el proveedor de datos .NET Framework para SQL Server no comprueba si los datos están realmente cifrados, o si se han cifrado correctamente mediante la misma clave, algoritmo y tipo de cifrado que la columna de destino.

La opción AllowEncryptedValueModifications está disponible en .NET Framework 4.6.1 y en versiones posteriores.

Aquí se muestra un ejemplo que copia datos de una tabla a otra. Se supone que las columnas SSN y BirthDate están cifradas.
        

```cs
static public void CopyTablesUsingBulk(string sourceTable, string targetTable)
{
   string sourceConnectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
   string targetConnectionString = "Data Source= server64; Initial Catalog=Clinic; Integrated Security=true";
   using (SqlConnection connSource = new SqlConnection(sourceConnectionString))
   {
      connSource.Open();
      using (SqlCommand cmd = new SqlCommand(string.Format("SELECT [PatientID], [SSN], [FirstName], [LastName], [BirthDate] FROM {0}", sourceTable), connSource))
      {
         using (SqlDataReader reader = cmd.ExecuteReader())
         {
            SqlBulkCopy copy = new SqlBulkCopy(targetConnectionString, SqlBulkCopyOptions.KeepIdentity | SqlBulkCopyOptions.AllowEncryptedValueModifications);
            copy.EnableStreaming = true;
            copy.DestinationTableName = targetTable;
            copy.WriteToServer(reader);
         }
      }
}
```


## <a name="always-encrypted-api-reference"></a>Referencia de la API de Always Encrypted

**Espacio de nombres:** [System.Data.SqlClient](/dotnet/api/system.data.sqlclient)

**Ensamblado:** System.Data (en System.Data.dll)




|Nombre|Descripción|Incluido en la versión de .NET
|:---|:---|:---
|[Clase SqlColumnEncryptionCertificateStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider)|Un proveedor de almacén de claves para el almacén de certificados de Windows.|  4.6
|[Clase SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider)|Un proveedor de almacén de claves para Microsoft Cryptography API: Next Generation (CNG).|  4.6.1
|[Clase SqlColumnEncryptionCspProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncspprovider)|Un proveedor de almacén de claves para los proveedores de servicio de cifrado (CSP) basados en CAPI de Microsoft.|4.6.1  
|[SqlColumnEncryptionKeyStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptionkeystoreprovider)|Clase base de los proveedores de almacén de claves.|  4.6
|[Enumeración SqlCommandColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlcommandcolumnencryptionsetting)|Opciones con las que habilitar el cifrado y el descifrado para una conexión de base de datos.|4.6  
|[Enumeración SqlConnectionColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlconnectioncolumnencryptionsetting)|Opciones con las que controlar el comportamiento de Always Encrypted para consultas individuales.|4.6  
| [SqlConnectionStringBuilder.ColumnEncryptionSetting Property](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting)|Obtiene y establece Always Encrypted en la cadena de conexión.|4.6|
| [propiedad SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled) | Habilita y deshabilita el almacenamiento en caché de los metadatos de consulta de cifrado. | 4.6.2
| [propiedad SqlConnection.ColumnEncryptionKeyCacheTtl](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionkeycachettl) | Obtiene y establece el período de vida de las entradas en la caché de clave de cifrado de columna. | 4.6.2
|[propiedad SqlConnection.ColumnEncryptionTrustedMasterKeyPaths](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths)|Permite establecer una lista de rutas de acceso de confianza a la clave para un servidor de base de datos. Si durante el procesamiento de una consulta de aplicación, el controlador recibe una ruta de acceso de clave que no se encuentre en la lista, la consulta generará error. Esta propiedad proporciona protección adicional contra los ataques de seguridad que implican un servidor SQL Server comprometido que proporciona rutas de acceso a la clave falsas que pueden provocar la divulgación de las credenciales del almacén de claves.|  4.6
|[Método SqlConnection.RegisterColumnEncryptionKeyStoreProviders](/dotnet/api/system.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders)|Permite registrar los proveedores del almacén de claves personalizados. Se trata de un diccionario que asigna nombres de proveedor del almacén de claves para las implementaciones de proveedor del almacén de claves.|  4.6
|[Constructor SqlCommand (cadena, SqlConnection, SqlTransaction y SqlCommandColumnEncryptionSetting)](https://msdn.microsoft.com/library/dn956511\(v=vs.110\).aspx)|Permite controlar el comportamiento de Always Encrypted para consultas individuales.|  4.6
|[propiedad SqlParameter.ForceColumnEncryption](/dotnet/api/system.data.sqlclient.sqlparameter.forcecolumnencryption)|Aplica el cifrado de un parámetro. Si SQL Server indica al controlador que no hace falta cifrar el parámetro, la consulta que utilice este último generará un error. Esta propiedad confiere una protección adicional frente a ataques contra la seguridad que utilice un servidor SQL Server comprometido que proporcione metadatos de cifrado incorrectos al cliente, lo cual podría provocar la divulgación de datos.|4.6  
|Nueva palabra clave de [cadena de conexión](/dotnet/api/system.data.sqlclient.sqlconnection.connectionstring) : `Column Encryption Setting=enabled`|Habilita o deshabilita la funcionalidad de Always Encrypted para la conexión.| 4.6 
  

## <a name="see-also"></a>Consulte también

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Blog de Always Encrypted](/archive/blogs/sqlsecurity/getting-started-with-always-encrypted)
- [Tutorial de SQL Database: Protección de datos confidenciales con Always Encrypted](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
