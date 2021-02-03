---
description: DECRYPTBYPASSPHRASE (Transact-SQL)
title: DECRYPTBYPASSPHRASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECRYPTBYPASSPHRASE
- DECRYPTBYPASSPHRASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- decryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], DECRYPTBYPASSPHRASE function
- DECRYPTBYPASSPHRASE function
ms.assetid: ca34b5cd-07b3-4dca-b66a-ed8c6a826c95
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 81e6e954a597e3ea6b7e2b6e983e063c4a1bf8e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207234"
---
# <a name="decryptbypassphrase-transact-sql"></a>DECRYPTBYPASSPHRASE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Esta función descifra los datos que se cifraron originalmente con una frase de contraseña.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DecryptByPassPhrase ( { 'passphrase' | @passphrase }   
    , { 'ciphertext' | @ciphertext }  
  [ , { add_authenticator | @add_authenticator }  
    , { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *passphrase*  
La frase de contraseña que se usa para generar la clave de descifrado.  
  
 @passphrase  
Variable de tipo **char**, **nchar**, **nvarchar** o **varchar** que contiene la frase de contraseña que se usa para generar la clave de descifrado.  
  
'*ciphertext*'  
La cadena de datos cifrados con la clave. *ciphertext* tiene un tipo de datos **varbinary**.  
 
@ciphertext  
Una variable de tipo **varbinary** que contiene los datos cifrados con la clave. La variable *\@ciphertext* tiene un tamaño máximo de 8000 bytes.  
  
*add_authenticator*  
Indica si el proceso de cifrado original incluía, y cifraba, un autenticador junto con el texto sin formato. *add_authenticator* tiene un valor de 1 si el proceso de cifrado usó un autenticador. *add_authenticator* tiene un tipo de datos **int**.  
  
@add_authenticator  
Una variable que indica si el proceso de cifrado original incluía, y cifraba, un autenticador junto con el texto sin formato. *\@add_authenticator* tiene un valor de 1 si el proceso de cifrado ha usado un autenticador. *\@add_authenticator* tiene un tipo de datos **int**.  

*authenticator*  
Los datos que se usaron como base para la generación del autenticador. *autenticador* tiene un tipo de datos **sysname**.  
  
@authenticator  
Una variable que contiene los datos que se usaron como base para la generación de los autenticadores. *\@authenticator* tiene un tipo de datos **sysname**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
**varbinary**, con un tamaño máximo de 8 000 bytes.  
  
## <a name="remarks"></a>Observaciones  
`DECRYPTBYPASSPHRASE` no requiere permisos para su ejecución. `DECRYPTBYPASSPHRASE` devuelve NULL si se recibe la frase de contraseña o información de autenticador erróneas.  
  
`DECRYPTBYPASSPHRASE` usa la frase de contraseña para generar una clave de descifrado. Esta clave de descifrado no se conservará.  
  
Si se incluyó un autenticador en el momento de cifrar el texto cifrado, `DECRYPTBYPASSPHRASE` debe recibir ese mismo autenticador para el proceso de descifrado. Si el valor del autenticador proporcionado para el proceso de descifrado no coincide con el valor del autenticador que se usó originalmente para cifrar los datos, se producirá un error en la operación `DECRYPTBYPASSPHRASE`.  
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se descifra el registro actualizado en [EncryptByPassPhrase](../../t-sql/functions/encryptbypassphrase-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
-- Get the passphrase from the user.  
DECLARE @PassphraseEnteredByUser NVARCHAR(128);  
SET @PassphraseEnteredByUser   
= 'A little learning is a dangerous thing!';  
  
-- Decrypt the encrypted record.  
SELECT CardNumber, CardNumber_EncryptedbyPassphrase   
    AS 'Encrypted card number', CONVERT(varchar,  
    DecryptByPassphrase(@PassphraseEnteredByUser, CardNumber_EncryptedbyPassphrase, 1   
    , CONVERT(varbinary, CreditCardID)))  
    AS 'Decrypted card number' FROM Sales.CreditCard   
    WHERE CreditCardID = '3681';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Elegir un algoritmo de cifrado](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ENCRYPTBYPASSPHRASE &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbypassphrase-transact-sql.md)  
  
  
