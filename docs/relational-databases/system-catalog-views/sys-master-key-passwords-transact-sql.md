---
description: sys.master_key_passwords (Transact-SQL)
title: sys.master_key_passwords (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/06/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.master_key_passwords_TSQL
- master_key_passwords_TSQL
- sys.master_key_passwords
- master_key_passwords
dev_langs:
- TSQL
helpviewer_keywords:
- sys.master_key_passwords catalog view
ms.assetid: b8e18cff-a9e6-4386-98ce-1cd855506e03
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e5aeeb3cf62d687b6f59a93ba07ed19b5f7e3fce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191344"
---
# <a name="sysmaster_key_passwords-transact-sql"></a>sys.master_key_passwords (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve una fila por cada contraseña de clave maestra de base de datos agregada mediante el **sp_control_dbmasterkey_password** procedimiento almacenado. Las contraseñas que se usan para proteger las claves maestras se almacenan en el almacén de credenciales. El nombre de la credencial sigue este formato: # #DBMKEY_<database_family_guid>_<random_password_guid> # #. La contraseña se almacena como el secreto de la credencial. Para cada contraseña que se agrega mediante **sp_control_dbmasterkey_password**, hay una fila en **Sys. Credentials**.  
  
 Cada fila de esta vista muestra un **credential_id** y el **family_guid** de una base de datos cuya clave maestra está protegida por la contraseña asociada a esa credencial. Una combinación con **Sys. Credentials** en el **credential_id** devolverá campos útiles, como el **create_date** y el nombre de la credencial.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**credential_id**|**int**|Id. de la credencial a la que pertenece la contraseña. Este identificador es único en la instancia del servidor.|  
|**family_guid**|**uniqueidentifier**|Id. único de la base de datos original cuando se creó. Este GUID sigue igual después de restaurar o adjuntar la base de datos, incluso si se cambia el nombre de la base de datos.<br /><br /> Si se produce un error al descifrar automáticamente la clave maestra de servicio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza el **family_guid** para identificar las credenciales que pueden contener la contraseña usada para proteger la clave maestra de la base de datos.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sp_control_dbmasterkey_password &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md)   
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
