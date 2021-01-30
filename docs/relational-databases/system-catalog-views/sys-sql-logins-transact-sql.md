---
description: sys.sql_logins (Transact-SQL)
title: sys.sql_logins (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sql_logins_TSQL
- sql_logins_TSQL
- sys.sql_logins
- sql_logins
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_logins catalog view
ms.assetid: 0d9c5b09-86fe-40ff-baab-00b7c051402f
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da35c043189954e8e4534ff640c3a9cdc9fc8078
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210278"
---
# <a name="syssql_logins-transact-sql"></a>sys.sql_logins (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]

  Devuelve una fila para cada inicio de sesión para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|--|Hereda de **Sys.server_principals**.|  
|**is_policy_checked**|**bit**|Se comprueba la directiva de contraseñas.|  
|**is_expiration_checked**|**bit**|Se comprueba la expiración de contraseñas.|  
|**password_hash**|**varbinary (256)**|Hash de la contraseña de inicio de sesión de SQL. A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], la información de la contraseña almacenada se calcula con las funciones SHA-512 de la contraseña salada.|  
  
 Para obtener una lista de las columnas que hereda esta vista, vea [sys.server_principals &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md). Las columnas `owning_principal_id` y `is_fixed_role` no se heredan de sys.server_principals.
  
## <a name="remarks"></a>Observaciones  
 Para ver los inicios de sesión [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de autenticación de y de autenticación de Windows, consulte [sys.server_principals &#40;&#41;de TRANSACT-SQL ](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Cuando los usuarios de base de datos independiente están habilitados, las conexiones se pueden realizar sin inicios de sesión. Para identificar esas cuentas, vea  [sys.database_principals &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Cualquier inicio de sesión para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede ver su propio nombre de inicio de sesión y el de sa. Para ver otros inicios de sesión, se requiere ALTER ANY LOGIN o un permiso en el inicio de sesión.  
 Para ver el contenido de la columna password_hash, se requiere el permiso CONTROL SERVER.
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Directiva de contraseñas](../../relational-databases/security/password-policy.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
