---
description: VERIFYSIGNEDBYCERT (Transact-SQL)
title: VERIFYSIGNEDBYCERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- VERIFYSIGNEDBYCERT
- VERIFYSIGNEDBYCERT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- digitally signed data for changes [SQL Server]
- verifying digitally signed data for changes
- testing digitally signed data for changes
- checking digitally signed data for changes
- VERIFYSIGNEDBYCERT
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 4e041f33-60c4-4190-91c7-220d51dd6c8f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6a0d08e5d4c85e34617685614b4305e3cd7b3529
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210618"
---
# <a name="verifysignedbycert-transact-sql"></a>VERIFYSIGNEDBYCERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Comprueba si se han cambiado los datos firmados digitalmente desde que se firmaron.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
VerifySignedByCert( Cert_ID , signed_data , signature )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *Cert_ID*  
 Es el identificador de un certificado de la base de datos. *Cert_ID* es **int**.  
  
 *signed_data*  
 Es una variable de tipo **nvarchar**, **char**, **varchar** o **nchar** que contiene los datos que se han firmado con un certificado.  
  
 *firma*  
 Es la firma adjunta a los datos firmados. *signature* es **varbinary**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
 Devuelve 1 cuando no se han cambiado los datos firmados; de lo contrario, devuelve 0.  
  
## <a name="remarks"></a>Comentarios  
 **VerifySignedBycert** descifra la firma de los datos utilizando la clave pública del certificado especificado y compara el valor descifrado con un hash MD5 de los datos calculado recientemente. Si los valores coinciden, se confirma que la firma es válida.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW DEFINITION en el certificado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-verifying-that-signed-data-has-not-been-tampered-with"></a>A. Comprobar que los datos firmados no se han manipulado  
 En el siguiente ejemplo se comprueba si la información en `Signed_Data` ha cambiado desde que se firmó con el certificado denominado `Shipping04`. La firma se almacena en `DataSignature`. El certificado, `Shipping04`, se pasa a `Cert_ID`, que devuelve el identificador del certificado en la base de datos. Si `VerifySignedByCert` devuelve 1, la firma es correcta. Si `VerifySignedByCert` devuelve 0, los datos de `Signed_Data` no son los datos que se usaron para generar `DataSignature`. En este caso, `Signed_Data` se ha cambiado desde que se firmó o `Signed_Data` se firmó con otro certificado.  
  
```sql
SELECT Data, VerifySignedByCert( Cert_Id( 'Shipping04' ),  
    Signed_Data, DataSignature ) AS IsSignatureValid  
FROM [AdventureWorks2012].[SignedData04]   
WHERE Description = N'data signed by certificate ''Shipping04''';  
GO  
```  
  
### <a name="b-returning-only-records-that-have-a-valid-signature"></a>B. Devolver solo los registros que tienen una firma válida  
 Esta consulta solo devuelve los registros que no han cambiado desde que se firmaron con el certificado `Shipping04`.  
  
```sql
SELECT Data FROM [AdventureWorks2012].[SignedData04]   
WHERE VerifySignedByCert( Cert_Id( 'Shipping04' ), Data,   
    DataSignature ) = 1   
AND Description = N'data signed by certificate ''Shipping04''';  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)   
 [SIGNBYCERT &#40;Transact-SQL&#41;](../../t-sql/functions/signbycert-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
