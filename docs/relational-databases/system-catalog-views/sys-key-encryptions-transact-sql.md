---
description: sys.key_encryptions (Transact-SQL)
title: sys.key_encryptions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.key_encryptions
- key_encryptions_TSQL
- sys.key_encryptions_TSQL
- key_encryptions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.key_encryptions catalog view
ms.assetid: c39cecf8-af63-40b9-98e5-f84a5bf3ae54
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a6ae4cc3ce18b6ca0ac94766441a5e17d8ae1fa2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191388"
---
# <a name="syskey_encryptions-transact-sql"></a>sys.key_encryptions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila por cada cifrado de clave simétrica especificado mediante el uso de la cláusula ENCRYPTION BY de la instrucción CREATE SYMMETRIC KEY.  

  
|Nombres de columna|Tipos de datos|Descripción|  
|------------------|----------------|-----------------|  
|**key_id**|**int**|Id. de la clave cifrada.|  
|**huella**|**varbinary(32)**|Hash SHA-1 del certificado o GUID de la clave simétrica con el que se cifra la clave.|  
|**crypt_type**|**Char (4)**|Tipo de cifrado:<br /><br /> ESKS = Cifrado mediante clave simétrica<br /><br /> ESKP, ESP2 o ESP3 = cifrado por contraseña<br /><br /> EPUC = Cifrado mediante certificado<br /><br /> EPUA = Cifrado mediante clave asimétrica<br /><br /> ESKM = Cifrado mediante clave maestra|  
|**crypt_type_desc**|**nvarchar(60)**|Descripción del tipo de cifrado:<br /><br /> ENCRYPTION BY SYMMETRIC KEY<br /><br /> ENCRYPTION BY PASSWORD <br />(A partir de [!INCLUDE[sssqlv14_md](../../includes/sssqlv14-md.md)] , incluye un número de versión para su uso con CSS).<br /><br /> ENCRYPTION BY CERTIFICATE<br /><br /> ENCRYPTION BY ASYMMETRIC KEY<br /><br /> ENCRYPTION BY MASTER KEY<br /><br /> Nota: Windows DPAPI se usa para proteger la clave maestra de servicio.|  
|**crypt_property**|**varbinary(max)**|Bits firmados o cifrados.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
  
