---
title: Configurar Always Encrypted con PowerShell | Microsoft Docs
description: Obtenga información sobre cómo importar y usar el módulo SqlServer PowerShell, que proporciona cmdlets para configurar Always Encrypted en Azure SQL Database y SQL Server.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 12f2bde5-e100-41fa-b474-2d2332fc7650
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c5768357f83252c4d71ac1aeff064364a63367b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345398"
---
# <a name="configure-always-encrypted-using-powershell"></a>Configure Always Encrypted using PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

En el módulo SqlServer PowerShell se proporcionan cmdlets para configurar [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) en [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

## <a name="security-considerations-when-using-powershell-to-configure-always-encrypted"></a>Consideraciones de seguridad al usar PowerShell para configurar Always Encrypted

Dado que el objetivo principal de Always Encrypted es garantizar la seguridad de la información confidencial cifrada, incluso si el sistema de base de datos está en peligro, la ejecución de un script de PowerShell que procese las claves o la información confidencial en el equipo con SQL Server puede reducir o anular las ventajas de la característica. Para obtener más recomendaciones relacionadas con la seguridad, vea [Security Considerations for Key Management](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)(Consideraciones de seguridad para la administración de claves).

Puede usar PowerShell para administrar claves de Always Encrypted con y sin separación de roles, lo que proporciona control sobre quién tiene acceso a las claves de cifrado reales del almacén de claves y quién tiene acceso a la base de datos.

 Para obtener más recomendaciones, vea [Security Considerations for Key Management](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)(Consideraciones de seguridad para la administración de claves).

## <a name="prerequisites"></a>Requisitos previos

Instale el [módulo SqlServer](/powershell/sqlserver/sqlserver/vlatest/sqlserver) en un equipo seguro distinto del equipo que hospeda la instancia de SQL Server. El módulo se puede instalar directamente desde la Galería de PowerShell.  Vea las instrucciones de [descarga](../../../powershell/download-sql-server-ps-module.md) para obtener más información.


## <a name="importing-the-sqlserver-module"></a><a name="importsqlservermodule"></a> Importar el módulo SqlServer 

Para cargar el módulo SqlServer:

1.  Use el cmdlet **Set-ExecutionPolicy** para establecer la directiva de ejecución de script apropiada.
2.  Use el cmdlet **Import-Module** para importar el módulo SqlServer.

En este ejemplo se carga el módulo SqlServer.

```
# Import the SQL Server Module.  
Import-Module "SqlServer" 
```

## <a name="connecting-to-a-database"></a><a name="connectingtodatabase"></a> Conectar a una base de datos

Algunos de los cmdlets de Always Encrypted funcionan con datos o metadatos de la base de datos y requieren que se conecte primero a la base de datos. Hay dos métodos recomendados para conectarse a una base de datos al configurar Always Encrypted con el módulo SqlServer: 
1. Conéctese con el cmdlet **Get-SqlDatabase**.
2. Conéctese con el proveedor de SQL Server PowerShell.

[!INCLUDE[freshInclude](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

### <a name="using-get-sqldatabase"></a>Uso de Get-SqlDatabase
El cmdlet **Get-SqlDatabase** le permite conectarse a una base de datos en SQL Server o en Azure SQL Database. Devuelve un objeto de base de datos, que luego se puede pasar con el parámetro **InputObject** de un cmdlet que se conecta a la base de datos. 

### <a name="using-sql-server-powershell"></a>Usar SQL Server PowerShell

```
# Import the SqlServer module
Import-Module "SqlServer"  

# Connect to your database
# Set the valid server name, database name and authentication keywords in the connection string
$serverName = "<Azure SQL server name>.database.windows.net"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Authentication = Active Directory Integrated"
$database = Get-SqlDatabase -ConnectionString $connStr

# List column master keys for the specified database.
Get-SqlColumnMasterKey -InputObject $database
```

También puede usar la canalización:


```
$database | Get-SqlColumnMasterKey
```

### <a name="using-sql-server-powershell-provider"></a>Uso del proveedor de SQL Server PowerShell
El [proveedor de SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md) expone la jerarquía de objetos de SQL Server en rutas similares a las rutas de acceso al sistema de archivos. Con SQL Server PowerShell, puede navegar por las rutas de acceso mediante alias de Windows PowerShell similares a los comandos que se usan normalmente para navegar por las rutas de acceso al sistema de archivos. Una vez que se haya desplazado a la instancia de destino y la base de datos, los cmdlets posteriores tendrán como destino esa base de datos, como se muestra en el ejemplo siguiente. 

> [!NOTE]
> Este método de conexión a una base de datos solo funciona para SQL Server (no se admite en Base de datos SQL de Azure).

```
# Import the SqlServer module.
Import-Module "SqlServer"
# Navigate to the database in the remote instance.
cd SQLSERVER:\SQL\servercomputer\DEFAULT\Databases\yourdatabase
# List column master keys in the above database.
Get-SqlColumnMasterKey
```

 
También puede especificar una ruta de acceso a la base de datos mediante el parámetro genérico **Path** , en lugar de desplazarse a la base de datos.


```
# Import the SqlServer module.
Import-Module "SqlServer" 
# List column master keys for the specified database.
Get-SqlColumnMasterKey -Path SQLSERVER:\SQL\servercomputer\DEFAULT\Databases\yourdatabase
```
 
## <a name="always-encrypted-tasks-using-powershell"></a>Tareas de Always Encrypted mediante PowerShell

- [Aprovisionamiento de claves de Always Encrypted mediante PowerShell](configure-always-encrypted-keys-using-powershell.md)
- [Rotar claves de Always Encrypted con PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)
- [Cifrado, recifrado o descifrado de columnas con Always Encrypted mediante PowerShell](configure-column-encryption-using-powershell.md)


##  <a name="always-encrypted-cmdlet-reference"></a><a name="aecmdletreference"></a> Referencia de cmdlets de Always Encrypted

Los siguientes cmdlets de PowerShell están disponibles para Always Encrypted:

|CMDLET |Descripción
|:---|:---
|**[Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)** |Realiza la autenticación en Azure y adquiere un token de autenticación.
|**[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)** |Agrega un nuevo valor cifrado para un objeto de clave de cifrado de columnas existente en la base de datos.
|**[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)** |Completa la rotación de una clave maestra de columna.
|**[Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)**   |Devuelve todos los objetos de clave de cifrado de columnas definidos en la base de datos, o devuelve un objeto de clave de cifrado de columnas con el nombre especificado.
|**[Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey)**   |Devuelve los objetos de clave maestra de columna definidos en la base de datos, o devuelve un objeto de clave maestra de columna con el nombre especificado.
|**[Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation)** |Inicia la rotación de una clave maestra de columna.
|**[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)** |Crea un objeto SqlColumnMasterKeySettings que describe una clave asimétrica almacenada en el Almacén de claves de Azure.
|**[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)** |Crea un objeto SqlColumnMasterKeySettings que describe una clave asimétrica almacenada en un almacén de claves compatible con la API Cryptography Next Generation (CNG).
|**[New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)**   |Crea un objeto de clave de cifrado de columnas en la base de datos.
|**[New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)**   |Produce un valor cifrado de una clave de cifrado de columnas.
|**[New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings)** |Crea un objeto SqlColumnEncryptionSettings que encapsula la información sobre el cifrado de una sola columna, incluida la CEK y el tipo de cifrado.
|**[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)**   |Crea un objeto de clave maestra de columna en la base de datos.
|**[New-SqlColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkeysettings)**|Crea un objeto SqlColumnMasterKeySettings para una clave maestra de columna con el proveedor y la ruta de acceso de la clave especificados.
|**[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)** |Crea un objeto SqlColumnMasterKeySettings que describe una clave asimétrica almacenada en un almacén de claves con un proveedor de servicios criptográficos (CSP) compatible con Cryptography API (CAPI).
|**[Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey)** |Quita de la base de datos el objeto de clave de cifrado de columnas.
|**[Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue)**   |Quita un valor cifrado de un objeto de clave de cifrado de columnas existente en la base de datos.
|**[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)** |Quita de la base de datos el objeto de clave maestra de columna.
|**[Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)** |Cifra, descifra o vuelve a cifrar las columnas especificadas de la base de datos.



## <a name="see-also"></a>Consulte también

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Información general sobre la administración de claves de Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Configuración de Always Encrypted con SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)