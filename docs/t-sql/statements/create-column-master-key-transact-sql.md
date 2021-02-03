---
description: CREATE COLUMN MASTER KEY (Transact-SQL)
title: CREATE COLUMN MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL13.SWB.NEWCOLUMNMASTERKEYDEF.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEYDEF.GENERAL.F1
- CREATE COLUMN MASTER KEY
- COLUMN MASTER KEY
- CREATE_COLUMN_MASTER_KEY_TSQL
- COLUMN_MASTER_KEY_TSQL
- SQL13.SWB.NEWCOLUMNMASTERKEY.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEY.GENERAL.F1
dev_langs:
- TSQL
helpviewer_keywords:
- column master key definition
- column master key, create
- CREATE COLUMN MASTER KEY statement
- Always Encrypted, create column master key
ms.assetid: f8926b95-e146-4e3f-b56b-add0c0d0a30e
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 6e18a1e0721428dcbfdb649be46ca83fd0d42972
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204939"
---
# <a name="create-column-master-key-transact-sql"></a>CREATE COLUMN MASTER KEY (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Crea un objeto de metadatos de clave maestra de columna en una base de datos. Una entrada de metadatos de la clave maestra de columna representa una clave, almacenada en un almacén de claves externo. Esta clave protege (cifra) las claves de cifrado de columna cuando se usa [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) o [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md). Varias claves maestras de columna permiten la rotación de claves; cambie periódicamente la clave para mejorar la seguridad. Cree una clave maestra de columna en un almacén de claves y su objeto de metadatos relacionado en la base de datos mediante el Explorador de objetos en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o PowerShell. Para más detalles, vea [Información general de administración de claves de Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 

