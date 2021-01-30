---
description: sys.server_principals (Transact-SQL)
title: sys.server_principals (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_principals
- sys.server_principals_TSQL
- sys.server_principals
- server_principals_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_principals catalog view
ms.assetid: c5dbe0d8-a1c8-4dc4-b9b1-22af20effd37
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d2059bf6c20da387bab7f0ea0e07ab2ad7ced79c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204860"
---
# <a name="sysserver_principals-transact-sql"></a>sys.server_principals (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Contiene una fila por cada entidad de seguridad a nivel de servidor.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre de la entidad de seguridad. Es exclusivo en el servidor.|  
|**principal_id**|**int**|Número de Id. de la entidad de seguridad. Es exclusivo en el servidor.|  
|**Junction**|**varbinary(85)**|SID (identificador de seguridad) de la entidad de seguridad. Si se trata de una entidad de seguridad de Windows, coincide con el SID de Windows.|  
|**type**|**char(1)**|Tipo de entidad de seguridad:<br /><br /> S = Inicio de sesión de SQL<br /><br /> U = Inicio de sesión de Windows<br /><br /> G = Grupo de Windows<br /><br /> R = Rol del servidor<br /><br /> C = Inicio de sesión asignado a un certificado<br /><br /> E = Inicio de sesión externo desde Azure Active Directory<br /><br /> X = grupo externo de Azure Active Directory grupo o aplicaciones<br /><br /> K = Inicio de sesión asignado a una clave asimétrica|  
|**type_desc**|**nvarchar(60)**|Descripción de los tipos de entidad de seguridad:<br /><br /> SQL_LOGIN<br /><br /> WINDOWS_LOGIN<br /><br /> WINDOWS_GROUP<br /><br /> SERVER_ROLE<br /><br /> CERTIFICATE_MAPPED_LOGIN<br /><br /> EXTERNAL_LOGIN<br /><br /> EXTERNAL_GROUP<br /><br /> ASYMMETRIC_KEY_MAPPED_LOGIN|  
|**is_disabled**|**int**|1 = Inicio de sesión deshabilitado|  
|**create_date**|**datetime**|Hora en que se creó la entidad de seguridad.|  
|**modify_date**|**datetime**|Hora en que se modificó por última vez la definición de la entidad de seguridad.|  
|**default_database_name**|**sysname**|Base de datos predeterminada para esta entidad de seguridad.|  
|**default_language_name**|**sysname**|Lenguaje predeterminado para esta entidad de seguridad.|  
|**credential_id**|**int**|Id. de una credencial asociada a esta entidad de seguridad. Si no se asocia ninguna credencial a esta entidad de seguridad, el valor de credential_id será NULL.|  
|**owning_principal_id**|**int**|**Principal_id** del propietario de un rol de servidor. Es NULL si la entidad de seguridad no es un rol fijo de servidor.|  
|**is_fixed_role**|**bit**|Devuelve 1 si la entidad de seguridad es una de las funciones de servidor integradas con permisos fijos. Para obtener más información, vea [Roles de nivel de servidor](../../relational-databases/security/authentication-access/server-level-roles.md).|  
  
## <a name="permissions"></a>Permisos  
 Cualquier inicio de sesión puede ver su propio nombre de inicio de sesión, los inicios de sesión del sistema y los roles fijos de servidor. Para ver otros inicios de sesión, se requiere ALTER ANY LOGIN o un permiso en el inicio de sesión. Para ver los roles de servidor definidos por el usuario, se requiere ALTER ANY SERVER ROLE o la pertenencia al rol.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
 La consulta siguiente enumera los permisos que se otorgan o deniegan específicamente a las entidades de seguridad de servidor.  
  
> [!IMPORTANT]  
>  Los permisos de los roles fijos de servidor (que no son públicos) no aparecen en sys.server_permissions. Por tanto, es posible que las entidades de seguridad de servidor tengan permisos adicionales que no aparezcan aquí.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pe.state_desc, pe.permission_name   
FROM sys.server_principals AS pr   
JOIN sys.server_permissions AS pe   
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)  
  
  
