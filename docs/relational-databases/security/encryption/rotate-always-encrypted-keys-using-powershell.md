---
description: Rotación de claves de Always Encrypted con PowerShell
title: Rotación de claves de Always Encrypted con PowerShell | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 5117b4fd-c8d3-48d5-87c9-756800769f31
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5d12d2e93c7580e3a70f7f63af085af335bbc5d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340149"
---
# <a name="rotate-always-encrypted-keys-using-powershell"></a>Rotación de claves de Always Encrypted con PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

En este artículo se proporcionan los pasos necesarios para rotar claves de Always Encrypted con el módulo SqlServer PowerShell. Para obtener información sobre el uso del módulo SqlServer PowerShell para Always Encrypted, vea [Configure Always Encrypted using PowerShell (Configurar Always Encrypted con PowerShell)](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).

La rotación de claves de Always Encrypted es el proceso de sustitución de una clave existente por una nueva. Puede que necesite rotar una clave si está en peligro, o bien para cumplir las directivas o los reglamentos de la organización que exigen que se roten las claves criptográficas de forma regular. 

Always Encrypted emplea dos tipos de claves, por lo que hay dos flujos de trabajo de rotación de claves de alto nivel: rotación de claves maestras de columna y rotación de claves de cifrado de columna.

* **Rotación de claves de cifrado de columna** : implica descifrar los datos cifrados con la clave actual y volver a cifrarlos con la nueva clave de cifrado de columna. Como la rotación de una clave de cifrado de columna exige el acceso a las claves y a la base de datos, la rotación de claves de cifrado de columna solo se puede realizar sin separación de roles.
* **Rotación de claves maestras de columna** : implica descifrar las claves de cifrado de columna que están protegidas con la clave maestra de columna actual, volver a cifrarlas con la nueva clave maestra de columna y actualizar los metadatos de ambos tipos de claves. La rotación de claves maestras de columna se puede realizar con o sin separación de roles (cuando se usa el módulo SqlServer PowerShell).

## <a name="column-master-key-rotation-without-role-separation"></a>Rotación de claves maestras de columna sin separación de roles

El método de rotación de una clave maestra de columna que se describe en esta sección no admite la separación de roles entre un administrador de seguridad y un administrador de base de datos. Algunos de los pasos siguientes combinan operaciones en las claves físicas con operaciones en los metadatos de las claves, así que este flujo de trabajo está recomendado para organizaciones que usen el modelo DevOps, o si la base de datos está hospedada en la nube y el objetivo principal es evitar que los administradores de la nube (pero no los DBA locales) accedan a información confidencial. No se recomienda si los posibles adversarios incluyen administradores de base de datos o si estos no deben tener acceso a información confidencial.  


