---
description: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
title: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_ENCRYPTION_KEY_TSQL
- ENCRYPTION_KEY_TSQL
- sql13.swb.dbencryptionkeyo.f1
- ENCRYPTION KEY
- DATABASE ENCRYPTION KEY
- CREATE_DATABASE_ENCRYPTION_KEY_TSQL
- CREATE DATABASE ENCRYPTION KEY
- sql13.swb.dbencryptionkeyg.f1
- CREATE DATABASE ENCRYPTION
- CREATE_DATABASE_ENCRYPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database encryption key
- CREATE DATABASE ENCRYPTION KEY statement
- database encryption key, create
ms.assetid: 2ee95a32-5140-41bd-9ab3-a947b9990688
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: af62041c92ca78b52204aac7d736465d5d01012b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270432"
---
# <a name="create-database-encryption-key-transact-sql"></a>CREATE DATABASE ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

 Crea una clave de cifrado que se utiliza para cifrar de forma transparente una base de datos. Para más información sobre el cifrado de bases de datos transparente, vea [Cifrado de datos transparente &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER   
    {  
        CERTIFICATE Encryptor_Name |  
        ASYMMETRIC KEY Encryptor_Name  
    }  
[ ; ]  
```  
  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name   
[ ; ]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

WITH ALGORITHM = { AES_128 \| AES_192 \| AES_256 \| TRIPLE_DES_3KEY  }  
Especifica el algoritmo de cifrado utilizado para la clave de cifrado.

> [!WARNING]
> A partir de SQL Server 2016, todos los algoritmos están en desuso, excepto AES_128, AES_192 y AES_256. Para usar algoritmos anteriores (no se recomienda), debe establecer la base de datos en el nivel de compatibilidad de base de datos 120 o inferior.  
  
ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name  
Especifica el nombre del sistema de cifrado utilizado para cifrar la clave de cifrado.  
  
ENCRYPTION BY SERVER ASYMMETRIC KEY Encryptor_Name  
Especifica el nombre de la clave asimétrica que se usa para cifrar la clave de cifrado de base de datos. Para cifrar la clave de cifrado de la base de datos con una clave asimétrica, la clave asimétrica debe residir en un proveedor extensible de administración de claves.  
  
## <a name="remarks"></a>Observaciones  
Se necesita una clave de cifrado de base de datos para que una base de datos se pueda cifrar mediante el *cifrado de datos transparente* (TDE). Cuando se cifra una base de datos de forma transparente, toda la base de datos se cifra en el nivel de archivo, sin ninguna modificación especial de código. El certificado o la clave asimétrica que se usa para cifrar la clave de cifrado de base de datos se debe ubicar en la base de datos maestra del sistema.  
  
Las instrucciones de cifrado de la base de datos se permiten únicamente en bases de datos de usuario.  
  
La clave de cifrado de la base de datos no se puede exportar de la base de datos. Solo está disponible para el sistema, para los usuarios que tienen los permisos de depuración en el servidor y para los usuarios que tienen acceso a los certificados que cifran y descifran la clave de cifrado de la base de datos.  
  
La clave de cifrado de base de datos no tiene que regenerarse cuando se cambia el propietario de la base de datos (dbo).  
  
Una clave de cifrado de base de datos se crea automáticamente para una base de datos [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. No es necesario crear una clave mediante la instrucción CREATE DATABASE ENCRYPTION KEY.  
  
## <a name="permissions"></a>Permisos  
Requiere el permiso CONTROL en la base de datos y el permiso VIEW DEFINITION en el certificado o la clave asimétrica utilizados para cifrar la clave de cifrado de base de datos.  
  
## <a name="examples"></a>Ejemplos  
Para obtener más ejemplos con TDE, vea [Cifrado de datos transparente &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md), [Habilitar TDE en SQL Server con EKM](../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md) y [Administración extensible de claves con el Almacén de claves de Azure &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  
  
En el siguiente ejemplo se crea una clave de cifrado de base de datos mediante el algoritmo `AES_256` y se protege la clave privada con un certificado denominado `MyServerCert`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
[Cifrado de datos transparente &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md)   
[Cifrado de SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
[SQL Server y claves de cifrado de base de datos &#40;motor de base de datos&#41;](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
[Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
[Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
[ALTER DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
[DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)   
[sys.dm_database_encryption_keys &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)  
    
