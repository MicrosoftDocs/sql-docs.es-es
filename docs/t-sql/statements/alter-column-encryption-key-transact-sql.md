---
description: ALTER COLUMN ENCRYPTION KEY (Transact-SQL)
title: ALTER COLUMN ENCRYPTION KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER COLUMN ENCRYPTION
- ALTER_COLUMN_ENCRYPTION_TSQL
- ALTER COLUMN ENCRYPTION KEY
- ALTER_COLUMN_ENCRYPTION_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column encryption key, alter
- ALTER COLUMN ENCRYPTION KEY statement
ms.assetid: c79a220d-e178-4091-a330-c924cc0f0ae0
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: b9aaf24adad7cc1451b32173bedefc30fbff60eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191047"
---
# <a name="alter-column-encryption-key-transact-sql"></a>ALTER COLUMN ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Modifica una clave de cifrado de columna en una base de datos agregando o quitando un valor cifrado. Una clave de cifrado de columna puede tener hasta dos valores de cara a la rotación de la correspondiente clave maestra de columna. La clave de cifrado de columna se usa para cifrar columnas mediante [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) o [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md). Antes de agregar un valor de clave de cifrado de columna, debe definir la clave maestra de columna que se usó para cifrar el valor mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o la instrucción [CREATE MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER COLUMN ENCRYPTION KEY key_name   
    [ ADD | DROP ] VALUE   
    (  
        COLUMN_MASTER_KEY = column_master_key_name   
        [, ALGORITHM = 'algorithm_name' , ENCRYPTED_VALUE =  varbinary_literal ]   
    ) [;]  
