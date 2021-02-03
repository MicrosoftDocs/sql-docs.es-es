---
description: CERTPROPERTY (Transact-SQL)
title: CERTPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CERTPROPERTY
- CERTPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], schema names
- schemas [SQL Server], names
- CERTPROPERTY function
ms.assetid: 966c09aa-bc4e-45b0-ba53-c8381871f638
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3b345594914d4fcb4439def47251cb63c22d07b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193436"
---
# <a name="certproperty-transact-sql"></a>CERTPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve el valor de una propiedad de certificado especificada.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CertProperty ( Cert_ID , '<PropertyName>' )  
  
<PropertyName> ::=  
   Expiry_Date | Start_Date | Issuer_Name   
   | Cert_Serial_Number | Subject | SID | String_SID   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*Cert_ID*  
El valor del identificador de certificado, con el tipo de datos int.
  
*Expiry_Date*  
La fecha de expiración del certificado.
  
*Start_Date*  
La fecha en la que el certificado pasa a ser válido.
  
*Issuer_Name*  
El nombre del emisor de certificado.
  
*Cert_Serial_Number*  
El número de serie del certificado.
  
*Subject*  
El asunto del certificado.
  
 *SID*  
El SID del certificado. También es el SID de cualquier inicio de sesión o usuario asignado a este certificado.
  
*String_SID*  
El SID del certificado como una cadena de caracteres. También es el SID de cualquier inicio de sesión o usuario asignado al certificado.
  
## <a name="return-types"></a>Tipos de valores devueltos
La especificación de propiedad debe estar entre comillas simples.
  
El tipo de valor devuelto depende de la propiedad especificada en la llamada de función. El tipo de valor devuelto **sql_variant** encapsula todos los valores devueltos.
-   *Expiry_Date* y *Start_Date* devuelven **datetime**.  
-   *Cert_Serial_Number*, *Issuer_Name*, *String_SID* y *Subject* devuelven **nvarchar**.  
-   *SID* devuelve **varbinary**.  
  
## <a name="remarks"></a>Comentarios  
Consulte la información de los certificados en la vista de catálogo [sys.certificates](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).
  
## <a name="permissions"></a>Permisos  
Es necesario tener los permisos apropiados en el certificado y que el autor de la llamada no tenga denegado el permiso VIEW en el certificado. Vea [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md) y [GRANT CERTIFICATE PERMISSIONS &#40;Transact-SQL&#41;](../../t-sql/statements/grant-certificate-permissions-transact-sql.md) para obtener más información sobre los permisos de certificado.
  
## <a name="examples"></a>Ejemplos  
En el siguiente ejemplo se devuelve el asunto del certificado.
  
```sql
-- First create a certificate.  
CREATE CERTIFICATE Marketing19 WITH   
    START_DATE = '04/04/2004' ,  
    EXPIRY_DATE = '07/07/2040' ,  
    SUBJECT = 'Marketing Print Division';  
GO  
  
-- Now use CertProperty to examine certificate  
-- Marketing19's properties.  
DECLARE @CertSubject sql_variant;  
set @CertSubject = CertProperty( Cert_ID('Marketing19'), 'Subject');  
PRINT CONVERT(nvarchar, @CertSubject);  
GO  
```  
  
## <a name="see-also"></a>Vea también
[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
[ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)  
[CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)
[Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)
[sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)
[Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)
  
  
