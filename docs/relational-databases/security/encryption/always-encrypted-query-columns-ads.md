---
description: Consulta de columnas mediante Always Encrypted con Azure Data Studio
title: Consulta de columnas mediante Always Encrypted con Azure Data Studio | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 826832a9653c0f3966a2919f7e32beaa938ae6d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345460"
---
# <a name="query-columns-using-always-encrypted-with-azure-data-studio"></a>Consulta de columnas mediante Always Encrypted con Azure Data Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

En este artículo se describe cómo consultar columnas, cifradas con [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) mediante [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md). Con Azure Data Studio, se podrá realizar lo siguiente:
- Recuperar los valores de texto cifrado almacenados en columnas cifradas. 
- Recuperar los valores de texto no cifrado almacenados en columnas cifradas.  
- Enviar valores de texto no cifrado dirigidos a columnas cifradas (por ejemplo, en instrucciones `INSERT` o `UPDATE` y como parámetros de búsqueda de las cláusulas `WHERE` en las instrucciones `SELECT`). 

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>Recuperación de los valores de texto cifrado almacenados en columnas cifradas    
En esta sección se describe cómo recuperar los datos almacenados en columnas cifradas como texto cifrado.

### <a name="steps"></a>Pasos
1. Asegúrese de que Always Encrypted esté deshabilitado para la conexión de base de datos de la ventana de consulta, desde donde se ejecutará una consulta `SELECT` que recupere los valores del texto cifrado. Consulte [Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos](#enabling-and-disabling-always-encrypted-for-a-database-connection) más adelante.     
1. Ejecute la consulta `SELECT`. Todos los datos que se recuperen de las columnas cifradas se devolverán como valores binarios (cifrados).   

### <a name="example"></a>Ejemplo
Suponiendo que `SSN` es una columna cifrada de la tabla `Patients` , la consulta que aparece a continuación recuperará los valores binarios de texto cifrado, si Always Encrypted está deshabilitado para la conexión de base de datos.   

![Captura de pantalla de la consulta SELECT * FROM [dbo].[Patients] y de los resultados de dicha consulta, que se muestran como valores binarios de texto cifrado.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>Recuperación de los valores de texto no cifrado almacenados en columnas cifradas    
En esta sección se describe cómo recuperar los datos almacenados en columnas cifradas como texto cifrado.

### <a name="prerequisites"></a>Requisitos previos
- Azure Data Studio, versión 17.1 o posterior.
- Necesita tener acceso a las claves maestras de columna y a los metadatos sobre las claves que protegen las columnas en las que está ejecutando la consulta. Para obtener más información, consulte [Permisos para consultar columnas cifradas](#permissions-for-querying-encrypted-columns) más abajo.
- Las claves maestras de columna deben almacenarse en Azure Key Vault o en el almacén de certificados de Windows. Azure Data Studio no admite otros almacenes de claves.

### <a name="steps"></a>Pasos
1.  Habilite Always Encrypted para la conexión de base de datos de la ventana de consulta, desde donde se ejecutará una consulta `SELECT` que recupere y descifre los datos. Esto indicará al [proveedor de datos Microsoft .NET para SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) (que usa Azure Data Studio) que descifre las columnas cifradas en el conjunto de resultados de la consulta. Consulte [Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos](#enabling-and-disabling-always-encrypted-for-a-database-connection) más adelante.
1.  Ejecute la consulta `SELECT`. Los datos que se recuperen de las columnas cifradas se devolverán como valores de texto no cifrado de los tipos de datos originales.
 
### <a name="example"></a>Ejemplo
Suponiendo que SSN es una columna cifrada de la tabla `Patients`, la consulta que se muestra a continuación devolverá valores de texto no cifrado si Always Encrypted está habilitado para la conexión de base de datos y si tiene acceso a la clave maestra de columna configurada para la columna `SSN`.   

![Captura de pantalla de la consulta SELECT * FROM [dbo].[Patients] y de los resultados de dicha consulta, que se muestran como valores de texto sin formato.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>Envío de valores de texto no cifrado con columnas cifradas como destino       
En esta sección se describe cómo ejecutar una consulta que envía valores que tienen como destino una columna cifrada. Por ejemplo, una consulta que inserta un valor almacenado en una columna cifrada, lo actualiza o filtra por este valor:

### <a name="prerequisites"></a>Requisitos previos
- Azure Data Studio, versión 18.1 o posterior.
- Necesita tener acceso a las claves maestras de columna y a los metadatos sobre las claves que protegen las columnas en las que está ejecutando la consulta. Para obtener más información, consulte [Permisos para consultar columnas cifradas](#permissions-for-querying-encrypted-columns) más abajo.
- Las claves maestras de columna deben almacenarse en Azure Key Vault o en el almacén de certificados de Windows. Azure Data Studio no admite otros almacenes de claves.

### <a name="steps"></a>Pasos
1. Habilite Always Encrypted para la conexión de base de datos de la ventana de consulta, desde donde se ejecutará una consulta `SELECT` que recupere y descifre los datos. Esto indicará al [proveedor de datos Microsoft .NET para SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) (que usa Azure Data Studio) que cifre los parámetros de consulta que tienen como destino las columnas cifradas y descifre los resultados recuperados de las columnas cifradas. Consulte [Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos](#enabling-and-disabling-always-encrypted-for-a-database-connection) más adelante. 
1. Habilite la parametrización de Always Encrypted para la ventana de consulta. Consulte [Parametrización de Always Encrypted](#parameterization-for-always-encrypted) que aparece más adelante para detalles.
1. Declare una variable Transact-SQL e inicialícela con un valor que quiera enviar (insertar, actualizar o por el que filtrar) a la base de datos. 
1. Ejecute la consulta; para ello, envíe el valor de la variable Transact-SQL a la base de datos. Azure Data Studio convertirá la variable en un parámetro de consulta y cifrará su valor antes de enviarlo a la base de datos.   

### <a name="example"></a>Ejemplo
Suponiendo que `SSN` sea una columna de `char(11)` cifrada en la tabla `Patients`, el script siguiente intentará buscar una fila que contenga `'795-73-9838'` en la columna SSN. Los resultados se devuelven si Always Encrypted está habilitado para la conexión de base de datos, la parametrización de Always Encrypted está habilitada para la ventana de consulta y tiene acceso a la clave maestra de columna configurada para la columna `SSN`.   

![Captura de pantalla de la consulta DECLARE @SSN char(11) = '795-73-9838' SELECT * FROM [dbo].[Patients] WHERE [SSN] = @SSN y de los resultados de dicha consulta.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-parameters.png)

## <a name="permissions-for-querying-encrypted-columns"></a>Permisos para consultar columnas cifradas

Para ejecutar en columnas cifradas, incluidas las consultas que recuperan datos en texto cifrado, necesita tener los permisos **VIEW ANY COLUMN MASTER KEY DEFINITION** y **VIEW ANY COLUMN ENCRYPTION KEY DEFINITION** en la base de datos.

Además de los permisos anteriores, para descifrar cualquier resultado de consulta o para cifrar cualquier parámetro de consulta (generados por la parametrización de las variables Transact-SQL), también debe tener acceso a la clave maestra de columna que protege las columnas de destino:

- **Almacén de certificados, equipo local:** debe tener acceso de **Lectura** al certificado que se usa como una clave maestra de columna, o bien ser el administrador del equipo.   
- **Azure Key Vault:** necesita los permisos **get**, **unwrapKey** y **verify** en el almacén de claves que contiene la clave maestra de columna.

Para obtener más información, vea [Create and Store Column Master Keys (Always Encrypted) (Crear y almacenar claves maestras de columna (Always Encrypted))](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a>Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos   

Cuando se conecta a una base de datos en Azure Data Studio, puede habilitar o deshabilitar Always Encrypted para la conexión de base de datos. De manera predeterminada, Always Encrypted está deshabilitado. 

Habilitar Always Encrypted para una conexión de base de datos indica al [proveedor de datos Microsoft .NET para SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md), que usa Azure Data Studio, que intente realizar lo siguiente de manera transparente:   
-   Descifrar los valores recuperados de las columnas cifradas y devueltos en los resultados de la consulta.   
-   Cifrar los valores de las variables Transact-SQL parametrizadas que tienen como destino las columnas de base de datos cifradas.   

Si no habilita Always Encrypted para una conexión, el proveedor de datos Microsoft .NET para SQL Server no intentará cifrar los parámetros de consulta ni descifrar los resultados.

Puede habilitar o deshabilitar Always Encrypted al conectarse a una base de datos. Para obtener información general sobre cómo conectarse a una base de datos, vea lo siguiente:
- [Inicio rápido: Conectarse y consultar SQL Server con Azure Data Studio](../../../azure-data-studio/quickstart-sql-server.md)
- [Inicio rápido: Uso de Azure Data Studio para conectarse a una base de datos de Azure SQL y consultarla](../../../azure-data-studio/quickstart-sql-database.md)

Para habilitar (deshabilitar) Always Encrypted:
1. En el cuadro de diálogo **Conexión**, haga clic en **Avanzada...** .
2. Para habilitar Always Encrypted para la conexión, establezca el campo **Always Encrypted** en **Habilitado**. Para deshabilitar Always Encrypted, deje en blanco el valor del campo **Always Encrypted** o establézcalo en **Deshabilitado**.
3. Haga clic en **Aceptar** para cerrar las **Propiedades avanzadas**.

![Vídeo breve en el que se muestran los pasos a seguir para habilitar Always Encrypted para la conexión.](../../../relational-databases/security/encryption/media/always-encrypted-ads-connect.gif)

Para ejecutar instrucciones que aprovechen un enclave seguro del lado servidor cuando se usa [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md), debe especificar un protocolo de atestación de enclave y una dirección URL de atestación de enclave, además de habilitar Always Encrypted para la conexión. Para obtener información detallada, vea [Requisitos previos para ejecutar instrucciones T-SQL con enclaves en Azure Data Studio](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-t-sql-statements-using-enclaves-in-azure-data-studio).

> [!TIP]
> Para alternar entre la habilitación y la deshabilitación de Always Encrypted en una ventana de consulta existente, haga clic en **Desconectar** y en **Conectar**, y complete los pasos anteriores para volver a conectarse a la base de datos con los valores del campo **Always Encrypted** de su elección. 

> [!NOTE] 
> El botón **Cambiar conexión** de una ventana de consulta no admite actualmente la alternancia entre la habilitación y la deshabilitación de Always Encrypted.

## <a name="parameterization-for-always-encrypted"></a>Parametrización de Always Encrypted

Parametrización de Always Encrypted es una característica de Azure Data Studio 18.1, y versiones posteriores, que convierte automáticamente las variables Transact-SQL en parámetros de consulta (instancias de [SqlParameter Class](/dotnet/api/microsoft.data.sqlclient.sqlparameter)). Esto permite que el [proveedor de datos Microsoft .NET para SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) subyacente detecte datos con columnas cifradas como destino y cifre dichos datos antes de enviarlos a la base de datos.
  
Sin la parametrización, el proveedor de datos Microsoft .NET para SQL Server pasa cada instrucción que se crea en la ventana de consulta como una consulta sin parámetros. Si la consulta contiene literales o variables Transact-SQL con columnas cifradas como destino, el proveedor de datos .NET Framework para SQL Server no podrá detectarlas ni cifrarlas antes de enviar la consulta a la base de datos. Como resultado, la consulta presentará un error debido a un error de coincidencia de tipos (entre la variable Transact-SQL literal de texto no cifrado y la columna cifrada). Por ejemplo, la consulta siguiente presentará un error sin parametrización, suponiendo que la columna `SSN` está cifrada.   

```sql
DECLARE @SSN CHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>Habilitación y deshabilitación de parametrización de Always Encrypted

Parametrización de Always Encrypted está deshabilitada de manera predeterminada.

A fin de habilitar o deshabilitar la Parametrización para Always Encrypted, realice lo siguiente:

1. Seleccione **Archivo** > **Preferencias** > **Configuración** (**Código** > **Preferencias** > **Configuración** en Mac).
2. Vaya a **Datos** > **Microsoft SQL Server**.
3. Seleccione o anule la selección de **Habilitar parametrización de Always Encrypted**.
4. Cierre la ventana **Configuración**.

![Vídeo breve en el que se muestra cómo habilitar o deshabilitar la parametrización para Always Encrypted.](../../../relational-databases/security/encryption/media/always-encrypted-ads-parameterization.gif)

> [!NOTE]
> Parametrización de Always Encrypted funciona solo en una consulta que use conexiones de base de datos con Always Encrypted habilitado (consulte [Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos](#enabling-and-disabling-always-encrypted-for-a-database-connection)). Ninguna variable Transact-SQL se parametrizará si la ventana de consulta usa una conexión de base de datos sin tener habilitado Always Encrypted.

### <a name="how-parameterization-for-always-encrypted-works"></a>Funcionamiento de parametrización de Always Encrypted

Si la parametrización de Always Encrypted y Always Encrypted están habilitadas para una ventana de consulta, Azure Data Studio intentará parametrizar las variables Transact-SQL que cumplen con las condiciones de requisitos previos siguientes:

- Se declaran e inicializan en la misma instrucción (inicialización alineada). Las variables declaradas que usan instrucciones `SET` independientes no se parametrizarán.
- Se inicializan con un solo literal. Las variables que se inicializan con expresiones que incluyen cualquier operador o función no se parametrizarán.

A continuación, aparecen ejemplos de variables que Azure Data Studio parametrizará.

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Aquí se muestran algunos ejemplos de variables que Azure Data Studio no intentará parametrizar:

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
Para que un intento de parametrización se realice correctamente:   
- El tipo de literal que se use para la inicialización de la variable que se va a parametrizar debe coincidir con el tipo de la declaración de variable.   
- Si el tipo declarado de la variable es un tipo de fecha o un tipo de hora, la variable se debe inicializar con una cadena usando uno de los siguientes formatos compatibles con ISO 8601.   

Estos son ejemplos de declaraciones de variables Transact-SQL que generarán errores de parametrización:

```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```

Azure Data Studio usa IntelliSense para notificar qué variables se pueden parametrizar correctamente y cuáles son los intentos de parametrización que presentan error (y el motivo).   

Una declaración de una variable que se puede parametrizar correctamente está marcada con un mensaje de información subrayado en la ventana de consulta. Si mantiene el cursor sobre una instrucción de declaración que se ha marcado con un mensaje de información subrayado, verá el mensaje que contiene los resultados del proceso de parametrización, incluidos los valores de las propiedades clave del objeto [Clase de SqlParameter](/dotnet/api/microsoft.data.sqlclient.sqlparameter) resultante (la variable se asigna a lo siguiente: [SqlDbType](/dotnet/api/microsoft.data.sqlclient.sqlparameter.dbtype), [Size](/dotnet/api/microsoft.data.sqlclient.sqlparameter.size), [Precision](/dotnet/api/microsoft.data.sqlclient.sqlparameter.precision), [Scale](/dotnet/api/microsoft.data.sqlclient.sqlparameter.scale) y [SqlValue](/dotnet/api/microsoft.data.sqlclient.sqlparameter.sqlvalue). También puede ver la lista completa de todas las variables que se han parametrizado correctamente en la vista **Problemas**. Para abrir la vista **Problemas**, seleccione **Ver** > **Problemas**.    



Si Azure Data Studio ha intentado parametrizar una variable, pero la parametrización ha producido un error, la declaración de la variable se marcará con un carácter de subrayado de error. Si mantiene el puntero sobre una instrucción de declaración que se marcó con un carácter de subrayado de error, recibirá los resultados del error. También podrá ver la lista completa de errores de parametrización de todas las variables en la vista **Problemas**.

 
> [!NOTE]
> Como Always Encrypted admite un subconjunto limitado de conversiones de tipo, en muchos casos se requiere que ese tipo de datos de una variable Transact-SQL sea el mismo tipo de la columna de base de datos de destino. Por ejemplo, suponiendo que el tipo de la columna `SSN` de la tabla `Patients` sea `char(11)`, la consulta siguiente presentará un error, debido a que el tipo de la variable `@SSN`, que es `nchar(11)`, no coincide con el tipo de la columna.   

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
> Sin parametrización, la consulta completa, incluidas las conversiones de tipo, se procesan dentro de SQL Server o de Azure SQL Database. Con la parametrización habilitada, el proveedor de datos Microsoft .NET para SQL Server realiza algunas conversiones de tipos para SQL Server en Azure Data Studio. Debido a las diferencias entre el sistema de tipo de Microsoft .NET y el de SQL Server (por ejemplo, una precisión distinta de algunos tipos, como float), una consulta que se ejecuta con parametrización habilitada puede generar resultados distintos a los de la consulta ejecutada sin la parametrización habilitada. 

## <a name="next-steps"></a>Pasos siguientes
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)


## <a name="see-also"></a>Consulte también
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)