---
description: CERTENCODED (Transact-SQL)
title: CERTENCODED (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CERTENCODED
- CERTENCODED_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CERTENCODED
ms.assetid: 677a0719-7b9a-4f0b-bc61-41634563f924
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 974485667667296edd9a8d4d6c4187fd93900c1e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193426"
---
# <a name="certencoded-transact-sql"></a>CERTENCODED (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Esta función devuelve la parte pública de un certificado en formato binario. Esta función toma un identificador de certificado como argumento y devuelve el certificado codificado. Para crear un certificado nuevo, pase el resultado binario a **CREATE CERTIFICATE ... WITH BINARY**.
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CERTENCODED ( cert_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*cert_id*  
**certificate_id** del certificado. Busque este valor en sys.certificates; la función [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md) también lo devolverá. *cert_id* tiene el tipo de datos **int**.
  
## <a name="return-types"></a>Tipos de valores devueltos
**varbinary**
  
## <a name="remarks"></a>Observaciones  
Use **CERTENCODED** y **CERTPRIVATEKEY** juntos para devolver diferentes partes de un certificado en formato binario.
  
## <a name="permissions"></a>Permisos  
**CERTENCODED** está disponible públicamente.
  
## <a name="examples"></a>Ejemplos  
  
### <a name="simple-example"></a>Ejemplo sencillo  
En este ejemplo se crea un certificado denominado `Shipping04` y se usa la función **CERTENCODED** para devolver la codificación binaria del certificado. Este ejemplo establece la fecha de expiración del certificado en 31 de octubre de 2040.
  
```sql
CREATE DATABASE TEST1;
GO
USE TEST1
CREATE CERTIFICATE Shipping04
ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'
WITH SUBJECT = 'Sammamish Shipping Records',
EXPIRY_DATE = '20401031';
GO
SELECT CERTENCODED(CERT_ID('Shipping04'));
  
```  
  
### <a name="b-copying-a-certificate-to-another-database"></a>B. Copiar un certificado a otra base de datos  
En el ejemplo más complejo se crean dos bases de datos, `SOURCE_DB` y `TARGET_DB`. Luego se crea un certificado en `SOURCE_DB` y se copia en `TARGET_DB`. Por último, se muestra que los datos cifrados en `SOURCE_DB` se pueden descifrar en `TARGET_DB` con la copia del certificado.
  
Para crear el entorno de ejemplo, cree las bases de datos `SOURCE_DB` y `TARGET_DB` y una clave maestra en cada base de datos. Luego, cree un certificado en `SOURCE_DB`.
  
```sql
USE master;  
GO  
CREATE DATABASE SOURCE_DB;  
GO  
USE SOURCE_DB;  
GO  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0URCE_DB KEY Pa$$W0rd';  
GO  
CREATE DATABASE TARGET_DB;  
GO  
USE TARGET_DB  
GO  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Pa$$W0rd in TARGET_DB';  
GO  
  
-- Create a certificate in SOURCE_DB  
USE SOURCE_DB;  
GO  
CREATE CERTIFICATE SOURCE_CERT WITH SUBJECT = 'SOURCE_CERTIFICATE';  
GO  
```  
  
Después, extraiga la descripción binaria del certificado.
  
```sql
DECLARE @CERTENC VARBINARY(MAX);  
DECLARE @CERTPVK VARBINARY(MAX);  
SELECT @CERTENC = CERTENCODED(CERT_ID('SOURCE_CERT'));  
SELECT @CERTPVK = CERTPRIVATEKEY(CERT_ID('SOURCE_CERT'),  
       'CertEncryptionPa$$word');  
SELECT @CERTENC AS BinaryCertificate;  
SELECT @CERTPVK AS EncryptedBinaryCertificate;  
GO  
```  
  
Luego, cree el certificado duplicado en la base de datos `TARGET_DB`. Modifique el código siguiente para que funcione. Para ello, inserte los dos valores binarios (@CERTENC y @CERTPVK) devueltos en el paso anterior. No especifique estos valores entre comillas.
  
```sql
-- Create the duplicate certificate in the TARGET_DB database  
USE TARGET_DB  
GO  
CREATE CERTIFICATE TARGET_CERT  
FROM BINARY = <insert the binary value of the @CERTENC variable>  
WITH PRIVATE KEY (  
BINARY = <insert the binary value of the @CERTPVK variable>  
, DECRYPTION BY PASSWORD = 'CertEncryptionPa$$word');  
-- Compare the certificates in the two databases  
-- The two certificates should be the same   
-- except for name and (possibly) the certificate_id  
SELECT * FROM SOURCE_DB.sys.certificates  
UNION  
SELECT * FROM TARGET_DB.sys.certificates;  
```  
  
En este código, que se ejecuta como un solo lote, se muestra que `TARGET_DB` puede descifrar datos que se cifraron en `SOURCE_DB`.
  
```sql
USE SOURCE_DB;  
  
DECLARE @CLEARTEXT nvarchar(100);  
DECLARE @CIPHERTEXT varbinary(8000);  
DECLARE @UNCIPHEREDTEXT_Source nvarchar(100);  
SET @CLEARTEXT = N'Hello World';  
SET @CIPHERTEXT = ENCRYPTBYCERT(CERT_ID('SOURCE_CERT'), @CLEARTEXT);  
SET @UNCIPHEREDTEXT_Source =   
    DECRYPTBYCERT(CERT_ID('SOURCE_CERT'), @CIPHERTEXT)  
-- Encryption and decryption result in SOURCE_DB  
SELECT @CLEARTEXT AS SourceClearText, @CIPHERTEXT AS SourceCipherText,   
       @UNCIPHEREDTEXT_Source AS SourceDecryptedText;  
  
-- SWITCH DATABASE  
USE TARGET_DB;  
  
DECLARE @UNCIPHEREDTEXT_Target nvarchar(100);  
SET @UNCIPHEREDTEXT_Target = DECRYPTBYCERT(CERT_ID('TARGET_CERT'), @CIPHERTEXT);  
-- Encryption and decryption result in TARGET_DB  
SELECT @CLEARTEXT AS ClearTextInTarget, @CIPHERTEXT AS CipherTextInTarget, @UNCIPHEREDTEXT_Target AS DecriptedTextInTarget;   
GO  
```  
  
## <a name="see-also"></a>Vea también
[Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
[CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
[sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)
  
  