```  

## <a name="arguments"></a>Argumentos
 *key_name*  
 Clave de cifrado de columna que va a cambiar.  
  
 *column_master_key_name*  
 Especifica el nombre de la clave maestra de columna que se usa para cifrar la clave de cifrado de columna.  
  
 *algorithm_name*  
 Nombre del algoritmo de cifrado usado para cifrar el valor. El algoritmo para los proveedores del sistema debe ser **RSA_OAEP**. Este argumento no es válido cuando se elimina un valor de clave de cifrado de columna.  
  
 *varbinary_literal*  
 BLOB de la clave de cifrado de columna con la clave de cifrado maestra especificada. Este argumento no es válido cuando se elimina un valor de clave de cifrado de columna.  
  
> [!WARNING]  
>  En esta instrucción no se deben pasar nunca valores de clave de cifrado de columna con texto simple. Si lo hace, perderá las ventajas que ofrece esta característica.  

## <a name="remarks"></a>Comentarios
Normalmente, una clave de cifrado de columna se crea con un solo valor cifrado. Cuando es necesario rotar una clave maestra de columna (la clave maestra de columna actual debe reemplazarse con la nueva clave maestra de columna), puede agregar un nuevo valor de la clave de cifrado de columna, que se cifra con la nueva clave maestra de columna. Este flujo de trabajo le permite garantizar que las aplicaciones cliente pueden acceder a los datos cifrados con la clave de cifrado de columna, mientras la nueva clave maestra de columna se pone a disposición de las aplicaciones cliente. Un controlador compatible con Always Encrypted en una aplicación cliente que no tenga acceso a la nueva clave maestra podrá usar el valor de clave de cifrado de columna que está cifrado con la clave maestra de columna para tener acceso a datos confidenciales. Los algoritmos de cifrado, compatibles con Always Encrypted, necesitan que el valor de texto simple tenga 256 bits. 
 
Se recomienda usar herramientas, como SQL Server Management Studio (SSMS) o PowerShell para rotar las claves maestras de columna. Consulte [Rotación de claves de Always Encrypted mediante SQL Server Management Studio](../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-ssms.md) y [Rotación de claves de Always Encrypted con de PowerShell](../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md).

Un valor cifrado se debe generar usando un proveedor de almacén de claves que encapsule el almacén de claves que contiene la clave maestra de columna.  

 Las claves maestras de columna se rotan por las siguientes razones:
- Puede que la normativa de cumplimiento exija que las claves se roten periódicamente.
- Una clave maestra de columna está en riesgo y es necesario rotarla por motivos de seguridad.
- Para habilitar o deshabilitar el uso compartido de claves de cifrado de columna con enclave seguro en el lado servidor. Por ejemplo, si la clave maestra de columna actual no admite cálculos de enclave (no se ha definido con la propiedad ENCLAVE_COMPUTATIONS) y quiere habilitar estos cálculos en columnas protegidas con una clave de cifrado de columna que se cifra con la clave maestra de columna, deberá reemplazar la clave maestra de columna por la nueva clave con la propiedad ENCLAVE_COMPUTATIONS. [Información general de administración de claves de Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md) y [Administración de claves para Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md).

Eche un vistazo a [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md), [sys.column_encryption_keys  &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) y [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md) para saber más sobre las claves de cifrado de columna.  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso **ALTER ANY COLUMN ENCRYPTION KEY** para la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-adding-a-column-encryption-key-value"></a>A. Agregar un valor de clave de cifrado de columna  
 En el siguiente ejemplo se modifica una clave de cifrado de columna denominada `MyCEK`.  
  
```sql  
ALTER COLUMN ENCRYPTION KEY MyCEK  
ADD VALUE  
(  
    COLUMN_MASTER_KEY = MyCMK2,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x016E000001630075007200720065006E00740075007300650072002F006D0079002F0064006500650063006200660034006100340031003000380034006200350033003200360066003200630062006200350030003600380065003900620061003000320030003600610037003800310066001DDA6134C3B73A90D349C8905782DD819B428162CF5B051639BA46EC69A7C8C8F81591A92C395711493B25DCBCCC57836E5B9F17A0713E840721D098F3F8E023ABCDFE2F6D8CC4339FC8F88630ED9EBADA5CA8EEAFA84164C1095B12AE161EABC1DF778C07F07D413AF1ED900F578FC00894BEE705EAC60F4A5090BBE09885D2EFE1C915F7B4C581D9CE3FDAB78ACF4829F85752E9FC985DEB8773889EE4A1945BD554724803A6F5DC0A2CD5EFE001ABED8D61E8449E4FAA9E4DD392DA8D292ECC6EB149E843E395CDE0F98D04940A28C4B05F747149B34A0BAEC04FFF3E304C84AF1FF81225E615B5F94E334378A0A888EF88F4E79F66CB377E3C21964AACB5049C08435FE84EEEF39D20A665C17E04898914A85B3DE23D56575EBC682D154F4F15C37723E04974DB370180A9A579BC84F6BC9B5E7C223E5CBEE721E57EE07EFDCC0A3257BBEBF9ADFFB00DBF7EF682EC1C4C47451438F90B4CF8DA709940F72CFDC91C6EB4E37B4ED7E2385B1FF71B28A1D2669FBEB18EA89F9D391D2FDDEA0ED362E6A591AC64EF4AE31CA8766C259ECB77D01A7F5C36B8418F91C1BEADDD4491C80F0016B66421B4B788C55127135DA2FA625FB7FD195FB40D90A6C67328602ECAF3EC4F5894BFD84A99EB4753BE0D22E0D4DE6A0ADFEDC80EB1B556749B4A8AD00E73B329C95827AB91C0256347E85E3C5FD6726D0E1FE82C925D3DF4A9  
);  
GO  
  
```  
  
### <a name="b-dropping-a-column-encryption-key-value"></a>B. Quitar un valor de clave de cifrado de columna  
 En el siguiente ejemplo se quita un valor para modificar una clave de cifrado de columna denominada `MyCEK`.  
  
```sql  
ALTER COLUMN ENCRYPTION KEY MyCEK  
DROP VALUE  
(  
    COLUMN_MASTER_KEY = MyCMK  
);  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md)   
 [DROP COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-column-encryption-key-transact-sql.md)   
 [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-master-key-transact-sql.md)   
 [Always Encrypted &#40;motor de base de datos&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [sys.column_encryption_keys  &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)   
 [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Información general sobre la administración de claves de Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
 [Administración de claves para Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
  
  
