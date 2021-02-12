---
title: Criptografía de Always Encrypted | Microsoft Docs
description: Obtenga información sobre los mecanismos y algoritmos de cifrado que extraen el material criptográfico que se usa en la característica Always Encrypted de SQL Server y Azure SQL Database.
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, cryptography system
ms.assetid: ae8226ff-0853-4716-be7b-673ce77dd370
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 04a4515467265456797fbff8f5dc0446748229ba
ms.sourcegitcommit: 6f4fb9cfd0cad06127a6328adc745e2ba7c191d1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99570476"
---
# <a name="always-encrypted-cryptography"></a>Criptografía de Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

  En este documento se describen los mecanismos y algoritmos de cifrado de los que extraer el material criptográfico que se usa en la característica [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)].  
  
## <a name="keys-key-stores-and-key-encryption-algorithms"></a>Claves, almacenes de claves y algoritmos de cifrado de claves
 En Always Encrypted se usan dos tipos de claves: claves maestras de columna y claves de cifrado de columna.  
  
 Una clave maestra de columna (CMK) es una clave de cifrado de claves (por ejemplo, una clave que se usa para cifrar otras claves). Asimismo, se encuentra siempre bajo el control del cliente y se almacena en un almacén de claves externo. Un controlador de cliente habilitado para Always Encrypted interactúa con el almacén de claves a través de un proveedor de almacén de CMK, que puede formar parte de la biblioteca de controladores (un proveedor del sistema o de [!INCLUDE[msCoName](../../../includes/msconame-md.md)]) o de la aplicación de cliente (un proveedor personalizado). Actualmente, las bibliotecas de controladores cliente incluyen proveedores de almacén de claves de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] para el [almacén de certificados de Windows Certificate Store](/windows/desktop/SecCrypto/using-certificate-stores) y módulos de seguridad de hardware (HSM). Para obtener la lista actual de proveedores, consulte [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md). Un desarrollador de aplicaciones puede proporcionar un proveedor personalizado para un almacén arbitrario.  
  
 Una clave de cifrado de columna (CEK) es una clave de cifrado de contenido (por ejemplo, una clave que se utiliza para proteger los datos) que está protegida por una CMK.  
  
 Todos los proveedores de almacenes CMK de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] cifran las CEK por medio de RSA con relleno óptimo de cifrado asimétrico (RSA OAEP). El proveedor de almacén de claves que admite Microsoft Cryptography API: Next Generation (CNG) en .NET Framework ([clase SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider)) usa los parámetros predeterminados especificados por RFC 8017 en la sección A.2.1. Estos parámetros predeterminados utilizan una función hash de SHA-1 y una función de generación de máscara de MGF1 con SHA-1. Todos los demás proveedores de almacenes de claves usan SHA-256. 

Always Encrypted usa internamente módulos criptográficos validados por FIPS 140-2. 

