---
description: sys.database_principals (Transact-SQL)
title: sys.database_principals (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_principals
- database_principals_TSQL
- sys.database_principals
- sys.database_principals_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_principals catalog view
ms.assetid: 8cb239e9-eb8c-4109-9cec-0d35de95fa0e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da1fff5e32ff60e17414cea92c48ec517de97bbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159894"
---
# <a name="sysdatabase_principals-transact-sql"></a>sys.database_principals (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila para cada entidad de seguridad de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre de la entidad de seguridad, único en la base de datos.|  
|**principal_id**|**int**|Id. de la entidad de seguridad, único en la base de datos.|  
|**type**|**char(1)**|Tipo de entidad de seguridad:<br /><br /> A = Rol de aplicación<br /><br /> C = Usuario asignado a un certificado<br /><br /> E = usuario externo de Azure Active Directory<br /><br /> G = Grupo de Windows<br /><br /> K = Usuario asignado a una clave asimétrica<br /><br /> R = Rol de base de datos<br /><br /> S = Usuario de SQL<br /><br /> U = Usuario de Windows<br /><br /> X = grupo externo de Azure Active Directory grupo o aplicaciones|  
|**type_desc**|**nvarchar(60)**|Descripción del tipo de entidad de seguridad.<br /><br /> APPLICATION_ROLE<br /><br /> CERTIFICATE_MAPPED_USER<br /><br /> EXTERNAL_USER<br /><br /> WINDOWS_GROUP<br /><br /> ASYMMETRIC_KEY_MAPPED_USER<br /><br /> DATABASE_ROLE<br /><br /> SQL_USER<br /><br /> WINDOWS_USER<br /><br /> EXTERNAL_GROUPS|  
|**default_schema_name**|**sysname**|Nombre que se utilizará cuando el nombre SQL no especifique un esquema. Null para entidades de seguridad que no son del tipo S, U o A.|  
|**create_date**|**datetime**|Hora en que se creó la entidad de seguridad.|  
|**modify_date**|**datetime**|Hora en que se modificó por última vez la entidad de seguridad.|  
|**owning_principal_id**|**int**|Id. de la entidad de seguridad propietaria de esta entidad de seguridad. De forma predeterminada, todos los roles fijos de base de datos pertenecen a **DBO** .|  
|**Junction**|**varbinary(85)**|SID (identificador de seguridad) de la entidad de seguridad.  NULL para SYS e INFORMATION SCHEMAS.|  
|**is_fixed_role**|**bit**|Si es 1, esta fila representa una entrada para uno de los roles fijos de base de datos: db_owner, db_accessadmin, db_datareader, db_datawriter, db_ddladmin, db_securityadmin, db_backupoperator, db_denydatareader, db_denydatawriter.|  
|**authentication_type**|**int**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Tipo de autenticación. A continuación se muestran los valores posibles y sus descripciones.<br /><br /> 0: sin autenticación<br />1: autenticación de la instancia<br />2: autenticación de base de datos<br />3: autenticación de Windows<br />4: autenticación Azure Active Directory|  
|**authentication_type_desc**|**nvarchar(60)**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Descripción del tipo de autenticación. A continuación se muestran los valores posibles y sus descripciones.<br /><br /> NINGUNO: sin autenticación<br />INSTANCIA: autenticación de la instancia<br />BASE de datos: autenticación de base de datos<br />WINDOWS: autenticación de Windows<br />EXTERNAL: autenticación Azure Active Directory|  
|**default_language_name**|**sysname**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Idioma predeterminado para esta entidad de seguridad.|  
|**default_language_lcid**|**int**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> LCID predeterminado para esta entidad de seguridad.|  
|**allow_encrypted_value_modifications**|**bit**|**Se aplica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]<br /><br /> Suprime las comprobaciones de metadatos criptográficos en el servidor en operaciones de copia masiva. Esto permite al usuario realizar una copia masiva de datos cifrados mediante Always Encrypted, entre tablas o bases de datos, sin descifrar los datos. El valor predeterminado es OFF. |      
  
## <a name="remarks"></a>Observaciones  
 Las propiedades de *PasswordLastSetTime* están disponibles en todas las configuraciones admitidas de SQL Server, pero las demás propiedades solo están disponibles cuando se ejecuta SQL Server en Windows Server 2003 o posterior y se habilitan tanto CHECK_POLICY como CHECK_EXPIRATION. Consulte [Directiva de contraseñas](../../relational-databases/security/password-policy.md) para obtener más información.
Los valores de la principal_id se pueden volver a usar en caso de que se hayan quitado las entidades de seguridad y, por lo tanto, no se garantice que se incremente cada vez.
  
## <a name="permissions"></a>Permisos  
 Cualquier usuario puede ver su propio nombre de usuario, los usuarios del sistema y los roles fijos de base de datos. Para ver otros usuarios, requiere ALTER ANY USER o un permiso en el usuario. Para ver los roles definidos por el usuario, se requiere ALTER ANY ROLE o la pertenencia al rol.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-all-the-permissions-of-database-principals"></a>A: Enumerar todos los permisos de entidades de seguridad de base de datos  
 La consulta siguiente enumera los permisos que se otorgan o deniegan específicamente a las entidades de seguridad de base de datos.  
  
> [!IMPORTANT]  
>  Los permisos de roles fijos de base de datos no aparecen en sys.database_permissions. Por tanto, es posible que las entidades de seguridad de base de datos tengan permisos adicionales que no aparezcan aquí.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="b-listing-permissions-on-schema-objects-within-a-database"></a>B: Enumerar permisos para objetos de esquema en una base de datos  
 La consulta siguiente se combina con sys.database_principals y sys.database_permissions para que sys.objects y sys.schemas enumeren los permisos otorgados o denegados a objetos de esquema específicos.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-listing-all-the-permissions-of-database-principals"></a>C: Mostrar todos los permisos de las entidades de seguridad de base de datos  
 La consulta siguiente enumera los permisos que se otorgan o deniegan específicamente a las entidades de seguridad de base de datos.  
  
> [!IMPORTANT]  
>  Los permisos de los roles fijos de base de datos no aparecen en `sys.database_permissions` . Por tanto, es posible que las entidades de seguridad de base de datos tengan permisos adicionales que no aparezcan aquí.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="d-listing-permissions-on-schema-objects-within-a-database"></a>D: enumerar los permisos de los objetos de esquema en una base de datos  
 La siguiente consulta se combina `sys.database_principals` y `sys.database_permissions` en `sys.objects` y `sys.schemas` para enumerar los permisos concedidos o denegados a objetos de esquema específicos.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Usuarios de bases de datos independientes: creación de una base de datos portátil](../../relational-databases/security/contained-database-users-making-your-database-portable.md)   
 [Conexión a SQL Database mediante autenticación de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview)  
  
