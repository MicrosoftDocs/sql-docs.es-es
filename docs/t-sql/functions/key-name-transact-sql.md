---
description: KEY_NAME (Transact-SQL)
title: KEY_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- KEY_NAME_TSQL
- KEY_NAME
dev_langs:
- TSQL
helpviewer_keywords:
- KEY_NAME function
ms.assetid: 7b693e5d-2325-4bf9-9b45-ad6a23374b41
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 857dff1b1ddb9e732cb065560d922fb5efbd6559
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183961"
---
# <a name="key_name-transact-sql"></a>KEY_NAME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el nombre de la clave simétrica a partir de un GUID de clave simétrica o de texto de cifrado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
KEY_NAME ( ciphertext | key_guid )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *ciphertext*  
 Es el texto cifrado por la clave simétrica. *cyphertext* es de tipo **varbinary(8000)**.  
  
 *key_guid*  
 Es el GUID de la clave simétrica. *key_guid* es de tipo **uniqueidentifier**.  
  
## <a name="returned-types"></a>Tipos devueltos  
 **varchar(128)**  
  
## <a name="permissions"></a>Permisos  
 A partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], la visibilidad de los metadatos se limita a los elementos protegibles que son propiedad de un usuario o sobre los que el usuario tienen algún permiso. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-displaying-the-name-of-a-symmetric-key-using-the-key_guid"></a>A. Mostrar el nombre de una clave simétrica utilizando key_guid  
 La base de datos **maestra** contiene una clave simétrica denominada ##MS_ServiceMasterKey##. En el ejemplo siguiente se obtiene el GUID de esa clave a partir de la vista de administración dinámica sys.symmetric_keys, se asigna a una variable y, a continuación, se pasa esa variable a la función KEY_NAME para mostrar la forma de devolver el nombre que corresponde al GUID.  
  
```sql  
USE master;  
GO  
DECLARE @guid uniqueidentifier ;  
SELECT @guid = key_guid FROM sys.symmetric_keys  
WHERE name = '##MS_ServiceMasterKey##' ;  
-- Demonstration of passing a GUID to KEY_NAME to receive a name  
SELECT KEY_NAME(@guid) AS [Name of Key];  
```  
  
### <a name="b-displaying-the-name-of-a-symmetric-key-using-the-cipher-text"></a>B. Mostrar el nombre de una clave simétrica utilizando el texto de cifrado  
 En el ejemplo siguiente se muestra el proceso completo de crear una clave simétrica y rellenar los datos en una tabla. A continuación, el ejemplo muestra cómo KEY_NAME devuelve el nombre de la clave al pasar el texto cifrado.  
  
```sql 
-- Create a symmetric key  
CREATE SYMMETRIC KEY TestSymKey   
   WITH ALGORITHM = AES_128,  
   KEY_SOURCE = 'The square of the hypotenuse is equal to the sum of the squares of the sides',  
   IDENTITY_VALUE = 'Pythagoras'  
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y' ;  
GO  
-- Create a table for the demonstration  
CREATE TABLE DemoKey  
(IDCol INT IDENTITY PRIMARY KEY,  
SecretCol VARBINARY(256) NOT NULL)  
GO  
-- Open the symmetric key if not already open  
OPEN SYMMETRIC KEY TestSymKey   
    DECRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y';  
GO  
-- Insert a row into the DemoKey table  
DECLARE @key_GUID uniqueidentifier;  
SELECT @key_GUID = key_guid FROM sys.symmetric_keys  
WHERE name LIKE 'TestSymKey' ;  
INSERT INTO DemoKey(SecretCol)  
VALUES ( ENCRYPTBYKEY (@key_GUID, 'EncryptedText'))  
GO  
-- Verify the DemoKey data  
SELECT * FROM DemoKey;  
GO  
-- Decrypt the data  
DECLARE @ciphertext VARBINARY(256);  
SELECT @ciphertext = SecretCol  
FROM DemoKey WHERE IDCol = 1 ;  
SELECT CAST (  
DECRYPTBYKEY( @ciphertext)  
AS VARCHAR(100) ) AS SecretText ;  
-- Use KEY_NAME to view the name of the key  
SELECT KEY_NAME(@ciphertext) AS [Name of Key] ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [DECRYPTBYKEYAUTOASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbykeyautoasymkey-transact-sql.md)  
  
  