## <a name="data-encryption-algorithm"></a>Algoritmo de cifrado de datos  
 Always Encrypted usa el algoritmo **AEAD_AES_256_CBC_HMAC_SHA_256** para cifrar los datos de la base de datos.  
  
 **AEAD_AES_256_CBC_HMAC_SHA_256** se deriva del borrador de especificación en [https://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05](https://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05). Utiliza un esquema de cifrado autenticado con asociados datos, con un enfoque conocido como "cifrar y, después, generar la MAC". Es decir, el texto no cifrado se cifra primero y se genera la MAC según el texto cifrado resultante.  
  
 Para ocultar los patrones, **AEAD_AES_256_CBC_HMAC_SHA_256** usa el modo de funcionamiento Cipher Block Chaining (CBC), con el que se transmite un valor inicial al sistema conocido como el "vector de inicialización" (IV). Encontrará una descripción completa del modo CBC en [https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf](https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf).  
  
 **AEAD_AES_256_CBC_HMAC_SHA_256** calcula un valor de texto cifrado para un valor de texto no cifrado determinado siguiendo estos pasos.  
  
### <a name="step-1-generating-the-initialization-vector-iv"></a>Paso 1: Generación del vector de inicialización (IV)  
 Always Encrypted admite dos variantes de **AEAD_AES_256_CBC_HMAC_SHA_256**:  
  
-   Aleatorio  
  
-   Determinista  
  
 Para el cifrado aleatorio, el IV se genera aleatoriamente. Como consecuencia, cada vez que se cifre el mismo texto no cifrado, se genera un texto cifrado distinto, lo que evita que se divulgue cualquier información.  
  
```  
When using randomized encryption: IV = Generate cryptographicaly random 128bits  
```  
  
 En el caso del cifrado determinista, el IV no se genera de forma aleatoria, sino que se genera a partir del valor de texto no cifrado mediante el siguiente algoritmo:  
  
```  
When using deterministic encryption: IV = HMAC-SHA-256( iv_key, cell_data ) truncated to 128 bits.  
```  
  
 Donde iv_key se deriva de la CEK del siguiente modo:  
  
```  
iv_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell IV key" + algorithm + CEK_length)  
```  
  
 Se realiza el truncamiento del valor HMAC para ajustar un bloque de datos según se necesite para el IV.
Como resultado, el cifrado determinista siempre genera el mismo texto cifrado para un valor de texto no cifrado concreto, lo que permite deducir si dos valores de texto no cifrado son iguales al comparar sus correspondientes valores de texto cifrado. Esta divulgación de información limitada permite al sistema de base de datos admitir la comparación de igualdad en valores de columna cifrados.  
  
 El cifrado determinista resulta más eficaz para ocultar los patrones, en comparación con las alternativas (como el uso de un valor de IV predefinido).  
  
### <a name="step-2-computing-aes_256_cbc-ciphertext"></a>Paso 2: Cálculo del texto cifrado AES_256_CBC  
 Tras calcular el IV, se genera el texto cifrado **AES_256_CBC** :  
  
```  
aes_256_cbc_ciphertext = AES-CBC-256(enc_key, IV, cell_data) with PKCS7 padding.  
```  
  
 Donde la clave de cifrado (enc_key) se deriva la CEK del siguiente modo:  
  
```  
enc_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell encryption key" + algorithm + CEK_length )  
```  
  
### <a name="step-3-computing-mac"></a>Paso 3: Cálculo de la dirección MAC  
 Posteriormente, la dirección MAC se calcula mediante el siguiente algoritmo:  
  
```  
MAC = HMAC-SHA-256(mac_key, versionbyte + IV + Ciphertext + versionbyte_length)  
```  
  
 Donde:  
  
```  
versionbyte = 0x01 and versionbyte_length = 1
mac_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell MAC key" + algorithm + CEK_length)  
```  
  
### <a name="step-4-concatenation"></a>Paso 4: Concatenación  
 Por último, el valor cifrado se genera mediante la concatenación del byte de la versión del algoritmo, la dirección MAC, el IV y el texto de cifrado AES_256_CBC:  
  
```  
aead_aes_256_cbc_hmac_sha_256 = versionbyte + MAC + IV + aes_256_cbc_ciphertext  
```  
  
## <a name="ciphertext-length"></a>Longitud del texto cifrado  
 Las longitudes (en bytes) de los componentes concretos del texto cifrado **AEAD_AES_256_CBC_HMAC_SHA_256** son las siguientes:  
  
-   versionbyte: 1  
  
-   MAC: 32  
  
-   IV: 16  
  
-   aes_256_cbc_ciphertext: `(FLOOR (DATALENGTH(cell_data)/ block_size) + 1)* block_size`, donde:  
  
    -   block_size es de 16 bytes.  
  
    -   cell_data es un valor de texto sin formato.  
  
     Por lo tanto, el tamaño mínimo de aes_256_cbc_ciphertext es de un bloque, lo que equivale a 16 bytes.  
  
 Por lo tanto, es posible calcular la longitud de un texto cifrado, que sea el resultado de cifrar valores de texto no cifrado determinados (cell_data), mediante la siguiente fórmula:  
  
```  
1 + 32 + 16 + (FLOOR(DATALENGTH(cell_data)/16) + 1) * 16  
```  
  
 Por ejemplo:  
  
-   Un valor de texto no cifrado **int** de tipo Long de 4 bytes se convierte en un valor binario Long de 65 bytes tras el cifrado.  
  
-   Un valor de texto no cifrado **nchar(1000)** de tipo Long de 2000 bytes se convierte en un valor binario Long de 2065 bytes tras el cifrado.  
  
 La siguiente tabla contiene una lista completa de tipos de datos y la longitud del texto cifrado para cada uno de ellos.  
  
|Tipo de datos|Longitud del texto cifrado [bytes]|  
|---------------|---------------------------------|  
|**bigint**|65|  
|**binary**|Varía. Utilice la fórmula anterior.|  
|**bit**|65|  
|**char**|Varía. Utilice la fórmula anterior.|  
|**date**|65|  
|**datetime**|65|  
|**datetime2**|65|  
|**datetimeoffset**|65|  
|**decimal**|81|  
|**float**|65|  
|**geography**|N/D (no compatible).|  
|**geometry**|N/D (no compatible).|  
|**hierarchyid**|N/D (no compatible).|  
|**image**|N/D (no compatible).|  
|**int**|65|  
|**money**|65|  
|**nchar**|Varía. Utilice la fórmula anterior.|  
|**ntext**|N/D (no compatible).|  
|**numeric**|81|  
|**nvarchar**|Varía. Utilice la fórmula anterior.|  
|**real**|65|  
|**smalldatetime**|65|  
|**smallint**|65|  
|**smallmoney**|65|  
|**sql_variant**|N/D (no compatible).|  
|**sysname**|N/D (no compatible).|  
|**text**|N/D (no compatible).|  
|**time**|65|  
|**timestamp**<br /><br /> (**rowversion**)|N/D (no compatible).|  
|**tinyint**|65|  
|**uniqueidentifier**|81|  
|**varbinary**|Varía. Utilice la fórmula anterior.|  
|**varchar**|Varía. Utilice la fórmula anterior.|  
|**xml**|N/D (no compatible).|  
  
## <a name="net-reference"></a>Referencia de .NET  
 Para obtener más información sobre los algoritmos que se han visto en este documento, consulte los archivos **SqlAeadAes256CbcHmac256Algorithm.cs**, **SqlColumnEncryptionCertificateStoreProvider.cs** y **SqlColumnEncryptionCertificateStoreProvider.cs** en la [referencia de .NET](https://referencesource.microsoft.com/).  
  
## <a name="see-also"></a>Consulte también  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 - [Desarrollo de aplicaciones con Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-client-development.md)  
  
