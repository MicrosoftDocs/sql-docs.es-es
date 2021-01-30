---
description: sp_helpuser (Transact-SQL)
title: sp_helpuser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpuser
- sp_helpuser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpuser
ms.assetid: 9c70b41d-ef4c-43df-92da-bd534c287ca1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ce2bf386978412c6b1409b31e371f8caa15eccdb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99103173"
---
# <a name="sp_helpuser-transact-sql"></a>sp_helpuser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información acerca de las entidades de seguridad de base de datos en la base de datos actual.  
  
> [!IMPORTANT]  
>  **sp_helpuser** no devuelve información acerca de los elementos protegibles que se introdujeron en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . En su lugar, use [Sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpuser [ [ @name_in_db = ] 'security_account' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name_in_db = ] 'security_account'` Es el nombre del usuario o el rol de base de datos en la base de datos actual. *security_account* debe existir en la base de datos actual. *security_account* es de **tipo sysname y su** valor predeterminado es NULL. Si no se especifica *security_account* , **sp_helpuser** devuelve información acerca de todas las entidades de seguridad de base de datos.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 En la tabla siguiente se muestra el conjunto de resultados cuando no se especifica una cuenta de usuario ni un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usuario de o de Windows para *security_account*.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**UserName**|**sysname**|Usuarios en la base de datos actual.|  
|**RoleName**|**sysname**|Roles a los que pertenece el **nombre de usuario** .|  
|**LoginName**|**sysname**|Inicio de sesión del **nombre de usuario**.|  
|**DefDBName**|**sysname**|Base de datos predeterminada de **nombre de usuario**.|  
|**DefSchemaName**|**sysname**|Esquema predeterminado del usuario de la base de datos.|  
|**UserID**|**smallint**|IDENTIFICADOR del **nombre de usuario** en la base de datos actual.|  
|**SID**|**smallint**|Número de identificación de seguridad del usuario (SID).|  
  
 En la siguiente tabla se muestra el conjunto de resultados cuando no se especifica una cuenta de usuario y existen alias en la base de datos actual.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|Inicios de sesión asociados con los usuarios en la base de datos actual.|  
|**UserNameAliasedTo**|**sysname**|Nombre de usuario en la base de datos actual al que está asociado el inicio de sesión.|  
  
 En la tabla siguiente se muestra el conjunto de resultados cuando se especifica un rol para *security_account*.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**Role_name**|**sysname**|Nombre del rol en la base de datos actual.|  
|**Role_id**|**smallint**|Id. de rol para el rol en la base de datos actual.|  
|**Users_in_role**|**sysname**|Miembro del rol en la base de datos actual.|  
|**Deberían**|**smallint**|IDENTIFICADOR de usuario del miembro del rol.|  
  
## <a name="remarks"></a>Observaciones  
 Para obtener información acerca de la pertenencia a roles de base de datos, use [Sys.database_role_members](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md). Para ver información acerca de los miembros del rol de servidor, use [Sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)y para ver información sobre las entidades de seguridad de nivel de servidor, use [Sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
 La información mostrada está sometida a restricciones de acceso a los metadatos. No se mostrarán las entidades en las que la entidad de seguridad no tiene permiso. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-all-users"></a>A. Presentar todos los usuarios  
 En el siguiente ejemplo se presentan todos los usuarios de la base de datos actual.  
  
```  
EXEC sp_helpuser;  
```  
  
### <a name="b-listing-information-for-a-single-user"></a>B. Presentar información de un solo usuario  
 En el siguiente ejemplo se presenta información acerca del propietario de la base de datos del usuario (`dbo`).  
  
```  
EXEC sp_helpuser 'dbo';  
```  
  
### <a name="c-listing-information-for-a-database-role"></a>C. Presentar información de un rol de base de datos  
 En el siguiente ejemplo se presenta información acerca del rol fijo de base de datos `db_securityadmin`.  
  
```  
EXEC sp_helpuser 'db_securityadmin';  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)  
  
  
