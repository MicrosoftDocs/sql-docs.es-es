---
description: DECRYPTBYKEYAUTOASYMKEY (Transact-SQL)
title: DECRYPTBYKEYAUTOASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECRYPTBYKEYAUTOASYMKEY_TSQL
- DECRYPTBYKEYAUTOASYMKEY
dev_langs:
- TSQL
helpviewer_keywords:
- DECRYPTBYKEYAUTOASYMSKEY function
ms.assetid: 5521d4cf-740c-4ede-98b6-4ba90b84e32d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 66764a72445cc161abe878f2713ec122d5627067
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207005"
---
# <a name="decryptbykeyautoasymkey-transact-sql"></a>DECRYPTBYKEYAUTOASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Esta función descifra los datos cifrados. Para ello, en primer lugar descifra una clave simétrica con una clave asimétrica independiente y, después, descifra los datos cifrados con la clave simétrica que se extraen en el primer "paso".  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DecryptByKeyAutoAsymKey ( akey_ID , akey_password   
    , { 'ciphertext' | @ciphertext }  
  [ , { add_authenticator | @add_authenticator }   
  [ , { authenticator | @authenticator } ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *akey_ID*  
Es el identificador de la clave asimétrica que se usa para cifrar la clave simétrica. *akey_ID* tiene un tipo de datos **int**.  
  
 *akey_password*  
La contraseña que protege la clave asimétrica. *akey_password* puede tener un valor NULL si la clave maestra de la base de datos protege la clave privada asimétrica. *akey_password* tiene un tipo de datos **nvarchar**.  
  
 *ciphertext* Los datos que se cifraron con la clave. *ciphertext* tiene un tipo de datos **varbinary**.  
  
 @ciphertext  
Una variable de tipo **varbinary** que contiene los datos cifrados con la clave simétrica.  
  
 *add_authenticator*  
Indica si el proceso de cifrado original incluía, y cifraba, un autenticador junto con el texto sin formato. Debe coincidir con el valor que se pasa a [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durante el proceso de cifrado de datos. *add_authenticator* tiene un valor de 1 si el proceso de cifrado usó un autenticador. *add_authenticator* tiene un tipo de datos **int**.  
  
 @add_authenticator  
Una variable que indica si el proceso de cifrado original incluía, y cifraba, un autenticador junto con el texto sin formato. Debe coincidir con el valor que se pasa a [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durante el proceso de cifrado de datos. *\@add_authenticator* tiene un tipo de datos **int**.
  
 *authenticator*  
Los datos que se usaron como base para la generación del autenticador. Debe coincidir con el valor que se proporcionó a [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *autenticador* tiene un tipo de datos **sysname**.  
  
 @authenticator  
Una variable que contiene datos a partir de los que se genera un autenticador. Debe coincidir con el valor que se proporcionó a [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *\@authenticator* tiene un tipo de datos **sysname**.  
  
@add_authenticator  
Una variable que indica si el proceso de cifrado original incluía, y cifraba, un autenticador junto con el texto sin formato. Debe coincidir con el valor que se pasa a [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) durante el proceso de cifrado de datos. *\@add_authenticator* tiene un tipo de datos **int**.  

*authenticator*  
Los datos que se usaron como base para la generación del autenticador. Debe coincidir con el valor que se proporcionó a [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *autenticador* tiene un tipo de datos **sysname**.

@authenticator  
Una variable que contiene datos a partir de los que se genera un autenticador. Debe coincidir con el valor que se proporcionó a [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *\@authenticator* tiene un tipo de datos **sysname**.  

## <a name="return-types"></a>Tipos de valor devuelto  
**varbinary**, con un tamaño máximo de 8 000 bytes.  
  
## <a name="remarks"></a>Observaciones  
`DECRYPTBYKEYAUTOASYMKEY` combina las funciones de `OPEN SYMMETRIC KEY` y `DECRYPTBYKEY`. En una sola operación, primero descifra una clave simétrica y después la usa para descifrar el texto cifrado.  
  
## <a name="permissions"></a>Permisos  
Se requiere el permiso `VIEW DEFINITION` en la clave simétrica y el permiso `CONTROL` en la clave asimétrica.  
  
## <a name="examples"></a>Ejemplos
En este ejemplo se muestra cómo `DECRYPTBYKEYAUTOASYMKEY` puede simplificar el código de descifrado. Este código se debería ejecutar en una base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] que aún no tenga una clave maestra de base de datos.  

```sql  
--Create the keys and certificate.  
USE AdventureWorks2012;  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'mzkvdMlk979438teag$$ds987yghn)(*&4fdg^';  
OPEN MASTER KEY DECRYPTION BY PASSWORD = 'mzkvdMlk979438teag$$ds987yghn)(*&4fdg^';  
CREATE ASYMMETRIC KEY SSN_AKey   
    WITH ALGORITHM = RSA_2048 ;   
GO  
CREATE SYMMETRIC KEY SSN_Key_02 WITH ALGORITHM = DES  
    ENCRYPTION BY ASYMMETRIC KEY SSN_AKey;  
GO  
--  
--Add a column of encrypted data.  
ALTER TABLE HumanResources.Employee  
    ADD EncryptedNationalIDNumber2 varbinary(128);   
OPEN SYMMETRIC KEY SSN_Key_02  
   DECRYPTION BY ASYMMETRIC KEY SSN_AKey;  
UPDATE HumanResources.Employee  
SET EncryptedNationalIDNumber2  
    = EncryptByKey(Key_GUID('SSN_Key_02'), NationalIDNumber);  
GO  
--Close the key used to encrypt the data.  
CLOSE SYMMETRIC KEY SSN_Key_02;  
--  
--There are two ways to decrypt the stored data.  
--  
--OPTION ONE, using DecryptByKey()  
--1. Open the symmetric key.  
--2. Decrypt the data.  
--3. Close the symmetric key.  
OPEN SYMMETRIC KEY SSN_Key_02  
   DECRYPTION BY ASYMMETRIC KEY SSN_AKey;  
SELECT NationalIDNumber, EncryptedNationalIDNumber2    
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber2))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
CLOSE SYMMETRIC KEY SSN_Key_02;  
--  
--OPTION TWO, using DecryptByKeyAutoAsymKey()  
SELECT NationalIDNumber, EncryptedNationalIDNumber2   
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKeyAutoAsymKey ( AsymKey_ID('SSN_AKey') , NULL ,EncryptedNationalIDNumber2))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbykey-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
