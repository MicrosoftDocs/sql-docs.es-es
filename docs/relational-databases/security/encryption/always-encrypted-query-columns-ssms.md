---
title: Consulta de columnas mediante Always Encrypted con SQL Server Management Studio | Microsoft Docs
description: Obtenga información sobre cómo consultar columnas en Always Encrypted mediante SQL Server Management Studio. Recupere valores de texto cifrado o texto almacenados en columnas cifradas.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ff7924bf1262ee29977d16548b6548c112bc8b5d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345451"
---
# <a name="query-columns-using-always-encrypted-with-sql-server-management-studio"></a>Consulta de columnas mediante Always Encrypted con SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

En este artículo se describe cómo consultar columnas, cifradas con [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) mediante [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md). Con SSMS, puede:
- Recuperar los valores de texto cifrado almacenados en columnas cifradas. 
- Recuperar los valores de texto no cifrado almacenados en columnas cifradas.   
- Enviar valores de texto no cifrado dirigidos a columnas cifradas (por ejemplo, en instrucciones `INSERT` o `UPDATE` y como parámetros de búsqueda de las cláusulas `WHERE` en las instrucciones `SELECT`).

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>Recuperación de los valores de texto cifrado almacenados en columnas cifradas    
La ejecución de consultas SELECT que recuperan texto cifrado de los datos almacenados en las columnas cifradas (sin descifrar los datos) no requiere tener acceso a las claves maestras de columna que protegen los datos. Para recuperar valores desde una columna cifrada como texto cifrado en SSMS:

