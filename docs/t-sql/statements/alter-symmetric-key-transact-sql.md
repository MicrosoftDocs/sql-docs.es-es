---
description: ALTER SYMMETRIC KEY (Transact-SQL)
title: ALTER SYMMETRIC KEY (Transact-SQL)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SYMMETRIC KEY
- ALTER_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- modifying symmetric keys
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], modifying
- cryptography [SQL Server], symmetric keys
- ALTER SYMMETRIC KEY statement
ms.assetid: d3c776a4-7d71-4e6f-84fc-1db47400c465
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: e233b2a262edf8866e2080c9718a5bbcd29411b6
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421933"
---
# <a name="alter-symmetric-key-transact-sql"></a>ALTER SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Cambia las propiedades de una clave simétrica.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER SYMMETRIC KEY Key_name <alter_option>  
  
<alter_option> ::=  
   ADD ENCRYPTION BY <encrypting_mechanism> [ , ... n ]  
   |   
   DROP ENCRYPTION BY <encrypting_mechanism> [ , ... n ]  
<encrypting_mechanism> ::=  
   CERTIFICATE certificate_name  
   |  
   PASSWORD = 'password'  
   |  
   SYMMETRIC KEY Symmetric_Key_Name  
   |  
   ASYMMETRIC KEY Asym_Key_Name  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *Key_name*  
 Es el nombre por el que se conoce la clave simétrica que se va a cambiar en la base de datos.  
  
 ADD ENCRYPTION BY   
 Agrega el cifrado utilizando el método especificado.  
  
 DROP ENCRYPTION BY  
 Quita el cifrado utilizando el método especificado. No es posible quitar todos los cifrados de una clave simétrica.  
  
 CERTIFICATE *Certificate_name*  
 Especifica el certificado que se utiliza para cifrar la clave simétrica. El certificado debe existir en la base de datos.  
  
 PASSWORD **='** _password_ **'**  
 Especifica la contraseña que se usa para cifrar la clave simétrica. *password* debe cumplir los requisitos de la directiva de contraseñas de Windows del equipo que ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 SYMMETRIC KEY *Symmetric_Key_Name*  
 Especifica la clave simétrica que se utiliza para cifrar la clave simétrica que va a cambiarse. La clave simétrica debe existir en la base de datos y debe estar abierta.  
  
 ASYMMETRIC KEY *Asym_Key_Name*  
 Especifica la clave asimétrica que se utiliza para cifrar la clave simétrica que va a cambiarse. La clave asimétrica debe existir en la base de datos.  
  
## <a name="remarks"></a>Observaciones  
  
> [!CAUTION]  
>  Si se utiliza una contraseña para cifrar una clave simétrica, en lugar de la clave pública de la clave maestra de base de datos, se utiliza el algoritmo de cifrado TRIPLE_DES. Por ello, las claves creadas con un algoritmo de cifrado seguro, como AES, se protegen mediante un algoritmo menos seguro.  
  
 Para cambiar el cifrado de la clave simétrica, utilice las frases ADD ENCRYPTION y DROP ENCRYPTION. No es posible dejar una clave completamente sin cifrado. Por ello, la práctica recomendada es agregar la nueva forma de cifrado antes de quitar la antigua forma de cifrado.  
  
 Para cambiar el propietario de una clave simétrica, use [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
> [!NOTE]  
>  El algoritmo RC4 se admite únicamente por razones de compatibilidad con versiones anteriores. El material nuevo solo se puede cifrar con RC4 o RC4_128 cuando la base de datos tenga el nivel de compatibilidad 90 o 100. (No se recomienda). Use un algoritmo más reciente como uno de los algoritmos AES en su lugar. En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] el material cifrado con RC4 o RC4_128 se puede descifrar en cualquier nivel de compatibilidad.  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso ALTER en la clave simétrica. Si se agrega el cifrado mediante una clave asimétrica o un certificado, es necesario el permiso VIEW DEFINITION en el certificado o en la clave asimétrica. Si se quita el cifrado mediante una clave asimétrica o un certificado, es necesario el permiso CONTROL en el certificado o en la clave asimétrica.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se cambia el método de cifrado que se utiliza para proteger una clave simétrica. La clave simétrica `JanainaKey043` se cifra mediante el certificado `Shipping04` al crear la clave. Dado que la clave no puede almacenarse sin cifrado, en este ejemplo el cifrado se agrega mediante contraseña y se quita mediante certificado.  
  
```sql  
CREATE SYMMETRIC KEY JanainaKey043 WITH ALGORITHM = AES_256   
    ENCRYPTION BY CERTIFICATE Shipping04;  
-- Open the key.   
OPEN SYMMETRIC KEY JanainaKey043 DECRYPTION BY CERTIFICATE Shipping04  
    WITH PASSWORD = '<enterStrongPasswordHere>';   
-- First, encrypt the key with a password.  
ALTER SYMMETRIC KEY JanainaKey043   
    ADD ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';  
-- Now remove encryption by the certificate.  
ALTER SYMMETRIC KEY JanainaKey043   
    DROP ENCRYPTION BY CERTIFICATE Shipping04;  
CLOSE SYMMETRIC KEY JanainaKey043;  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [CLOSE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
