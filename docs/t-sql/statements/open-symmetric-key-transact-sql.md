---
description: OPEN SYMMETRIC KEY (Transact-SQL)
title: OPEN SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPEN SYMMETRIC KEY
- OPEN_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], opening
- OPEN SYMMETRIC KEY statement
ms.assetid: ff019a7c-c373-46c7-ac43-ffb7e2ee60b3
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 8939563a1c5cfc2c9ca44527493fdf120d9b3897
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421906"
---
# <a name="open-symmetric-key-transact-sql"></a>OPEN SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Descifra una clave simétrica, que queda disponible para su uso.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
OPEN SYMMETRIC KEY Key_name DECRYPTION BY <decryption_mechanism>  
  
<decryption_mechanism> ::=  
    CERTIFICATE certificate_name [ WITH PASSWORD = 'password' ]  
    |  
    ASYMMETRIC KEY asym_key_name [ WITH PASSWORD = 'password' ]  
    |  
    SYMMETRIC KEY decrypting_Key_name  
    |  
    PASSWORD = 'decryption_password'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *Key_name*  
 Es el nombre de la clave simétrica que se va a abrir.  
  
 CERTIFICATE *certificate_name*  
 Es el nombre de un certificado cuya clave privada se usará para descifrar la clave simétrica.  
  
 ASYMMETRIC KEY *asym_key_name*  
 Es el nombre de una clave asimétrica cuya clave privada se usará para descifrar la clave simétrica.  
  
 WITH PASSWORD ='*password*'  
 Es la contraseña utilizada para cifrar la clave privada del certificado o clave asimétrica.  
  
 SYMMETRIC KEY *decrypting_key_name*  
 Es el nombre de una clave simétrica que se usará para descifrar la clave simétrica que se va a abrir.  
  
 PASSWORD ='*password*'  
 Es la contraseña utilizada para proteger la clave simétrica.  
  
## <a name="remarks"></a>Comentarios  
 Las claves simétricas abiertas están enlazadas a la sesión, no al contexto de seguridad. Una clave abierta seguirá disponible hasta que se cierre explícitamente o hasta que finalice la sesión. Si se abre una clave simétrica y después se cambia el contexto, la clave permanecerá abierta y estará disponible en el contexto representado. Puede consultar la información sobre las claves simétricas abiertas en la vista de catálogo [sys.openkeys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-openkeys-transact-sql.md).  
  
 Si la clave simétrica se cifró con otra clave, debe abrirse esta clave primero.  
  
 Si la clave simétrica ya está abierta, la consulta es **NO_OP**.  
  
 Si la contraseña, la clave o el certificado suministrado para descifrar la clave simétrica es incorrecto, la consulta dará un error.  
  
 Las claves simétricas creadas a partir de proveedores de cifrado no pueden abrirse. Las operaciones de cifrado y descifrado mediante este tipo de clave simétrica se realizan correctamente sin la instrucción **OPEN** porque el proveedor de cifrado está abriendo y cerrando la clave.  
  
## <a name="permissions"></a>Permisos  
 El autor de la llamada debe tener algún permiso sobre la clave y no debe tener denegado el permiso VIEW DEFINITION sobre la clave. Según el mecanismo de descifrado, se aplican requisitos adicionales:  
  
-   DECRYPTION BY CERTIFICATE: tener el permiso CONTROL sobre el certificado y conocer la contraseña que cifra la clave privada.  
  
-   DECRYPTION BY ASYMMETRIC KEY: tener el permiso CONTROL sobre la clave asimétrica y conocer la contraseña que cifra su clave privada.  
  
-   DECRYPTION BY PASSWORD: conocer una de las contraseñas utilizadas para cifrar la clave simétrica.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-opening-a-symmetric-key-by-using-a-certificate"></a>A. Abrir una clave simétrica usando un certificado  
 En el ejemplo siguiente se abre la clave simétrica `SymKeyMarketing3` y se descifra mediante la clave privada del certificado `MarketingCert9`.  
  
```sql  
USE AdventureWorks2012;  
OPEN SYMMETRIC KEY SymKeyMarketing3   
    DECRYPTION BY CERTIFICATE MarketingCert9;  
GO  
```  
  
### <a name="b-opening-a-symmetric-key-by-using-another-symmetric-key"></a>B. Abrir una clave simétrica usando otra clave simétrica  
 En el ejemplo siguiente se abre la clave simétrica `MarketingKey11` y se descifra mediante la clave simétrica `HarnpadoungsatayaSE3`.  
  
```sql  
USE AdventureWorks2012;  
-- First open the symmetric key that you want for decryption.  
OPEN SYMMETRIC KEY HarnpadoungsatayaSE3   
    DECRYPTION BY CERTIFICATE sariyaCert01;  
-- Use the key that is already open to decrypt MarketingKey11.  
OPEN SYMMETRIC KEY MarketingKey11   
    DECRYPTION BY SYMMETRIC KEY HarnpadoungsatayaSE3;  
GO   
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [CLOSE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Administración extensible de claves &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