1. Asegúrese de que tiene acceso a los metadatos sobre las claves que protegen las columnas en las que está ejecutando la consulta. Aunque no es necesario tener acceso a las claves maestras de columna reales, sí se necesitan permisos de nivel de base de datos para ver los objetos de metadatos de clave maestra de columna y de clave de cifrado de columna en la base de datos. Para obtener más información, consulte [Permisos para consultar columnas cifradas](#permissions-for-querying-encrypted-columns) más abajo.
1. Asegúrese de que Always Encrypted esté deshabilitado para la conexión de base de datos de la ventana del Editor de consultas, desde donde ejecuta una consulta `SELECT` que recupere los valores del texto cifrado. Consulte [Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos](#en-dis) más adelante.     
1. Ejecute la consulta `SELECT`. Todos los datos que se recuperen de las columnas cifradas se devolverán como valores binarios (cifrados).   

### <a name="example"></a>Ejemplo
Suponiendo que `SSN` es una columna cifrada de la tabla `Patients` , la consulta que aparece a continuación recuperará los valores binarios de texto cifrado, si Always Encrypted está deshabilitado para la conexión de base de datos.   

![Captura de pantalla de la consulta SELECT [SSN] FROM [dbo].[Patients] y de los resultados de dicha consulta, que se muestran como valores binarios de texto cifrado.](../../../relational-databases/security/encryption/media/always-encrypted-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>Recuperación de los valores de texto no cifrado almacenados en columnas cifradas    
Para recuperar valores desde una columna cifrada como texto no cifrado (para descifrar los valores):   
1. Asegúrese de que tiene acceso a las claves maestras de columna y a los metadatos sobre las claves que protegen las columnas en las que está ejecutando la consulta. Para obtener más información, consulte [Permisos para consultar columnas cifradas](#permissions-for-querying-encrypted-columns) más abajo.
1.  Asegúrese de que Always Encrypted esté habilitado para la conexión de base de datos de la ventana del Editor de consultas, desde donde ejecuta una consulta `SELECT` que recupere y descifre los datos. Esto indicará al proveedor de datos .NET Framework para SQL Server (que SSMS usa) que descifre las columnas cifradas en el conjunto de resultados de consulta. Consulte [Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos](#en-dis) más adelante.
1.  Ejecute la consulta `SELECT`. Los datos que se recuperen de las columnas cifradas se devolverán como valores de texto no cifrado de los tipos de datos originales.
 
### <a name="example"></a>Ejemplo
Suponiendo que SSN es una columna `char(11)` cifrada de la tabla `Patients` , la columna que se muestra a continuación devolverá valores de texto no cifrado si Always Encrypted está habilitado para la conexión de base de datos y si tiene acceso a la clave maestra de columna configurada para la columna `SSN` .   

![Captura de pantalla de la consulta SELECT [SSN] FROM [Clinic].[dbo].[Patients] y de los resultados de dicha consulta, que se muestran como valores de texto sin formato.](../../../relational-databases/security/encryption/media/always-encrypted-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>Envío de valores de texto no cifrado con columnas cifradas como destino       
Para ejecutar una consulta que envía un valor con una columna cifrada como destino, por ejemplo, una consulta que inserta o actualiza un valor almacenado, o filtra según él, en una columna cifrada, haga lo siguiente:

1. Asegúrese de que tiene acceso a las claves maestras de columna y a los metadatos para las claves que protegen las columnas en las que se ejecuta la consulta. Para obtener más información, consulte [Permisos para consultar columnas cifradas](#permissions-for-querying-encrypted-columns) más abajo.
1.  Asegúrese de que Always Encrypted esté habilitado para la conexión de base de datos de la ventana del Editor de consultas, desde donde ejecuta una consulta `SELECT` que recupere y descifre los datos. Esto indicará al proveedor de datos .NET Framework para SQL Server (que SSMS usa) que descifre las columnas cifradas en el conjunto de resultados de consulta. Consulte [Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos](#en-dis) más adelante.
1. Asegúrese de que la parametrización de Always Encrypted esté habilitada para la ventana del Editor de consultas. (Requiere al menos la versión 17.0 de SSMS) Declare una variable Transact-SQL e inicialícela con un valor que desea enviar (insertar, actualizar o por el que filtrar) a la base de datos. Consulte [Parametrización de Always Encrypted](#param) que aparece más adelante para detalles.

1. Ejecute la consulta; para ello, envíe el valor de la variable Transact-SQL a la base de datos. SSMS convertirá la variable en un parámetro de consulta y cifrará su valor antes de enviarlo a la base de datos.   

### <a name="example"></a>Ejemplo
Suponiendo que `SSN` es una columna `char(11)` cifrada de la tabla `Patients` , el script a continuación intentará encontrar una fila que contenga `'795-73-9838'` en la columna SSN y devolverá el valor de la columna `LastName` , siempre que Always Encrypted esté habilitado para la conexión de base de datos, que Parametrización para Always Encrypted esté habilitado para la ventana del Editor de consultas y que usted tenga acceso a la clave maestra de columna configurada para la columna `SSN` .   

![Captura de pantalla de la consulta DECLARE @SSN CHAR(11) = '795-73-9838' SELECT [LastName] FROM [dbo].[Patients] WHERE [SSN] = @SSN y de los resultados de dicha consulta.](../../../relational-databases/security/encryption/media/always-encrypted-patients.png)

## <a name="permissions-for-querying-encrypted-columns"></a>Permisos para consultar columnas cifradas

Para ejecutar cualquier consulta contra las columnas cifradas, incluidas las consultas que recuperan datos en texto cifrado, necesita los permisos `VIEW ANY COLUMN MASTER KEY DEFINITION` y `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` pen la base de datos.

Además de los permisos anteriores, para descifrar cualquier resultado de consulta o para cifrar cualquier parámetro de consulta (generados por la parametrización de las variables Transact-SQL), también debe tener acceso a la clave maestra de columna que protege las columnas de destino:

- **Almacén de certificados: equipo local**: debe tener acceso `Read` al certificado que se usa en una clave maestra de columna, o bien puede ser el administrador del equipo.   
- **Azure Key Vault** : necesita los permisos `get`, `unwrapKey` y `verify` en el almacén que contiene la clave maestra de columna.
- **Proveedor del almacén de claves (KSP)** : el permiso y las credenciales necesarios que se le podrían solicitar al usar una clave o un almacén de claves dependen de la configuración del almacén o del KSP.   
- **Proveedor de servicios criptográficos (CSP)** : el permiso y las credenciales necesarios que se le podrían solicitar al usar una clave o un almacén de claves dependen del almacén y de la configuración del proveedor de servicios criptográficos (CSP).

Para obtener más información, vea [Create and Store Column Master Keys (Always Encrypted) (Crear y almacenar claves maestras de columna (Always Encrypted))](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a><a name="en-dis"></a> Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos   
Cuando se conecta a una base de datos en SSMS, puede habilitar o deshabilitar Always Encrypted para la conexión de base de datos. De manera predeterminada, Always Encrypted está deshabilitado. 

Habilitar Always Encrypted para una conexión de base de datos indica al proveedor de datos .NET Framework para SQL Server, que SQL Server Management Studio usa, que intente realizar lo siguiente de manera transparente:   
-   Descifrar los valores que se recuperan de las columnas cifradas y se devuelven en los resultados de la consulta.   
-   Cifrar los valores de las variables Transact-SQL parametrizadas que tienen como destino las columnas de base de datos cifradas.   

Si no habilita Always Encrypted para una conexión, el proveedor de datos de .NET Framework para SQL Server, que SSMS utiliza, no intentará cifrar los parámetros de consulta ni descifrar los resultados.

Puede habilitar o deshabilitar Always Encrypted cuando cree una conexión nueva o cambie una conexión existente mediante el cuadro de diálogo **Conectar a servidor**. 

Para habilitar (deshabilitar) Always Encrypted:
1. Abra el cuadro de diálogo **Conectar a servidor** (consulte [Conectarse a una instancia de SQL Server](../../../ssms/quickstarts/ssms-connect-query-sql-server.md#connect-to-a-sql-server-instance) para obtener más información).
1. Haga clic en **Opciones >>** .
1. Si utiliza SSMS 18 o una versión más reciente:
    1. Seleccione la pestaña **Always Encrypted**.
    1. Para habilitar Always Encrypted, seleccione **Habilitar Always Encrypted (cifrado de columna)** . Para deshabilitar Always Encrypted, asegúrese de que **Habilitar Always Encrypted (cifrado de columna)** no esté seleccionado.
1. Si usa SSMS 17 o anterior:
    1. Seleccione la pestaña **Propiedades adicionales**.
    1. Para habilitar Always Encrypted, escriba `Column Encryption Setting = Enabled`. Para deshabilitar Always Encrypted, especifique `Column Encryption Setting = Disabled` o quite la configuración del **valor de cifrado de columnas** de la pestaña **Propiedades adicionales** (su valor predeterminado es **Deshabilitado**).   
 1. Haga clic en **Conectar**.

Para ejecutar instrucciones que aprovechen un enclave seguro del lado servidor cuando se usa [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md), debe especificar una dirección URL de atestación de enclave, además de habilitar Always Encrypted para la conexión. Para obtener información detallada, vea [Requisitos previos para ejecutar instrucciones T-SQL con enclaves en SSMS](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-t-sql-statements-using-enclaves-in-ssms).

> [!TIP]
> Para alternar entre Always Encrypted habilitado y deshabilitado para una ventana existente del Editor de consultas, haga lo siguiente:   
> 1.    Haga clic con el botón derecho en cualquier parte de la ventana del Editor de consultas.
> 2.    Seleccione **Conexión** > **Cambiar conexión...** Se abrirá el cuadro de diálogo **Conectar a servidor** para la conexión actual de la ventana del editor de consultas. 
> 2.    Habilite o deshabilite Always Encrypted siguiendo los pasos anteriores y haga clic en **Conectar**.  
   
## <a name="parameterization-for-always-encrypted"></a><a name="param"></a>Parametrización de Always Encrypted   
 
Parametrización de Always Encrypted es una característica de SQL Server Management Studio que convierte automáticamente las variables Transact-SQL en parámetros de consulta (instancias de [SqlParameter Class](/dotnet/api/system.data.sqlclient.sqlparameter)). (Requiere al menos la versión 17.0 de SSMS) Esto permite que el proveedor de datos .NET Framework para SQL Server subyacente detecte datos con columnas cifradas como destino y cifre dichos datos antes de enviarlos a la base de datos. 
  
Sin la parametrización, el proveedor de datos .NET Framework pasa cada instrucción que usted crea en el Editor de consultas como una consulta no parametrizada. Si la consulta contiene literales o variables Transact-SQL con columnas cifradas como destino, el proveedor de datos .NET Framework para SQL Server no podrá detectarlas ni cifrarlas antes de enviar la consulta a la base de datos. Como resultado, la consulta presentará un error debido a un error de coincidencia de tipos (entre la variable Transact-SQL literal de texto no cifrado y la columna cifrada). Por ejemplo, la consulta siguiente presentará un error sin parametrización, suponiendo que la columna `SSN` está cifrada.   

```sql
DECLARE @SSN NCHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>Habilitación y deshabilitación de parametrización de Always Encrypted

Parametrización de Always Encrypted está deshabilitada de manera predeterminada.

Para habilitar/deshabilitar parametrización de Always Encrypted para la ventana actual del Editor de consultas, haga lo siguiente:

1. Seleccione **Consulta** en el menú principal.
2. Seleccione **Opciones de consulta…** .
3. Vaya a **Ejecución** > **Avanzadas**.
4. Seleccione o anule la selección de **Habilitar parametrización de Always Encrypted**.
5. Haga clic en **OK**.

Para habilitar/deshabilitar parametrización de Always Encrypted para ventanas futuras del Editor de consultas, haga lo siguiente:

1. Seleccione **Herramientas** en el menú principal.
2. Seleccione **Opciones...** .
3. Vaya a **Ejecución de consulta** > **SQL Server** > **Avanzadas**.
4. Seleccione o anule la selección de **Habilitar parametrización de Always Encrypted**.
5. Haga clic en **OK**.

Si ejecuta una consulta en una ventana del Editor de consultas que usa una conexión de base de datos con Always Encrypted habilitado, pero la parametrización no está habilitada para la ventana del Editor de consultas, se le pedirá que la habilite.

> [!NOTE]
> Parametrización de Always Encrypted funciona solo en las ventanas del Editor de consultas que usan conexiones de base de datos con Always Encrypted habilitadas (consulte [Habilitación y deshabilitación de parametrización de Always Encrypted](#enabling-and-disabling-parameterization-for-always-encrypted)). Ninguna variable Transact-SQL será parametrizada si la ventana del Editor de consultas usa una conexión de base de datos sin tener habilitado Always Encrypted.

### <a name="how-parameterization-for-always-encrypted-works"></a>Funcionamiento de parametrización de Always Encrypted

Si la parametrización para Always Encrypted y el comportamiento de Always Encrypted en la conexión de base de datos están habilitados para una ventana del Editor de consultas, SQL Server Management Studio intentará parametrizar las variables Transact-SQL que cumplen con las condiciones de requisitos previos siguientes:

- Se declaran e inicializan en la misma instrucción (inicialización alineada). Las variables declaradas que usan instrucciones `SET` independientes no se parametrizarán.
- Se inicializan con un solo literal. Las variables que se inicializan con expresiones que incluyen cualquier operador o función no se parametrizarán.

A continuación, aparecen ejemplos de variables que SQL Server Management Studio parametrizará.

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Y, a continuación, algunos ejemplos de variables que SQL Server Management Studio no intentará parametrizar:

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
Para que un intento de parametrización se realice correctamente:   
- El tipo de literal que se use para la inicialización de la variable que se parametrizará debe coincidir con el tipo de la declaración de variable.   
- Si el tipo declarado de la variable es un tipo de fecha o de hora, la variable se debe inicializar mediante una cadena con uno de los siguientes [formatos compatibles con ISO 8601](../../../t-sql/functions/cast-and-convert-transact-sql.md#date-and-time-styles).    

Estos son ejemplos de declaraciones de variable Transact-SQL que generarán errores de parametrización:   
```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```
SQL Server Management Studio usa IntelliSense para informarle las variables que se pueden parametrizar correctamente y cuáles son los intentos de parametrización que presentan error (y por qué).   

Una declaración de una variable que se puede parametrizar correctamente está marcada con un carácter de subrayado de advertencia en el Editor de consultas. Si mantiene el puntero sobre una instrucción de declaración que se ha marcado con un carácter de subrayado de advertencia, verá los resultados del proceso de parametrización, incluidos los valores de las propiedades clave del objeto [SqlParameter](/dotnet/api/system.data.sqlclient.sqlparameter) resultante (al que se asigna la variable): [SqlDbType](/dotnet/api/system.data.sqlclient.sqlparameter.sqldbtype), [Size](/dotnet/api/system.data.sqlclient.sqlparameter.size), [Precision](/dotnet/api/system.data.sqlclient.sqlparameter.precision), [Scale](/dotnet/api/system.data.sqlclient.sqlparameter.scale), [SqlValue](/dotnet/api/system.data.sqlclient.sqlparameter.sqlvalue). También puede ver la lista completa de todas las variables que se parametrizaron correctamente en la pestaña **Advertencia** de la vista **Lista de errores** . Para abrir la vista **Lista de errores** , seleccione **Vista** en el menú principal y, luego, seleccione **Lista de errores**.    

Si SQL Server Management Studio intentó parametrizar una variable, pero la parametrización presentó un error, la declaración de la variable se marcará con un carácter de subrayado de error. Si mantiene el puntero sobre una instrucción de declaración que se marcó con un carácter de subrayado de error, recibirá los resultados del error. También podrá ver la lista completa de errores de parametrización de todas las variables en la pestaña **Error** de la vista **Lista de errores** . Para abrir la vista **Lista de errores** , seleccione **Vista** en el menú principal y, luego, seleccione **Lista de errores**.   

La captura de pantalla siguiente muestra un ejemplo de 6 declaraciones de variable. SQL Server Management Studio parametrizó correctamente las 3 primeras variables. Las últimas tres variables no cumplieron las condiciones requeridas para la parametrización y, por tanto, SQL Server Management Studio no intentó parametrizarlas (sus declaraciones no están marcadas de ninguna manera).

![Captura de pantalla en la que se muestran seis declaraciones de variables de ejemplo, tres parametrizadas correctamente y tres con errores, así como los mensajes de advertencia asociados.](../../../relational-databases/security/encryption/media/always-encrypted-parameter-warnings.png)
 
Otro ejemplo que aparece a continuación muestra 2 variables que cumplen con las condiciones requeridas para la parametrización, pero el intento de parametrización presenta un error porque las variables se inicializaron de manera incorrecta.    
 
![Captura de pantalla en la que se muestra un ejemplo de dos declaraciones de variables que, en última instancia, producen un error y el mensaje de error asociado.](../../../relational-databases/security/encryption/media/always-encrypted-error.png)
 
> [!NOTE]
> Como Always Encrypted admite un subconjunto limitado de conversiones de tipo, en muchos casos se requiere que ese tipo de datos de una variable Transact-SQL sea el mismo tipo de la columna de base de datos de destino. Por ejemplo, suponiendo que el tipo de la columna `SSN` de la tabla `Patients` sea `char(11)`, la consulta siguiente presentará un error, debido a que el tipo de la variable `@SSN` , que es `nchar(11)`, no coincide con el tipo de la columna.   

```sql
DECLARE @SSN nchar(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN;
```

```output
Msg 402, Level 16, State 2, Line 5   
The data types char(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') collation_name = 'Latin1_General_BIN2' 
and nchar(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') are incompatible in the equal to operator.
```

> [!NOTE]
> Sin parametrización, la columna completa, incluidas las conversiones de tipo, se procesan dentro de SQL Server/Azure SQL Database. Con la parametrización habilitada, .NET Framework ejecuta algunas conversiones de tipo dentro de SQL Server Management Studio. Debido a las diferencias entre el sistema de tipo de .NET Framework y el sistema de tipo de SQL Server (por ejemplo, una precisión distinta de algunos tipos, como float), una consulta que se ejecuta con parametrización habilitada puede generar resultados distintos a los de la consulta ejecutada sin la parametrización habilitada. 

## <a name="next-steps"></a>Pasos siguientes
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)


## <a name="see-also"></a>Consulte también
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)