| Tarea | Artículo | Accede a claves de texto no cifrado o a almacén de claves| Accede a base de datos
|:---|:---|:---|:---
|Paso 1. Cree una nueva clave maestra de columna en un almacén de claves.<br><br>**Nota:** El módulo SqlServer de PowerShell no admite este paso. Para realizar esta tarea desde la línea de comandos, debe usar herramientas específicas del almacén de claves. | [Creación y almacenamiento de claves maestras de columna para Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Sí | No
|Paso 2. Inicie un entorno de PowerShell e importe el módulo SqlServer. | [Importar el módulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | No
|Paso 3. Conecte con el servidor y la base de datos. | [Conexión a una base de datos](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | No | Sí
|Paso 4. Cree un objeto SqlColumnMasterKeySettings que contenga información sobre la ubicación de la nueva clave maestra de columna. SqlColumnMasterKeySettings es un objeto que existe en memoria (en PowerShell). Para crearlo, debe usar el cmdlet específico del almacén de claves. |[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)<br> | No | No
|Paso 5. Cree los metadatos sobre la nueva clave maestra de columna en la base de datos. | [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Nota:** En segundo plano, este cmdlet emite la instrucción [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) para crear metadatos de clave. | No | Sí
|Paso 6. Autentíquese en Azure si la clave maestra de columna actual o la nueva clave maestra de columna se almacenan en el Almacén de claves de Azure. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Sí | No
|Paso 7. Inicie la rotación al cifrar las claves de cifrado de columna que actualmente están protegidas con la antigua clave maestra de columna con la nueva clave maestra de columna. Después de este paso, cada clave de cifrado de columna afectada (asociada a la antigua clave maestra de columna y rotada) se cifra con la antigua y la nueva clave maestra de columna y tiene dos valores cifrados en los metadatos de la base de datos.| [Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation) | Sí | Sí
|Paso 8. Coordínese con los administradores de todas las aplicaciones que consultan columnas cifradas de la base de datos (y protegidas con la clave maestra de columna anterior) para que puedan garantizar el acceso de las aplicaciones a la nueva clave maestra de columna.| [Crear y almacenar claves maestras de columna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md) | Sí | No
|Paso 9. Finalice la rotación.<br><br>**Nota:** Antes de ejecutar este paso, asegúrese de que todas las aplicaciones que consultan columnas cifradas protegidas con la clave maestra de columna anterior se hayan configurado para usar la nueva clave maestra de columna. Si realiza este paso antes de tiempo, es posible que algunas de esas aplicaciones no puedan descifrar los datos. Finalice la rotación mediante la eliminación de los valores cifrados de la base de datos que se crearon con la clave maestra de columna anterior. Esto quita la asociación entre la antigua clave maestra de columna y las claves de cifrado de columna que protege. |[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)| No | Sí
|Paso 10. Quite los metadatos de la clave maestra de columna antigua. |[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| No | Sí

> [!NOTE]
> Es muy recomendable no eliminar permanentemente la antigua clave maestra de columna después de la rotación. Debería conservarla en su almacén de claves actual o archivarla en otra ubicación segura. Si restaura la base de datos desde un archivo de copia de seguridad a un punto en el tiempo *anterior* a la configuración de la nueva clave maestra de columna, necesitará la clave antigua para acceder a los datos.

### <a name="rotating-a-column-master-key-without-role-separation-windows-certificate-example"></a>Rotación de una clave maestra de columna sin separación de roles (ejemplo de certificado de Windows)

El siguiente script es un ejemplo completo que reemplaza una clave maestra de columna existente (CMK1) por una nueva clave maestra de columna (CMK2).

```powershell
# Create a new column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048

# Import the SqlServer module
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Create metadata for your new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings

# Initiate the rotation from the current column master key to the new column master key.
$oldCmkName = "CMK1"
Invoke-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName -TargetColumnMasterKeyName $newCmkName -InputObject $database

# Complete the rotation of the old column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key metadata.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="column-master-key-rotation-with-role-separation"></a>Rotación de claves maestras de columna con separación de roles

El flujo de trabajo de rotación de claves maestras de columna que se describe en esta sección garantiza la separación entre un Administrador de seguridad y un DBA.

> [!IMPORTANT]
> Antes de ejecutar cualquiera de los pasos donde *Accede a claves de texto no cifrado o a almacén de claves*=**Sí** en la tabla siguiente (pasos que acceden a claves de texto no cifrado o a almacén de claves), asegúrese de que el entorno de PowerShell se ejecuta en un equipo seguro distinto al que hospeda la base de datos. Para obtener más información, vea [Security Considerations for Key Management (Consideraciones de seguridad para la administración de claves)](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).

### <a name="part-1-dba"></a>Parte 1: DBA

Un DBA recupera metadatos sobre la clave maestra de columna que se va a rotar y sobre las claves de cifrado de columna afectadas asociadas a la clave maestra de columna actual. El DBA comparte toda esta información con un Administrador de seguridad.


| Tarea | Artículo | Accede a claves de texto no cifrado o a almacén de claves| Accede a base de datos
|:---|:---|:---|:---
|Paso 1. Inicie un entorno de PowerShell e importe el módulo SqlServer. | [Importar el módulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | None
|Paso 2. Conecte con el servidor y una base de datos. | [Conexión a una base de datos](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | No | Sí
|Paso 3. Recupere los metadatos sobre la clave maestra de columna antigua.| [Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey) | No | Sí
|Paso 4. Recupere los metadatos sobre las claves de cifrado de columna protegidas por la antigua clave maestra de columna, incluidos sus valores cifrados. | [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey) | No | Sí
|Paso 5. Comparta la ubicación de la clave maestra de columna (el nombre del proveedor y una ruta de acceso de la clave maestra de columna) y los valores cifrados de las claves de cifrado de columna correspondientes protegidos con la clave maestra de columna anterior.| Consulte los ejemplos siguientes. | No | No

### <a name="part-2-security-administrator"></a>Parte 2: Administrador de seguridad

El Administrador de seguridad genera una nueva clave maestra de columna, vuelve a cifrar las claves de cifrado de columna afectadas con la nueva clave maestra de columna y comparte la información sobre la nueva clave maestra de columna, así como el conjunto de nuevos valores cifrados de las claves de cifrado de columna afectadas, con el DBA.

| Tarea | Artículo | Acceso a claves de texto no cifrado o a almacén de claves| Accede a base de datos
|:---|:---|:---|:---
|Paso 1. Obtenga la ubicación de la clave maestra de columna antigua y los valores cifrados de las claves de cifrado de columna correspondientes protegidos con la clave maestra de columna antigua del DBA.|N/D<br>Consulte los ejemplos siguientes.|No| No
|Paso 2. Cree una nueva clave maestra de columna en un almacén de claves.<br><br>**Nota:** El módulo SqlServer no admite este paso. Para realizar esta tarea desde una línea de comandos, debe usar herramientas específicas del tipo del almacén de claves.|[Creación y almacenamiento de claves maestras de columna para Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Sí | No
|Paso 3. Inicie un entorno de PowerShell e importe el módulo SqlServer. | [Importar el módulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | No
|Paso 4. Cree un objeto SqlColumnMasterKeySettings que contenga información sobre la ubicación de la **antigua** clave maestra de columna. SqlColumnMasterKeySettings es un objeto que existe en memoria (en PowerShell). |New-SqlColumnMasterKeySettings| No | No
|Paso 5. Cree un objeto SqlColumnMasterKeySettings que contenga información sobre la ubicación de la **nueva** clave maestra de columna. SqlColumnMasterKeySettings es un objeto que existe en memoria (en PowerShell). Para crearlo, debe usar el cmdlet específico del almacén de claves. | [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)| No | No
|Paso 6. Autentíquese en Azure si la clave maestra de columna antigua (actual) o la nueva clave maestra de columna se almacenan en el Almacén de claves de Azure. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Sí | No
|Paso 7. Vuelva a cifrar cada valor de la clave de cifrado de columna que actualmente está protegida con la antigua clave maestra de columna con la nueva clave maestra de columna. | [New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)<br><br>**Nota:** Al llamar a este cmdlet, pase los objetos SqlColumnMasterKeySettings de la antigua y la nueva clave maestra de columna, junto con un valor de la clave de cifrado de columna, para que se vuelvan a cifrar.|Sí|No
|Paso 8. Comparta la ubicación de la nueva clave maestra de columna (el nombre del proveedor y la ruta de acceso de la clave maestra de columna) y el conjunto de valores cifrados nuevos de las claves de cifrado de columna con el DBA.| Consulte los ejemplos siguientes. | No | No

> [!NOTE]
> Es muy recomendable no eliminar permanentemente la antigua clave maestra de columna después de la rotación. Debería conservarla en su almacén de claves actual o archivarla en otra ubicación segura. Si restaura la base de datos desde un archivo de copia de seguridad a un punto en el tiempo *anterior* a la configuración de la nueva clave maestra de columna, necesitará la clave antigua para acceder a los datos.

### <a name="part-3-dba"></a>Parte 3: DBA

El DBA crea metadatos para la nueva clave maestra de columna y actualiza los metadatos de las claves de cifrado de columna afectadas para agregar el nuevo conjunto de valores cifrados. En este paso, el DBA también se coordina con los administradores de las aplicaciones que consultan las columnas de cifrado para asegurarse de que la aplicación pueda acceder a la nueva clave maestra de columna. Una vez que todas las aplicaciones están configuradas para usar la nueva clave maestra de columna, el DBA quita el conjunto anterior de valores cifrados y los metadatos antiguos de claves maestras de columna.

| Tarea | Artículo | Acceso a claves de texto no cifrado o a almacén de claves| Accede a base de datos
|:---|:---|:---|:---
|Paso 1. Obtenga la ubicación de la nueva clave maestra de columna y el nuevo conjunto de valores cifrados de las claves de cifrado de columna correspondientes protegidos con la clave maestra de columna antigua del Administrador de seguridad.| Consulte los ejemplos siguientes. | No | No
|Paso 2. Inicie un entorno de PowerShell e importe el módulo SqlServer. | [Importar el módulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | No
|Paso 3. Conecte con el servidor y una base de datos. | [Conexión a una base de datos](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | No | Sí
|Paso 4. Cree un objeto SqlColumnMasterKeySettings que contenga información sobre la ubicación de la nueva clave maestra de columna. SqlColumnMasterKeySettings es un objeto que existe en memoria (en PowerShell). |New-SqlColumnMasterKeySettings| No| No
|Paso 5. Cree los metadatos sobre la nueva clave maestra de columna en la base de datos.|[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Nota:** En segundo plano, este cmdlet emite la instrucción [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) para crear metadatos de clave. | No | Sí
|Paso 6. Recupere los metadatos sobre las claves de cifrado de columna protegidas por la antigua clave maestra de columna.| [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)| No | Sí
|Paso 7. Agregue un nuevo valor cifrado (generado con la nueva clave maestra de columna) a los metadatos de cada clave de cifrado de columna afectada.|[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)|No|Sí
|Paso 8. Coordínese con los administradores de todas las aplicaciones que consultan columnas cifradas de la base de datos (y protegidas con la clave maestra de columna anterior) para que puedan garantizar el acceso de las aplicaciones a la nueva clave maestra de columna.|[Creación y almacenamiento de claves maestras de columna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| No|No
|Paso 9. Finalice la rotación mediante la eliminación de los valores cifrados asociados a la clave maestra de columna anterior de la base de datos.<br><br>**Nota:** Antes de ejecutar este paso, asegúrese de que todas las aplicaciones que consultan columnas cifradas protegidas con la clave maestra de columna antigua se hayan configurado para usar la nueva clave maestra de columna. Si realiza este paso antes de tiempo, es posible que algunas de esas aplicaciones no puedan descifrar los datos.<br><br>Este paso quita una asociación entre la antigua clave maestra de columna y las claves de cifrado de columna que protege. | [Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)<br><br>También puede usar [Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue) | No|Sí
|Paso 10. Quite los metadatos de la clave maestra de columna antigua de la base de datos.| [Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| No|Sí

### <a name="rotating-a-column-master-key-with-role-separation-windows-certificate-example"></a>Rotación de una clave maestra de columna con separación de roles (ejemplo de certificado de Windows)

El siguiente script es un ejemplo completo para generar una nueva clave maestra de columna que es certificado en el almacén de certificados de Windows y rotar una clave maestra de columna existente (actual) para reemplazarla por la nueva clave maestra de columna. El script da por hecho que la base de datos de destino contiene la clave maestra de columna, denominada CMK1 (que se va a rotar), que cifra algunas claves de cifrado de columna.

Parte 1: DBA

```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Retrieve the data about the old column master key, which needs to be rotated.
$oldCmkName = "CMK1"
$oldCmk = Get-SqlColumnMasterKey -Name $oldCmkName -InputObject $database


# Share the location of the old column master key with a Security Administrator, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $oldCmkDataFile
$oldCmk.KeyStoreProviderName +", " + $oldCmk.KeyPath >> $oldCmkDataFile


# Find column encryption keys associated with the old column master key and provide the encrypted values of column encryption keys to the Security Administrator, via a CSV file on a share drive.
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
$oldCekValuesFile = "Z:\oldcekvalues.txt"
"CEKName, CEKEncryptedValue" > $oldCekValuesFile 
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {
        # This column encryption has 2 encrypted values - let's check, if it is associated with the old column master key.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {# This column encryption key has 1 encrypted value that was produced using the old column master key
        # Save the name and the encrypted value of the column encryption key in the file.
        $encryptedValue =  "0x" + -join ($ceks[$i].ColumnEncryptionKeyValues[0].EncryptedValue |  foreach {$_.ToString("X2") } )
        $ceks[$i].Name + "," + $encryptedValue >> $oldCekValuesFile
    }
} 
```


Parte 2: Administrador de seguridad

```powershell
# Obtain the location of the old column master key and the encrypted values of the corresponding column encryption keys, from your DBA, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
$oldCmkData = Import-Csv $oldCmkDataFile
$oldCekValuesFile = "Z:\oldcekvalues.txt"
$oldCekValues = @(Import-Csv $oldCekValuesFile)

# Create a new column master key in Windows Certificate Store.
$storeLocation = "CurrentUser"
$certPath = "Cert:\" + $storeLocation + "\My"
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation $certPath -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Create a SqlColumnMasterKeySettings object for your old column master key. 
$oldCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $oldCmkData.KeyStoreProviderName -KeyPath $oldCmkData.KeyPath

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation $storeLocation -Thumbprint $cert.Thumbprint


# Prepare a CSV file, you will use to share the encrypted values of column encryption keys, produced using the new column master key.
$newCekValuesFile = "Z:\newcekvalues.txt"
"CEKName, CEKEncryptedValue" > $newCekValuesFile

# Re-encrypt each value with the new column master key and save the new encrypted value in the file.
for($i=0; $i -lt $oldCekValues.Count; $i++){
    # Re-encrypt each value with the new CMK
    $newValue = New-SqlColumnEncryptionKeyEncryptedValue -TargetColumnMasterKeySettings $newCmkSettings -ColumnMasterKeySettings $oldCmkSettings -EncryptedValue $oldCekValues[$i].CEKEncryptedValue
    $oldCekValues[$i].CEKName + ", " + $newValue >> $newCekValuesFile
}

# Share the new column master key data with your DBA, via a CSV file.
$newCmkDataFile = $home + "\newcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $newCmkDataFile
$newCmkSettings.KeyStoreProviderName +", " + $newCmkSettings.KeyPath >> $newCmkDataFile
```


Parte 3: DBA

```powershell
# Obtain the location of the new column master key and the new encrypted values of the corresponding column encryption keys, from your Security Administrator, via a CSV file on a share drive.
$newCmkDataFile = "Z:\newcmkdata.txt"
$newCmkData = Import-Csv $newCmkDataFile
$newCekValuesFile = "Z:\newcekvalues.txt"
$newCekValues = @(Import-Csv $newCekValuesFile)

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $newCmkData.KeyStoreProviderName -KeyPath $newCmkData.KeyPath
# Create metadata for the new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings


# Get all CEK objects
$oldCmkName = "CMK1"
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {# This column encryption key has 2 encrypted values. Let's check, if it is associated with the old CMK.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {
        # Find the corresponding new encrypted value, received from the Security Administrator.
        $newValueRow = ($newCekValues| Where-Object {$_.CEKName -eq $ceks[$i].Name }[0])
        # Update the column encryption key metadata object by adding the new encrypted value
        Add-SqlColumnEncryptionKeyValue -ColumnMasterKeyName $newCmkName -Name $ceks[$i].Name -EncryptedValue $newValueRow.CEKEncryptedValue -InputObject $database 
    }
}

# Complete the rotation of the current column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="rotating-a-column-encryption-key"></a>Rotación de una clave de cifrado de columna

La rotación de una clave de cifrado de columna implica descifrar los datos de todas las columnas cifrados con la clave que se va a rotar y volver a cifrarlos con la nueva clave de cifrado de columna. Este flujo de trabajo de rotación exige acceso a las claves y a la base de datos y, por tanto, no se puede realizar con separación de roles. La rotación de una clave de cifrado de columna puede tardar mucho tiempo si las tablas que contienen columnas cifradas con la clave que se va a rotar son grandes. Por lo tanto, la organización tiene que planear cuidadosamente cualquier rotación de claves de cifrado de columna.

Puede girar una clave de cifrado de columna con un enfoque sin conexión o un enfoque en línea. El primer método probablemente sea más rápido, pero las aplicaciones no pueden escribir en las tablas afectadas. El último enfoque probablemente demore más, pero puede limitar el intervalo de tiempo durante el cual las tablas afectadas no estarán disponibles para las aplicaciones. Vea [Configuración del cifrado de columna mediante Always Encrypted con PowerShell](configure-column-encryption-using-powershell.md) y [Set-SqlColumnEncryption](/powershell/module/sqlserver/set-sqlcolumnencryption/) para obtener más detalles.

| Tarea | Artículo | Accede a claves de texto no cifrado o a almacén de claves| Accede a base de datos
|:---|:---|:---|:---
|Paso 1. Inicie un entorno de PowerShell e importe el módulo SqlServer. | [Importar el módulo SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | No | No
|Paso 2. Conecte con el servidor y una base de datos. | [Conexión a una base de datos](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | No | Sí
|Paso 3. Autentíquese en Azure si la clave maestra de columna (que protege la clave de cifrado de columna que se va a rotar) se almacena en el Almacén de claves de Azure. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Sí | No
|Paso 4. Genere una nueva clave de cifrado de columna, cífrela con la clave maestra de columna y cree los metadatos de clave de cifrado de columna en la base de datos.  | [New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)<br><br>**Nota:** Use una variación del cmdlet que genera y cifra internamente una clave de cifrado de columna.<br>En segundo plano, este cmdlet emite la instrucción [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) para crear los metadatos de clave. | Sí | Sí
|Paso 5. Busque todas las columnas cifradas con la clave de cifrado de columna anterior. | [Guía de programación para objetos de administración de SQL Server (SMO)](../../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md) | No | Sí
|Paso 6. Cree un objeto *SqlColumnEncryptionSettings* para cada columna afectada.  SqlColumnMasterKeySettings es un objeto que existe en memoria (en PowerShell). Especifica el esquema de cifrado de destino de una columna. En este caso, el objeto debe especificar que la columna afectada debe cifrarse con la nueva clave de cifrado de columna. | [New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings) | No | No
|Paso 7. Vuelva a cifrar las columnas identificadas en el paso 5 con la nueva clave de cifrado de columna. | [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)<br><br>**Nota:** Este paso puede llevar mucho tiempo. Las aplicaciones no podrán tener acceso a las tablas durante toda la operación o parte de esta, dependiendo del enfoque (en línea o sin conexión) que seleccione. | Sí | Sí
|Paso 8. Quite los metadatos de la clave de cifrado de columna anterior. | [Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey) | No | Sí

### <a name="example---rotating-a-column-encryption-key"></a>Ejemplo: rotación de una clave de cifrado de columna

El siguiente script muestra la rotación de una clave de cifrado de columna.  El script da por hecho que la base de datos de destino contiene algunas columnas cifradas con una clave de cifrado de columna denominada CEK1 (que se va a rotar), que está protegida con una clave maestra de columna denominada CMK1 (la clave maestra de columna no se almacena en Azure Key Vault). 


```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Generate a new column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cmkName = "CMK1"
$newCekName = "CEK2"
New-SqlColumnEncryptionKey -Name $newCekName -InputObject $database -ColumnMasterKey $cmkName 


# Find all columns encrypted with the old column encryption key, and create a SqlColumnEncryptionSetting object for each column.
$ces = @()
$oldCekName = "CEK1"
$tables = $database.Tables
for($i=0; $i -lt $tables.Count; $i++){
    $columns = $tables[$i].Columns
    for($j=0; $j -lt $columns.Count; $j++) {
        if($columns[$j].isEncrypted -and $columns[$j].ColumnEncryptionKeyName -eq $oldCekName) {
            $threeColPartName = $tables[$i].Schema + "." + $tables[$i].Name + "." + $columns[$j].Name 
            $ces += New-SqlColumnEncryptionSettings -ColumnName $threeColPartName -EncryptionType $columns[$j].EncryptionType -EncryptionKey $newCekName
        }
     }
}

# Re-encrypt all columns, currently encrypted with the old column encryption key, using the new column encryption key.
Set-SqlColumnEncryption -ColumnEncryptionSettings $ces -InputObject $database -UseOnlineApproach -MaxDowntimeInSeconds 120 -LogFileDirectory .

# Remove the old column encryption key metadata.
Remove-SqlColumnEncryptionKey -Name $oldCekName -InputObject $database
```

## <a name="next-steps"></a>Pasos siguientes
- [Consulta de columnas mediante Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)
  
## <a name="see-also"></a>Consulte también
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Información general sobre la administración de claves de Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
- [Configurar Always Encrypted con PowerShell](configure-always-encrypted-using-powershell.md)
- [Rotación de claves de Always Encrypted mediante SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)