---
description: ENCRYPTBYASYMKEY (Transact-SQL)
title: ENCRYPTBYASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENCRYPTBYASYMKEY
- ENCRYPTBYASYMKEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTBYASYMKEY function
- encryption [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], ENCRYPTBYASYMKEY function
ms.assetid: 86bb2588-ab13-4db2-8f3c-42c9f572a67b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3c947213e205a823e16fd2f83256c452c6da0f19
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158334"
---
# <a name="encryptbyasymkey-transact-sql"></a>ENCRYPTBYASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Esta función cifra los datos con una clave asimétrica.  
  
 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
EncryptByAsymKey ( Asym_Key_ID , { 'plaintext' | @plaintext } )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*asym_key_ID*  
Id. de una clave asimétrica en la base de datos. *asym_key_ID* tiene un tipo de datos **int**.  
  
*cleartext*  
Cadena de datos que `ENCRYPTBYASYMKEY` cifrará con la clave asimétrica. *cleartext* puede tener un tipo de
 
+ **binary**
+ **char**
+ **nchar**
+ **nvarchar**
+ **varbinary**
  
o bien
  
+ **varchar**
 
datos.  
  
**\@plaintext**  
Una variable que contiene un valor que `ENCRYPTBYASYMKEY` cifrará con la clave asimétrica. **\@plaintext** puede tener un
  
+ **binary**
+ **char**
+ **nchar**
+ **nvarchar**
+ **varbinary**
  
o bien
  
+ **varchar**
 
datos.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
**varbinary**, con un tamaño máximo de 8 000 bytes.  
  
## <a name="remarks"></a>Observaciones  
Las operaciones de cifrado y descifrado en las que se usan claves asimétricas consumen muchos recursos y, por tanto, son muy costosas en comparación con el descifrado y cifrado de claves simétricas. Se recomienda que los desarrolladores eviten el cifrado y descifrado de claves asimétricas en grandes conjuntos de datos (por ejemplo, conjuntos de datos de usuario almacenados en tablas de base de datos). En su lugar, se recomienda que los desarrolladores cifren primero los datos con una clave simétrica segura y, después, cifren esa clave simétrica con una clave asimétrica.  
  
En función del algoritmo, `ENCRYPTBYASYMKEY` devuelve **NULL** si la entrada supera un número determinado de bytes. Los límites específicos:

+ una clave RSA de 512 bits puede cifrar hasta 53 bytes
+ una clave de 1024 bits puede cifrar hasta 117 bytes
+ una clave de 2048 bits puede cifrar hasta 245 bytes

En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tanto los certificados como las claves asimétricas sirven de contenedores de claves RSA.  
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se cifra el texto almacenado en `@cleartext` con la clave asimétrica `JanainaAsymKey02`. La instrucción inserta los datos cifrados en la tabla `ProtectedData04`.  
  
```sql  
INSERT INTO AdventureWorks2012.Sales.ProtectedData04   
    VALUES( N'Data encrypted by asymmetric key ''JanainaAsymKey02''',  
    EncryptByAsymKey(AsymKey_ID('JanainaAsymKey02'), @cleartext) );  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [DECRYPTBYASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbyasymkey-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
