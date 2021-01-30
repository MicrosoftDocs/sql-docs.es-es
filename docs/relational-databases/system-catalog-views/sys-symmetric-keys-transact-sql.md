---
description: sys.symmetric_keys (Transact-SQL)
title: sys.symmetric_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- symmetric_keys
- sys.symmetric_keys
- sys.symmetric_keys_TSQL
- symmetric_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.symmetric_keys catalog view
ms.assetid: d410eae1-3a52-45de-b9a1-52d2bd93a8eb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 52ea043fb9d8274e30eba1f2d68962e6ec999187
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202007"
---
# <a name="syssymmetric_keys-transact-sql"></a>sys.symmetric_keys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada clave simétrica creada con la instrucción CREATE SYMMETRIC KEY.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre de la clave. Es único en la base de datos.|  
|**principal_id**|**int**|Identificador de la entidad de seguridad de la base de datos que posee la clave.|  
|**symmetric_key_id**|**int**|Id. de la clave. Es único en la base de datos.|  
|**key_length**|**int**|Longitud de la clave, en bits.|  
|**key_algorithm**|**char(2)**|Algoritmo utilizado con la clave:<br /><br /> R2 = RC2<br /><br /> R4 = RC4<br /><br /> D = DES<br /><br /> D3 = Triple DES<br /><br /> DT = TRIPLE_DES_3KEY<br /><br /> DX = DESX<br /><br /> A1 = AES 128<br /><br /> A2 = AES 192<br /><br /> A3 = AES 256<br /><br /> NA = clave EKM|  
|**algorithm_desc**|**nvarchar(60)**|Descripción del algoritmo usado con la clave:<br /><br /> RC2<br /><br /> RC4<br /><br /> DES<br /><br /> Triple_DES<br /><br /> TRIPLE_DES_3KEY<br /><br /> DESX<br /><br /> AES_128<br /><br /> AES_192<br /><br /> AES_256<br /><br /> Descripción de la Administración extensible de claves, solamente los algoritmos (NULL)|  
|**create_date**|**datetime**|Fecha de creación de la clave.|  
|**modify_date**|**datetime**|Fecha de modificación de la clave.|  
|**key_guid**|**uniqueidentifier**|Identificador único global (GUID) asociado a la clave. Se genera automáticamente para claves permanentes. Los GUID para claves temporales se obtienen a partir de la frase de contraseña suministrada por el usuario.|  
|**key_thumbprint**|**sql_variant**|Hash de la tecla 1 SHA. El hash es globalmente único. Para las claves que no sean de Administración extensible de claves, este valor será NULL.|  
|**provider_type**|**nvarchar(120)**|Tipo de proveedor criptográfico:<br /><br /> CRYPTOGRAPHIC PROVIDER = claves de Administración extensible de claves<br /><br /> NULL = Claves que no sean de Administración extensible de claves|  
|**cryptographic_provider_guid**|**uniqueidentifier**|GUID del proveedor criptográfico. Para las claves que no sean de Administración extensible de claves, este valor será NULL.|  
|**cryptographic_provider_algid**|**sql_variant**|Identificador del algoritmo del proveedor criptográfico. Para las claves que no sean de Administración extensible de claves, este valor será NULL.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Observaciones  
 El algoritmo RC4 está obsoleto. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
> [!NOTE]  
>  El algoritmo RC4 se admite únicamente por razones de compatibilidad con versiones anteriores. El material nuevo solo se puede cifrar con RC4 o RC4_128 cuando la base de datos tenga el nivel de compatibilidad 90 o 100. (No se recomienda). Use un algoritmo más reciente como uno de los algoritmos AES en su lugar. En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] el material cifrado con RC4 o RC4_128 se puede descifrar en cualquier nivel de compatibilidad.  
  
 **Clarificación con respecto a los algoritmos DES:**  
  
-   DESX se denominó incorrectamente. Las claves simétricas creadas con ALGORITHM = DESX realmente utilizan el cifrado TRIPLE DES con una clave de 192 bits. No se proporciona el algoritmo DESX. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
-   Las claves simétricas creadas con ALGORITHM = TRIPLE_DES_3KEY utilizan TRIPLE DES con una clave de 192 bits.  
  
-   Las claves simétricas creadas con ALGORITHM = TRIPLE_DES utilizan TRIPLE DES con una clave de 128 bits.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Administración extensible de claves &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
  