> [!IMPORTANT]
> Si desea crear claves habilitadas para enclaves (con ENCLAVE_COMPUTATIONS) se requiere [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="syntax"></a>Sintaxis  

```syntaxsql
CREATE COLUMN MASTER KEY key_name   
    WITH (  
        KEY_STORE_PROVIDER_NAME = 'key_store_provider_name',  
        KEY_PATH = 'key_path'   
        [,ENCLAVE_COMPUTATIONS (SIGNATURE = signature)]
         )   
[;]  
```  

## <a name="arguments"></a>Argumentos
*key_name*  
El nombre de la clave maestra de columna en la base de datos.  
  
*key_store_provider_name*  
Especifica el nombre de un proveedor del almacén de claves. Un proveedor del almacén de claves es un componente de software de cliente que contiene un almacén de claves que tiene la clave maestra de columna. 

Un controlador cliente, habilitado con Always Encrypted:

- Usa un nombre de proveedor de almacenamiento de claves 
- Busca el proveedor de almacenamiento de claves en el registro del controlador de proveedores de almacenamiento de claves 

A continuación, el controlador usa el proveedor para descifrar claves de cifrado de columna. Una clave maestra de columna protege las claves de cifrado de columna. La clave maestra de columna se almacena en el almacén de claves subyacente. Después se usa un valor de texto simple de la clave de cifrado de columna para cifrar los parámetros de consulta que corresponden a las columnas de la base de datos cifrada. O bien, la clave de cifrado de columna descifra los resultados de consulta de las columnas cifradas.  
  
Las bibliotecas de controladores cliente compatibles con Always Encrypted incluyen proveedores de almacenamiento de claves para los almacenes de claves populares.   
  
Un conjunto de proveedores disponibles depende del tipo y la versión del controlador cliente. Consulte información sobre controladores determinados en la documentación de Always Encrypted: [Desarrollo de aplicaciones con Always Encrypted](../../relational-databases/security/encryption/always-encrypted-client-development.md).


En la siguiente tabla se muestran los nombres de los proveedores del sistema:  
  
|Nombre de proveedor de almacenamiento de claves|Almacén de claves subyacente|  
    |-----------------------------|--------------------------|
    |'MSSQL_CERTIFICATE_STORE'|Almacén de certificados de Windows| 
    |'MSSQL_CSP_PROVIDER'|Un almacén, como por ejemplo un módulo de seguridad de hardware (HSM), que es compatible con Microsoft CryptoAPI.|
    |'MSSQL_CNG_STORE'|Un almacén, como un módulo de seguridad de hardware (HSM), que es compatible con la API de Cryptography Next Generation.|  
    |'AZURE_KEY_VAULT'|Vea [Introducción a Azure Key Vault](/azure/key-vault/general/overview)|  
    |'MSSQL_JAVA_KEYSTORE'| Almacén de claves de Java.
  

En su controlador cliente compatible con Always Encrypted, puede configurar un proveedor de almacenamiento de claves personalizado que almacena claves maestras de columna para las que no hay ningún proveedor de almacenamiento de claves integrado. Los nombres de proveedores de almacenamiento de claves personalizados no pueden empezar por 'MSSQL_', ya que es un prefijo reservado para proveedores de almacenamiento de claves de [!INCLUDE[msCoName](../../includes/msconame-md.md)]. 

  
key_path  
La ruta de acceso de la clave en el almacén de claves maestras de columna. La ruta de acceso de la clave debe ser válida para cada aplicación cliente prevista para cifrar o descifrar datos. Los datos se almacenan en una columna a la que protege (indirectamente) la clave maestra de columna a la que se hace referencia. La aplicación cliente debe tener acceso a la clave. El formato de la ruta de acceso de la clave es específico del proveedor de almacenamiento de claves. En esta lista se describe el formato de las rutas de acceso de claves para determinados proveedores de almacenamiento de claves del sistema de Microsoft.  
  
-   **Nombre de proveedor:** MSSQL_CERTIFICATE_STORE  
  
    **Formato de ruta de acceso de la clave:** *CertificateStoreName*/*CertificateStoreLocation*/*CertificateThumbprint*  
  
     Donde:  
  
    *CertificateStoreLocation*  
    Ubicación del almacén de certificados, que debe ser Usuario actual o Máquina local. Para más información, vea [Local Machine and Current User Certificate Stores](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) (Almacenes de certificados Máquina local y Usuario actual).  
  
    *CertificateStore*  
    Nombre del almacén de certificados, como por ejemplo, 'My'.  
  
    *CertificateThumbprint*  
    Huella digital del certificado.  
  
    **Ejemplos:**  
  
    ```  
    N'CurrentUser/My/BBF037EC4A133ADCA89FFAEC16CA5BFA8878FB94'  
  
    N'LocalMachine/My/CA5BFA8878FB94BBF037EC4A133ADCA89FFAEC16'  
    ```  
  
-   **Nombre de proveedor:** MSSQL_CSP_PROVIDER  
  
    **Formato de ruta de acceso de la clave:** *ProviderName*/*KeyIdentifier*  
  
    Donde:  
  
    *ProviderName*  
    El nombre de un Cryptographic Service Provider (CSP), que implementa CAPI para el almacén de claves maestras de columna. Si usa un HSM como un almacén de claves, el nombre del proveedor debe ser el nombre del CSP proporcionado por el proveedor de HSM. El proveedor debe estar instalado en un equipo cliente.  
  
    *KeyIdentifier*  
    Identificador de la clave, usado como una clave maestra de columna en el almacén de claves.  
  
    **Ejemplos:**  
  
    ```  
    N'My HSM CSP Provider/AlwaysEncryptedKey1'  
    ```  
  
-   **Nombre de proveedor:** MSSQL_CNG_STORE  
  
    **Formato de ruta de acceso de la clave:** *ProviderName*/*KeyIdentifier*  
  
    Donde:  
  
    *ProviderName*  
    Nombre del proveedor de almacenamiento de claves (KSP) que implementa la API de Cryptography Next Generation (CNG) para el almacén de claves maestras de columna. Si usa un HSM como almacén de claves, el nombre del proveedor debe ser el nombre del KSP que el proveedor de HSM proporciona. El proveedor debe estar instalado en un equipo cliente.  
  
    *KeyIdentifier*  
    Identificador de la clave, usado como una clave maestra de columna en el almacén de claves.  
  
    **Ejemplos:**  
  
    ```  
    N'My HSM CNG Provider/AlwaysEncryptedKey1'  
    ```  

-   **Nombre del proveedor:** AZURE_KEY_STORE  
  
    **Formato de clave de la ruta de acceso:** *KeyUrl*  
  
    Donde:  
  
    *KeyUrl*  
    La dirección URL de la clave en Azure Key Vault.

ENCLAVE_COMPUTATIONS  
Especifica que la clave maestra de columna está habilitada para el enclave. Puede compartir todas las claves de cifrado de columna, cifradas con la clave maestra de columna, con un enclave seguro del lado servidor, y usarlas para cálculos dentro del enclave. Para más información, consulte [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

*firma*  
Un literal binario que es un resultado de la firma digital de la *ruta de acceso de la clave* y el valor ENCLAVE_COMPUTATIONS con la clave maestra de columna. La firma refleja si ENCLAVE_COMPUTATIONS se especifica o no. La firma evita que los usuarios no autorizados modifiquen los valores firmados. Un controlador cliente habilitado para Always Encrypted comprueba la firma y devuelve un error a la aplicación si la firma no es válida. La firma debe haberse generada utilizando herramientas de cliente. Para más información, consulte [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="remarks"></a>Comentarios

Cree primero una entrada de metadatos de la clave maestra de columna para poder crear una entrada de metadatos de la clave de cifrado de columna en la base de datos y para poder cifrar cualquier columna de la base de datos mediante Always Encrypted. Una entrada de la clave maestra de columna en los metadatos no contiene la clave maestra de columna real. La clave maestra de columna debe almacenarse en un almacén de claves de columna externo (fuera de SQL Server). El nombre del proveedor de almacenamiento de claves y la ruta de acceso de la clave maestra de columna en los metadatos deben ser válidos para una aplicación cliente. La aplicación cliente debe usar la clave maestra de columna para descifrar una clave de cifrado de columna. La clave de cifrado de columna se cifra con la clave maestra de columna. La aplicación cliente también debe consultar columnas de cifrado.

Se recomienda usar herramientas, como SQL Server Management Studio (SSMS) o PowerShell para administrar claves maestras de columna. Estas herramientas generan firmas (si usa Always Encrypted con enclaves seguros) y emiten automáticamente instrucciones de `CREATE COLUMN MASTER KEY` para crear objetos de metadatos de la clave de cifrado de columna. Consulte [Aprovisionamiento de claves de Always Encrypted mediante SQL Server Management Studio](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-ssms.md) y [Aprovisionamiento de claves de Always Encrypted con PowerShell](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md). 

  
## <a name="permissions"></a>Permisos  
Necesita el permiso **ALTER ANY COLUMN MASTER KEY**.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-column-master-key"></a>A. Crear una clave maestra de columna  
En el siguiente ejemplo se crea una entrada de metadatos de la clave maestra de columna para una clave maestra de columna. La clave maestra de columna está almacenada en el almacén de certificados para las aplicaciones cliente que usan el proveedor MSSQL_CERTIFICATE_STORE para obtener acceso a la clave maestra de columna:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
```  
  
Cree una entrada de metadatos de la clave maestra de columna para una clave maestra de columna. Las aplicaciones cliente, que usan el proveedor MSSQL_CNG_STORE, obtienen acceso a la clave maestra de columna:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'MSSQL_CNG_STORE',    
    KEY_PATH = N'My HSM CNG Provider/AlwaysEncryptedKey'  
);  
```  
  
Cree una entrada de metadatos de la clave maestra de columna para una clave maestra de columna. La clave maestra de columna está almacenada en Azure Key Vault para las aplicaciones cliente que usan el proveedor AZURE_KEY_VAULT para obtener acceso a la clave maestra de columna.  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/  
        MyCMK/4c05f1a41b12488f9cba2ea964b6a700');  
```  
  
Cree una entrada de metadatos de la clave maestra de columna para una clave maestra de columna. La clave maestra de columna está almacenada en un almacén de claves maestras de columna personalizado:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = 'CUSTOM_KEY_STORE',    
    KEY_PATH = 'https://contoso.vault/sales_db_tce_key'  
);  
```  
### <a name="b-creating-an-enclave-enabled-column-master-key"></a>B. Crear una clave maestra de columna habilitada para enclaves  
En el siguiente ejemplo se crea una entrada de metadatos de la clave maestra de columna para una clave maestra de columna habilitada para el enclave. La clave maestra de columna habilitada para el enclave está almacenada en el almacén de certificados para las aplicaciones cliente que usan el proveedor MSSQL_CERTIFICATE_STORE para obtener acceso a la clave maestra de columna:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
     ENCLAVE_COMPUTATIONS (SIGNATURE = 0xA80F5B123F5E092FFBD6014FC2226D792746468C901D9404938E9F5A0972F38DADBC9FCBA94D9E740F3339754991B6CE26543DEB0D094D8A2FFE8B43F0C7061A1FFF65E30FDDF39A1B954F5BA206AAC3260B0657232020542419990261D878318CC38EF4E853970ED69A8D4A306693B8659AAC1C4E4109DE5EB148FD0E1FDBBC32F002C1D8199D313227AD689279D8DEEF91064DF122C19C3767C463723AB663A6F8412AE17E745922C0E3A257EAEF215532588ACCBD440A03C7BC100A38BD0609A119E1EF7C5C6F1B086C68AB8873DBC6487B270340E868F9203661AFF0492CEC436ABF7C4713CE64E38CF66C794B55636BFA55E5B6554AF570CF73F1BE1DBD)
  );  
```  
  
Cree una entrada de metadatos de la clave maestra de columna para una clave maestra de columna habilitada para el enclave. La clave maestra de columna habilitada para el enclave está almacenada en Azure Key Vault para las aplicaciones cliente que usan el proveedor AZURE_KEY_VAULT para obtener acceso a la clave maestra de columna.  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/MyCMK/4c05f1a41b12488f9cba2ea964b6a700');
    ENCLAVE_COMPUTATIONS (SIGNATURE = 0xA80F5B123F5E092FFBD6014FC2226D792746468C901D9404938E9F5A0972F38DADBC9FCBA94D9E740F3339754991B6CE26543DEB0D094D8A2FFE8B43F0C7061A1FFF65E30FDDF39A1B954F5BA206AAC3260B0657232020582413990261D878318CC38EF4E853970ED69A8D4A306693B8659AAC1C4E4109DE5EB148FD0E1FDBBC32F002C1D8199D313227AD689279D8DEEF91064DF122C19C3767C463723AB663A6F8412AE17E745922C0E3A257EAEF215532588ACCBD440A03C7BC100A38BD0609A119E1EF7C5C6F1B086C68AB8873DBC6487B270340E868F9203661AFF0492CEC436ABF7C4713CE64E38CF66C794B55636BFA55E5B6554AF570CF73F1BE1DBD)
  );
```  
  
## <a name="see-also"></a>Vea también
 
* [DROP COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-column-master-key-transact-sql.md)   
* [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md)
* [sys.column_master_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
* [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
* [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md)   
* [Información general de administración de claves de Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
* [Administración de claves para Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
