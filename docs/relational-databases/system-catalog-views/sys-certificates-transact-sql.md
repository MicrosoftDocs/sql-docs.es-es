---
description: sys.certificates (Transact-SQL)
title: Sys. Certificates (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- certificates
- certificates_TSQL
- sys.certificates_TSQL
- sys.certificates
dev_langs:
- TSQL
helpviewer_keywords:
- sys.certificates catalog view
ms.assetid: e5046102-a65c-401e-b80d-05636884dec9
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 44572405e33911014f6865333e292f7374bc4733
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202076"
---
# <a name="syscertificates-transact-sql"></a>sys.certificates (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada certificado en la base de datos.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del certificado. Es único en la base de datos.|  
|**certificate_id**|**int**|Id. del certificado. Es único en la base de datos.|  
|**principal_id**|**int**|Id. de la entidad de seguridad de la base de datos que posee este certificado.|  
|**pvt_key_encryption_type**|**char(2)**|Indica cómo se ha cifrado la clave privada.<br /><br /> NA = No hay clave privada para el certificado<br /><br /> MK = La clave privada se ha cifrado mediante la clave maestra<br /><br /> PW = La clave privada se ha cifrado mediante una contraseña definida por el usuario<br /><br /> SK = La clave privada se ha cifrado mediante la clave maestra del servicio|  
|**pvt_key_encryption_type_desc**|**nvarchar(60)**|Descripción de cómo se ha cifrado la clave privada.<br /><br /> NO_PRIVATE_KEY<br /><br /> ENCRYPTED_BY_MASTER_KEY<br /><br /> ENCRYPTED_BY_PASSWORD<br /><br /> ENCRYPTED_BY_SERVICE_MASTER_KEY|  
|**is_active_for_begin_dialog**|**bit**|Si el valor es 1, este certificado se utiliza para iniciar diálogos de servicio cifrados.|  
|**issuer_name**|**nvarchar (442)**|Nombre del emisor del certificado.|  
|**cert_serial_number**|**nvarchar (64)**|Número de serie del certificado.|  
|**Junction**|**varbinary(85)**|SID (identificador de seguridad) de inicio de sesión para este certificado.|  
|**string_sid**|**nvarchar(128)**|Representación en forma de cadena del SID de inicio de sesión para este certificado.|  
|**subject**|**nvarchar(4000)**|Asunto del certificado.|  
|**expiry_date**|**datetime**|Fecha en que expira el certificado.|  
|**start_date**|**datetime**|Fecha en que se valida el certificado.|  
|**huella**|**varbinary(32)**|Hash SHA-1 del certificado. El hash SHA-1 es único globalmente.|  
|**attested_by**|**nvarchar(260)**|Solo para uso del sistema.|  
|**pvt_key_last_backup_date**|**datetime**|Fecha y hora en que se exportó por última vez la clave privada del certificado.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
  
  
