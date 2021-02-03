---
description: ENCRYPTBYPASSPHRASE (Transact-SQL)
title: ENCRYPTBYPASSPHRASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENCRYPTBYPASSPHRASE
- ENCRYPTBYPASSPHRASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTBYPASSPHRASE function
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], ENCRYPTBYPASSPHRASE function
ms.assetid: f8dbb9e6-94d6-40d7-8b38-6833a409d597
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2e9af2f7806a7d4435bd5bd2d417a3a217afc3df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199562"
---
# <a name="encryptbypassphrase-transact-sql"></a>ENCRYPTBYPASSPHRASE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cifra datos mediante una frase de contraseña usando el algoritmo TRIPLE DES con una longitud de clave de 128 bits.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
EncryptByPassPhrase ( { 'passphrase' | @passphrase }   
    , { 'cleartext' | @cleartext }  
  [ , { add_authenticator | @add_authenticator }  
    , { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *passphrase*  
 Frase de contraseña a partir de la cual se genera una clave simétrica.  
  
 @passphrase  
 Variable de tipo **nvarchar**, **char**, **varchar**, **binary**, **varbinary** o **nchar** que contiene una frase de contraseña a partir de la cual se genera una clave simétrica.  
  
 *texto no cifrado*  
 Texto no cifrado que se va a cifrar.  
  
 @cleartext  
 Una variable de tipo **nvarchar**, **char**, **varchar**, **binary**, **varbinary** o **nchar** que contiene el texto sin cifrar. El tamaño máximo es de 8.000 bytes.  
  
 *add_authenticator*  
 Indica si se cifrará un autenticador junto con el texto sin cifrar. 1 si se va a agregar un autenticador. **int**.  
  
 @add_authenticator  
 Indica si se cifrará un hash junto con el texto no cifrado.  
  
 *authenticator*  
 Datos a partir de los cuales se obtiene un autenticador. **sysname**.  
  
 @authenticator  
 Variable que contiene datos a partir de los cuales se obtiene un autenticador.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **varbinary** con un tamaño máximo de 8000 bytes.  
  
## <a name="remarks"></a>Comentarios  
 Una frase de contraseña es una contraseña que incluye espacios. La ventaja de usar una frase de contraseña es que es más fácil recordar una frase con significado que una cadena larga de caracteres.  
  
 Esta función no comprueba la complejidad de la contraseña.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se actualiza un registro de la tabla `SalesCreditCard` y se cifra el valor del número de la tarjeta de crédito almacenado en la columna `CardNumber_EncryptedbyPassphrase` utilizando la clave principal como un autenticador.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create a column in which to store the encrypted data.  
ALTER TABLE Sales.CreditCard   
    ADD CardNumber_EncryptedbyPassphrase VARBINARY(256);   
GO  
-- First get the passphrase from the user.  
DECLARE @PassphraseEnteredByUser NVARCHAR(128);  
SET @PassphraseEnteredByUser   
    = 'A little learning is a dangerous thing!';  
  
-- Update the record for the user's credit card.  
-- In this case, the record is number 3681.  
UPDATE Sales.CreditCard  
SET CardNumber_EncryptedbyPassphrase = EncryptByPassPhrase(@PassphraseEnteredByUser  
    , CardNumber, 1, CONVERT(varbinary, CreditCardID))  
WHERE CreditCardID = '3681';  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [DECRYPTBYPASSPHRASE &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbypassphrase-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
